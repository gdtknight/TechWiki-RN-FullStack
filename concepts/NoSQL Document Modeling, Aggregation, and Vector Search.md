---
type: concept
title: "NoSQL Document Modeling, Aggregation, and Vector Search"
category: "H. Information Systems/Database"
tags: [mongodb, nosql, postgresql, schema-design, aggregation-pipeline, indexing, acid-transactions, vector-search, rag, full-stack]
sources:
  - "[[2025-MongoDB 8.0 in Action, Third Edition]]"
  - "[[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]"
  - "[[2026-Full-Stack React TypeScript and Node 2nd Edition]]"
  - "[[2026-Build AI-Enhanced Web Apps]]"
created: 2026-10-19
updated: 2026-10-19
---

# NoSQL Document Modeling, Aggregation, and Vector Search

## 1. 개요 (Overview)
풀스택 개발에서 데이터 영속화 계층은 크게 **관계형 데이터베이스(RDBMS: PostgreSQL + Prisma 7)** 와 **도큐먼트 지향 NoSQL 데이터베이스(MongoDB 8.0)** 의 두 축으로 나뉜다([[2026-Full-Stack React TypeScript and Node 2nd Edition]] Ch 10; [[2025-MongoDB 8.0 in Action, Third Edition]] Ch 1).
- 관계형 DB가 데이터를 정규화(Normalization)된 테이블에 나누어 담고 읽기 시점에 `JOIN`으로 조립한다면, **MongoDB**는 애플리케이션의 [[JavaScript (ECMAScript)|JavaScript]]/[[TypeScript]] 객체 구조와 1:1로 매핑되는 **BSON(Binary JSON) 도큐먼트** 단위로 함께 읽히는 데이터를 한곳에 모아 저장함으로써 읽기 성능과 수평적 확장성(Sharding)을 극대화한다.

---

## 2. 도큐먼트 스키마 설계: 내장(Embedding) vs 참조(Referencing)

[[2025-MongoDB 8.0 in Action, Third Edition]] (Ch 5)는 "MongoDB는 스키마가 없는(Schemaless) 것이 아니라 **유연한 동적 스키마(Dynamic Schema)를 애플리케이션 접근 패턴(Workload)에 맞춰 설계하는 것**"이라고 강조한다.

| 비교 항목 | 내장 (Embedding / 역정규화) | 참조 (Referencing / 정규화) |
| :--- | :--- | :--- |
| **구조** | 부모 도큐먼트 내부에 중첩 객체나 서브도큐먼트 배열로 함께 저장 | 별도 컬렉션으로 분리하고 `ObjectId` 참조만 저장 (필요 시 `$lookup` 조인) |
| **적합한 관계** | `1:1`, `1:Few` (예: 사용자의 배송지 목록, 게시글의 태그 배열) | `1:Many (수천 개 이상)`, `1:Squillions`, `N:M` (예: 상품에 달린 10만 개 리뷰, IoT 센서 로그) |
| **장점** | **단 한 번의 디스크 I/O로 전체 데이터 조회 가능**, 단일 도큐먼트 수준에서 100% 원자적(Atomic) 업데이트 보장 | 도큐먼트 최대 크기 제한(**16MB**) 초과 방지, 자주 변경되는 공유 데이터의 중복 갱신 방지 |
| **치명적 안티패턴** | **무한 증식 배열(Unbounded Array)**: 댓글이나 로그를 한 도큐먼트 배열에 끝없이 `$push`하면 16MB 한계에 도달하거나 인덱스/메모리 성능이 붕괴함 | 함께 항상 조회되는 `1:1` 데이터를 RDBMS 습관대로 무조건 쪼개어 매번 `$lookup`을 남발하는 것 |

### 실전 5대 스키마 디자인 패턴 (`Ch 5.3`)
1. **Subset Pattern (부분집합 패턴)**: 상품 도큐먼트에는 **가장 최근 리뷰 10개만 내장(Embed)** 하여 첫 화면을 즉시 렌더링하고, 전체 리뷰는 별도 `reviews` 컬렉션에 참조로 분리한다.
2. **Extended Reference Pattern (확장 참조 패턴)**: 주문(`orders`) 도큐먼트에 고객 ID만 넣는 대신, 주문 화면에 늘 함께 표시되는 고객 이름과 연락처 일부만 복사해 넣어 `$lookup` 조인을 제거한다.
3. **Computed Pattern (사전 계산 패턴)**: 조회할 때마다 수만 건의 리뷰 평점을 `AVG()`로 계산하는 대신, 새 리뷰가 작성될 때 상품 도큐먼트의 `ratingSum`, `reviewCount`, `avgRating` 필드를 `$inc`로 미리 갱신한다.
4. **Bucket Pattern (버킷 패턴)**: 시계열(Time-series) 데이터나 채팅 메시지를 1건당 1도큐먼트가 아니라 '1시간 단위' 또는 '50개 메시지 단위' 버킷 도큐먼트로 묶어 인덱스 크기를 줄인다.
5. **Schema Validation (`$jsonSchema`)**: 동적 스키마의 오염을 막기 위해 컬렉션 수준에서 `$jsonSchema` 규칙을 설정하고, 애플리케이션 경계에서는 [[Type-Safe Contract-First and Runtime Boundary Architecture|Zod 런타임 검증]]을 결합한다.

---

## 3. 집계 파이프라인(Aggregation Pipeline)과 ESR 인덱싱 규칙

### 3.1 집계 파이프라인 (`Ch 6`)
Unix 파이프(`|`)처럼 여러 스테이지를 순차 통과하며 도큐먼트를 필터링·조인·그룹화·변환한다:
```typescript
const topCategories = await db.collection('orders').aggregate([
  // 1. $match: 인덱스(Index)를 타도록 반드시 파이프라인 최상단에 배치!
  { $match: { status: 'PAID', createdAt: { $gte: new Date('2026-01-01') } } },
  // 2. $unwind: 주문 내 상품 배열(items)을 개별 도큐먼트로 펼침
  { $unwind: '$items' },
  // 3. $group: 카테고리별 매출 합계($sum)와 주문 건수 집계
  {
    $group: {
      _id: '$items.category',
      totalRevenue: { $sum: { $multiply: ['$items.price', '$items.qty'] } },
      orderCount: { $sum: 1 },
    },
  },
  // 4. $sort & $limit: 매출 상위 5개 카테고리 추출
  { $sort: { totalRevenue: -1 } },
  { $limit: 5 },
]).toArray();
```

### 3.2 복합 인덱스 설계의 황금률: ESR 규칙 (`Ch 7`)
복합 인덱스(Compound Index)의 필드 순서를 정할 때는 반드시 **E-S-R 순서**를 따른다:
1. **E (Equality — 동등 비교)**: `{ status: 'ACTIVE' }`처럼 정확히 일치하는 필드를 맨 앞에 둔다.
2. **S (Sort — 정렬)**: `{ createdAt: -1 }`처럼 정렬 기준 필드를 두 번째에 두어 메모리 내 정렬(In-memory Sort, 100MB 제한)을 방지한다.
3. **R (Range — 범위 조건)**: `{ price: { $gte: 1000, $lte: 5000 } }`처럼 범위 스캔 필드를 마지막에 배치한다.

---

## 4. 멀티 도큐먼트 ACID 트랜잭션과 Change Streams (`Ch 8–9`)

- **WiredTiger 멀티 도큐먼트 ACID 트랜잭션**: 단일 도큐먼트 수정은 기본적으로 원자적이지만, 계좌 이체나 주문-재고 차감처럼 여러 컬렉션을 동시에 수정해야 할 때는 `session.withTransaction(async () => { ... })`으로 RDBMS와 동일한 ACID 트랜잭션을 수행한다. (단, 트랜잭션 실행 시간은 기본적으로 60초 이내로 짧게 유지해야 락 경합을 피할 수 있다.)
- **Change Streams (`collection.watch()`)**: 오플로그(oplog) 기반으로 컬렉션의 삽입·수정·삭제 이벤트를 실시간 푸시 스트림으로 구독하여, 폴링(Polling) 없이 웹소켓 알림이나 캐시 무효화를 트리거한다.

---

## 5. MongoDB Atlas Vector Search와 RAG AI 챗봇 통합 (`Ch 12–14`)

[[2025-MongoDB 8.0 in Action, Third Edition]] (Ch 12, Ch 14)와 [[2026-Build AI-Enhanced Web Apps]] (Ch 7)가 보여주듯, 별도의 전용 벡터 DB(Pinecone 등)를 추가해 인프라 복잡도와 데이터 동기화 부담을 늘리는 대신, **운영 도큐먼트와 벡터 임베딩(`embedding: number[]`)을 동일한 MongoDB 컬렉션에 함께 저장**하고 **`$vectorSearch` 집계 스테이지**로 하이브리드 검색을 수행할 수 있다:
- **사전 필터링(Pre-filtering) + HNSW 코사인 유사도 검색**: 사용자의 권한(`tenantId`), 날짜 범위 등 일반 BSON 필드 필터링과 고차원 벡터 근사 최근접 이웃(ANN) 검색을 단일 쿼리(`$vectorSearch`)로 처리하여 [[Agent Memory and Knowledge|RAG 파이프라인]]과 LangChain.js 에이전트에 즉시 컨텍스트를 공급한다.

---

## 6. 관련 문서 (See Also)
- [[TypeScript Core Syntax and Full-Stack Type Annotations]] — 백엔드 데이터베이스 엔티티 및 DTO 타이핑
- [[Type-Safe Contract-First and Runtime Boundary Architecture]] — DB 입출력 및 API 페이로드의 런타임 스키마 검증
- [[Agent Memory and Knowledge]] — 벡터 임베딩 및 RAG 지식 검색 아키텍처
- [[2025-MongoDB 8.0 in Action, Third Edition]]
- [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]
- [[2026-Full-Stack React TypeScript and Node 2nd Edition]]
