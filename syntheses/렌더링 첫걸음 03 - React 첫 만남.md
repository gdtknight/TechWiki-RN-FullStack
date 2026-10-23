---
type: synthesis
title: "렌더링 첫걸음 03 - React 첫 만남"
category: "H. Information Systems/React"
sources:
  - "[[2026-React Official Documentation]]"
tags:
  - react
  - component
  - jsx
  - props
  - state
  - usestate
  - beginner
  - rendering-first-steps
created: 2026-10-21
updated: 2026-10-24
---

# 렌더링 첫걸음 03 - React 첫 만남

> **이전**: [[렌더링 첫걸음 02 - 프레임과 이벤트 루프|02 프레임과 이벤트 루프]] · **다음**: [[렌더링 첫걸음 04 - 렌더와 커밋|04 렌더와 커밋]] · **목록**: [[렌더링 첫걸음 00 - 로드맵|로드맵]]

## 이 레슨에서 배우는 것

1. **컴포넌트**: 화면 조각을 만드는 함수
2. **JSX**: JavaScript 안에 쓰는 HTML 비슷한 문법
3. **props**: 컴포넌트에 건네는 값
4. **state**: 컴포넌트의 기억, 그리고 `useState`
5. state는 **스냅샷**처럼 동작한다
6. 목록을 그릴 때의 **key**

> 이 레슨의 모든 예제는 React 19.3.1 + react-dom + jsdom에서 실행해 결과를 확인했습니다. 자신의 컴퓨터에서 해 보려면 Expo나 Vite 같은 도구로 프로젝트를 만들어야 합니다. 이 시리즈에서는 결과를 읽는 것으로 충분합니다.

---

## 1. 왜 React를 쓸까

[[렌더링 첫걸음 01 - 브라우저는 화면을 어떻게 그릴까|레슨 01]]에서는 `document.querySelector`로 DOM을 직접 찾아 바꿨습니다. 화면이 작을 때는 괜찮지만, 버튼이 수십 개이고 값이 서로 얽히면 "어느 DOM을 언제 바꿔야 하는지" 추적하기 어려워집니다.

React는 생각을 바꿉니다. **"지금 데이터가 이러면 화면은 이렇게 생겼다"** 만 적으면, DOM을 어떻게 바꿀지는 React가 알아서 합니다. 이 위키의 [[React]] 페이지는 이것을 `UI = f(state)`로 요약합니다. 이 레슨에서는 그 최소한의 문법을 배우고, 레슨 04부터 React가 실제로 화면을 바꾸는 순서를 봅니다.

---

## 2. 컴포넌트: 화면 조각을 만드는 함수

### 비유로 이해하기: 레고 블록

레고로 집을 만들 때 벽, 창문, 지붕을 따로 만들어 끼웁니다. React에서도 화면을 **작은 조각**으로 나눠 만들고 조립합니다. 이 조각을 **컴포넌트**(component)라고 합니다.

컴포넌트는 [[JS 첫걸음 07 - 함수|JS 첫걸음 07]]의 **함수**입니다. react.dev: "React components are regular JavaScript functions, but **their names must start with a capital letter** or they won't work!" (*Your First Component*)

```jsx
function Greeting() {
  return <p>안녕하세요!</p>;
}
```

- 함수 이름은 **대문자로 시작**합니다 (`Greeting`).
- 화면에 보일 모양을 **`return`** 합니다.

다른 컴포넌트 안에서 HTML 태그처럼 씁니다.

```jsx
function App() {
  return (
    <div>
      <Greeting />
      <Greeting />
    </div>
  );
}
```

`<Greeting />`이 대문자로 시작하므로 React는 이것이 우리가 만든 컴포넌트라고 압니다 (react.dev: "`<Profile />` starts with a capital `P`, so React knows that we want to use our component called `Profile`.").

---

## 3. JSX: JavaScript 안의 HTML

### 3.1 JSX란

위의 `<p>안녕하세요!</p>`는 따옴표 없이 JavaScript 안에 들어 있습니다. 이 문법을 **JSX**라고 합니다. react.dev: "*JSX* is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file." (*Writing Markup with JSX*)

### 3.2 JSX의 세 가지 규칙 (*Writing Markup with JSX*)

| 규칙 | 뜻 | 예 |
|---|---|---|
| 1. 바깥 태그는 하나 | `return`하는 JSX는 하나의 태그로 감싸야 함 | `<div>…</div>` 또는 `<>…</>` |
| 2. 모든 태그를 닫기 | `<img>` 같은 태그도 스스로 닫음 | `<img />`, `<br />` |
| 3. 속성 이름은 대부분 camelCase | HTML의 `class`는 JavaScript 예약어라서 `className` | `className="title"`, `strokeWidth` |

react.dev: "Since `class` is a reserved word, in React you write `className` instead". ([[JS 첫걸음 02 - 변수|JS 첫걸음 02]]의 예약어와 같은 이유)

### 3.3 중괄호 `{ }`: JSX 안의 JavaScript 창문

JSX 안에서 **중괄호**를 열면 JavaScript 값을 넣을 수 있습니다. react.dev는 이것을 "A window into the JavaScript world"라고 부르고, "Any JavaScript expression will work between curly braces"라고 설명합니다 (*JavaScript in JSX with Curly Braces*).

```jsx
const name = "철수";
return <p>안녕, {name}! 내년에는 {12 + 1}살.</p>;
```

[[JS 첫걸음 03 - 자료형|JS 첫걸음 03]]의 템플릿 리터럴 `${ }`와 비슷한 역할입니다.

---

## 4. props: 컴포넌트에 건네는 값

`Greeting`이 항상 같은 말만 하면 재미없습니다. 누구에게 인사할지 **밖에서** 정해 줍시다.

```jsx
function Greeting({ name, age = 10 }) {
  return <p>안녕, {name}! 너는 {age}살이구나.</p>;
}

function App() {
  return (
    <div>
      <Greeting name="철수" age={12} />
      <Greeting name="영희" />
    </div>
  );
}
```

화면 (실행 결과):

```html
<div><p>안녕, 철수! 너는 12살이구나.</p><p>안녕, 영희! 너는 10살이구나.</p></div>
```

- 태그에 붙인 `name="철수"`, `age={12}`를 **props**라고 합니다. react.dev: "Every parent component can pass some information to its child components by giving them props." (*Passing Props to a Component*)
- 컴포넌트는 props를 **객체 하나**로 받습니다. `{ name, age = 10 }`은 그 객체에서 꺼내면서 기본값을 정하는 문법입니다 ([[JS 첫걸음 07 - 함수|JS 첫걸음 07]]의 기본값과 같은 생각).
- 숫자처럼 문자열이 아닌 값은 `age={12}`처럼 **중괄호**로 건넵니다.
- props는 읽기만 합니다: "Props are read-only snapshots in time: every render receives a new version of props." (*Passing Props*)
- props로 **함수**도 넘길 수 있습니다. 자식이 부모의 함수를 불러 부모의 state를 바꾸는 방법은 보충 레슨 [[렌더링 첫걸음 10 - 자식이 부모 함수를 부를 때|10]]에서 자세히 봅니다.

---

## 5. state: 컴포넌트의 기억

### 5.1 보통 변수로는 안 된다

버튼을 누를 때마다 숫자를 올리고 싶습니다. [[JS 첫걸음 02 - 변수|변수]]로 해 봅시다.

```jsx
function BrokenCounter() {
  let count = 0;
  return (
    <button onClick={() => { count = count + 1; }}>
      지역변수 {count}
    </button>
  );
}
```

두 번 눌러 보면 콘솔의 `count`는 1, 2로 올라가지만, **화면은 계속 `지역변수 0`** 입니다 (실행 결과). react.dev는 이유를 두 가지로 설명합니다 (*State: A Component's Memory*):

1. "**Local variables don't persist between renders.**" 컴포넌트가 다시 그려질 때 함수가 처음부터 다시 실행되어 `count`가 또 0이 됩니다.
2. "**Changes to local variables won't trigger renders.**" React는 변수가 바뀐 줄 모르므로 다시 그리지 않습니다.

### 5.2 `useState`

필요한 것은 두 가지입니다: "A **state variable** to retain the data between renders." / "A **state setter function** to update the variable and trigger React to render the component again." (*State: A Component's Memory*)

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);   // 처음 값은 0
  return (
    <button onClick={() => setCount(count + 1)}>
      눌렀어요 {count}번
    </button>
  );
}
```

두 번 누르면 화면이 `눌렀어요 2번`이 됩니다 (실행 결과).

| 부분 | 뜻 |
|---|---|
| `useState(0)` | 기억 칸을 하나 만들고 처음 값을 0으로 |
| `count` | 지금 기억하고 있는 값 |
| `setCount(새 값)` | 값을 바꾸고 **다시 그려 달라고 요청** |

`use`로 시작하는 이런 함수를 **훅**(Hook)이라고 부릅니다. 레슨 05의 `useEffect`도 훅입니다.

### 비유로 이해하기: React의 선반

react.dev는 state가 함수 밖, **React 쪽 선반**에 산다고 비유합니다: "State actually "lives" in React itself--as if on a shelf!--outside of your function." (*State as a Snapshot*) 함수가 끝나도 선반 위의 값은 남아 있고, 다음에 함수를 부를 때 React가 그 값을 건네줍니다. [[JS 첫걸음 10 - 스코프와 클로저|JS 첫걸음 10]]의 클로저가 바깥 변수를 기억하던 것과 비슷한 느낌입니다 (판단).

---

## 6. state는 스냅샷이다

### 6.1 바꿔도 지금 값은 그대로

```jsx
function Snapshot() {
  const [n, setN] = useState(0);
  return (
    <button onClick={() => {
      setN(n + 5);
      console.log("setN 직후 n:", n);
    }}>
      스냅샷 {n}
    </button>
  );
}
```

누르면 콘솔에는 **`setN 직후 n: 0`** 이 찍히고, 화면은 `스냅샷 5`가 됩니다 (실행 결과).

react.dev: "state behaves more like a snapshot. Setting it does not change the state variable you already have, but instead triggers a re-render." (*State as a Snapshot*) `setN`은 "다음 그림에서는 5로 그려 줘"라는 **요청**이고, 지금 실행 중인 함수 안의 `n`은 이번 그림(스냅샷)의 값 0 그대로입니다.

### 6.2 세 번 더해도 1만 오르는 이유

```jsx
<button onClick={() => {
  setN(n + 1);
  setN(n + 1);
  setN(n + 1);
}}>+3</button>
```

결과: **1** (실행 결과). 세 줄 모두 "이번 스냅샷의 `n`(0) + 1"을 요청했기 때문입니다.

이전 값을 바탕으로 여러 번 바꾸려면 **함수**를 건넵니다 (*Queueing a Series of State Updates*).

```jsx
setN(x => x + 1);
setN(x => x + 1);
setN(x => x + 1);
```

결과: **+3** (실행 결과).

### 6.3 다시 그리기는 한 번만

세 번 `setN`을 불러도 컴포넌트는 **한 번만** 다시 그려졌습니다 (실행 결과: 클릭 1번 → 렌더 1번). react.dev: "**React waits until *all* code in the event handlers has run before processing your state updates.**" 이것을 **배칭**(batching)이라고 합니다 (*Queueing a Series of State Updates*). [[렌더링 첫걸음 02 - 프레임과 이벤트 루프|레슨 02]]에서 배운 "작업이 끝난 뒤에 화면을 그린다"는 원리와 맞아떨어집니다 (판단).

---

## 7. 목록 그리기와 key

[[JS 첫걸음 08 - 배열|JS 첫걸음 08]]의 `map`으로 배열을 JSX 목록으로 바꿉니다.

```jsx
const fruits = [
  { id: 1, name: "사과" },
  { id: 2, name: "바나나" },
];

function FruitList() {
  return (
    <ul>
      {fruits.map(f => <li key={f.id}>{f.name}</li>)}
    </ul>
  );
}
```

`key`를 빼면 콘솔에 경고가 뜹니다 (실행 결과): `Each child in a list should have a unique "key" prop.`

react.dev: "Keys tell React which array item each component corresponds to, so that it can match them up later. This becomes important if your array items can move (e.g. due to sorting), get inserted, or get deleted." (*Rendering Lists*) key는 레슨 04에서 React가 "무엇이 바뀌었는지" 비교할 때 다시 나옵니다.

---

## 8. 정리

| 낱말 | 뜻 |
|---|---|
| 컴포넌트 | 화면 조각을 돌려주는 함수. 이름은 대문자로 시작 |
| JSX | JavaScript 안에 쓰는 HTML 비슷한 문법 |
| `{ }` (JSX 안) | JavaScript 값을 넣는 창문 |
| props | 부모가 자식 컴포넌트에 건네는 값. 읽기 전용 |
| state | 다시 그려도 남아 있는 컴포넌트의 기억 |
| `useState` | `[값, 바꾸는 함수]`를 돌려주는 훅 |
| 스냅샷 | 이번 렌더의 state 값. 바꾸는 함수를 불러도 지금 값은 그대로 |
| 배칭 | 이벤트 처리기가 끝난 뒤 state 변경을 모아 한 번에 다시 그리기 |
| key | 목록의 각 항목을 구별하는 고유한 값 |

---

## 9. 자주 하는 실수

| 실수 | 결과 (실행 결과) | 바로잡기 |
|---|---|---|
| 보통 변수로 숫자를 올림 | 화면이 안 바뀜 | `useState`를 씁니다 (5절) |
| 컴포넌트 이름을 소문자로 (`function greeting`) | `<greeting>`이 HTML 태그로 취급되고 경고: `The tag <greeting> is unrecognized in this browser. If you meant to render a React component, start its name with an uppercase letter.` (첫 줄) | 대문자로 시작합니다 |
| `setN(n + 1)`을 연달아 세 번 | 1만 오름 | `setN(x => x + 1)` (6.2절) |
| `setN` 직후 `n`을 읽어 새 값을 기대 | 이전 값이 나옴 | 새 값이 필요하면 변수에 계산해 두고 씁니다 (6.1절) |
| `map`으로 만든 목록에 `key` 없음 | 경고 `Each child in a list should have a unique "key" prop.` | 데이터의 고유 `id`를 `key`로 |
| JSX에 `class="…"` | (React는 `className`을 씀) | `className="…"` (3.2절) |

---

## 10. 확인 퀴즈

**Q1.** 다음 중 React 컴포넌트로 올바르게 쓸 수 있는 이름은? ① `userCard` ② `UserCard` ③ `user-card`

> [!question]- 정답 보기
> ②입니다. 컴포넌트 이름은 대문자로 시작해야 합니다.

**Q2.** 아래 화면에는 무엇이 보일까요?
```jsx
function Price({ item, price = 1000 }) {
  return <p>{item}: {price}원</p>;
}
// <Price item="연필" />
```

> [!question]- 정답 보기
> `연필: 1000원`입니다. `price`를 건네지 않아 기본값 1000이 쓰입니다.

**Q3.** 버튼을 한 번 누르면 화면의 숫자는 얼마가 될까요? (처음 값 0)
```jsx
onClick={() => { setN(n + 2); setN(n + 2); }}
```

> [!question]- 정답 보기
> 2입니다. 두 줄 모두 이번 스냅샷의 `n`(0) + 2를 요청합니다. 4를 원하면 `setN(x => x + 2)`를 두 번 씁니다.

**Q4.** 보통 변수(`let count`)로는 화면이 안 바뀌는 이유 두 가지는?

> [!question]- 정답 보기
> ① 다시 그릴 때 함수가 처음부터 실행되어 변수가 초기화되고, ② 변수를 바꿔도 React가 다시 그려야 한다는 것을 모릅니다 (react.dev *State: A Component's Memory*).

**Q5.** `fruits.map(f => <li>{f.name}</li>)`에 무엇을 더해야 할까요? 왜?

> [!question]- 정답 보기
> `key={f.id}`입니다. React가 목록의 각 항목을 구별해, 순서가 바뀌거나 항목이 추가·삭제돼도 올바르게 맞춰 보기 위해서입니다.

---

## 11. 더 깊이 (심화)

### 11.1 컴포넌트는 순수해야 한다

react.dev는 컴포넌트가 **순수 함수**이기를 기대합니다: "**React assumes that every component you write is a pure function.**" 순수 함수란 "It minds its own business"(함수 밖의 것을 바꾸지 않음), "Same inputs, same output"(같은 입력이면 같은 결과)입니다 (*Keeping Components Pure*). 렌더 도중에 `document.getElementById(…).className = …`처럼 DOM을 직접 바꾸거나 바깥 변수를 바꾸면 안 됩니다. 이런 실수를 찾아 주려고 **Strict Mode**는 개발 중에 컴포넌트 함수를 두 번 부릅니다: "By calling the component functions twice, Strict Mode helps find components that break these rules." 그리고 "Strict Mode has no effect in production". 레슨 06에서 Effect의 두 번 실행과 함께 다시 봅니다.

### 11.2 state는 컴포넌트 "위치"에 붙는다

같은 `Counter`를 두 번 그리면 각자 자기 state를 가집니다. state는 함수가 아니라 화면의 **그 자리**에 붙기 때문입니다. 자세한 규칙(같은 위치·같은 컴포넌트면 state 유지, `key`가 바뀌면 초기화)은 [[2026-React Official Documentation]] 2.1절과 [[React Hooks and Component Design Patterns]].

→ 이어서: [[React]], [[React Hooks and Component Design Patterns]]

---

## 관점 비교 / 출처별 차이

| 항목 | 설명 |
|---|---|
| JSX 없이 쓰기 | react.dev: "JSX and React are two separate things." JSX 없이 `React.createElement`로도 쓸 수 있지만 대부분 JSX를 씀 (*Writing Markup with JSX*). 이 시리즈의 검증 스크립트도 JSX를 esbuild로 변환해 실행함 |
| 클래스 컴포넌트 | 옛 도서는 `class … extends Component`로 state를 다루는 경우가 있음. react.dev는 새 코드에 클래스를 권하지 않음 ("we don't recommend using them in new code", *Component*). 대응 관계는 [[React Component Lifecycle and Effects]] 6절 |

---

## 참고 출처

- [[2026-React Official Documentation]]
- [react.dev, *Your First Component*](https://react.dev/learn/your-first-component)
- [react.dev, *Writing Markup with JSX*](https://react.dev/learn/writing-markup-with-jsx)
- [react.dev, *JavaScript in JSX with Curly Braces*](https://react.dev/learn/javascript-in-jsx-with-curly-braces)
- [react.dev, *Passing Props to a Component*](https://react.dev/learn/passing-props-to-a-component)
- [react.dev, *State: A Component's Memory*](https://react.dev/learn/state-a-components-memory)
- [react.dev, *State as a Snapshot*](https://react.dev/learn/state-as-a-snapshot)
- [react.dev, *Queueing a Series of State Updates*](https://react.dev/learn/queueing-a-series-of-state-updates)
- [react.dev, *Rendering Lists*](https://react.dev/learn/rendering-lists)
- [react.dev, *Keeping Components Pure*](https://react.dev/learn/keeping-components-pure)
