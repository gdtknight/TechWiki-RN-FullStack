---
type: source
title: "JavaScript Design Patterns"
authors:
  - "Hugo Di Francesco"
publisher: "Packt Publishing"
year: 2024
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2024-JavaScript Design Patterns-Hugo Di Francesco-Packt Publishing.pdf"
status: ingested
tags:
  - javascript
  - design-patterns
  - proxy-reflect
  - react-patterns
  - async-patterns
  - abort-controller
  - performance
sources: ["[[2024-JavaScript Design Patterns]]"]
created: 2026-10-19
updated: 2026-10-19
---

# JavaScript Design Patterns (2024)

> **서지 정보**: Hugo Di Francesco | Packt Publishing (2024) | 308 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2024-JavaScript Design Patterns-Hugo Di Francesco-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2024-JavaScript%20Design%20Patterns-Hugo%20Di%20Francesco-Packt%20Publishing.pdf)

---

## 핵심 요약 (Core Thesis)

고전적인 객체지향 디자인 패턴을 넘어, **모던 [[JavaScript (ECMAScript)]]의 일급 함수·클로저·ES6 모듈·`Proxy`/`Reflect`를 활용한 실용적 디자인 패턴(Part 1)** 과 **[[React]] 반응형 뷰 패턴 및 렌더링 전략(Part 2)**, 그리고 실무에서 가장 빈번히 마주치는 **고성능 비동기 제어(`AbortController`, Throttling/Debouncing/Batching), WebView-Native 브릿지 메시징 보안, 코드 스플리팅 및 워커 스레드 오프로딩(Part 3)** 을 아우르는 2024년 최신 실전 패턴집이다.

---

## 챕터별 핵심 내용 요약

### Part 1: Core JavaScript Design Patterns (Ch 1 – Ch 3, p.20–99)
- **Ch 1. Creational Design Patterns (p.22–43)**
  - `Object.create()`와 클래스 기반 Prototype 패턴, ES 모듈 스코프와 클로저를 활용한 Eager/Lazy Singleton 패턴, 팩토리 함수 패턴 ([[JavaScript Execution Context, Closures, and Prototype]]).
- **Ch 2. Structural Design Patterns (p.44–71)**
  - **ES6 `Proxy`와 `Reflect`를 활용한 Proxy 패턴 (p.45)**: 객체 속성 접근/변경을 가로채 유효성 검사, 로깅, 반응성(Reactivity — MobX/Vue 3의 기반 원리, [[React Native Architecture (Bridge to New Architecture)|Hermes 엔진]]이 지원)을 구현.
  - 고차 함수 기반 Decorator, 메모리 절약을 위한 Flyweight, 외부 API 인터페이스를 통일하는 Adapter 패턴.
- **Ch 3. Behavioral Design Patterns (p.72–98)**
  - 상태 구독/알림의 기반인 **Observer 패턴(Pub/Sub, p.73)**, 조건문 분기를 함수 맵으로 대체하는 간결한 State & Strategy 패턴(p.80), Visitor 패턴.

### Part 2: Architecture and UI Patterns (Ch 4 – Ch 6, p.100–199)
- **Ch 4. Exploring Reactive View Library Patterns (p.102–129)**
  - [[React Hooks and Component Design Patterns]]의 진화 과정 비교: Render Props 패턴(p.103) → 고차 컴포넌트(HOC, p.115) → **Hooks 패턴(p.117)** → **Provider 패턴(p.122)**.
- **Ch 5 – Ch 6. Rendering Strategies, Hydration, Micro Frontends & Islands (p.130–199)**
  - CSR/SSR/SSG 및 점진적 하이드레이션(Hydration), Next.js Zones와 Islands 아키텍처.

### Part 3: Performance and Security Patterns (Ch 7 – Ch 10, p.200–297)
- **Ch 7. Asynchronous Programming Performance Patterns (p.202–227)**
  - **[[JavaScript Event Loop and Asynchronous Patterns]] 핵심 챕터**:
    - 순차 비동기 실행 vs **`Promise.all` / `Promise.allSettled` 병렬 처리 패턴(p.208)**.
    - **`AbortController`를 활용한 비동기 요청 취소 및 타임아웃 제어(p.215)**: React/React Native에서 화면 언마운트 시 진행 중인 `fetch` 요청을 안전하게 취소해 메모리 누수와 경쟁 상태 방지.
    - **Throttling, Debouncing, Batching(p.219)**: 고빈도 스크롤/검색 입력 이벤트 제어 및 다중 요청 배칭.
- **Ch 8. Event-Driven Programming Patterns (p.228–251)**
  - 이벤트 위임(Event Delegation)과 **모바일 네이티브 앱–WebView 간 브릿지 메시징 보안 패턴(Origin 검증, 메시지 스키마 검증, p.237–249)**.
- **Ch 9 – Ch 10. Code Splitting & Off-Main-Thread Execution (p.252–297)**
  - 동적 `import()` 기반 지연 로딩과 무거운 연산을 메인(UI/JS) 스레드 밖의 Worker 스레드로 분리하는 기법.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[JavaScript Event Loop and Asynchronous Patterns]] — `Promise.all`/`allSettled`, `AbortController` 취소/타임아웃, Throttling/Debouncing/Batching (Ch 7)
- [[JavaScript Execution Context, Closures, and Prototype]] — 모듈 싱글톤, 클로저 데코레이터, `Proxy` & `Reflect` 메타프로그래밍 (Ch 1–3)
- [[React Hooks and Component Design Patterns]] — Render Props, HOC에서 Hooks 및 Provider 패턴으로의 진화 (Ch 4)

### Entities
- [[JavaScript (ECMAScript)]] — 모던 ES6+ 디자인 패턴 및 비동기 제어 API
- [[React]] — 반응형 뷰 라이브러리 디자인 패턴
