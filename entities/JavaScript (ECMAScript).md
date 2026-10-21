---
type: entity
title: "JavaScript (ECMAScript)"
entity_type: "language"
category: "E. Programming Languages/JavaScript-TypeScript"
sources:
  - "[[2015-인사이드 자바스크립트]]"
  - "[[2021-JavaScript from Beginner to Professional]]"
  - "[[2024-JavaScript Design Patterns]]"
  - "[[2022-React and React Native, Fourth Edition]]"
tags:
  - javascript
  - ecmascript
  - es6
  - v8
  - hermes
  - javascriptcore
created: 2026-10-19
updated: 2026-10-21
---

# JavaScript (ECMAScript)

## 개요

**JavaScript (표준 명칭: ECMAScript)** 는 웹 브라우저, 서버(Node.js/Deno/Bun), 그리고 [[React Native]]를 통한 모바일 네이티브 앱의 비즈니스 로직을 구동하는 **동적 타입 기반의 멀티패러다임(프로토타입 기반 객체지향 + 일급 함수 기반 함수형) 프로그래밍 언어**다 ([[2015-인사이드 자바스크립트]], Ch 1 p.16–21; [[2021-JavaScript from Beginner to Professional]], Ch 1 p.26).

단일 스레드 콜 스택 위에서 [[JavaScript Event Loop and Asynchronous Patterns|이벤트 루프(Event Loop)와 비동기 Promise]]로 동작하며, 대규모 애플리케이션 개발 시에는 정적 타입 슈퍼셋인 [[TypeScript]]와 함께 사용되는 것이 현대 표준이다.

---

## 호스트 환경별 JavaScript 런타임 엔진 비교

JavaScript 코드는 실행되는 플랫폼에 따라 서로 다른 가상 머신(Engine) 위에서 실행되며, 특히 [[React Native]]를 공부할 때는 브라우저 엔진(V8)과 모바일 전용 엔진(**Hermes**)의 차이를 이해하는 것이 중요하다 ([[2022-React and React Native, Fourth Edition]], Ch 15 p.314):

| JS 엔진 | 개발사 및 주요 채택 환경 | 컴파일/실행 방식 | 핵심 특징 및 모바일 영향 |
|---|---|---|---|
| **V8** | Google (Chrome, Node.js, Deno, Android 구형 옵션) | JIT (Just-In-Time) 컴파일러 중심 | 장기 실행 서버 및 데스크톱 웹에서 최고 수준의 처리량 발휘. 단, JIT 워밍업 시간과 메모리 사용량이 커서 모바일 앱 초기 구동에는 부담이 될 수 있음. |
| **JavaScriptCore (JSC)** | Apple (Safari, iOS 기본 WebKit, 구형 React Native 기본 엔진) | 인터프리터 + 다단계 JIT (iOS 서드파티 앱에서는 보안 정책상 JIT 비활성화 가능) | [[React Native]]는 초기에 JSCore만 지원했음 (`2022-RN 4e` PDF p.314). 0.70부터 Hermes가 기본 엔진이 되었고 (RN 0.84 릴리스 글: "Hermes (the default since 0.70)"), 0.79부터 JSC는 커뮤니티 패키지 `@react-native-community/javascriptcore`로 이관됨. 런타임에 JS 소스를 파싱하므로 번들이 클수록 앱 시작(TTI)이 지연됨. |
| **Hermes** | Meta (**모던 [[React Native]] Android/iOS 표준 엔진**) | **빌드 타임 AOT(Ahead-Of-Time) 바이트코드 프리컴파일** | 빌드 시점에 JS를 바이트코드로 미리 컴파일한다. 도서가 꼽는 개선점은 **앱 시작 시간 단축, 다운로드 용량 축소, 메모리 사용 감소**, 그리고 `react-native-firebase`·`mobx`를 쓸 수 있게 하는 내장 `Proxy` 지원이다 (`2022-RN 4e` PDF p.314). 0.84부터 **Hermes V1**이 iOS·Android 기본 엔진이다 ([[2026-React Native Official Documentation]]). |

---

## 모듈 시스템 비교: CommonJS vs ES Modules (ESM)

[[2026-Clean Code with TypeScript]] (Ch 4, p.121–126)와 [[2024-JavaScript Design Patterns]] (Ch 9, p.252)에서 다루는 두 모듈 표준:

- **CommonJS (`require` / `module.exports`)**: 레거시 Node.js 및 일부 번들러 설정 파일에서 사용되는 동적·동기식 모듈 로드 방식. 런타임에 조건부 로드가 가능하나 정적 분석이 어려워 미사용 코드를 제거하는 **트리 셰이킹(Tree Shaking)** 이 불가능하다.
- **ES Modules (`import` / `export`, ESM)**: ECMAScript 공식 표준 모듈 시스템. 파일 최상단에서 의존성 그래프가 정적으로 확정되므로 Vite, Metro, Webpack 등의 번들러가 **트리 셰이킹과 코드 스플리팅(`import()`)** 을 효과적으로 수행할 수 있다 ([[2026-Clean Code with TypeScript]], p.125, p.239).

---

## 관련 링크

- **연관 개념**: [[JavaScript Core Syntax and Modern ES6+ Fundamentals]], [[JavaScript Execution Context, Closures, and Prototype]], [[JavaScript Event Loop and Asynchronous Patterns]], [[React Native Architecture (Bridge to New Architecture)]]
- **연관 엔티티**: [[TypeScript]], [[React]], [[React Native]]
- **출처 문헌**: [[2015-인사이드 자바스크립트]], [[2021-JavaScript from Beginner to Professional]], [[2021-JavaScript Cookbook 3e]], [[2022-자바스크립트로 배우는 SICP]], [[2024-JavaScript Design Patterns]], [[2022-React and React Native, Fourth Edition]] (p.314), [[2026-Clean Code with TypeScript]] (p.121–126)
