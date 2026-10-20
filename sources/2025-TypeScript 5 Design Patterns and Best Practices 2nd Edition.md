---
type: source
title: "TypeScript 5 Design Patterns and Best Practices, 2nd Edition"
authors:
  - "Theofanis Despoudis"
publisher: "Packt Publishing"
year: 2025
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition-Theofanis Despoudis-Packt Publishing.pdf"
status: ingested
tags:
  - typescript
  - typescript-5
  - design-patterns
  - functional-programming
  - monads
  - solid
  - ddd
  - trpc
  - apollo-client
sources: ["[[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]]"]
created: 2026-10-19
updated: 2026-10-19
---

# TypeScript 5 Design Patterns and Best Practices, 2nd Edition (2025)

> **서지 정보**: Theofanis Despoudis | Packt Publishing (2025) | 424 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition-Theofanis Despoudis-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2025-TypeScript%205%20Design%20Patterns%20and%20Best%20Practices%202nd%20Edition-Theofanis%20Despoudis-Packt%20Publishing.pdf)

---

## 핵심 요약 (Core Thesis)

**[[TypeScript]] 5의 최신 타입 시스템 기능(`satisfies`, `const` 타입 파라미터, 표준 데코레이터, Branded Types, 판별 유니온)** 을 바탕으로 GoF 디자인 패턴(Creational, Structural, Behavioral)과 **함수형 프로그래밍(불변성, Lenses, `Option`/`Either` 모나드)**, **반응형 비동기 패턴**, **DDD & SOLID 원칙**, 그리고 실제 오픈소스(**Apollo Client**와 **tRPC**)에 녹아 있는 아키텍처 패턴까지 집대성한 2025년 심화서다.

---

## 챕터별 핵심 내용 요약

### Part 1: TypeScript 5 핵심 원리와 고급 타입 (Ch 1 – Ch 2, p.22–103)
- **Ch 1. Getting Started with TypeScript 5 (p.24–65)**
  - **TypeScript 5 핵심 신기능 (p.33–36)**:
    - **`satisfies` 연산자**: 타입 추론의 구체성(Narrowing)을 잃지 않으면서 특정 인터페이스 규격을 만족하는지 검증.
    - **`const` Type Parameters**: 제네릭 함수 호출 시 `as const` 없이도 리터럴 타입을 그대로 추론.
    - ECMAScript Stage 3 표준 데코레이터 지원.
- **Ch 2. TypeScript 5 Core Principles (p.66–103)**
  - **[[TypeScript Type System and Advanced Generics]] 심화**: 조건부 타입, **Branded Types (명목적 타이핑 흉내 내기, p.69)**, 유틸리티 타입, **판별 유니온(Discriminated Unions)과 Exhaustiveness Checking(`never` 활용, p.75)**.

### Part 2: TypeScript Core Design Patterns (Ch 3 – Ch 6, p.104–259)
- **Ch 3. Creational Design Patterns (p.106–151)**
  - Singleton, Prototype, **타입 안전한 Builder 패턴(메서드 체이닝과 제네릭 상태 추적, p.128)**, Factory Method, Abstract Factory.
- **Ch 4. Structural Design Patterns (p.152–199)**
  - Adapter, TS 5 표준 Decorator(p.160), Facade, Composite, Proxy, Bridge, Flyweight.
- **Ch 5 – Ch 6. Behavioral Design Patterns (p.200–259)**
  - Strategy, Chain of Responsibility, Command, Mediator, Observer(p.222), Iterator, Memento, State(p.242), Template Method, Visitor.

### Part 3: 함수형/반응형 프로그래밍, 안티패턴, 오픈소스 아키텍처 (Ch 7 – Ch 11, p.260–409)
- **Ch 7. Functional Programming with TypeScript (p.262–307)**
  - 순수 함수, 불변성, 고차 함수, 함수 합성, 중첩된 불변 상태를 우아하게 갱신하는 **Functional Lenses(p.284)**, 그리고 `null`/예외 처리를 타입으로 안전하게 캡슐화하는 **Monads (`Maybe`/`Option`, `Either`, p.299)**.
- **Ch 8. Reactive and Asynchronous Programming (p.308–337)**
  - Push vs Pull 모델, Promises/Futures, RxJS Observables를 활용한 비동기 이벤트 스트림 제어 ([[JavaScript Event Loop and Asynchronous Patterns]]).
- **Ch 9 – Ch 10. SOLID, DDD, Anti-Patterns and Gotchas (p.338–389)**
  - 유틸리티 타입과 DDD(도메인 주도 설계), SOLID 5대 원칙 적용(Ch 9).
  - **TypeScript 주요 안티패턴 (Ch 10, p.370–388)**: 불필요한 클래스 남용(Class overuse), `any`/`Function`/`Object` 같은 과도하게 느슨한 타입(Overly permissive types), 제네릭 남용 및 타입 추론 함정.
- **Ch 11. Design Patterns in Open Source Architectures: Apollo Client & tRPC (p.390–408)**
  - [[Offline-First and Mobile State Management]]에서 쓰이는 **Apollo Client** 내부의 Observer/Normalized Cache/Link Chain 패턴(p.398)과, 코드 생성 없이 백엔드 라우터 타입을 프런트엔드로 즉시 공유하는 **tRPC**의 Builder/Proxy/Adapter 패턴 분석(p.402) ([[Type-Safe Contract-First and Runtime Boundary Architecture]]).

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[TypeScript Type System and Advanced Generics]] — `satisfies`, `const` 타입 파라미터, Branded Types, 판별 유니온(`never` 완전성 검사), 안티패턴 (Ch 1–2, Ch 10)
- [[Type-Safe Contract-First and Runtime Boundary Architecture]] — tRPC와 Apollo Client를 통한 엔드투엔드 타입 공유 아키텍처 (Ch 11)

### Entities
- [[TypeScript]] — TypeScript 5 언어 스펙과 디자인 패턴/함수형 패러다임 통합
