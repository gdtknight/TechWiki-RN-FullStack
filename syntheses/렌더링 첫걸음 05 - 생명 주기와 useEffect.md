---
type: synthesis
title: "렌더링 첫걸음 05 - 생명 주기와 useEffect"
category: "H. Information Systems/React"
sources:
  - "[[2026-React Official Documentation]]"
tags:
  - react
  - lifecycle
  - useeffect
  - cleanup
  - dependencies
  - beginner
  - rendering-first-steps
created: 2026-10-21
updated: 2026-10-21
---

# 렌더링 첫걸음 05 - 생명 주기와 useEffect

> **이전**: [[렌더링 첫걸음 04 - 렌더와 커밋|04 렌더와 커밋]] · **다음**: [[렌더링 첫걸음 06 - Effect 심화|06 Effect 심화]] · **목록**: [[렌더링 첫걸음 00 - 로드맵|로드맵]]

## 이 레슨에서 배우는 것

1. 컴포넌트의 **생명 주기**: 마운트 → 업데이트 → 언마운트
2. **Effect**가 무엇이고, 렌더·커밋과 어떻게 다른지
3. `useEffect`의 세 부분: **설정 코드**, **의존성 배열**, **정리 함수**
4. Effect가 **실행되는 순서** (실험)
5. 정리 함수를 빠뜨렸을 때 생기는 일

> 예제는 React 19.3.1 + react-dom + jsdom에서 실행해 결과를 확인했습니다. jsdom에는 화면 그리기(페인트)가 없으므로, 페인트와의 순서는 공식 문서를 근거로 씁니다.

---

## 1. 컴포넌트의 생명 주기

### 비유로 이해하기: 극장 무대 배우

연극 배우는 **무대에 등장하고**(마운트), 장면이 바뀔 때마다 **대사와 표정을 바꾸고**(업데이트), 마지막에 **퇴장합니다**(언마운트).

react.dev: "Components may mount, update, or unmount." (*Lifecycle of Reactive Effects*)

| 단계 | 뜻 | 언제 |
|---|---|---|
| **마운트** (mount) | 화면에 처음 나타남 | 첫 렌더 후 커밋 |
| **업데이트** (update) | 다시 렌더되어 바뀜 | state·props 변경 ([[렌더링 첫걸음 04 - 렌더와 커밋\|레슨 04]]의 트리거) |
| **언마운트** (unmount) | 화면에서 사라짐 | 조건부 렌더로 빠지거나, 다른 컴포넌트로 바뀌거나, `key`가 바뀔 때 |

---

## 2. Effect: 렌더 바깥 세상과 맞추기

### 2.1 렌더에서 할 수 없는 일

[[렌더링 첫걸음 04 - 렌더와 커밋|레슨 04]]에서 렌더는 **순수한 계산**이어야 한다고 배웠습니다. 그런데 앱에는 순수하지 않은 일도 필요합니다.

- 서버에 연결하기 (채팅방 접속)
- 타이머 돌리기
- 브라우저 탭 제목(`document.title`) 바꾸기
- 화면 크기 변화 구독하기

이런 일은 **렌더 결과가 화면에 반영된 뒤**, React 바깥의 시스템과 **맞추는(동기화하는)** 일입니다. 이것을 **Effect**라고 부릅니다. react.dev: "*Effects* let you specify side effects that are caused by rendering itself, rather than by a particular event." / "Effects run at the end of a commit after the screen updates." (*Synchronizing with Effects*)

> 버튼 클릭처럼 **특정 사건** 때문에 하는 일은 Effect가 아니라 **이벤트 처리기**(`onClick`)에서 합니다. Effect는 "화면이 이 상태로 보이는 동안 계속 맞춰 둬야 하는 일"입니다 (판단, 위 정의에서). 레슨 06에서 자세히 구분합니다.

### 2.2 첫 Effect: 탭 제목 맞추기

```jsx
import { useState, useEffect } from "react";

function TitleCounter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `${count}번 눌렀어요`;   // 화면 밖(탭 제목)과 맞추기
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>+</button>;
}
```

결과 (실행 결과): 처음 `0번 눌렀어요`, 한 번 누른 뒤 `1번 눌렀어요`.

---

## 3. `useEffect`의 세 부분

react.dev는 Effect를 세 단계로 씁니다 (*Synchronizing with Effects*):

1. "**Declare an Effect.** By default, your Effect will run after every commit."
2. "**Specify the Effect dependencies.** Most Effects should only re-run *when needed* rather than after every render."
3. "**Add cleanup if needed.** Some Effects need to specify how to stop, undo, or clean up whatever they were doing."

```jsx
useEffect(() => {
  // ① 설정(setup): 연결, 구독, 타이머 시작 …
  return () => {
    // ③ 정리(cleanup): 연결 끊기, 구독 해지, 타이머 멈춤 …
  };
}, [/* ② 의존성 배열 */]);
```

### 3.1 의존성 배열: 언제 다시 실행할까

| 쓰는 법 | 실행 시점 (react.dev 예제 주석) |
|---|---|
| `useEffect(fn)` | "This runs after every render" |
| `useEffect(fn, [])` | "This runs only on mount (when the component appears)" |
| `useEffect(fn, [a, b])` | "This runs on mount *and also* if either a or b have changed since the last render" |

무엇을 넣어야 할까요? react.dev: "All values inside the component (including props, state, and variables in your component's body) are reactive." / "Any reactive value can change on a re-render, so you need to include reactive values as Effect's dependencies." (*Lifecycle of Reactive Effects*) 즉 **Effect 안에서 쓰는 props·state·컴포넌트 안 변수는 모두** 넣습니다.

### 3.2 정리 함수: 언제 실행될까

"React will call your cleanup function each time before the Effect runs again, and one final time when the component unmounts (gets removed)." (*Synchronizing with Effects*)

| 상황 | 정리 함수 |
|---|---|
| 의존성이 바뀌어 Effect를 다시 실행하기 **직전** | 이전 Effect의 정리 실행 |
| 컴포넌트가 **언마운트**될 때 | 마지막 정리 실행 |

### 비유로 이해하기: 수도꼭지

Effect는 "수도꼭지 열기", 정리 함수는 "잠그기"입니다. 물을 다른 방으로 돌리려면(의존성 변경) **먼저 지금 꼭지를 잠그고** 새 꼭지를 엽니다. 집을 나갈 때(언마운트)는 반드시 잠급니다 (판단).

---

## 4. 실행 순서 (실험)

`Counter`(부모) 안에 `Child`(자식)를 두고, 렌더·`useEffect`에 로그를 넣어 실행했습니다 (실행 결과, 일부 줄임).

**마운트**
```
[render] Counter      ← 렌더 중: DOM은 아직 비어 있음
[render] Child
[effect] Child        ← 자식 먼저
[effect] Counter
```

**업데이트** (`count` 변경)
```
[render] Counter → [render] Child
[effect cleanup] Child → [effect cleanup] Counter   ← 정리 먼저 전부
[effect] Child → [effect] Counter                   ← 그다음 새 Effect
```

**언마운트**
```
[effect cleanup] Counter → [effect cleanup] Child
```

관찰한 것:
- **렌더가 다 끝나고 커밋된 뒤에** Effect가 실행됩니다.
- 마운트·업데이트에서 **자식의 Effect가 부모보다 먼저** 실행됩니다.
- 업데이트 때는 **정리가 모두 끝난 뒤에** 새 Effect가 실행됩니다.
- 의존성 배열이 `[]`인 Effect는 업데이트 때 다시 실행되지 않았고, `[count]`인 Effect는 `count`가 그대로인 업데이트(부모만 다시 렌더, prop만 바뀜)에서 다시 실행되지 않았습니다.

**페인트와의 관계**: jsdom에는 페인트가 없어 직접 볼 수 없습니다. react.dev는 "If your Effect wasn't caused by an interaction (like a click), React will generally let the browser paint the updated screen first before running your Effect."라고 설명합니다 (*useEffect* Caveats). 클릭 때문에 생긴 Effect는 예외가 있는데, 레슨 06에서 다룹니다.

```
트리거 → 렌더 → 커밋 → (대개) 페인트 → useEffect
```

전체 실험 기록: [[React Component Lifecycle and Effects]] 2.2절.

---

## 5. 정리 함수를 빠뜨리면

### 5.1 타이머가 계속 돈다

```jsx
function LeakyTimer() {
  useEffect(() => {
    setInterval(() => console.log("tick"), 1000);   // ❌ 멈추는 코드 없음
  }, []);
  return <p>타이머</p>;
}
```

컴포넌트를 언마운트한 뒤에도 타이머 콜백이 **계속 실행되었습니다** (실행 결과: 언마운트 후 카운트 증가 `true`). 검증 스크립트 자체도 이 타이머 때문에 끝나지 않아 강제로 멈춰야 했습니다.

정리 함수로 고칩니다.

```jsx
function GoodTimer() {
  useEffect(() => {
    const id = setInterval(() => console.log("tick"), 1000);
    return () => clearInterval(id);   // ✅ 언마운트 때 멈춤
  }, []);
  return <p>타이머</p>;
}
```

결과: 언마운트 후 카운트 증가 `false` (실행 결과).

### 5.2 연결은 끊고, 구독은 해지하고

| 설정 | 정리 |
|---|---|
| `setInterval` / `setTimeout` | `clearInterval` / `clearTimeout` |
| `addEventListener` | `removeEventListener` |
| 서버 연결 `connect()` | `disconnect()` |
| 구독 `subscribe()` | 해지 `unsubscribe()` / `remove()` |

(판단: [[JS 첫걸음 10 - 스코프와 클로저|JS 첫걸음 10]]의 클로저 덕분에 정리 함수는 설정 때 만든 `id`나 연결을 기억합니다.)

---

## 6. 정리

| 낱말 | 뜻 |
|---|---|
| 마운트 / 업데이트 / 언마운트 | 화면에 나타남 / 다시 렌더되어 바뀜 / 화면에서 사라짐 |
| Effect | 커밋 뒤에 React 바깥 시스템과 동기화하는 코드 |
| 설정 코드 | Effect 함수 본문. 연결·구독·타이머 시작 |
| 정리 함수 | Effect가 `return`하는 함수. 다시 실행 직전과 언마운트 때 실행 |
| 의존성 배열 | Effect를 다시 실행할 조건. 안에서 쓰는 props·state를 모두 넣음 |
| `[]` | 마운트 때만 실행 (정리는 언마운트 때) |

---

## 7. 자주 하는 실수

| 실수 | 결과 | 바로잡기 |
|---|---|---|
| 정리 함수 없이 타이머·구독 시작 | 언마운트 후에도 계속 실행 (5.1절) | 정리 함수에서 멈추고 해지 |
| 의존성 배열 없이 Effect 안에서 state 변경 (`useEffect(() => { setN(n + 1); })`) | 무한 반복. 경고 (실행 결과): `Maximum update depth exceeded. This can happen when a component calls setState inside useEffect, but useEffect either doesn't have a dependency array, or one of …` | Effect가 정말 필요한지 확인하고 (레슨 06), 필요하면 의존성 배열을 정확히 |
| Effect 안에서 쓰는 state를 의존성 배열에서 빠뜨림 | 예전 값을 계속 씀 (오래된 클로저) | 쓰는 반응형 값을 모두 넣음 (3.1절) |
| 클릭 처리를 Effect로 함 | 흐름이 복잡해짐 | 클릭 때문에 하는 일은 `onClick`에서 (레슨 06) |
| 렌더 중에 `document.title`을 바꿈 | 렌더가 순수하지 않음 | Effect에서 (2.2절) |

---

## 8. 확인 퀴즈

**Q1.** 다음 Effect는 언제 실행될까요? ① `useEffect(fn)` ② `useEffect(fn, [])` ③ `useEffect(fn, [userId])`

> [!question]- 정답 보기
> ① 매 렌더(커밋) 뒤, ② 마운트 때만, ③ 마운트 때와 `userId`가 바뀌었을 때

**Q2.** 의존성이 `[roomId]`인 Effect가 `roomId`가 `"general"`에서 `"travel"`로 바뀔 때 실행되는 순서는?

> [!question]- 정답 보기
> `"general"` Effect의 정리 함수 → `"travel"` Effect의 설정 코드. "React will call your cleanup function each time before the Effect runs again" (react.dev).

**Q3.** 부모 `A` 안에 자식 `B`가 있고 둘 다 `useEffect`가 있습니다. 마운트 때 어느 Effect가 먼저 실행될까요?

> [!question]- 정답 보기
> 자식 `B`가 먼저입니다 (4절의 실험).

**Q4.** 아래 코드의 문제는?
```jsx
useEffect(() => {
  window.addEventListener("resize", onResize);
}, []);
```

> [!question]- 정답 보기
> 정리 함수가 없어 언마운트 후에도 `onResize`가 계속 불립니다. `return () => window.removeEventListener("resize", onResize);`를 추가합니다.

**Q5. (직접 해 보기)** 1초마다 `seconds`를 1씩 올리는 `Stopwatch` 컴포넌트를 `useState`와 `useEffect`로 만들어 보세요. 언마운트될 때 타이머가 멈춰야 합니다.

> [!question]- 예시 답안
> ```jsx
> function Stopwatch() {
>   const [seconds, setSeconds] = useState(0);
>   useEffect(() => {
>     const id = setInterval(() => setSeconds(s => s + 1), 1000);
>     return () => clearInterval(id);
>   }, []);
>   return <p>{seconds}초</p>;
> }
> ```
> `setSeconds(s => s + 1)`처럼 함수형 업데이트를 쓰면 의존성 배열에 `seconds`를 넣지 않아도 됩니다 ([[렌더링 첫걸음 03 - React 첫 만남|레슨 03]] 6.2절).

---

## 9. 더 깊이 (심화)

### 9.1 컴포넌트가 아니라 Effect의 관점에서

react.dev는 "마운트 때 이걸, 언마운트 때 저걸"이라는 생명 주기 관점보다, **각 Effect가 무엇을 동기화하는지**로 생각하라고 권합니다: "An Effect can only do two things: to start synchronizing something, and later to stop synchronizing it." / "think from each individual Effect's perspective (how to start and stop synchronization) rather than from the component's perspective (how it mounts, updates, or unmounts)." (*Lifecycle of Reactive Effects*) 채팅방 Effect라면 "지금 `roomId` 방에 연결해 두기"만 생각하면, 마운트·`roomId` 변경·언마운트가 모두 "연결 → 끊기"의 반복으로 설명됩니다.

### 9.2 클래스 컴포넌트의 생명 주기 메서드

옛 코드의 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`는 함께 쓰면 `useEffect` 하나에 대응합니다: "For many use cases, defining `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` together in class components is equivalent to calling `useEffect` in function components." (react.dev *Component*) → [[React Component Lifecycle and Effects]] 6절

### 9.3 Effect는 서버에서 돌지 않는다

"Effects only run on the client. They don't run during server rendering." (react.dev *useEffect*) Next.js 같은 서버 렌더링에서 브라우저 전용 코드를 Effect에 넣는 이유입니다 ([[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]] 3.1절).

→ 전체 정리: [[React Component Lifecycle and Effects]]

---

## 관점 비교 / 출처별 차이

| 항목 | 설명 |
|---|---|
| "`[]`는 마운트 때만" | react.dev도 예제 주석에서 이렇게 설명하지만, 생명 주기 대신 "동기화 시작과 끝"으로 생각하라고 권함 (9.1절). 개발 모드 Strict Mode에서는 마운트 때 한 번 더 "정리 → 설정"이 일어남 (레슨 06) |
| 실행 시점 | 이 위키의 [[React Hooks and Component Design Patterns]]는 "`useEffect`는 페인트 후"로 요약했으나, 공식 문서 기준 상호작용이 원인이면 페인트 전일 수 있음 (2026-10-21 보정) |

---

## 참고 출처

- [[2026-React Official Documentation]]
- [react.dev, *Synchronizing with Effects*](https://react.dev/learn/synchronizing-with-effects)
- [react.dev, *Lifecycle of Reactive Effects*](https://react.dev/learn/lifecycle-of-reactive-effects)
- [react.dev, *useEffect*](https://react.dev/reference/react/useEffect)
- [react.dev, *Component*](https://react.dev/reference/react/Component)
