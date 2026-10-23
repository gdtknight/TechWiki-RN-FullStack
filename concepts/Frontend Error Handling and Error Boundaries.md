---
type: concept
title: "Frontend Error Handling and Error Boundaries"
category: "H. Information Systems/React"
sources:
  - "[[2025-Full Stack JavaScript Strategies]]"
  - "[[2026-React Official Documentation]]"
tags:
  - error-handling
  - error-boundary
  - react-error-boundary
  - form-validation
  - api-errors
  - error-logging
created: 2026-10-23
updated: 2026-10-23
---

# Frontend Error Handling and Error Boundaries

## 정의 (Definition)

프론트엔드 오류 처리는 렌더링 오류, 사용자 입력 오류, API 오류를 사용자가 이해할 수 있는 화면으로 바꾸고, 개발자가 원인을 찾을 수 있게 기록하는 일이다. [[2025-Full Stack JavaScript Strategies]]는 그 목적을 이렇게 쓴다: "Error handling needs to be thorough so that you don't leak any sensitive information to users or malicious parties who are trying to attack your app and so that the app keeps a consistent UX." (p.241)

[[React]]에서 렌더링 오류를 막는 장치는 **Error Boundary**다. react.dev: "By default, if your application throws an error during rendering, React will remove its UI from the screen. To prevent this, you can wrap a part of your UI into an *Error Boundary*." (*Component*) 책도 경계가 없으면 "users will typically see a blank screen in production"이라고 쓴다 (p.242).

---

## 1. 왜 `try`/`catch`로는 안 되나

책: 렌더링 함수는 내가 아니라 React가 호출하므로 `try`/`catch`가 기대대로 잡지 못한다. "That's because unlike on the backend, React is calling the functions instead of you, which is a more declarative approach." (p.242)

정리하면 (판단):

| 오류가 나는 곳 | 누가 호출하나 | 잡는 방법 |
|---|---|---|
| 렌더링 중 (컴포넌트 함수 본문) | React | Error Boundary |
| 이벤트 처리기 | 브라우저 → React의 이벤트 시스템 | 처리기 안의 `try`/`catch` |
| `setTimeout`·Promise 콜백 | 브라우저의 작업 큐 ([[JavaScript Event Loop and Asynchronous Patterns]]) | `try`/`catch` 또는 `.catch()` |

---

## 2. Error Boundary가 잡지 못하는 것

react.dev *Component*의 Note를 그대로 옮긴다. "Error boundaries do not catch errors for:"

- "Event handlers"
- "Server side rendering"
- "Errors thrown in the error boundary itself (rather than its children)"
- "Asynchronous code (e.g. `setTimeout` or `requestAnimationFrame` callbacks); an exception is the usage of the `startTransition` function returned by the `useTransition` Hook."

책의 목록(p.243: "async errors, errors that happen in event handlers, and errors thrown in the boundary itself")은 이 중 서버 렌더링을 뺀 세 가지와 같다.

### 2.1 `useEffect` 안의 오류 — 실행 확인

책은 비동기 코드와 함께 "lifecycle management hooks, such as `useEffect`"에서 난 오류를 `useErrorBoundary`로 넘기라고 한다 (p.244). 직접 실행해 보면 **동기 오류와 비동기 오류가 다르게 처리된다**.

실행 환경: React 19.3.1 + jsdom. 경계는 `getDerivedStateFromError`를 쓴 클래스 컴포넌트, 루트에는 `onCaughtError`를 붙였다.

| 오류를 던진 곳 | 결과 |
|---|---|
| `useEffect(() => { throw ... })` (동기) | `onCaughtError` 호출, 화면이 fallback으로 바뀜 |
| `useEffect` 안의 `Promise.then`에서 던짐 (비동기) | 경계가 못 잡음, 처리되지 않은 Promise 거부(`unhandledRejection`)로 남음, 화면은 그대로 |
| `onClick` 처리기에서 던짐 | 경계가 못 잡음, `window`의 `error` 이벤트로 감, 화면은 그대로 |

그러니 `useErrorBoundary`가 필요한 것은 Effect 안의 **비동기** 작업(데이터 요청 등)과 이벤트 처리기다 (판단: 위 실행 결과와 react.dev의 "Asynchronous code" 항목에서).

`react-error-boundary` README도 같은 범위를 말한다: "Use `useErrorBoundary` to pass caught errors to the nearest boundary." React 19에서는 대안도 있다: "errors thrown from a function passed to the returned `startTransition` function are caught by the nearest boundary." ([README](https://github.com/bvaughn/react-error-boundary))

---

## 3. 경계를 어디에 둘까: 세 수준

[[2025-Full Stack JavaScript Strategies]] p.243:

| 수준 | 책의 설명 | 언제 |
|---|---|---|
| **앱** | 컴포넌트 트리 꼭대기. "This should be in place in every project" | 항상. 마지막 안전망 |
| **레이아웃** | 상태를 공유하는 컴포넌트 묶음. "if anything happens in one component, then the error boundary shows for all the components in that layout" | 한 페이지의 관련 영역 |
| **컴포넌트** | "the most granular level" | 따로 API를 호출하거나 상태가 독립된 컴포넌트 |

함수 컴포넌트만으로는 Error Boundary를 만들 수 없다. react.dev는 `static getDerivedStateFromError`(fallback 표시)와 선택적인 `componentDidCatch`(기록)를 가진 **클래스 컴포넌트**로 구현한다 (*Component*). 책은 클래스를 직접 쓰지 않으려고 `react-error-boundary` 패키지를 쓴다 (p.243).

**주의**: 라이브러리가 자체 경계를 가지고 있으면 오류가 앱 수준까지 올라오지 않는다. 책의 예는 React Router다: "React Router currently catches errors at the `<RouteProvider>` level." (p.246) 책은 v6 기준이라 문서를 다시 확인하라고 덧붙인다 (p.244).

---

## 4. 오류 기록

책: fallback 화면만으로는 부족하고, Sentry·LogRocket·Datadog 같은 외부 도구에 기록해야 한다 (p.247). 책의 `logError`는 `console.log`만 하면서 주석으로 "you should really be logging to some external tool"이라고 적는다 (p.247).

React 19의 공식 방법은 루트 옵션이다. react.dev *createRoot*:
- `onCaughtError`: "Callback called when React catches an error in an Error Boundary."
- `onUncaughtError`: "Callback called when an error is thrown and not caught by an Error Boundary."

로그에 무엇을 남기지 말아야 하는지는 [[Frontend Security and Session Management]] 4절 (OWASP: 액세스 토큰·세션 ID는 로그에 그대로 남기지 않음).

---

## 5. 사용자 입력 오류와 API 오류

### 5.1 입력 검증 오류 (p.248–250)

| 방법 | 예 |
|---|---|
| HTML 속성 | `required`, `minLength`, `maxLength`. 메시지는 브라우저 기본 검증 메시지 (p.249) |
| 폼 라이브러리 규칙 | React Hook Form의 `register('search', { required, maxLength, minLength, pattern })` (p.249) |
| 검증 스키마 | Yup·Zod·joi 스키마를 `resolver`로 연결 (p.250) |

언제 보여 줄지(입력 중, 포커스를 잃을 때, 제출할 때)는 디자인 팀과 정한다 (p.248, p.250).

### 5.2 API 오류 (p.250–252)

- 사용자가 할 수 있는 게 없는 오류(예: 422, 500)에도 무언가를 보여 줘야 한다 (p.251).
- 백엔드의 오류 메시지를 그대로 보여 주지 말 것: "a malicious or curious user will be able to see the response in the browser tools. You typically want to send a redacted version of the error to the frontend" (p.252).

**책 안의 불일치** (판단): 책의 범용 fallback 컴포넌트는 `{error.message}`를 화면에 그대로 출력한다 (p.246). 개발 중에는 원인을 보기 좋지만, 위 p.252의 권고와 맞추려면 운영 환경에서는 일반 문구를 보여 주고 상세는 로그로 보내는 편이 일관된다.

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[React Component Lifecycle and Effects]], [[JavaScript Event Loop and Asynchronous Patterns]], [[Frontend Security and Session Management]], [[Frontend Testing Strategy (Unit, Mocking, and E2E)]], [[Full-Stack Authentication, Token Refresh, and Observability]]
- **구현 기술/엔티티**: [[React]], [[TanStack Query and Zustand]]
- **출처 문헌**: [[2025-Full Stack JavaScript Strategies]] (Ch 18, p.241–252), react.dev [*Component*](https://react.dev/reference/react/Component) ("Catching rendering errors with an Error Boundary"), react.dev [*createRoot*](https://react.dev/reference/react-dom/client/createRoot), [react-error-boundary README](https://github.com/bvaughn/react-error-boundary)
