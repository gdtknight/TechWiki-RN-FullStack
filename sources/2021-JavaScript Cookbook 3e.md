---
type: source
title: "JavaScript Cookbook, Third Edition"
authors:
  - "Adam D. Scott"
  - "Matthew MacDonald"
  - "Shelley Powers"
publisher: "O'Reilly Media"
year: 2021
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2021-JavaScript Cookbook 3e-Adam D. Scott,Matthew MacDonald 외-OReilly Media.pdf"
status: ingested
tags:
  - javascript
  - cookbook
  - regex
  - async-generators
  - web-workers
  - indexeddb
  - nodejs
sources: ["[[2021-JavaScript Cookbook 3e]]"]
created: 2026-10-19
updated: 2026-10-19
---

# JavaScript Cookbook, Third Edition (2021)

> **서지 정보**: Adam D. Scott, Matthew MacDonald, Shelley Powers | O'Reilly Media (2021) | 538 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2021-JavaScript Cookbook 3e-Adam D. Scott,Matthew MacDonald 외-OReilly Media.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2021-JavaScript%20Cookbook%203e-Adam%20D.%20Scott,Matthew%20MacDonald%20%EC%99%B8-OReilly%20Media.pdf)

---

## 핵심 요약 (Core Thesis)

실무 개발 중 마주치는 구체적인 문제("어떻게 객체를 깊은 복사하는가?", "어떻게 비동기 제너레이터와 Web Worker로 메인 스레드 블로킹을 막는가?", "어떻게 `IndexedDB`에 대용량 데이터를 영속화하는가?")에 대해 **즉시 적용 가능한 표준 모던 [[JavaScript (ECMAScript)]] 해법(Problem → Solution → Discussion)을 제시하는 O'Reilly 실전 레시피 사전**이다.

---

## 파트별 핵심 레시피 요약

### Part I: The JavaScript Language (Ch 1 – Ch 10, p.19–270)
- **Ch 1 – Ch 4. 개발 환경(ESLint/Prettier), 문자열/정규표현식, 숫자(`BigInt`/암호학적 난수), 날짜(`Date`/`Intl`) (p.21–106)**
  - 유니코드 정규표현식과 명명된 캡처 그룹, `crypto.getRandomValues()` 보안 난수 생성, 부동소수점 정밀도 보정, `BigInt` 연산.
- **Ch 5. Arrays (p.107–136)**
  - 배열 불변 복제·병합, `Set`을 이용한 중복 제거(5.11), 2차원 배열 평탄화(`flatMap`), 조건 검색/변환(`map`, `filter`, `reduce`), `Map`을 활용한 키 인덱스 컬렉션 구축 ([[JavaScript Execution Context, Closures, and Prototype]]).
- **Ch 6 – Ch 8. Functions, Objects, Classes (p.137–220)**
  - 클로저를 이용한 상태 보관 함수(6.6), **제너레이터 함수(`function*` / `yield`, 6.7)**, 부분 적용(Partial Application, 6.8).
  - `Object.defineProperty` 속성 기술자 제어(7.7), `Object.freeze` 불변 동결(7.8), **`Proxy`를 이용한 객체 가로채기(7.9)**, 깊은 복사(Deep Clone, 7.11), `Symbol` 고유 키 생성(7.12).
- **Ch 9 – Ch 10. Asynchronous Programming, Errors & Testing (p.221–270)**
  - **[[JavaScript Event Loop and Asynchronous Patterns]] 심화 레시피**:
    - `requestAnimationFrame`으로 루프 중 UI 응답성 유지(9.1), 콜백 함수의 Promisify 변환(9.3), `Promise.all` 동시 실행(9.4).
    - **비동기 제너레이터(`async function*` & `for await...of`, 9.6, p.238)**: 페이지네이션이나 스트리밍 데이터를 비동기 이터레이터로 소비하는 패턴.
    - **Web Worker 백그라운드 태스크 및 진행률(`postMessage`) 보고(9.7–9.8, p.240–246)**.
    - 커스텀 `Error` 클래스 상속 및 비동기/전역 미처리 에러(`unhandledrejection`) 감지, Jest 단위 테스트(Ch 10).

### Part II & Part III: Browser APIs & Node.js (Ch 11 – Ch 21, p.271–520)
- **Ch 11 – Ch 13. 브라우저 프로파일링(Lighthouse), DOM/접근성(ARIA Live Region), 원격 데이터 통신 (p.273–344)**
  - `Fetch` API, 바이너리 Blob/Image 로딩, **WebSockets 양방향 실시간 통신(13.9)** 및 Long Polling(13.10).
- **Ch 14. Data Persistence (p.345–364)**
  - 쿠키, `sessionStorage`, `localStorage`, 그리고 대용량 구조화 데이터를 클라이언트에 저장하는 **`IndexedDB` 레시피(14.4–14.5)** ([[Offline-First and Mobile State Management]]의 웹 대응 계층).
- **Ch 15 – Ch 16. 미디어(SVG/D3/Canvas/Audio/Video), PWA(Service Worker), Web Components (p.365–414)**.
- **Ch 17 – Ch 21. Node.js 이벤트 루프, 멀티플랫폼 NPM 모듈 배포(ESM/CJS), CLI 유틸리티, Express REST/GraphQL/OAuth 2 서버 구축 (p.415–520)**.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[JavaScript Core Syntax and Modern ES6+ Fundamentals]] — 배열 고차 메서드(`map`, `filter`, `reduce`), `Map`/`Set` 컬렉션, `#private` 클래스 및 모듈 레시피 (Ch 5–8)
- [[JavaScript Event Loop and Asynchronous Patterns]] — 비동기 제너레이터(`for await...of`), Web Worker 백그라운드 오프로딩, WebSockets (Ch 9, Ch 13)
- [[JavaScript Execution Context, Closures, and Prototype]] — 클로저, 제너레이터(`yield`), 부분 적용, `Proxy`, `Object.freeze`, `Symbol` (Ch 5–7)
- [[Offline-First and Mobile State Management]] — 브라우저 `localStorage`/`IndexedDB` 및 PWA Service Worker 오프라인 영속화 (Ch 14, Ch 16)

### Entities
- [[JavaScript (ECMAScript)]] — 언어 표준 API 및 브라우저/Node.js 실전 레시피 총람
