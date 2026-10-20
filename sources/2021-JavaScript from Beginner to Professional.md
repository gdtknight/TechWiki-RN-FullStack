---
type: source
title: "JavaScript from Beginner to Professional"
authors:
  - "Laurence Lars Svekis"
  - "Maaike van Putten"
  - "Rob Percival"
publisher: "Packt Publishing"
year: 2021
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2021-JavaScript from Beginner to Professional-Laurence Lars Svekis,Maaike van Putten 외-Packt Publishing.pdf"
status: ingested
tags:
  - javascript
  - es6
  - event-loop
  - promises
  - async-await
  - dom
sources: ["[[2021-JavaScript from Beginner to Professional]]"]
created: 2026-10-19
updated: 2026-10-19
---

# JavaScript from Beginner to Professional (2021)

> **서지 정보**: Laurence Lars Svekis, Maaike van Putten, Rob Percival | Packt Publishing (2021) | 545 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2021-JavaScript from Beginner to Professional-Laurence Lars Svekis,Maaike van Putten 외-Packt Publishing.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2021-JavaScript%20from%20Beginner%20to%20Professional-Laurence%20Lars%20Svekis,Maaike%20van%20Putten%20%EC%99%B8-Packt%20Publishing.pdf)

---

## 핵심 요약 (Core Thesis)

모던 **ES6+ [[JavaScript (ECMAScript)]]** 의 기초 문법(`let`/`const`, 화살표 함수, 구조 분해 할당, 스프레드 연산자, `class`)부터 내장 배열/문자열/객체 메서드, 브라우저 DOM 제어, 그리고 [[React]] 및 [[React Native]] 개발에 가장 중요한 **비동기 동시성 모델([[JavaScript Event Loop and Asynchronous Patterns|Callbacks, Promises, `async`/`await`, Event Loop]], Ch 13)** 까지 연습문제와 퀴즈 중심으로 체계적으로 다지는 입문·중급서다.

---

## 챕터별 핵심 내용 요약

### 1. ES6+ 코어 문법과 객체/함수/클래스 (Ch 1 – Ch 8)
- **Ch 1 – Ch 3. 변수, 원시 타입, 배열과 객체 (p.26–95)**
  - `let`, `const` 블록 스코프와 원시 데이터 타입(`String`, `Number`, `BigInt`, `Boolean`, `Symbol`, `undefined`, `null`), 배열 고차 메서드와 다차원 배열/객체 중첩 구조.
- **Ch 4 – Ch 6. 제어문, 반복문 (`for...of`, `for...in`), 함수 (p.96–173)**
  - 화살표 함수(Arrow Functions), 기본 매개변수(Default Parameters), 스프레드(`...`) 및 나머지 매개변수(Rest Parameters, p.148), 렉시컬 스코프와 재귀/익명 함수, 즉시 실행 함수(IIFE).
- **Ch 7. Classes (p.174–191)**
  - ES6 `class` 문법(`constructor`, getter/setter, `extends`, `super`)과 그 이면의 프로토타입(`prototype`) 동작 원리 ([[JavaScript Execution Context, Closures, and Prototype]]).
- **Ch 8. Built-In JavaScript Methods (p.192–229)**
  - 배열 메서드(`forEach`, `filter`, `map`, `find`, `every`, `some`, `reduce`), 문자열/숫자/`Math`/`Date` 내장 API 총정리.

### 2. DOM 이벤트, 중급 개념, 비동기 동시성 (Ch 9 – Ch 15)
- **Ch 9 – Ch 11. BOM/DOM 조작과 이벤트 흐름 (p.230–325)**
  - DOM 트리 탐색 및 동적 엘리먼트 생성, 이벤트 리스너와 DOM 이벤트 플로우(버블링 vs 캡처링, p.300) — [[React]]의 `SyntheticEvent` 이해를 위한 선수 지식.
- **Ch 12. Intermediate JavaScript (p.326–369)**
  - 정규표현식(RegExp), 호이스팅(Hoisting, p.340), `'use strict'` 엄격 모드, 디버깅, `localStorage` 및 **`JSON.stringify` / `JSON.parse` 직렬화(p.358)**.
- **Ch 13. Concurrency (p.370–387)**
  - **[[JavaScript Event Loop and Asynchronous Patterns]] 핵심 기초**:
    - 콜백(Callbacks)과 콜백 지옥(Callback Hell, p.371).
    - **Promises (p.374)**: `resolve`/`reject` 상태 전이와 `.then()`/`.catch()` 체이닝.
    - **`async` and `await` (p.378)**: 비동기 Promise 코드를 동기 코드처럼 읽기 쉽게 작성하는 문법적 설탕(Syntactic Sugar).
    - **Event Loop (p.380)**: 단일 스레드(Single-threaded)인 JS가 Call Stack과 Callback Queue(Task Queue), Web/Host API를 조합해 논블로킹 비동기 작업을 수행하는 원리.
- **Ch 14 – Ch 15. HTML5 APIs & Next Steps (p.388–449)**
  - Canvas, Geolocation, 그리고 프런트엔드 프레임워크(React, Vue, Angular) 및 Node.js/Express 백엔드 생태계로의 확장 안내.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[JavaScript Core Syntax and Modern ES6+ Fundamentals]] — `let`/`const`/TDZ, 화살표 함수, 구조 분해 할당과 스프레드(`...`), 5대 배열 고차 메서드, ES6 `class`, JSON 직렬화 기초 문법 (Ch 1–8, Ch 12)
- [[JavaScript Event Loop and Asynchronous Patterns]] — Callbacks, Promises, `async`/`await`, Call Stack과 Event Loop의 동작 원리 (Ch 13)
- [[JavaScript Execution Context, Closures, and Prototype]] — 블록 스코프(`let`/`const`), 화살표 함수, 호이스팅, ES6 `class`와 프로토타입 (Ch 6–7, Ch 12)

### Entities
- [[JavaScript (ECMAScript)]] — ES6+ 표준 문법 및 내장 메서드 체계
