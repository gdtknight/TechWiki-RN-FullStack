---
type: entity
title: "TypeScript"
entity_type: "language"
category: "E. Programming Languages/JavaScript-TypeScript"
sources:
  - "[[2021-Mastering TypeScript Fourth Edition]]"
  - "[[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]]"
  - "[[2026-Clean Code with TypeScript]]"
  - "[[2024-React in Depth]]"
  - "[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]"
  - "[[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]"
tags:
  - typescript
  - static-typing
  - tsconfig
  - declaration-files
  - microsoft
created: 2026-10-19
updated: 2026-10-19
---

# TypeScript

## 개요

**TypeScript**는 Microsoft가 개발한 오픈소스 프로그래밍 언어로, [[JavaScript (ECMAScript)]]의 모든 문법을 포함하는 **정적 타입 슈퍼셋(Superset)** 이다 ([[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]], Ch 1 p.25–38; [[2026-Clean Code with TypeScript]], Ch 1 p.33).

코드가 실행되기 전인 **컴파일 타임(Compile-time) 및 IDE 작성 시점**에 [[TypeScript Type System and Advanced Generics|구조적 타이핑과 제네릭 추론]]을 통해 타입 오류와 `null`/`undefined` 참조 버그를 잡아내며, 트랜스파일 후에는 모든 타입 정보가 소거(Type Erasure)된 순수 JavaScript 코드를 생성한다. 현재 [[React]]와 [[React Native]](0.71+ 기본 템플릿) 프로덕션 생태계의 사실상 필수 표준 언어다.

---

## 핵심 아키텍처 및 빌드 파이프라인 역할 분담

[[2021-Mastering TypeScript Fourth Edition]] (Ch 7–8, p.202–245)과 [[2021-Do it! 리액트 네이티브 앱 프로그래밍]] (Ch 02, p.56)에 따른 TypeScript 컴파일 및 번들링 구조:

1. **타입 검사기 (`tsc --noEmit`) vs 트랜스파일러 (Babel / Metro / Vite / SWC)**:
   - 모던 [[React]] 및 [[React Native]] 프로젝트에서는 빠른 빌드와 Fast Refresh를 위해 **실제 `.ts/.tsx` → `.js` 변환(타입 어노테이션 제거)은 Metro 번들러(Babel)나 Vite(esbuild/SWC)가 수행**하고, 전체 프로젝트의 엄격한 정적 타입 검사는 IDE 언어 서버와 CI 파이프라인의 `tsc --noEmit`이 전담하는 이원화 구조를 주로 채택한다.
2. **선언 파일 (`.d.ts`, Declaration Files, `2021-Mastering TS 4e` Ch 7 p.203)**:
   - 구현 코드는 없이 타입 시그니처만 담은 파일로, 순수 JS로 작성된 라이브러리나 네이티브 에셋(`*.png`, `*.svg`)을 TypeScript 프로젝트에서 안전하게 임포트할 수 있게 해준다 (`@types/*` DefinitelyTyped 생태계).

---

## `tsconfig.json` 필수 Strict 컴파일러 옵션

[[2021-Mastering TypeScript Fourth Edition]] (Ch 8, p.226–245)과 [[2026-Clean Code with TypeScript]] (Ch 1, p.58–61)에서 프로덕션 프로젝트에 반드시 활성화하도록 권장하는 핵심 옵션:

| 옵션명 | 역할 및 방지하는 버그 유형 |
|---|---|
| **`"strict": true`** | 아래의 모든 엄격한 타입 검사 플래그들을 일괄 활성화하는 마스터 스위치 (신규 프로젝트 필수). |
| **`noImplicitAny`** | 타입이 명시되지 않고 추론도 불가능한 매개변수/변수가 암묵적으로 `any`가 되는 것을 금지하여 타입 사각지대 제거 (`2021-Mastering TS 4e` p.230). |
| **`strictNullChecks`** | `null`과 `undefined`를 모든 타입의 하위 타입에서 분리. 값이 없을 가능성이 있으면 반드시 `string \| null`로 명시하고 정제(Narrowing) 후 접근하도록 강제하여 "Cannot read properties of undefined" 런타임 에러를 원천 차단 (p.232). |
| **`noUncheckedIndexedAccess`** | 배열이나 사전 객체(`Record<string, T>`)를 인덱스로 조회할 때 결과 타입에 자동으로 `\| undefined`를 추가하여 범위 밖 인덱스 접근 사고 방지. |
| **`exactOptionalPropertyTypes`** | 선택적 속성(`prop?: string`)에 아예 키를 생략하는 것과 명시적으로 `undefined`를 할당하는 것을 엄격히 구분. |
| **`isolatedDeclarations`** *(TS 5.5+ 공식 표준)* | 다른 파일을 참조하지 않고 단일 파일만으로 `.d.ts` 선언 파일을 즉시 생성할 수 있도록 모듈 export 시 명시적 반환 타입 표기를 강제함. Nx/Turborepo 모노레포 및 Vite/SWC 병렬 빌드 속도를 극대화. |
| **`erasableSyntaxOnly`** *(TS 5.8+ 공식 표준)* | Node.js 22.6+/23+의 네이티브 TypeScript 실행(`node --experimental-strip-types`)과 100% 호환되도록, 타입 제거(Erasure)만으로 처리할 수 없고 런타임 JS 코드를 생성하는 문법(**`enum`, `namespace`, 클래스 생성자 매개변수 속성**) 사용을 컴파일 에러로 차단. |

---

## 관련 링크

- **연관 개념**: [[TypeScript Core Syntax and Full-Stack Type Annotations]], [[TypeScript Type System and Advanced Generics]], [[Type-Safe Contract-First and Runtime Boundary Architecture]], [[Full-Stack Authentication, Token Refresh, and Observability]], [[AI-Enhanced React Applications and Specification-Driven Development]]
- **연관 엔티티**: [[JavaScript (ECMAScript)]], [[React]], [[React Native]], [[TanStack Query and Zustand]]
- **출처 문헌**: [[2021-Mastering TypeScript Fourth Edition]], [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]], [[2026-Clean Code with TypeScript]], [[2026-Full-Stack React TypeScript and Node 2nd Edition]], [[2024-React in Depth]] (Ch 5–6), [[2021-Do it! 리액트 네이티브 앱 프로그래밍]], [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]], TypeScript 공식 문서 (`typescriptlang.org`)
