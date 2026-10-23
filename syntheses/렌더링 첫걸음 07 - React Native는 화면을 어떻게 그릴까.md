---
type: synthesis
title: "렌더링 첫걸음 07 - React Native는 화면을 어떻게 그릴까"
category: "H. Information Systems/Mobile"
sources:
  - "[[2026-React Native Official Documentation]]"
  - "[[2026-React Official Documentation]]"
  - "[[2022-React and React Native, Fourth Edition]]"
tags:
  - react-native
  - render-pipeline
  - shadow-tree
  - yoga
  - threading-model
  - uselayouteffect
  - beginner
  - rendering-first-steps
created: 2026-10-21
updated: 2026-10-21
---

# 렌더링 첫걸음 07 - React Native는 화면을 어떻게 그릴까

> **이전**: [[렌더링 첫걸음 06 - Effect 심화|06 Effect 심화]] · **다음**: [[렌더링 첫걸음 08 - 앱과 화면의 생명 주기|08 앱과 화면의 생명 주기]] · **목록**: [[렌더링 첫걸음 00 - 로드맵|로드맵]]

## 이 레슨에서 배우는 것

1. 브라우저가 없는 React Native에서 **누가 화면을 그리는지**
2. React Native의 렌더 파이프라인: **Render → Commit → Mount**
3. state가 바뀌면 **바뀐 뷰만** 고치는 원리
4. **UI 스레드와 JavaScript 스레드**, 그리고 두 개의 프레임 레이트
5. React Native에서 `useLayoutEffect`가 의미하는 것

> React Native 예제는 앱 실행 환경이 없어 **실행하지 않았습니다**. 모두 공식 문서(reactnative.dev)를 근거로 씁니다.

---

## 1. 브라우저가 없다

### 비유로 이해하기: 설계도 → 모형 → 실제 건물

[[렌더링 첫걸음 01 - 브라우저는 화면을 어떻게 그릴까|레슨 01]]에서 웹 화면은 **브라우저**가 HTML을 읽어 DOM을 만들고, 레이아웃을 계산해 칠했습니다. React Native 앱에는 브라우저도 HTML도 DOM도 없습니다. 대신 **iPhone·Android가 원래 가진 화면 부품(네이티브 뷰)** 을 씁니다.

| 비유 | React Native에서 | 어디에 있나 |
|---|---|---|
| 설계도 | **React Element Tree** (컴포넌트가 반환한 `<View>`, `<Text>`) | JavaScript |
| 치수를 잰 모형 | **React Shadow Tree** (크기·위치를 계산한 나무) | C++ |
| 실제 건물 | **Host View Tree** (OS의 진짜 화면 부품) | iOS / Android |

### 같은 코드, 다른 부품

```jsx
function MyComponent() {
  return (
    <View>
      <Text>Hello, World</Text>
    </View>
  );
}
```

(reactnative.dev *Render, Commit, and Mount*의 예제)

공식 문서는 이 코드가 Android에서는 `<View>` → `android.view.ViewGroup`, `<Text>` → `android.widget.TextView`가 되고, "Similarly for iOS a `UIView` is created and text is populated with a call to `NSLayoutManager`."라고 설명합니다 (*Render, Commit, and Mount*). 웹의 `<div>`·`<p>`에 해당하는 것이 OS별 부품입니다.

| | 웹 React (레슨 03–06) | React Native |
|---|---|---|
| 기본 부품 | `<div>`, `<p>`, `<button>` | `<View>`, `<Text>`, `<Pressable>` |
| 화면을 그리는 것 | 브라우저 (DOM → 레이아웃 → 페인트) | OS의 네이티브 뷰 |
| 레이아웃 계산 | 브라우저 | **Yoga** (C++ 레이아웃 엔진) |

> 컴포넌트, props, state, 훅, Effect 규칙은 **레슨 03–06과 같습니다**. 달라지는 것은 "커밋 다음에 누가 그리느냐"입니다.

---

## 2. Render → Commit → Mount

공식 문서는 React Native의 렌더 파이프라인을 세 단계로 나눕니다 (*Render, Commit, and Mount*).

```
[Render]  컴포넌트 함수 실행 → React Element Tree (JS)
              └ 동시에 React Shadow Tree (C++) 만들기
   ↓
[Commit]  Yoga로 레이아웃 계산 → "다음에 붙일 나무"로 올리기
   ↓
[Mount]   이전 나무와 비교 → 바뀐 부분만 네이티브 뷰에 반영 (UI 스레드)
```

### 2.1 Render: 설계도와 모형을 함께 만든다

"React executes product logic which creates a React Element Trees in JavaScript. From this tree, the renderer creates a React Shadow Tree in C++." (*Render, Commit, and Mount*)

모형(Shadow Node)은 **진짜 화면 부품이 될 것**에만 만들어집니다. "This happens only for _React Host Components_, not for React Composite Components." 위 예에서 `<View>`는 `ViewShadowNode`, `<Text>`는 `TextShadowNode`가 되지만, "Notably, there is never a _React Shadow Node_ that directly represents `<MyComponent>`." (같은 문서)

> 내가 만든 컴포넌트(`MyComponent`)는 **설계도를 정리하는 함수**일 뿐, 화면 부품이 아닙니다. 웹에서도 `MyComponent`라는 DOM 요소가 생기지 않는 것과 같습니다 (판단).

### 2.2 Commit: 치수를 재고 "다음 차례"로 올린다

커밋은 두 가지 일입니다 (같은 문서):

- **레이아웃 계산**: "In React Native, this involves invoking Yoga to calculate the layout of each _React Shadow Node_." 스타일(`flex`, `margin` 등)로 각 부품의 크기와 위치를 정합니다.
- **나무 올리기**: 새 Shadow Tree를 "next tree"로 올립니다. "The "next tree" mounts on the next "tick" of the UI Thread."

웹에서는 커밋(DOM 변경) **뒤에** 브라우저가 레이아웃을 계산했지만([[렌더링 첫걸음 04 - 렌더와 커밋|레슨 04]] 4.3절), React Native에서는 **커밋 단계 안에서** Yoga가 계산합니다 (판단: 위 두 설명을 비교).

### 2.3 Mount: 실제 부품에 반영

"The React Shadow Tree, now with the results of layout calculation, is transformed into a Host View Tree." 이 단계는 세 걸음입니다 (같은 문서).

1. **Tree Diffing**: 이전에 그린 나무와 다음 나무를 C++에서 비교해 `createView`, `updateView`, `removeView` 같은 **변경 목록**을 만듭니다.
2. **Tree Promotion**: 다음 나무를 "이전에 그린 나무"로 바꿔 둡니다 (다음 비교를 위해).
3. **View Mounting**: 변경 목록을 실제 뷰에 적용합니다. "This step executes in the _host platform_ on UI thread."

---

## 3. state가 바뀌면: 바뀐 부품만

### 3.1 빨강 → 노랑

공식 문서의 예: 상자 두 개 중 첫 번째의 배경색이 `'red'`에서 `'yellow'`로 바뀝니다.

```jsx
<View>
  <View style={{backgroundColor: 'yellow', height: 20, width: 20}} />
  <View style={{backgroundColor: 'blue', height: 20, width: 20}} />
</View>
```

Shadow Tree는 **고칠 수 없는(immutable)** 나무라서, 바뀐 노드와 그 **조상들만 복제**하고 나머지는 이전 나무와 **함께 씁니다**. "**React will only clone a React Element if it requires an update to its props, style, or children.** Any _React Elements_ that are unchanged by the state update are shared by the old and new trees." 파란 상자(Node 4)는 두 나무가 공유합니다 (*Render, Commit, and Mount*).

마운트 단계에서 나오는 변경 목록은 단 하나입니다: `UpdateView(Node 3, {backgroundColor: 'yellow'})`. "only the `backgroundColor` of **View 3** will be updated (to yellow)." (같은 문서)

### 3.2 레슨 04와 비교

| | 웹 React | React Native |
|---|---|---|
| 비교 결과 | 바뀐 DOM 노드만 수정 ([[렌더링 첫걸음 04 - 렌더와 커밋\|레슨 04]] 4.2절 실험) | 바뀐 네이티브 뷰만 수정 (`UpdateView`) |
| 바뀌지 않은 부분 | DOM 노드 재사용 | Shadow Node 공유 (구조 공유) |

원리는 같습니다: **React는 설계도를 새로 그리지만, 실제 부품은 바뀐 만큼만 고칩니다** (판단).

---

## 4. 두 개의 스레드

### 4.1 UI 스레드와 JavaScript 스레드

웹 브라우저에서는 JavaScript와 레이아웃·페인트가 대부분 **메인 스레드 하나**에서 돌았습니다 ([[렌더링 첫걸음 02 - 프레임과 이벤트 루프|레슨 02]]). React Native는 일을 두 스레드에 나눕니다 (*Threading Model*).

| 스레드 | 공식 설명 | 하는 일 |
|---|---|---|
| **UI 스레드** (main) | "The only thread that can manipulate host views." | 마운트, 스크롤, 네이티브 애니메이션 |
| **JavaScript 스레드** | "This is where React's render phase, as well as layout, are executed." | 컴포넌트 함수, 이벤트 처리기, Effect, Yoga 레이아웃 |

"This is the most common scenario where most of the render pipeline happens on JavaScript thread." (*Threading Model*, "Render in a JS Thread")

### 4.2 1초에 60장, 그런데 두 개

React Native도 프레임 예산은 같습니다: "iOS and Android devices display at least 60 frames per second, which gives you and the UI system at most 16.67ms to do all of the work needed to generate the static image (frame)" (reactnative.dev *Performance Overview*). 레슨 02의 16.67 ms와 같은 숫자입니다.

다른 점은 **프레임 레이트가 두 개**라는 것입니다. 개발 메뉴의 Perf Monitor는 **JS 프레임 레이트**와 **UI 프레임 레이트**를 따로 보여 줍니다 (같은 문서).

| 프레임이 떨어지는 스레드 | 공식 문서의 예 |
|---|---|
| JavaScript 스레드 | 무거운 다시 렌더가 200 ms 걸리면 "result in 12 frames being dropped"; JavaScript로 제어하는 애니메이션이 멈춤; `TouchableOpacity`가 터치에 늦게 반응 |
| UI 스레드 | 뷰를 움직일 때(스크롤·이동·회전), 특히 Android에서 이미지 위 투명 배경 글자처럼 매 프레임 다시 합성해야 할 때; iOS에서 `Image`의 너비·높이를 애니메이션할 때 |

반대로, JavaScript 스레드가 막혀도 **스크롤은 됩니다**: "you can happily scroll up and down through a `ScrollView` when the JavaScript thread is locked up because the `ScrollView` lives on the main thread." 네이티브 스택 내비게이터의 화면 전환 애니메이션도 "executed on the native main UI thread, so they are not interrupted by frame drops on the JavaScript thread." (*Performance Overview*)

> 웹에서는 메인 스레드가 막히면 거의 모든 것이 멈췄지만(레슨 02), React Native에서는 **UI 스레드가 맡은 일**은 계속 움직일 수 있습니다. 대신 화면 **내용**을 바꾸는 렌더는 JavaScript 스레드가 비어야 진행됩니다 (판단).

---

## 5. `useLayoutEffect`가 제대로 동작한다

### 5.1 예전: 한 프레임 늦게

[[렌더링 첫걸음 06 - Effect 심화|레슨 06]]의 툴팁 문제를 React Native에서 풀려면 예전에는 `onLayout` 이벤트를 썼습니다. 공식 블로그: "In the old architecture, you needed to use the asynchronous `onLayout` event to read layout information of a view (which was also asynchronous). As a result there would be at least one frame where the layout was incorrect until the layout was read and updated, causing issues like tooltips placed in the wrong position" (*The New Architecture is Here*, 2024-10-23)

### 5.2 지금: 같은 프레임 안에서

```jsx
// New Architecture (블로그 예제 요약, 실행하지 않음)
useLayoutEffect(() => {
  const rect = ref.current?.getBoundingClientRect(); // 동기적으로 레이아웃 읽기
  setPosition(rect);
}, []);
```

"This change allows you to read layout information synchronously and update the UI in the same frame, allowing you to position elements correctly before they are displayed to the user" (같은 블로그). 웹에서 `useLayoutEffect`가 "페인트 전"을 보장하는 것과 같은 역할을 React Native에서도 하게 된 것입니다 (판단).

---

## 6. 정리

| 낱말 | 뜻 |
|---|---|
| 네이티브 뷰 (Host View) | iOS·Android가 가진 실제 화면 부품 (`UIView`, `ViewGroup` 등) |
| React Shadow Tree | 렌더 결과를 C++에 옮긴, 레이아웃 계산용 나무. 고칠 수 없고 복제·공유로 갱신 |
| Yoga | 커밋 단계에서 Flexbox 스타일로 크기·위치를 계산하는 레이아웃 엔진 |
| Render → Commit → Mount | 나무 만들기 → 레이아웃·다음 나무 올리기 → 비교 후 바뀐 뷰만 반영 |
| UI 스레드 | 네이티브 뷰를 만질 수 있는 유일한 스레드 |
| JavaScript 스레드 | 렌더 단계와 레이아웃이 실행되는 곳 |
| JS / UI 프레임 레이트 | 두 스레드 각각의 초당 프레임 수 |

---

## 7. 자주 하는 실수

| 실수 | 결과 | 바로잡기 |
|---|---|---|
| "React Native는 웹뷰 안에서 HTML을 그린다"고 생각 | 구조를 잘못 이해 | 네이티브 뷰로 그림 (1절) |
| 내 컴포넌트마다 네이티브 뷰가 하나씩 생긴다고 생각 | 불필요한 걱정 | Shadow Node는 Host Component에만 생김 (2.1절) |
| 무거운 계산을 이벤트 처리기·렌더에서 그대로 | JS 프레임 하락, 터치 반응 지연 | 작업 나누기, 지연, 메모이제이션 ([[렌더링 첫걸음 02 - 프레임과 이벤트 루프\|레슨 02]] 원리와 같음) |
| 개발 모드에서 성능을 측정 | 실제보다 훨씬 느리게 보임 | 릴리스 빌드로 측정 (9.3절) |
| 보이기 전 위치 보정을 `onLayout`으로 | 한 프레임 잘못된 위치 | New Architecture에서는 `useLayoutEffect` (5절) |

---

## 8. 확인 퀴즈

**Q1.** 아래 코드에서 React Shadow Node는 몇 개 만들어질까요?
```jsx
function Card() {
  return (
    <View>
      <Text>제목</Text>
    </View>
  );
}
```

> [!question]- 정답 보기
> 2개입니다. `<View>`와 `<Text>`(Host Component)에만 만들어지고, `Card`(내가 만든 컴포넌트)에는 만들어지지 않습니다. (View Flattening은 9.1절)

**Q2.** 웹 React와 React Native에서 레이아웃은 각각 누가 계산할까요?

> [!question]- 정답 보기
> 웹은 커밋 뒤 **브라우저**가, React Native는 커밋 단계에서 **Yoga**가 계산합니다.

**Q3.** 버튼을 누르면 2초 동안 계산하는 코드가 JavaScript 스레드에서 돌고 있습니다. 그동안 `ScrollView`를 스크롤할 수 있을까요? 화면의 숫자는 바뀔까요?

> [!question]- 정답 보기
> 스크롤은 됩니다. `ScrollView`는 UI 스레드에 있기 때문입니다. 하지만 state를 바꿔 숫자를 새로 그리는 렌더는 JavaScript 스레드가 비어야 진행되므로, 계산이 끝날 때까지 바뀌지 않습니다.

**Q4.** 상자 세 개 중 하나의 색만 바뀌었습니다. 마운트 단계에서 네이티브 뷰는 몇 개 수정될까요?

> [!question]- 정답 보기
> 1개입니다. Tree Diffing이 바뀐 부분만 변경 목록(`UpdateView`)으로 만듭니다 (3.1절 공식 예제).

**Q5.** React Native에서 툴팁을 보이기 전에 정확한 위치에 두려면 어떤 훅을 쓸까요?

> [!question]- 정답 보기
> New Architecture에서는 `useLayoutEffect`입니다. 레이아웃을 동기적으로 읽고 같은 프레임 안에서 고칠 수 있습니다.

---

## 9. 더 깊이 (심화)

### 9.1 View Flattening: 보이지 않는 상자 없애기

`margin`만 주려고 `<View>`로 감싸는 일이 많습니다. 공식 문서는 이런 노드를 **"Layout-Only"** 노드라고 부르고, 렌더러가 **비교(diffing) 단계에서** 이를 합쳐 네이티브 뷰 계층을 얕게 만든다고 설명합니다. "This algorithm takes into consideration props like: `margin`, `padding`, `backgroundColor`, `opacity`, etc." / "From the user's perspective there are no visible changes on the screen." (*View Flattening*)

얼마나 줄어드는지: "In current production tests, a _React Shadow Tree_ typically consists of about 600-1000 _React Shadow Nodes_ (before view flattening), the trees get reduced to ~200 nodes after view flattening." (*Render, Commit, and Mount*)

### 9.2 스레드 시나리오와 우선순위

공식 문서는 흔한 경우(JS 스레드) 외의 시나리오도 보여 줍니다 (*Threading Model*):

| 시나리오 | 설명 |
|---|---|
| Render in the UI Thread | "When there is a high priority event on the UI Thread, the renderer is able to execute all the render pipeline synchronously on the UI thread." |
| Discrete event interruption | 렌더 단계는 중단될 수 있고, 높은 우선순위 이벤트와 합쳐져 UI 스레드에서 동기적으로 실행됨 |
| C++ State update | UI 스레드에서 시작되고 렌더 단계를 건너뜀 (예: `ScrollView`의 스크롤 위치) |

New Architecture의 **이벤트 루프**는 HTML 표준을 따르도록 정의되어, "React is able to predictably order updates and events. This allows React to interrupt a low priority update with an urgent user event" (*The New Architecture is Here*). 레슨 02의 이벤트 루프 모델이 React Native에서도 점점 같은 의미가 되어 간다는 뜻입니다 (판단).

### 9.3 성능 측정 전에 확인할 것

- 개발 모드: "JavaScript thread performance suffers greatly when running in dev mode." → 릴리스 빌드에서 측정 (*Performance Overview*)
- `console.log`: "When running a bundled app, these statements can cause a big bottleneck in the JavaScript thread." (같은 문서)
- 큰 목록: `FlatList`가 느리면 `getItemLayout`을 구현하라고 안내 (같은 문서)

### 9.4 예전 구조(Bridge)와의 차이

예전 아키텍처 설명에서는 Yoga가 별도의 **Shadow Thread**에서 실행된다고 했습니다 ([[React Native Architecture (Bridge to New Architecture)]], 도서 p.317). 현재 공식 문서는 렌더 파이프라인 대부분이 **JavaScript 스레드**에서 실행된다고 설명합니다. 구조 전체(JSI, Fabric, TurboModules)는 그 페이지와 [[React and React Native Internal Implementation Deep Dive]]에서 다룹니다.

→ 전체 정리: [[React Native Rendering and App Lifecycle]] 1–3절

---

## 관점 비교 / 출처별 차이

| 항목 | 출처별 서술 |
|---|---|
| 레이아웃 스레드 | [[2022-React and React Native, Fourth Edition]] (PDF p.317): "a separate thread called Shadow. It recalculates the layout of the application using the Yoga engine". 현재 공식 *Threading Model*: "React's render phase, as well as layout" → JavaScript 스레드 |
| 커밋이 실행되는 스레드 | *Render, Commit, and Mount*의 커밋 단계 설명: "These operations are asynchronously executed on a background thread." *Threading Model*: 흔한 경우 렌더 파이프라인 대부분이 JavaScript 스레드. JavaScript 스레드를 UI 스레드 기준의 "background thread"로 부른 것으로 보이나, 문서에서 이를 명시한 문장은 확인하지 못함 (미확인: 두 문서의 그림 *case-1.jpg*를 보면 확인 가능) |
| 성능 문서의 서술 | *Performance Overview*: "Updates to native-backed views are batched and sent over to the native side at the end of each iteration of the event loop" — Bridge 시절 표현이 남아 있을 수 있음 (판단) |

---

## 참고 출처

- [[2026-React Native Official Documentation]]
- [reactnative.dev, *Render, Commit, and Mount*](https://reactnative.dev/architecture/render-pipeline)
- [reactnative.dev, *Threading Model*](https://reactnative.dev/architecture/threading-model)
- [reactnative.dev, *View Flattening*](https://reactnative.dev/architecture/view-flattening)
- [reactnative.dev, *Performance Overview*](https://reactnative.dev/docs/performance)
- [React Native 블로그, *The New Architecture is Here*](https://reactnative.dev/blog/2024/10/23/the-new-architecture-is-here)
- [[2022-React and React Native, Fourth Edition]] Ch 15 (PDF p.317)
- [[React Native Architecture (Bridge to New Architecture)]] (도서 기반 구조 설명)
