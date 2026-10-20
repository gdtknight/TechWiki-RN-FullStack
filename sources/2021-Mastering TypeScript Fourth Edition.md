---
type: source
title: "Mastering TypeScript, Fourth Edition"
authors:
  - "Nathan Rozentals"
publisher: "Packt Publishing"
year: 2021
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2021-Mastering TypeScript Fourth Edition-Nathan Rozentals-Packt Publishing.pdf"
status: ingested
tags:
  - typescript
  - type-system
  - generics
  - declaration-files
  - tsconfig
  - strict-mode
  - jest
sources: ["[[2021-Mastering TypeScript Fourth Edition]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Mastering TypeScript, Fourth Edition (2021)

> **서지 정보**: Nathan Rozentals | Packt Publishing (2021) | 539 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2021-Mastering TypeScript Fourth Edition-Nathan Rozentals-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2021-Mastering%20TypeScript%20Fourth%20Edition-Nathan%20Rozentals-Packt%20Publishing.pdf)

---

## 핵심 요약 (Core Thesis)

엔터프라이즈 환경에서 **[[TypeScript]]의 타입 시스템 기초부터 고급 타입 추론(`keyof`, Mapped Types, Conditional Types, `infer`), 서드파티 JS 라이브러리용 선언 파일(`.d.ts`) 작성법, 그리고 `tsconfig.json`의 Strict 컴파일러 옵션 체계**까지 빈틈없이 다루는 종합 바이블이다.

특히 Ch 7(Declaration Files)과 Ch 8(Strict Compiler Options)은 [[React Native]] 프로젝트에서 타입 정의가 없는 레거시 네이티브/JS 패키지를 연동하거나 컴파일러 에러를 디버깅할 때 결정적인 실무 지식을 제공한다 ([[TypeScript Type System and Advanced Generics]] 참조).

---

## 챕터별 핵심 내용 요약

### 1. 타입 시스템 코어, OOP, 제네릭과 고급 타입 추론 (Ch 1 – Ch 6)
- **Ch 1 – Ch 2. TypeScript 기초와 원시/유니온/튜플 타입 (p.22–91)**
  - 기본 타입 주석(Type Annotation), 타입 추론(Type Inference), `any` vs `unknown` vs `never` (p.60–68), 유니온 타입(`|`), 타입 가드(Type Guards), 타입 별칭(`type`), 열거형(`enum` / `const enum`), 객체 스프레드와 튜플(Tuples).
- **Ch 3. Interfaces, Classes, Inheritance, and Modules (p.92–131)**
  - 인터페이스(`interface`), 선택적/읽기 전용(`readonly`) 속성, 클래스 접근 제어자(`public`, `private`, `protected`), 추상 클래스(`abstract`), `instanceof`와 사용자 정의 타입 가드(`is`), ES 모듈(`export`/`import`).
- **Ch 4. Generics and Advanced Type Inference (p.132–157)**
  - **[[TypeScript Type System and Advanced Generics]] 핵심 기초**:
    - 제네릭 함수/인터페이스/클래스와 `keyof` 제약 조건(`T extends keyof U`, p.138).
    - **Mapped Types (매핑된 타입, p.143)**: `Partial<T>`, `Readonly<T>`, `Pick<T, K>`, `Record<K, T>`의 내부 구현 원리.
    - **Conditional Types & `infer` (조건부 타입과 타입 추론, p.147–156)**: `T extends U ? X : Y` 구조와 `infer` 키워드를 통해 함수의 매개변수 타입이나 Promise 반환 타입을 추출하는 기법.
- **Ch 5 – Ch 6. 비동기 프로그래밍(`Promise`, `async`/`await`) 및 데코레이터 (p.158–201)**
  - 제네릭 `Promise<T>` 타이핑과 메타데이터 리플렉션(`reflect-metadata`) 기반 클래스/메서드/프로퍼티 데코레이터.

### 2. JS 생태계 통합(`.d.ts`), Strict 옵션, 풀스택 프레임워크 (Ch 7 – Ch 16)
- **Ch 7. Integration with JavaScript (p.202–225)**
  - **선언 파일 (`.d.ts`, Declaration Files)**: 순수 JavaScript로 작성된 외부 모듈이나 전역 변수(`declare var`, `declare module`, DefinitelyTyped `@types/*`)에 정적 타입 인터페이스를 부여하는 방법.
- **Ch 8. Strict Compiler Options (p.226–245)**
  - `tsconfig.json` 상속(`extends`)과 **`"strict": true` 산하 핵심 플래그 상세 분석**: `noImplicitAny`, `strictNullChecks`, `strictFunctionTypes`, `strictBindCallApply`, `strictPropertyInitialization`, `noImplicitThis`, `noUnusedLocals`.
- **Ch 9 – Ch 10. RxJS Observables & Jest TDD (p.246–309)**
  - 반응형 스트림(`Observable`, `Subject`)과 Jest 기반 단위/비동기 테스트.
- **Ch 11 – Ch 16. 프런트엔드(React, Angular, Vue) 및 백엔드(Express, AWS Serverless) 실전 (p.310–525)**
  - Ch 12(p.348–381)에서 TypeScript 기반 [[React]] 컴포넌트 Props/State/이벤트 타이핑을 다룬다.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[TypeScript Core Syntax and Full-Stack Type Annotations]] — 원시/튜플/유니온 기본 타입 표기, `interface` vs `type`, 함수/클래스 타이핑, 5대 타입 좁히기 및 내장 유틸리티 타입 (Ch 1–4, Ch 12)
- [[TypeScript Type System and Advanced Generics]] — `unknown` vs `never`, 사용자 정의 타입 가드(`is`), 제네릭 제약(`keyof`), Mapped Types, Conditional Types와 `infer` (Ch 2–4)

### Entities
- [[TypeScript]] — `tsconfig.json` Strict 컴파일러 옵션 체계와 `.d.ts` 선언 파일 생태계 (Ch 7–8)
