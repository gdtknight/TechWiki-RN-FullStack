---
type: source
title: "MongoDB 8.0 in Action, Third Edition"
authors: ["Arek Borucki"]
publisher: "Manning Publications"
year: 2025
category: "Apple Books/Database & Backend"
epub_path: "/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents/MongoDB 80 in Action Third Edition.epub"
status: ingested
tags: [mongodb, nosql, database, schema-design, aggregation-pipeline, indexing, acid-transactions, vector-search, rag, nodejs]
sources: ["[[2025-MongoDB 8.0 in Action, Third Edition]]"]
created: 2026-10-19
updated: 2026-10-19
---

# MongoDB 8.0 in Action, Third Edition

## 1. 핵심 테제 (Core Thesis)
MongoDB Champion이자 SRE 전문가인 **Arek Borucki**는 최신 **MongoDB 8.0**과 **MongoDB Atlas** 데이터 플랫폼을 통해 모던 웹 및 AI 애플리케이션의 데이터 계층을 설계·확장·운영하는 방법을 집대성한다. 과거 NoSQL이 단순 키-값이나 느슨한 JSON 저장소에 머물렀다면, 현대의 MongoDB 8.0은 **WiredTiger 엔진 기반 멀티 도큐먼트 ACID 트랜잭션**, 고성능 **집계 파이프라인(Aggregation Pipeline)**, 수평적 샤딩(Sharding), Apache Lucene 기반 **전문 검색(Atlas Search)**, 그리고 LLM/RAG 애플리케이션을 위한 **벡터 검색(Atlas Vector Search, `$vectorSearch`)** 과 실시간 스트림 처리를 단일 쿼리 API로 통합한 범용 개발자 데이터 플랫폼이다.

---

## 2. 챕터별 핵심 요약 (Chapter-by-Chapter Summary)

### Part 1: A Database for Modern Web Applications (도큐먼트 모델, 스키마 설계, 집계, 인덱싱, 트랜잭션)
- **Ch 1–3. Understanding MongoDB, Atlas & Drivers**:
  - BSON(Binary JSON) 도큐먼트 데이터 모델의 구조, MongoDB 8.0의 TCMalloc 성능 개선, MongoDB Wire Protocol, `mongosh` 셸, 그리고 **Node.js 드라이버**를 통한 비동기 연결 풀 관리.
- **Ch 4. Executing CRUD Operations**:
  - 도큐먼트 및 중첩 배열 쿼리, 연산자(`$set`, `$inc`, `$push`, `$addToSet`, `$pull`), 커서(Cursor) 순회, 대량 쓰기 최적화를 위한 `bulkWrite()`, 그리고 하위 호환성을 보장하는 MongoDB Stable API.
- **Ch 5. Designing a MongoDB Schema**:
  - **내장(Embedding, 역정규화) vs 참조(Referencing, 정규화)** 의 정량적 선택 기준(1:1, 1:Few, 1:Many, 1:Squillions 관계).
  - 실전 스키마 디자인 패턴(Subset, Extended Reference, Computed, Bucket, Outlier 패턴), `$jsonSchema`를 이용한 스키마 유효성 검증, 그리고 무한히 커지는 배열(Unbounded Arrays) 등 대표적 안티패턴 방지법.
- **Ch 6. Building Aggregation Pipelines**:
  - `$match` -> `$project` -> `$lookup`(컬렉션 간 조인) -> `$unwind`(배열 분해) -> `$group`(누산기 집계) -> `$sort`로 이어지는 데이터 변환·분석 파이프라인 설계와 최적화.
- **Ch 7. Indexing for Query Performance**:
  - 쿼리 플래너(`explain("executionStats")`) 분석, 단일/복합(Compound) 인덱스(ESR 규칙: Equality -> Sort -> Range), 멀티키(배열) 인덱스, TTL 인덱스, 부분(Partial) 인덱스, 그리고 인덱스가 오히려 성능을 저하시키는 경우.
- **Ch 8–9. Multidocument ACID Transactions, Replication & Sharding**:
  - WiredTiger 스토리지 엔진의 단일 도큐먼트 원자성과 **멀티 도큐먼트 ACID 트랜잭션(`session.withTransaction`)** 설계 시 주의점(Ch 8).
  - 레플리카 셋(Replica Set) 고가용성 장애 조치, 실시간 데이터 변경 이벤트 스트림인 **Change Streams(`watch()`)**, 그리고 MongoDB 8.0 샤딩 클러스터와 Write/Read Concern 일관성 제어(Ch 9).

### Part 2: MongoDB Atlas Data Platform (전문 검색, 벡터 검색, RAG 챗봇, 이벤트 스트리밍)
- **Ch 10–11. Database as a Service & Full-Text Search (Atlas Search)**:
  - Atlas 클러스터 티어(M0/Flex/Dedicated), Apache Lucene 역색인(Inverted Index)을 내장한 `$search` 및 `$searchMeta` 전문 검색 파이프라인 구축.
- **Ch 12–14. Semantic Techniques, Atlas Vector Search & Building RAG AI Chatbots**:
  - 텍스트 임베딩(Embedding) 원리와 **Atlas Vector Search(`$vectorSearch`)** 인덱스(HNSW/KNN 알고리즘) 생성(Ch 12).
  - Atlas CLI를 이용한 로컬 AI 개발 환경 구축(Ch 13) 및 **LangChain + MongoDB Atlas Vector Search를 결합한 RAG(검색 증강 생성) AI 챗봇** 구현(Ch 14).
- **Ch 15–19. Event-Driven Stream Processing, Data Federation, Archive & Atlas SQL**:
  - Kafka/변경 스트림과 연동하는 Atlas Stream Processing(Ch 15), S3와 MongoDB를 통합 쿼리하는 Data Federation(Ch 16), Online Archive(Ch 17), 그리고 SQL 문법(`$sql`)으로 도큐먼트를 조회하는 Atlas SQL 인터페이스(Ch 18).

### Part 3: MongoDB Security and Operations (보안 및 운영)
- **Ch 20–21. Security Features & Operational Excellence**:
  - 인증(SCRAM, X.509, AWS IAM), 역할 기반 접근 제어(RBAC), 고객 관리형 키(CMK) 암호화, 네트워크 격리(VPC Peering / PrivateLink), 백업/복구 및 성능 모니터링.

---

## 3. 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[NoSQL Document Modeling, Aggregation, and Vector Search]] — BSON 도큐먼트 모델링(Embedding vs Referencing), 스키마 디자인 패턴, 집계 파이프라인, ESR 인덱스 규칙, 멀티 도큐먼트 ACID 트랜잭션 및 `$vectorSearch` RAG 통합 (Ch 1, Ch 4–9, Ch 12–14)
- [[Agent Memory and Knowledge]] — MongoDB Atlas Vector Search와 LangChain을 결합한 벡터 스토어 및 시맨틱 RAG 파이프라인 구축 (Ch 12, Ch 14)

### Entities
- [[JavaScript (ECMAScript)]] / [[TypeScript]] — Node.js MongoDB 드라이버 및 BSON 직렬화 연동 (Ch 3–4)
