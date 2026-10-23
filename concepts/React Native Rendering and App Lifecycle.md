---
type: concept
title: "React Native Rendering and App Lifecycle"
category: "H. Information Systems/Mobile"
sources:
  - "[[2026-React Native Official Documentation]]"
  - "[[2026-Expo Official Documentation]]"
tags:
  - react-native
  - render-pipeline
  - fabric
  - yoga
  - threading-model
  - appstate
  - navigation-lifecycle
  - usefocuseffect
created: 2026-10-21
updated: 2026-10-21
---

# React Native Rendering and App Lifecycle

## 정의 (Definition)

[[React Native]]는 [[React]]와 같은 컴포넌트·훅·Effect 규칙을 쓰지만, **화면을 그리는 것은 브라우저가 아니라 각 OS의 네이티브 뷰**다. 그래서 웹의 [[Browser Rendering Pipeline and Frame Lifecycle|브라우저 렌더링 파이프라인]] 대신 React Native 고유의 **렌더 파이프라인(Render → Commit → Mount)** 이 있고, 웹에 없는 **앱 생명 주기**(포그라운드·백그라운드)와 **화면 생명 주기**(내비게이션 포커스)가 있다.

입문자용 설명은 [[렌더링 첫걸음 00 - 로드맵|렌더링 첫걸음 시리즈]] 레슨 07–08.

---

## 1. 렌더 파이프라인: Render → Commit → Mount

reactnative.dev *Render, Commit, and Mount*:

| 단계 | 공식 설명 | 웹과 비교 (판단) |
|---|---|---|
| **Render** | "React executes product logic which creates a React Element Trees in JavaScript. From this tree, the renderer creates a React Shadow Tree in C++." | React의 렌더 단계와 같음. 결과물이 DOM 변경 목록이 아니라 C++ Shadow Tree |
| **Commit** | "After a React Shadow Tree is fully created, the renderer triggers a commit. … This also schedules calculation of its layout information." 레이아웃은 "invoking Yoga to calculate the layout of each React Shadow Node" | 웹에서는 커밋 뒤 **브라우저**가 레이아웃을 계산하지만, RN에서는 **Yoga**가 계산 |
| **Mount** | "The React Shadow Tree, now with the results of layout calculation, is transformed into a Host View Tree." / "This step executes in the host platform on UI thread." | 웹의 페인트·합성에 해당하는 일을 OS의 네이티브 뷰가 맡음 |

커밋이 백그라운드 스레드에서 실행되면 "the mounting phase is scheduled for the next 'tick' of UI thread." (*Render, Commit, and Mount*)

더 깊은 구조(JSI, Fabric, TurboModules, Codegen)는 [[React Native Architecture (Bridge to New Architecture)]], [[React and React Native Internal Implementation Deep Dive]].

---

## 2. 스레드 모델

reactnative.dev *Threading Model*:

| 스레드 | 공식 설명 |
|---|---|
| **UI 스레드** (main) | "The only thread that can manipulate host views." |
| **JavaScript 스레드** | "This is where React's render phase, as well as layout, are executed." |

"This is the most common scenario where most of the render pipeline happens on JavaScript thread." (*Threading Model*)

웹과 비교하면 (판단):

| | 웹 브라우저 | React Native |
|---|---|---|
| JavaScript 실행 | 메인 스레드 | JavaScript 스레드 (엔진: [[JavaScript (ECMAScript)\|Hermes]]) |
| 레이아웃 계산 | 브라우저 레이아웃 (메인 스레드) | Yoga (주로 JavaScript 스레드) |
| 화면 조작 | 메인 스레드의 페인트 + 컴포지터 스레드의 합성 | UI 스레드만 네이티브 뷰를 조작 |
| 엔티티 | [[Web Browser Architecture (Chromium Blink)]] | [[React Native]] |

JavaScript 스레드가 긴 작업으로 막히면 다음 렌더가 늦어지는 원리는 웹과 같다 (판단: [[Web Performance Metrics and Core Web Vitals]]의 긴 작업 개념).

---

## 3. `useLayoutEffect`와 동기 레이아웃 (New Architecture)

React Native 공식 블로그 *The New Architecture is Here* (2024-10-23):
- "the Event Loop and the New Renderer changes to support reading layout synchronously allow React Native to add proper support for `useLayoutEffect` to read layout information synchronously and update the UI in the same frame. This allows you to position elements correctly before they are displayed to the user."
- "The New Architecture adds full support for modern React features, including Suspense, Transitions, automatic batching, and `useLayoutEffect`."

React의 `useLayoutEffect` 규칙("fires before the browser repaints the screen", react.dev)이 RN에서는 "같은 프레임에서 사용자가 보기 전에"로 대응된다. 웹 쪽 설명은 [[React Component Lifecycle and Effects]] 2절.

---

## 4. 앱 생명 주기: `AppState`

"`AppState` can tell you if the app is in the foreground or background, and notify you when the state changes." (reactnative.dev *AppState*)

| 상태 | 공식 설명 |
|---|---|
| `active` | "The app is running in the foreground" |
| `background` | "The app is running in the background." 다른 앱, 홈 화면, (Android) 다른 Activity |
| `inactive` (iOS) | 포그라운드와 백그라운드 사이를 오갈 때, 멀티태스킹 화면, 알림 센터, 전화 수신 등 |

웹에서 가장 가까운 것은 **탭** 단위의 Page Visibility API다: 창을 최소화하거나 다른 탭으로 바꾸면 "the API sends a `visibilitychange` event" (MDN *Page Visibility API*). 또 브라우저는 숨은 탭에서 대부분 `requestAnimationFrame` 콜백을 멈추고 타이머를 늦춘다 (같은 문서). 앱에서는 백그라운드로 갈 때 타이머·구독을 멈추고, 돌아오면 데이터를 새로 고치는 식으로 쓴다 (판단).

구독 예 — 공식 문서 *AppState*의 Basic Usage 예제를 줄인 것. 이전 상태를 `useRef`에 기억해 두고 `inactive|background → active` 전환을 "포그라운드로 돌아옴"으로 판정한다:

```tsx
const appState = useRef(AppState.currentState);
useEffect(() => {
  const subscription = AppState.addEventListener('change', nextAppState => {
    if (appState.current?.match(/inactive|background/) && nextAppState === 'active') {
      console.log('App has come to the foreground!');
    }
    appState.current = nextAppState;
  });
  return () => {
    subscription.remove();
  };
}, []);
```

Android 전용 `focus`/`blur` 이벤트도 있다: 알림 서랍을 내릴 때처럼 "`AppState` won't change but the `blur` event will get fired." (*AppState*)

---

## 5. 화면 생명 주기: 화면은 언마운트되지 않는다

### 5.1 웹과 다른 점

[[React Navigation]] 공식 문서 *Navigation lifecycle*: "Unlike web browsers, React Navigation doesn't unmount screens when navigating away." 스택에서 Home → Profile로 이동해도 Home은 **마운트된 채로** 남는다.

결과적으로 `useEffect`의 정리 함수는 "화면을 떠날 때"가 아니라 "화면이 스택에서 완전히 제거될 때" 실행된다 (판단: 위 문장과 react.dev *Synchronizing with Effects*의 정리 함수 설명 "one final time when the component unmounts (gets removed)"에서).

### 5.2 포커스 기반 Effect

| 도구 | 공식 설명 | 쓰는 곳 |
|---|---|---|
| `focus` / `blur` 이벤트 | "To detect when a screen gains or loses focus, we can listen to `focus` and `blur` events" (*Navigation lifecycle*) | 이벤트 구독 |
| `useFocusEffect` | "The `useFocusEffect` is analogous to React's `useEffect` hook. The only difference is that it only runs if the screen is currently focused." 콜백은 `useCallback`으로 감싸야 함 (*useFocusEffect*) | 화면에 있을 때만 구독·폴링·카메라 |
| `useIsFocused` | "returns a boolean indicating whether the screen is focused" (*Navigation lifecycle*) | 포커스 여부로 다른 UI 렌더링 |

[[Expo]] Router도 같은 이름의 `useFocusEffect`를 제공한다: "Hook to run an effect whenever a route is focused." 콜백은 "should be wrapped in React.useCallback to avoid running the effect too often." (docs.expo.dev *Router hooks*)

### 5.3 무엇을 언제 쓸까 (판단)

| 하고 싶은 일 | 웹 React | React Native |
|---|---|---|
| 컴포넌트가 처음 나타날 때 한 번 | `useEffect(fn, [])` | 같음 |
| 화면이 보일 때마다 시작, 안 보이면 멈춤 | `useEffect` (라우트 이동 시 보통 언마운트됨) | `useFocusEffect` |
| 앱(웹은 탭)이 백그라운드로 가면 멈춤 | `visibilitychange` 이벤트 (Page Visibility API) | `AppState` 구독 |
| 레이아웃을 읽고 보이기 전에 위치 보정 | `useLayoutEffect` | `useLayoutEffect` (New Architecture) |

---

## 6. 관점 비교 / 출처별 차이

| 주제 | 기존 위키 서술 | 이 페이지 / 공식 문서 기준 |
|---|---|---|
| "컴포넌트 생명주기 & Hooks" | [[Web React vs React Native Shared Core and Differences]]: "웹과 100% 동일" | 훅의 문법과 규칙은 같지만, 화면 단위 생명 주기는 다름 (5.1절). 이 표현을 2026-10-21에 그 페이지에서 보정함 |
| `useFocusEffect` 정리 시점 | React Navigation 7.x *useFocusEffect*: "when the screen unmounts or blurs" | Expo *Router hooks*: "runs when the screen loses focus — not on unmount." 표현이 다름, 실제 동작 미확인 (확인 방법: Expo Router 앱에서 정리 함수에 로그를 넣고 포커스된 화면을 스택에서 제거) |
| 레이아웃 계산 위치 | [[React Native Architecture (Bridge to New Architecture)]]: Yoga Shadow 스레드 (구형 아키텍처 설명) | 현재 문서: Yoga 레이아웃은 커밋 단계에서 계산되며, 흔한 경우 JavaScript 스레드에서 렌더 파이프라인 대부분이 실행됨 (*Threading Model*) |

---

## 관련 개념 및 엔티티

- **입문 레슨**: [[렌더링 첫걸음 00 - 로드맵]] (레슨 07–08)
- **상위/연관 개념**: [[React Component Lifecycle and Effects]], [[Browser Rendering Pipeline and Frame Lifecycle]], [[React Native Architecture (Bridge to New Architecture)]], [[Web React vs React Native Shared Core and Differences]], [[Offline-First and Mobile State Management]]
- **구현 기술/엔티티**: [[React Native]], [[React Navigation]], [[Expo]], [[Web Browser Architecture (Chromium Blink)]]
- **출처 문헌**: [[2026-React Native Official Documentation]], [[2026-Expo Official Documentation]], reactnative.dev [*Render, Commit, and Mount*](https://reactnative.dev/architecture/render-pipeline), [*Threading Model*](https://reactnative.dev/architecture/threading-model), [*AppState*](https://reactnative.dev/docs/appstate), [*The New Architecture is Here*](https://reactnative.dev/blog/2024/10/23/the-new-architecture-is-here), React Navigation [*Navigation lifecycle*](https://reactnavigation.org/docs/navigation-lifecycle), [*useFocusEffect*](https://reactnavigation.org/docs/use-focus-effect), Expo [*Router hooks*](https://docs.expo.dev/router/reference/hooks/), MDN [*Page Visibility API*](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API)
