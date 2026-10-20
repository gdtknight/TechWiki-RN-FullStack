---
type: concept
title: "TypeScript Type System and Advanced Generics"
category: "E. Programming Languages/JavaScript-TypeScript"
sources:
  - "[[2021-Mastering TypeScript Fourth Edition]]"
  - "[[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]]"
  - "[[2026-Clean Code with TypeScript]]"
  - "[[2024-React in Depth]]"
tags:
  - typescript
  - type-system
  - structural-typing
  - discriminated-unions
  - generics
  - mapped-types
  - conditional-types
  - utility-types
created: 2026-10-19
updated: 2026-10-19
---

# TypeScript Type System and Advanced Generics

## 정의 (Definition)

[[TypeScript]]의 타입 시스템은 클래스 이름이나 명시적 상속 계층이 아니라 객체의 **실제 프로퍼티 구조(Shape)** 가 일치하는지로 호환성을 판단하는 **구조적 서브타이핑(Structural Subtyping)** 기반 시스템이다 ([[2021-Mastering TypeScript Fourth Edition]], Ch 2–4; [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]], Ch 1–2).

여기에 **유니온/교차 타입(Algebraic Data Types)**, **제네릭(Generics)**, 그리고 타입을 입력받아 새로운 타입을 계산해내는 **메타 타입 프로그래밍(Mapped Types, Conditional Types, `infer`, Template Literal Types)** 이 결합되어, [[React]] 및 [[React Native]]의 복잡한 컴포넌트 Props, Hooks, API 응답 스키마를 컴파일 타임에 완벽하게 검증한다 ([[2024-React in Depth]], Ch 5–6 p.143–204).

---

## 1. 안전한 타입 계층과 판별 유니온 (Discriminated Unions)

### 1.1 `any` vs `unknown` vs `never` ([[2021-Mastering TypeScript Fourth Edition]], p.60–68)
- **`any` (타입 검사 포기 — 안티패턴)**: 모든 타입 검사를 무력화하여 런타임 에러를 유발한다 ([[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]], Ch 10 p.376).
- **`unknown` (타입 안전한 최상위 Top Type)**: 어떤 값이든 담을 수 있지만, `typeof`, `instanceof`, 사용자 정의 타입 가드(`x is User`)나 스키마 파서를 통해 **타입을 좁히기(Type Narrowing) 전에는 프로퍼티 접근이나 호출을 허용하지 않는다**.
- **`never` (공집합 Bottom Type)**: 절대 발생할 수 없는 값을 나타내며, `switch`문이나 조건문에서 모든 유니온 케이스를 다 처리했는지 컴파일 타임에 검증하는 **완전성 검사(Exhaustiveness Checking)** 에 활용된다.

### 1.2 판별 유니온 (Discriminated Unions / Tagged Unions)
[[2024-React in Depth]] (Ch 5, p.148), [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]] (Ch 2, p.75), [[2026-Clean Code with TypeScript]] (Ch 12, p.416)가 모두 **가장 중요한 실무 패턴 1순위**로 꼽는 기법이다:

```ts
type AsyncState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: Error };
```
- `data?: T; error?: Error; isLoading: boolean;`처럼 옵셔널 필드를 나열하면 "로딩 중이면서 동시에 에러와 데이터가 모두 있는" 불가능한 상태(Impossible State)가 허용된다.
- 반면 공통 리터럴 판별자(`status`)를 가진 **판별 유니온**으로 선언하면, `if (state.status === "success")` 분기 안에서만 `state.data` 접근이 자동 허용되어 불가능한 상태를 원천 봉쇄한다.

---

## 2. 고급 제네릭, Mapped Types, Conditional Types

[[2021-Mastering TypeScript Fourth Edition]] (Ch 4, p.132–157)과 [[2026-Clean Code with TypeScript]] (Ch 9, p.291–317)에서 정리하는 타입 변환 4대 도구:

| 기법 | 문법 구조 및 핵심 역할 | 대표 내장 유틸리티 타입 |
|---|---|---|
| **Generic Constraints (제네릭 제약)** | `<T extends object, K extends keyof T>(obj: T, key: K): T[K]` — 타입 변수가 특정 인터페이스나 객체의 키 집합(`keyof`) 안에 속하도록 강제 | 프로퍼티 안전 접근 함수, 제네릭 리스트/페이지네이션 컴포넌트 (`2024-React in Depth` p.195) |
| **Mapped Types (매핑된 타입)** | `{ [P in keyof T]?: T[P] }` — 기존 객체 타입의 키들을 순회하며 `readonly`나 선택적(`?`) 수식어를 일괄 변환 (키 재매핑 `as` 지원) | `Partial<T>`, `Required<T>`, `Readonly<T>`, `Pick<T, K>`, `Omit<T, K>`, `Record<K, V>` |
| **Conditional Types (조건부 타입)** | `T extends U ? X : Y` — 타입 삼항 연산자로 입력 타입이 특정 조건을 만족하는지에 따라 분기 | `Exclude<T, U>`, `Extract<T, U>`, `NonNullable<T>` |
| **Type Inference (`infer`)** | `T extends (...args: any[]) => infer R ? R : any` — 조건부 타입 내부에서 함수 반환값, 배열 요소, Promise 내부 값의 타입을 패턴 매칭으로 추출 | `ReturnType<T>`, `Parameters<T>`, `Awaited<T>` |

---

## 3. TypeScript 5+ 핵심 기법: `satisfies`, `const` Generics, Branded Types

[[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]] (Ch 1–2, p.33–74)에서 강조하는 최신 실전 기법:

1. **`satisfies` 연산자 (p.33)**:
   - 변수에 `: Record<string, string | number>` 타입 주석을 달면 각 속성의 구체적인 타입 정보가 넓은 유니온으로 뭉개지지만, `const config = { port: 8080, host: "localhost" } satisfies Record<string, string | number>`를 쓰면 **규격 준수 여부를 검증하면서도 `config.port`가 `number`라는 좁은 추론 결과를 그대로 보존**한다.
2. **`const` Type Parameters (p.35)**:
   - `function createRoutes<const T extends readonly string[]>(routes: T)`처럼 선언하면 호출 측에서 `as const`를 붙이지 않아도 배열 요소를 리터럴 튜플 타입으로 정확히 추론한다.
3. **Branded Types (명목적 타이핑 구현, p.69)**:
   - 구조적 타이핑에서는 `type UserId = string`과 `type OrderId = string`이 서로 섞여 들어가는 실수를 막지 못한다. `type Brand<K, T> = K & { __brand: T }`를 통해 `UserId`와 `OrderId`를 컴파일 타임에 완전히 다른 타입으로 격리할 수 있다.
4. **공식 문서(`typescriptlang.org`) 최신 보강 — `NoInfer<T>` (TS 5.4) & 자동 타입 서술어 추론 (TS 5.5)**:
   - **`NoInfer<T>` 내장 유틸리티 타입 (TS 5.4)**: `function createStreetLight<C extends string>(colors: C[], defaultColor: NoInfer<C>)`처럼 선언하면, 두 번째 인자(`defaultColor`)에 오타가 들어왔을 때 전체 제네릭 `C`의 추론 범위가 원치 않게 넓어지는(Widening) 현상을 차단하고 첫 번째 인자(`colors`)로부터 추론된 타입을 기준으로만 엄격히 검증한다.
   - **Inferred Type Predicates (타입 서술어 자동 추론, TS 5.5)**: 구형 도서(`2021-Mastering TS 4e`)에서는 배열에서 `null`/`undefined`를 걸러낼 때 `items.filter((x): x is NonNullable<typeof x> => x !== null)`처럼 수동 타입 가드를 작성해야만 반환 타입이 정제되었다. **TypeScript 5.5 공식 릴리스**부터는 단순히 `items.filter(x => x !== null)`만 작성해도 컴파일러가 제어 흐름을 분석해 자동으로 `x is T` 타입 서술어를 추론하고 결과 배열 타입에서 `null`을 제거한다.

---

## 관련 개념 및 엔티티

- **선행/연관 개념**: [[TypeScript Core Syntax and Full-Stack Type Annotations]], [[Type-Safe Contract-First and Runtime Boundary Architecture]], [[React Hooks and Component Design Patterns]], [[JavaScript Execution Context, Closures, and Prototype]]
- **구현 기술/엔티티**: [[TypeScript]], [[React]], [[React Native]]
- **출처 문헌**: [[2021-Mastering TypeScript Fourth Edition]] (Ch 2–4), [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]] (Ch 1–2, Ch 7, Ch 10), [[2026-Clean Code with TypeScript]] (Ch 1, Ch 9, Ch 12), [[2024-React in Depth]] (Ch 5–6), [[2026-Full-Stack React TypeScript and Node 2nd Edition]] (Ch 1–2), TypeScript 공식 문서 (`typescriptlang.org`)
