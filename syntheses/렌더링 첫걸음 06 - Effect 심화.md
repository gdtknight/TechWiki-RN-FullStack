---
type: synthesis
title: "렌더링 첫걸음 06 - Effect 심화"
category: "H. Information Systems/React"
sources:
  - "[[2026-React Official Documentation]]"
  - "[[2026-Web Performance Engineering in the Age of AI]]"
tags:
  - react
  - uselayouteffect
  - strict-mode
  - race-condition
  - derived-state
  - beginner
  - rendering-first-steps
created: 2026-10-21
updated: 2026-10-21
---

# 렌더링 첫걸음 06 - Effect 심화

> **이전**: [[렌더링 첫걸음 05 - 생명 주기와 useEffect|05 생명 주기와 useEffect]] · **다음**: [[렌더링 첫걸음 07 - React Native는 화면을 어떻게 그릴까|07 React Native는 화면을 어떻게 그릴까]] · **목록**: [[렌더링 첫걸음 00 - 로드맵|로드맵]]

## 이 레슨에서 배우는 것

1. **`useLayoutEffect`**: 화면을 칠하기 **전에** 실행되는 Effect
2. 클릭 때문에 생긴 Effect는 **페인트 전에** 실행될 수도 있다
3. 개발 모드 **Strict Mode**가 Effect를 두 번 실행하는 이유
4. **Effect가 필요 없는 경우** 세 가지
5. 데이터 가져오기의 **경쟁 상태**와 `ignore` 플래그

> 예제는 React 19.3.1 + react-dom + jsdom에서 실행해 결과를 확인했습니다. 페인트와의 순서는 공식 문서를 근거로 씁니다.

---

## 1. `useLayoutEffect`: 칠하기 전에

### 1.1 페인트 전과 후

[[렌더링 첫걸음 05 - 생명 주기와 useEffect|레슨 05]]의 `useEffect`는 대개 **페인트 뒤**에 실행됩니다. 그런데 화면에 보이기 **전에** 꼭 해야 하는 일이 있습니다. react.dev: "`useLayoutEffect` is a version of `useEffect` that fires before the browser repaints the screen." / "React guarantees that the code inside `useLayoutEffect` and any state updates scheduled inside it will be processed **before the browser repaints the screen.**" (*useLayoutEffect*)

```
렌더 → 커밋 → useLayoutEffect → [페인트] → useEffect (대개)
```

실험에서도 `useLayoutEffect`가 모두 끝난 뒤에 `useEffect`가 실행되었고, `useLayoutEffect` 안에서는 이미 새 DOM이 보였습니다 ([[React Component Lifecycle and Effects]] 2.2절).

### 1.2 언제 쓰나: 툴팁 위치 맞추기

react.dev의 예: 툴팁을 버튼 위에 띄울지 아래에 띄울지는 **툴팁의 실제 높이**를 알아야 정할 수 있습니다. "In order to render the tooltip at the right final position, you need to know its height (i.e. whether it fits at the top)." (*useLayoutEffect*)

```jsx
function Tooltip() {
  const ref = useRef(null);
  const [tooltipHeight, setTooltipHeight] = useState(0); // 아직 모름

  useLayoutEffect(() => {
    const { height } = ref.current.getBoundingClientRect();
    setTooltipHeight(height); // 실제 높이를 알았으니 다시 렌더
  }, []);
  // ...tooltipHeight로 위치 계산...
}
```

(react.dev *useLayoutEffect*의 예제 코드)

`useEffect`로 하면 "잘못된 위치로 한 번 칠한 뒤 → 다시 고쳐 칠하기"가 되어 **깜빡임**이 보일 수 있습니다. react.dev: "If your Effect is doing something visual (for example, positioning a tooltip), and the delay is noticeable (for example, it flickers), replace `useEffect` with `useLayoutEffect`." (*useEffect* Caveats)

### 1.3 하지만 기본은 `useEffect`

"`useLayoutEffect` can hurt performance. Prefer `useEffect` when possible." (*useLayoutEffect*) 페인트를 **붙잡고** 실행되므로, 여기서 무거운 일을 하면 [[렌더링 첫걸음 02 - 프레임과 이벤트 루프|레슨 02]]의 프레임 예산을 넘겨 화면이 늦게 나옵니다 (판단). react.dev도 "this shouldn't be needed for the vast majority of Effects."라고 합니다 (*useEffect* Troubleshooting).

---

## 2. 클릭 때문에 생긴 Effect는 다르다

레슨 05에서 `useEffect`는 "**대개**" 페인트 뒤라고 했습니다. 예외가 있습니다. react.dev: "If your Effect is caused by an interaction (like a click), **React may run your Effect before the browser paints the updated screen**. This ensures that the result of the Effect can be observed by the event system." (*useEffect* Caveats)

| Effect의 원인 | `useEffect` 실행 |
|---|---|
| 첫 렌더, 데이터 도착 등 (상호작용 아님) | 대개 페인트 **후** |
| 클릭 같은 상호작용 | 페인트 **전**일 수 있음 |

그래서 "`useEffect`는 항상 화면이 그려진 다음"이라고 외우면 틀립니다. 꼭 페인트 뒤에 해야 하는 일(예: `alert()`)이라면 react.dev는 `setTimeout`으로 미루라고 안내합니다 (같은 Caveats).

반대 방향의 주의도 있습니다. Effect가 페인트 전에 실행되더라도, **그 안에서 바꾼 state**가 반영되기 전에 페인트가 먼저 일어날 수 있습니다: "Even if your Effect was caused by an interaction (like a click), **React may allow the browser to repaint the screen before processing the state updates inside your Effect.**" 페인트를 반드시 막아야 한다면 `useLayoutEffect`를 씁니다 (같은 Caveats). 정리하면, **페인트 전을 보장받는 것은 `useLayoutEffect`뿐**입니다 (판단, 위 두 문장에서).

---

## 3. Strict Mode: 개발 중에 두 번

### 3.1 무슨 일이 일어나나

`<StrictMode>`로 감싼 개발 빌드에서 채팅 연결 Effect를 실행해 봤습니다.

```jsx
function ChatRoom({ roomId }) {
  useEffect(() => {
    console.log(`✅ ${roomId} 연결`);
    return () => console.log(`❌ ${roomId} 연결 끊기`);
  }, [roomId]);
  return <p>{roomId}</p>;
}
```

| 상황 | 로그 (실행 결과) |
|---|---|
| 개발 빌드 + StrictMode 마운트 | `✅ general 연결` → `❌ general 연결 끊기` → `✅ general 연결` |
| 개발 빌드, 이어서 `roomId` 변경 | `❌ general 연결 끊기` → `✅ travel 연결` (두 번 하지 않음) |
| 프로덕션 빌드 (`NODE_ENV=production`) + StrictMode 마운트 | `✅ general 연결` (한 번) |

### 3.2 왜 두 번 할까

react.dev: "In Strict Mode, React mounts components twice (in development only!) to stress-test your Effects." / "By remounting your component, React verifies that navigating away and back would not break your code." (*Synchronizing with Effects*)

**정리 함수를 제대로 만들었는지 시험**하는 것입니다. 정리가 없으면 개발 중에 연결이 두 개 생기는 등 문제가 바로 드러납니다.

### 3.3 막으려 하지 말고 고치기

"The right question isn't "how to run an Effect once", but "how to fix my Effect so that it works after remounting"." / "Usually, the answer is to implement the cleanup function." 기준: "the user shouldn't be able to distinguish between the Effect running once (as in production) and a *setup → cleanup → setup* sequence (as you'd see in development)." (*Synchronizing with Effects*)

흔한 잘못은 `useRef`로 "이미 연결했으면 건너뛰기"를 만드는 것입니다. 로그는 한 번만 찍히지만 정리가 없으니, 화면을 떠났다 돌아올 때마다 연결이 쌓입니다. react.dev: "This makes it so you only see `"✅ Connecting..."` once in development, but it doesn't fix the bug." (*Synchronizing with Effects*, "Don't use refs to prevent Effects from firing")

> [[렌더링 첫걸음 04 - 렌더와 커밋|레슨 04]]에서 본 "컴포넌트 함수를 두 번 부르기"(순수성 검사)와 같은 Strict Mode의 개발 전용 기능입니다. "Strict Mode has no effect in production" (*Keeping Components Pure*)

---

## 4. Effect가 필요 없는 경우

react.dev *You Might Not Need an Effect*는 두 가지를 먼저 꼽습니다: "**You don't need Effects to transform data for rendering.**" / "**You don't need Effects to handle user events.**"

### 4.1 다른 값으로 계산할 수 있으면 state로 만들지 않는다

```jsx
// ❌ Effect로 계산해 state에 넣기
const [fullName, setFullName] = useState("");
useEffect(() => {
  setFullName(last + first);
}, [first, last]);

// ✅ 렌더 중에 바로 계산
const fullName = last + first;
```

react.dev: "**When something can be calculated from the existing props or state, don't put it in state. Instead, calculate it during rendering.**"

실험 (실행 결과):

| 방법 | 마운트 렌더 횟수 | 이름 바꾼 뒤 추가 렌더 |
|---|---|---|
| Effect로 계산 | 2번 | 2번 |
| 렌더 중 계산 | 1번 | 1번 |

Effect 방식은 "빈 값으로 한 번 커밋 → Effect가 state 변경 → 다시 렌더"가 되어 **두 배로 일합니다**. 첫 커밋에서는 이름이 빈 채로 그려질 수도 있습니다 (판단: 1번째 커밋의 `fullName`은 `""`).

### 4.2 클릭 때문에 하는 일은 이벤트 처리기에서

"구매 버튼을 누르면 서버에 보낸다"는 클릭이라는 **사건**의 결과이므로 `onClick` 안에서 합니다 (react.dev가 꼽은 두 번째 경우). Effect는 "화면이 이 상태인 동안 맞춰 둬야 하는 일"에만 씁니다 ([[렌더링 첫걸음 05 - 생명 주기와 useEffect|레슨 05]] 2.1절).

### 4.3 대상이 바뀌면 `key`로 초기화

"사용자가 바뀌면 입력값을 비운다"를 Effect로 하지 말고 `key`를 씁니다. react.dev: "To reset the state of an entire component tree, pass a different `key` to it."

```jsx
<Profile userId={userId} key={userId} />
```

[[렌더링 첫걸음 04 - 렌더와 커밋|레슨 04]] 5.2절에서 실험한 그대로입니다.

---

## 5. 데이터 가져오기와 경쟁 상태

### 5.1 늦게 온 응답이 이긴다

검색어가 바뀔 때마다 결과를 가져오는 Effect입니다.

```jsx
useEffect(() => {
  fetchResults(query).then(json => setResults(json));
}, [query]);
```

실험: "사과" 검색 응답은 200 ms, "바나나"는 50 ms 걸리게 만들고, "사과"를 검색한 직후 "바나나"로 바꿨습니다.

| 버전 | 최종 화면 (실행 결과) |
|---|---|
| 위 코드 | **`바나나 검색 → 사과 결과`** ❌ |
| 아래 `ignore` 버전 | `바나나 검색 → 바나나 결과` ✅ |

"바나나" 결과가 먼저 왔지만, 뒤늦게 도착한 "사과" 결과가 덮어썼습니다. 이렇게 **도착 순서에 따라 결과가 틀어지는 문제**를 **경쟁 상태**(race condition)라고 합니다.

### 5.2 해결: 정리 함수에서 "이제 무시해"

react.dev의 해결법 (*You Might Not Need an Effect*):

```jsx
useEffect(() => {
  let ignore = false;
  fetchResults(query).then(json => {
    if (!ignore) {
      setResults(json);
    }
  });
  return () => {
    ignore = true;   // 다음 Effect가 시작되기 전에 이전 요청은 무시
  };
}, [query]);
```

"You can fetch data with Effects, but you need to implement cleanup to avoid race conditions." `query`가 바뀌면 이전 Effect의 정리 함수가 먼저 실행되어(레슨 05 3.2절), 이전 요청의 `ignore`가 `true`가 됩니다. [[JS 첫걸음 10 - 스코프와 클로저|JS 첫걸음 10]]의 클로저 덕분에 각 요청은 **자기 Effect의** `ignore`를 기억합니다.

> 실무에서는 [[TanStack Query and Zustand|TanStack Query]] 같은 데이터 라이브러리가 이런 처리를 대신해 주는 경우가 많습니다 (판단).

---

## 6. 정리

| 낱말 | 뜻 |
|---|---|
| `useLayoutEffect` | 페인트 전에 실행되는 Effect. 레이아웃 측정·위치 보정용. 기본은 `useEffect` |
| 상호작용이 원인인 Effect | 페인트 전에 실행될 수 있음 |
| Strict Mode 두 번 실행 | 개발 중에만 마운트 → 정리 → 마운트로 정리 함수를 시험 |
| 파생 값 | 다른 state·props로 계산할 수 있는 값. state·Effect 대신 렌더 중 계산 |
| 경쟁 상태 | 늦게 도착한 옛 응답이 새 결과를 덮어쓰는 문제 |
| `ignore` 플래그 | 정리 함수에서 이전 요청 결과를 무시하게 하는 패턴 |

---

## 7. 자주 하는 실수

| 실수 | 결과 | 바로잡기 |
|---|---|---|
| 측정·위치 보정을 `useEffect`로 | 잘못된 위치가 잠깐 보였다가 고쳐짐 (깜빡임) | `useLayoutEffect` (1.2절) |
| 모든 Effect를 `useLayoutEffect`로 | 페인트가 늦어짐 | 기본은 `useEffect` (1.3절) |
| Strict Mode에서 두 번 실행되는 걸 막으려고 `useRef`로 "한 번만" 처리 | 정리 버그가 숨음 | 정리 함수를 만듦 (3.3절) |
| 계산 가능한 값을 Effect + state로 | 렌더 2배, 빈 값이 한 번 보일 수 있음 | 렌더 중 계산 (4.1절) |
| 검색 Effect에 정리 없음 | 엉뚱한 결과가 남음 | `ignore` 플래그 (5.2절) |
| `useEffect`가 항상 페인트 후라고 가정 | 클릭이 원인이면 페인트 전일 수 있음 | 2절 |

---

## 8. 확인 퀴즈

**Q1.** 화면에 보이기 전에 요소 크기를 재서 위치를 정해야 합니다. 어떤 훅을 쓸까요?

> [!question]- 정답 보기
> `useLayoutEffect`입니다. 페인트 전에 실행되어 잘못된 위치가 보이지 않습니다.

**Q2.** 개발 모드에서 Effect 로그가 "연결 → 끊기 → 연결"로 찍혔습니다. 버그일까요?

> [!question]- 정답 보기
> 버그가 아니라 Strict Mode가 정리 함수를 시험하는 것입니다. 프로덕션에서는 한 번만 실행됩니다. 이 순서로 실행돼도 사용자에게 차이가 없도록 정리 함수를 만들면 됩니다.

**Q3.** 아래 코드를 Effect 없이 고치세요.
```jsx
const [items, setItems] = useState([]);
const [visible, setVisible] = useState([]);
useEffect(() => {
  setVisible(items.filter(i => i.done === false));
}, [items]);
```

> [!question]- 정답 보기
> ```jsx
> const [items, setItems] = useState([]);
> const visible = items.filter(i => i.done === false);
> ```
> `visible`은 `items`로 계산할 수 있으므로 렌더 중에 계산합니다. 목록이 매우 크다면 `useMemo`를 검토합니다 ([[React Hooks and Component Design Patterns]]).

**Q4.** 5.1절의 경쟁 상태에서 `ignore` 플래그가 결과를 바로잡는 원리를 설명해 보세요.

> [!question]- 정답 보기
> `query`가 바뀌면 새 Effect를 실행하기 전에 이전 Effect의 정리 함수가 실행되어 이전 요청의 `ignore`를 `true`로 만듭니다. 이전 요청의 응답이 늦게 도착해도 `if (!ignore)`에 걸려 state를 바꾸지 못합니다.

**Q5.** "결제 완료" 알림을 결제 버튼을 누른 결과로 보여 주고 싶습니다. Effect와 이벤트 처리기 중 어디에 쓸까요?

> [!question]- 정답 보기
> 이벤트 처리기(`onClick`)입니다. 클릭이라는 사건의 결과이므로 Effect가 필요 없습니다 ("You don't need Effects to handle user events").

---

## 9. 더 깊이 (심화)

### 9.1 Effect와 성능 지표

[[2026-Web Performance Engineering in the Age of AI]]는 AI가 만든 React 코드의 흔한 문제로 `useEffect`에서 데이터를 받아 목록을 한꺼번에 넣는 패턴을 듭니다. 자리를 잡아 두지 않으면 목록이 들어오는 순간 아래 내용이 밀려 **CLS**가 나빠진다는 것입니다 (p.69). 해결책은 `min-height`, 스켈레톤 화면 같은 **자리 미리 잡기**입니다 (p.70). 또 입력할 때마다 렌더 경로에서 큰 목록을 필터링하면 **INP**가 나빠진다고 설명합니다 (p.72). → [[Web Performance Metrics and Core Web Vitals]] 4절

### 9.2 `useLayoutEffect`와 React Native

React Native의 New Architecture도 `useLayoutEffect`에서 레이아웃을 **동기적으로 읽고 같은 프레임 안에서** 고칠 수 있게 되었습니다 (공식 블로그 *The New Architecture is Here*). 레슨 07에서 봅니다.

### 9.3 Effect는 "무엇이 일어났는지" 모른다

이벤트 처리기를 권하는 이유를 react.dev는 이렇게 설명합니다: "In the Buy button click event handler, you know exactly what happened. By the time an Effect runs, you don't know *what* the user did (for example, which button was clicked)." (*You Might Not Need an Effect*) Effect는 "화면이 지금 이 상태다"만 알고, **어떻게 이 상태가 되었는지**는 모릅니다. 그래서 "구매 버튼으로 바뀌었을 때만 알림"처럼 원인에 따라 달라지는 일은 Effect로 표현하기 어렵습니다 (판단).

> 서버 렌더링에서 Effect가 실행되지 않는 점은 [[렌더링 첫걸음 05 - 생명 주기와 useEffect|레슨 05]] 9.3절, 하이드레이션 비용은 [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]] 3.1.1절.

→ 전체 정리: [[React Component Lifecycle and Effects]] 2·4·5절

---

## 관점 비교 / 출처별 차이

| 항목 | 설명 |
|---|---|
| `useEffect` 타이밍 | "페인트 후"로 단순화해 설명하기 쉬움 (판단). 공식 문서는 상호작용이 원인이면 페인트 전일 수 있다고 명시 (*useEffect* Caveats) |
| 데이터 가져오기 | react.dev는 Effect로 가져올 수 있지만 정리가 필요하다고 함. Osmani는 성능 관점(CLS, 로딩 상태 표시)에서 같은 패턴을 다룸 (p.69–70) |
| 렌더 중 계산 | react.dev는 파생 값을 렌더 중에 계산하라고 함. Osmani는 큰 목록을 입력마다 렌더 경로에서 필터링하면 INP가 나빠진다고 경고하고, 디바운스·웹 워커·메모이제이션을 제시 (p.72). 둘은 충돌하지 않음: Effect + state로 옮겨도 계산량은 줄지 않으므로, 무거우면 `useMemo` 등으로 계산 자체를 줄인다 (판단) |

---

## 참고 출처

- [[2026-React Official Documentation]]
- [react.dev, *useLayoutEffect*](https://react.dev/reference/react/useLayoutEffect)
- [react.dev, *useEffect*](https://react.dev/reference/react/useEffect)
- [react.dev, *Synchronizing with Effects*](https://react.dev/learn/synchronizing-with-effects)
- [react.dev, *You Might Not Need an Effect*](https://react.dev/learn/you-might-not-need-an-effect)
- [react.dev, *Keeping Components Pure*](https://react.dev/learn/keeping-components-pure)
- [[2026-Web Performance Engineering in the Age of AI]] Ch 4 (PDF p.69–72)
- [React Native 블로그, *The New Architecture is Here*](https://reactnative.dev/blog/2024/10/23/the-new-architecture-is-here)
