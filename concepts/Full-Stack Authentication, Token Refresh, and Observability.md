---
type: concept
title: "Full-Stack Authentication, Token Refresh, and Observability"
category: "H. Information Systems/React"
tags: [authentication, jwt, argon2, refresh-token, race-condition, axios-interceptor, opentelemetry, observability, prometheus, grafana, security, full-stack]
sources:
  - "[[2026-Full-Stack React TypeScript and Node 2nd Edition]]"
  - "[[2025-Advanced Front-End Development]]"
  - "[[2025-Open Source Observability]]"
  - "[[2021-컨테이너 인프라 환경 구축을 위한 쿠버네티스 도커]]"
  - "[[2022-Real-World Next.js]]"
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
created: 2026-10-19
updated: 2026-10-19
---

# Full-Stack Authentication, Token Refresh, and Observability

## 1. 개요 (Overview)
프로덕션 [[React]] 웹, Next.js([[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]]), 및 [[React Native]] 모바일 애플리케이션에서 가장 빈번하게 장애와 보안 사고가 발생하는 지점은 **사용자 인증(Authentication), 만료된 토큰의 자동 갱신(Token Refresh), 그리고 프론트엔드-백엔드-인프라 간 분산 추적(Observability)** 이다. [[2026-Full-Stack React TypeScript and Node 2nd Edition]] (Ch 10–14), [[2025-Advanced Front-End Development]] (Ch 15, 18), [[2025-Open Source Observability]] (Ch 1–7), [[2021-컨테이너 인프라 환경 구축을 위한 쿠버네티스 도커]] (6장), [[2022-Real-World Next.js]] (Ch 12), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 09)의 설계를 종합하여 엔터프라이즈 풀스택 인증 및 관측성 아키텍처를 정리한다.

---

## 2. 패스워드 해싱과 이중 토큰(Access / Refresh Token) 아키텍처

### 2.1 Argon2 기반 패스워드 해싱
[[2026-Full-Stack React TypeScript and Node 2nd Edition]] (Ch 10, p.279–312)은 GPU/ASIC 크래킹 공격에 취약해진 구형 `bcrypt`나 단순 SHA 해시 대신, 메모리 하드(Memory-hard) 알고리즘인 **Argon2(`argon2id`)** 를 사용해 패스워드를 해싱하고 검증할 것을 권장한다.

### 2.2 `jose` 기반 JWT 서명과 이중 토큰 분리
- **Access Token (단기 수명, 예: 5–15분)**: API 요청 인가(`Authorization: Bearer <token>`)에 사용되며, 탈취되더라도 피해 시간을 최소화하기 위해 유효기간을 짧게 설정한다. Node.js/Edge 표준 Web Crypto API와 호환되는 **`jose`** 라이브러리로 서명(`SignJWT`) 및 검증(`jwtVerify`)한다.
- **Refresh Token (장기 수명, 예: 7–30일)**: Access Token이 만료되었을 때 재로그인 없이 새 Access Token을 발급받기 위한 전용 토큰이다.
  - **웹 및 Next.js SSR 환경 보안 보관 ([[2025-Advanced Front-End Development]] Ch 15, p.305; [[2022-Real-World Next.js]] Ch 12)**: XSS 공격으로 인한 토큰 탈취를 막기 위해 Refresh Token은 자바스크립트(`localStorage`)에서 접근할 수 없는 **`HttpOnly` + `Secure` + `SameSite=Strict/Lax` 쿠키**에 보관하고, Access Token은 메모리(React 상태/클로저)에 보관하는 전략이 권장된다. Next.js SSR/RSC에서는 서버가 요청 쿠키를 즉시 검증해 비인가 사용자를 화면 깜빡임 없이 리다이렉트한다.
  - **React Native 모바일 환경 보관 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]] Ch 09, p.616)**: 쿠키 대신 디바이스 보안 스토리지(Keychain / Keystore 또는 암호화된 AsyncStorage)에 Refresh Token을 보관한다.
- **Refresh Token Rotation (토큰 순환 및 폐기)**: Refresh Token이 사용될 때마다 기존 Refresh Token을 DB에서 즉시 폐기(Revoke)하고 새 Refresh Token을 함께 발급하여, 탈취된 Refresh Token의 재사용(Replay Attack)을 감지·차단한다.

---

## 3. 클라이언트 401 자동 갱신과 동시 요청 경쟁 상태(Race Condition) 해결
Access Token이 만료된 직후 화면의 여러 컴포넌트(예: 대시보드의 프로필, 알림, 피드 쿼리)가 동시에 API를 호출하면 여러 개의 `401 Unauthorized` 응답이 동시에 돌아온다.

### 3.1 문제점: 중복 Refresh 요청으로 인한 세션 파괴
각 API 호출마다 개별적으로 `/auth/refresh`를 호출하면, 첫 번째 요청이 Refresh Token Rotation을 수행해 기존 토큰을 폐기하는 순간 두 번째 요청이 **이미 폐기된 토큰으로 재요청**하게 되어 서버가 이를 토큰 탈취로 오인하고 사용자를 강제 로그아웃시키는 경쟁 상태(Race Condition)가 발생한다([[2026-Full-Stack React TypeScript and Node 2nd Edition]] Ch 13, p.385–419).

### 3.2 해결 패턴: Single-Flight Refresh Promise (뮤텍스/대기열 패턴)
HTTP 클라이언트 래퍼(`fetch` 래퍼 또는 Axios 응답 인터셉터) 내부에 진행 중인 갱신 작업을 가리키는 단일 변수(`let refreshPromise: Promise<string | null> | null = null`)를 둔다:
1. 첫 번째 `401` 응답이 도착하면 `refreshPromise`를 생성하여 `/auth/refresh` 엔드포인트를 단 한 번만 호출한다.
2. 갱신이 진행되는 동안 도착하는 후속 `401` 요청들은 새로운 `/auth/refresh` 요청을 보내지 않고, **기존 `refreshPromise`가 완료될 때까지 `await`하며 대기열(Queue)에서 기다린다**.
3. `refreshPromise`가 새 Access Token을 반환하면 대기 중이던 모든 원래 요청들의 `Authorization` 헤더를 새 토큰으로 교체해 일제히 재시도(Retry)하고, `finally` 블록에서 `refreshPromise = null`로 초기화한다.
4. **공식 표준 보강 (`MDN Promise.withResolvers()`, ES2024)**: 기존에는 외부 대기열을 제어하기 위해 `let resolve, reject; new Promise((res, rej) => { ... })` 보일러플레이트를 작성해야 했으나, ES2024 표준 내장 메서드인 **`const { promise, resolve, reject } = Promise.withResolvers<string | null>()`** 를 사용하면 토큰 갱신 락(Lock/Deferred) 객체를 한 줄로 깔끔하고 타입 안전하게 구현할 수 있다([[JavaScript Event Loop and Asynchronous Patterns]] 참조).

---

## 4. 풀스택 관측성(Observability 2.0)과 OpenTelemetry 분산 추적
인증 및 비즈니스 API가 복잡해질수록 단순 `console.log`만으로는 프론트엔드 버튼 클릭부터 백엔드 DB 쿼리까지의 병목이나 실패 원인을 파악할 수 없다([[2026-Full-Stack React TypeScript and Node 2nd Edition]] Ch 14, p.420–455; [[2025-Open Source Observability]] Ch 1–7).

- **OpenTelemetry 3대 축 (Traces, Metrics, Logs)**:
  - **Trace & Span**: 하나의 사용자 요청이 프론트엔드 -> Express/Hono 라우터 -> 인증 미들웨어 -> Prisma 7 PostgreSQL / MongoDB 쿼리로 이어지는 전체 경로를 고유한 `traceId`로 묶고, 각 구간별 소요 시간을 `Span`으로 계측한다.
  - **구조화된 로깅 (Structured JSON Logging)**: 모든 서버 로그에 `traceId`, `spanId`, `userId`, `route`를 자동 주입하여 에러 발생 시 해당 요청의 전체 실행 타임라인을 즉각 역추적한다.
  - **Prometheus & Grafana 대시보드 ([[2021-컨테이너 인프라 환경 구축을 위한 쿠버네티스 도커]] 6장)**: Prometheus가 Pull 방식으로 수집한 시계열 메트릭을 **PromQL**로 질의하여 API 응답 지연 시간 분위수(p50, p95, p99), HTTP 상태 코드 비율(401/500 급증 감지), 쿠버네티스 파드/노드 자원 및 DB 커넥션 풀 상태를 Grafana에서 실시간 시각화한다.
- **분리형 오픈소스 관측성 스택 (Disaggregated Observability Stack, `2025-Open Source Observability` Ch 2–5)**:
  - 단일 상용 APM 벤더에 종속되는 대신, **① 수집/계측(OpenTelemetry Collector + Kafka 스트리밍 배압 제어) -> ② 고카디널리티(High Cardinality: 수억 개의 `userId`, `traceId`, `podId`) 및 고차원 JSON 분석 스토리지(ClickHouse, Apache Pinot, Prometheus) -> ③ 통합 시각화(Grafana)** 로 계층을 분리하여 대규모 트래픽에서도 비용을 절감하고 데이터 주권을 확보한다.

---

## 5. 관련 문서 (See Also)
- [[Type-Safe Contract-First and Runtime Boundary Architecture]] — 인증 및 비즈니스 API 페이로드의 런타임/컴파일 타임 타입 검증
- [[Offline-First and Mobile State Management]] — 모바일 보안 스토리지 및 오프라인 재연결 시 인증 세션 동기화
- [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]] — 쿠버네티스 클러스터 모니터링 및 컨테이너 배포
- [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]] — Next.js 서버 사이드 쿠키/세션 인증
- [[2026-Full-Stack React TypeScript and Node 2nd Edition]]
- [[2025-Open Source Observability]]
- [[2021-컨테이너 인프라 환경 구축을 위한 쿠버네티스 도커]]
- [[2025-Advanced Front-End Development]]
- [[2022-Real-World Next.js]]
- [[2021-Do it! 리액트 네이티브 앱 프로그래밍]]
