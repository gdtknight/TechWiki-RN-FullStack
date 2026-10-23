---
type: concept
title: "React Component Lifecycle and Effects"
category: "H. Information Systems/React"
sources:
  - "[[2026-React Official Documentation]]"
  - "[[2026-Web Performance Engineering in the Age of AI]]"
tags:
  - react
  - lifecycle
  - render-phase
  - commit-phase
  - useeffect
  - uselayouteffect
  - cleanup
  - strict-mode
  - browser-paint
created: 2026-10-21
updated: 2026-10-21
---

# React Component Lifecycle and Effects

## 정의 (Definition)

React 컴포넌트는 화면에 **나타나고(mount)**, 바뀐 state·props로 **다시 그려지고(update)**, 화면에서 **사라진다(unmount)**. react.dev: "Components may mount, update, or unmount." (*Lifecycle of Reactive Effects*)

React가 화면을 바꾸는 한 번의 과정은 **트리거 → 렌더 → 커밋**이고, 그 뒤 브라우저가 **페인트**한다 (*Render and Commit*). **Effect**는 이 과정의 끝에서 외부 시스템과 **동기화**하는 코드다: "Effects let you specify side effects that are caused by rendering itself, rather than by a particular event." / "Effects run at the end of a commit after the screen updates." (*Synchronizing with Effects*)

이 페이지는 React의 단계와 [[Browser Rendering Pipeline and Frame Lifecycle|브라우저 렌더링 파이프라인]]이 **어떤 순서로 맞물리는지**를 정리한다. 입문자용 설명은 [[렌더링 첫걸음 00 - 로드맵|렌더링 첫걸음 시리즈]] 레슨 04–06.

---

## 1. 트리거 → 렌더 → 커밋 → 페인트

react.dev *Render and Commit*:

| 단계 | 공식 설명 | 하는 일 |
|---|---|---|
| **트리거** | "Triggering a render (delivering the guest's order to the kitchen)" | 첫 렌더, 또는 "The component's (or one of its ancestors') state has been updated." |
| **렌더** | ""Rendering" is React calling your components." | 컴포넌트 함수를 호출해 다음 UI를 계산. **DOM은 아직 바뀌지 않음** |
| **커밋** | "Committing to the DOM" | 첫 렌더는 `appendChild()`, 다시 렌더할 때는 "the minimal necessary operations … to make the DOM match the latest rendering output". "React only changes the DOM nodes if there's a difference between renders." |
| **페인트** (브라우저) | "After rendering is done and React updated the DOM, the browser will repaint the screen." | 스타일 → 레이아웃 → 페인트 → 합성 ([[Browser Rendering Pipeline and Frame Lifecycle]]) |

내부 구현(Fiber, render/commit 단계의 자료 구조)은 [[React Fiber and Reconciliation]], [[React and React Native Internal Implementation Deep Dive]].

---

## 2. Effect의 실행 시점

### 2.1 공식 설명

| 훅 | 페인트 기준 시점 | 근거 |
|---|---|---|
| `useLayoutEffect` | **페인트 전** | "a version of `useEffect` that fires before the browser repaints the screen." / "React guarantees that the code inside `useLayoutEffect` and any state updates scheduled inside it will be processed before the browser repaints the screen." (*useLayoutEffect*) |
| `useEffect` (상호작용이 원인이 아닐 때) | 대개 **페인트 후** | "If your Effect wasn't caused by an interaction (like a click), React will generally let the browser paint the updated screen first before running your Effect." (*useEffect* Caveats) |
| `useEffect` (클릭 등 상호작용이 원인일 때) | **페인트 전일 수 있음** | "If your Effect is caused by an interaction (like a click), React may run your Effect before the browser paints the updated screen." (*useEffect* Caveats) |

- `useLayoutEffect`의 비용: "`useLayoutEffect` can hurt performance. Prefer `useEffect` when possible." (*useLayoutEffect*) 페인트를 막으므로, 무거운 일을 하면 프레임 예산을 넘긴다 ([[Web Performance Metrics and Core Web Vitals]]의 INP와 연결 — 판단).
- 서버 렌더링: "Effects only run on the client. They don't run during server rendering." (*useEffect*)

### 2.2 실행 순서 (실험)

React 19.3.1 + react-dom + jsdom에서 `useLayoutEffect`·`useEffect`에 로그를 넣어 실행한 결과 (작업 기록 2026-10-21). jsdom에는 페인트가 없으므로 **페인트와의 순서는 2.1의 공식 설명**을 따른다.

**마운트 (부모 `Counter` 안에 자식 `Child`)**
```
[render] Counter        ← 이때 DOM은 아직 비어 있음
[render] Child
[layout effect] Child   ← 자식 먼저
[layout effect] Counter ← 여기서는 새 DOM이 보임
[effect] Child          ← 자식 먼저
[effect] Counter
```

**업데이트 (state 변경)**
```
[render] Counter → [render] Child
[layout cleanup] Child → [layout cleanup] Counter
[layout effect] Child → [layout effect] Counter
[effect cleanup] Child → [effect cleanup] Counter
[effect] Child → [effect] Counter
```

**언마운트**
```
[layout cleanup] Counter → [layout cleanup] Child
[effect cleanup] Counter → [effect cleanup] Child
```

관찰 결과:
- **렌더 중에는 DOM이 이전 상태**이고, 레이아웃 Effect에서 새 DOM이 보인다 (렌더는 계산, 커밋이 DOM 반영).
- 마운트·업데이트에서 **자식의 Effect가 부모보다 먼저** 실행된다.
- 업데이트 때 같은 종류의 **정리(cleanup)가 모두 끝난 뒤** 새 Effect가 실행된다.
- **갱신의 원인에 따라 `useEffect` 시점이 달랐다**: 클릭(`dispatchEvent`)으로 state를 바꾸면 렌더 → 레이아웃 Effect → `useEffect`가 연달아 실행되어, 처리기에서 예약한 마이크로태스크보다도 먼저 끝났다. `setTimeout` 콜백에서 바꾸면 `useEffect`는 그 뒤 예약한 `setTimeout`보다도 늦게 실행됐다. 공식 설명("If your Effect is caused by an interaction (like a click), React may run your Effect before the browser paints the updated screen.", 2.1)과 맞는 결과다 (상세: [[렌더링 첫걸음 09 - 버튼 하나를 누르면]] 2.1–2.2절).
- 언마운트 때는 이 실험에서 부모의 정리가 자식보다 먼저 실행됐다. 공식 문서에서 이 순서를 보장한다는 문장은 확인하지 못했으므로, 순서에 기대는 코드는 쓰지 않는 것이 안전하다 (판단).

---

## 3. 의존성 배열과 정리 함수

### 3.1 의존성 배열

react.dev *Synchronizing with Effects*의 예:

| 쓰는 법 | 실행 시점 |
|---|---|
| `useEffect(fn)` | "This runs after every render" |
| `useEffect(fn, [])` | "This runs only on mount (when the component appears)" |
| `useEffect(fn, [a, b])` | "This runs on mount *and also* if either a or b have changed since the last render" |

- 무엇을 넣어야 하나: "All values inside the component (including props, state, and variables in your component's body) are reactive." / "Any reactive value can change on a re-render, so you need to include reactive values as Effect's dependencies." (*Lifecycle of Reactive Effects*)
- 실험 2.2의 3단계: 부모가 다시 렌더링되어 prop만 바뀌고 `count`는 그대로일 때, `[count]` Effect는 다시 실행되지 않았고 의존성 없는 Effect만 다시 실행됐다.

### 3.2 정리 함수

"React will call your cleanup function each time before the Effect runs again, and one final time when the component unmounts (gets removed)." (*Synchronizing with Effects*)

### 3.3 컴포넌트가 아니라 Effect의 관점에서 생각하기

"An Effect can only do two things: to start synchronizing something, and later to stop synchronizing it." / "think from each individual Effect's perspective (how to start and stop synchronization) rather than from the component's perspective (how it mounts, updates, or unmounts)." (*Lifecycle of Reactive Effects*)

---

## 4. Strict Mode의 두 번 실행 (개발 모드)

- "In Strict Mode, React mounts components twice (in development only!) to stress-test your Effects." / "By remounting your component, React verifies that navigating away and back would not break your code." (*Synchronizing with Effects*)
- 대응: "The right question isn't "how to run an Effect once", but "how to fix my Effect so that it works after remounting"." / "Usually, the answer is to implement the cleanup function." 기준: "the user shouldn't be able to distinguish between the Effect running once (as in production) and a *setup → cleanup → setup* sequence".
- 실험: 개발 빌드 + `<StrictMode>`에서 `✅ 연결 → ❌ 끊기 → ✅ 연결`, 컴포넌트 함수 호출 2회. `NODE_ENV=production` 빌드에서는 `✅ 연결` 1회, 호출 1회.

> 이 이유로 [[React Navigation]]에서 화면을 떠났다가 돌아오는 상황도 정리 함수가 제대로 있으면 문제가 없다 (판단). 다만 React Navigation은 화면을 떠날 때 언마운트하지 않으므로 화면 포커스는 별도로 다룬다 ([[React Native Rendering and App Lifecycle]]).

---

## 5. Effect가 필요 없는 경우

react.dev *You Might Not Need an Effect*:
- "You don't need Effects to transform data for rendering." → "When something can be calculated from the existing props or state, don't put it in state. Instead, calculate it during rendering."
- "You don't need Effects to handle user events." → 이벤트 처리기에서 처리.
- 상태 초기화: "To reset the state of an entire component tree, pass a different `key` to it."
- 데이터 가져오기는 Effect로 할 수 있지만 "you need to implement cleanup to avoid race conditions" (`ignore` 플래그 패턴).

[[2026-Web Performance Engineering in the Age of AI]]도 렌더 경로의 무거운 계산과, `useEffect`에서 데이터를 받아 목록을 한꺼번에 넣어 레이아웃이 밀리는 패턴을 성능 문제로 든다 (p.69, p.72).

---

## 6. 클래스 컴포넌트 생명 주기와의 대응

옛 도서([[2019-React Native in Action]] 등)는 클래스 컴포넌트를 쓴다. react.dev *Component*:

| 클래스 메서드 | 호출 시점 (공식) | 함수 컴포넌트 |
|---|---|---|
| `componentDidMount` | "when your component is added *(mounted)* to the screen" | `useEffect(fn, [])` 의 설정 부분 |
| `componentDidUpdate` | "immediately after your component has been re-rendered with updated props or state" | 의존성이 바뀔 때 다시 실행되는 Effect |
| `componentWillUnmount` | "before your component is removed *(unmounted)* from the screen" | Effect의 정리 함수 |

"For many use cases, defining `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` together in class components is equivalent to calling `useEffect` in function components. In the rare cases where it's important for the code to run before browser paint, `useLayoutEffect` is a closer match." 단, 새 코드에는 클래스를 권하지 않는다: "we don't recommend using them in new code." (*Component*)

---

## 7. 관점 비교 / 출처별 차이

| 주제 | 도서 | 공식 문서 / 실험 |
|---|---|---|
| `useEffect` 실행 시점 | [[React Hooks and Component Design Patterns]]: "`useEffect`는 화면이 그려진(Paint) 후 비동기로 실행" | 상호작용이 원인이 아닐 때는 "generally" 페인트 후, 클릭 등이 원인이면 "React may run your Effect before the browser paints the updated screen" (*useEffect* Caveats). 항상 페인트 후는 아님 |
| 의존성 배열 | 도서는 "마운트 때만 실행"하는 `[]`를 생명 주기 대용으로 설명하는 경우가 많음 | react.dev는 생명 주기 대신 "동기화의 시작과 끝"으로 생각하라고 권함 (3.3절) |

---

## 관련 개념 및 엔티티

- **입문 레슨**: [[렌더링 첫걸음 00 - 로드맵]] (레슨 04–06)
- **상위/연관 개념**: [[Browser Rendering Pipeline and Frame Lifecycle]], [[React Hooks and Component Design Patterns]], [[React Fiber and Reconciliation]], [[React Native Rendering and App Lifecycle]], [[Web Performance Metrics and Core Web Vitals]], [[JavaScript Execution Context, Closures, and Prototype]] (Stale Closure)
- **구현 기술/엔티티**: [[React]], [[React Native]], [[React Navigation]]
- **출처 문헌**: [[2026-React Official Documentation]], react.dev [*Render and Commit*](https://react.dev/learn/render-and-commit), [*Synchronizing with Effects*](https://react.dev/learn/synchronizing-with-effects), [*Lifecycle of Reactive Effects*](https://react.dev/learn/lifecycle-of-reactive-effects), [*You Might Not Need an Effect*](https://react.dev/learn/you-might-not-need-an-effect), [*useEffect*](https://react.dev/reference/react/useEffect), [*useLayoutEffect*](https://react.dev/reference/react/useLayoutEffect), [*Component*](https://react.dev/reference/react/Component), [[2026-Web Performance Engineering in the Age of AI]] (Ch 4)
