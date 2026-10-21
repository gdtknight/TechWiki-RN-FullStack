---
type: source
source_type: official-docs
title: "React Official Documentation"
authors: ["React Team"]
publisher: "React Foundation (Linux Foundation)"
year: 2026
version: "React 19 (19.0–19.3)"
category: "Official Documentation/Frontend"
url: "https://react.dev"
status: ingested
tags:
  - react
  - react-19
  - hooks
  - rsc
  - actions
  - compiler
  - official-docs
sources: ["[[2026-React Official Documentation]]"]
created: 2026-10-21
updated: 2026-10-21
---

# React Official Documentation

> **출처**: [react.dev](https://react.dev) (GitHub: [`reactjs/react.dev`](https://github.com/reactjs/react.dev))
> **주관/게시**: React Team. 2026-02-24부터 React Foundation(Linux Foundation 산하)이 소유하며, 그 전에는 Meta가 소유했다. 근거: "React, React Native, and supporting projects like JSX are no longer owned by Meta — they are now owned by the React Foundation, an independent foundation hosted by the Linux Foundation."
> **대상 버전**: `React 19` (19.0 → 19.3, 2026-09-09 기준. 릴리스 이력은 4절)
> **분류**: `Official Documentation / Frontend`

---

## 1. 개요 및 핵심 테제 (Core Thesis)

**React 공식 문서(`react.dev`)** 는 사용자 인터페이스(UI)를 구축하기 위한 선언적·컴포넌트 기반 JavaScript 라이브러리인 [[React]]의 표준 명세이자 최신 공식 레퍼런스이다.

React의 근본 철학은 **"UI는 상태의 순수한 투영이다($UI = f(state)$)"** 라는 불변성 원칙 위에 성립한다. 컴포넌트는 입력(Props와 State)이 같으면 항상 동일한 JSX를 반환하는 **순수 함수(Pure Functions)** 여야 하며, 렌더링 도중 외부 DOM이나 변수를 직접 변이(Mutation)시키는 부수 효과(Side Effects)를 철저히 배제한다. React 19에 이르러서는 수동 최적화(메모이제이션)와 비동기 폼 제어의 보일러플레이트를 대폭 걷어내고, **컴파일러 기반 자동 최적화와 네이티브 비동기 액션(Actions)** 을 코어에 통합하는 현대적 패러다임을 확립했다.

---

## 2. 핵심 아키텍처 및 렌더링 규칙

### 2.1 트리 위치에 따른 상태 보존 및 리셋 규칙 (`preserving-and-resetting-state`)
공식 문서는 가상 DOM 재조정([[React Fiber and Reconciliation]])의 핵심 동작을 다음과 같은 불변식으로 명시한다:
1. **동일 위치, 동일 컴포넌트 = 상태 유지**: React는 JSX 태그 자체가 아니라 **UI 트리 상의 위치(Position in the Tree)** 를 기준으로 컴포넌트의 Fiber 노드를 추적한다. 동일한 위치에 동일한 컴포넌트가 렌더링되는 한 내부 훅 상태(`memoizedState`)는 보존된다.
2. **동일 위치, 다른 컴포넌트 = 서브트리 완전 파괴 및 리셋**: 동일한 위치라도 컴포넌트 타입이 바뀌면(예: `<Profile />` → `<Empty />`), React는 기존 서브트리의 모든 DOM과 상태를 완전히 파괴(`destroyDOM`)하고 새 컴포넌트를 마운트한다.
3. **`key`를 통한 상태 명시적 리셋**: 동일한 위치의 컴포넌트라도 고유한 `key`가 변경되면(예: `<Chat key={user.id} />`), React는 기존 상태를 보존하지 않고 즉시 초기화한 뒤 새로 마운트한다. 공식 문서는 폼이나 대화창 입력값을 전환할 때 `useEffect`로 상태를 수동 초기화하지 말고 **`key` 변경 패턴**을 사용할 것을 강력히 권장한다.

---

## 3. React 19 핵심 혁신 및 신규 API 명세

### 3.1 `ref`의 일반 Prop화 (`forwardRef` 폐기)
- 기존에는 함수형 컴포넌트가 부모로부터 `ref`를 전달받으려면 반드시 `React.forwardRef((props, ref) => ...)` 고차 컴포넌트로 감싸야 했다.
- React 19부터는 함수형 컴포넌트의 첫 번째 매개변수 `props`에 `ref`가 일반 속성(`props.ref`)으로 기본 포함되어, `forwardRef` 없이도 자식 컴포넌트의 DOM이나 명령형 메서드에 접근할 수 있다 (`forwardRef`는 점진적 Deprecated).

### 3.2 `<Context>` 직접 렌더링
- 과거 `<MyContext.Provider value={val}>` 형태로만 제공 가능했던 Context 프로바이더가 React 19에서는 `<MyContext value={val}>`처럼 **Context 객체 자체를 JSX 엘리먼트로 직접 렌더링**할 수 있도록 간소화되었다.

### 3.3 비동기 액션(Actions)과 폼 훅 삼총사
React 19는 비동기 요청과 폼 처리를 네이티브 레벨에서 다루는 **Actions 패러다임**을 도입했다:
1. **`useActionState`**: 비동기 액션 함수를 실행하고 결과 상태, 폼 제출 핸들러, 대기 상태(`isPending`)를 한 번에 반환한다 (`const [state, formAction, isPending] = useActionState(fn, initialState)`).
2. **`useFormStatus`**: 부모 `<form>`의 제출 상태(`pending`, `data`, `method`, `action`)를 별도의 Props 전달 없이 하위 컴포넌트에서 Context처럼 즉시 읽을 수 있다.
3. **`useOptimistic`**: 비동기 서버 응답이 도착하기 전에 UI를 낙관적으로 즉시 갱신하고, 서버 에러 발생 시 원래 상태로 자동 롤백한다.

### 3.4 자원 읽기 API: `use(Resource)`
- 기존 Hook들은 조건문이나 루프 내부에서 호출할 수 없는 엄격한 규칙이 있었다.
- React 19의 **`use()` API**는 Promise나 Context 같은 비동기 자원을 읽을 수 있으며, **`if` 조건문이나 루프 블록 내부에서도 자유롭게 호출 가능**하다.
- Promise가 아직 대기(Pending) 중이면 가장 가까운 `<Suspense>` 경계를 일시 정지(Suspend)시키고, 해결(Resolved)되면 해당 값을 즉시 반환한다.

### 3.5 문서 메타데이터 & 스타일시트 호이스팅
- 별도의 라이브러리(React Helmet 등) 없이도 컴포넌트 트리 깊은 곳에서 `<title>`, `<meta>`, `<link rel="stylesheet">`를 선언하면 React가 자동으로 문서 `<head>`로 호이스팅하여 중복 없이 삽입·관리한다.

### 3.6 React Compiler (자동 메모이제이션)
- 과거 성능 최적화를 위해 개발자가 수동으로 작성하던 `useMemo`, `useCallback`, `React.memo`는 코드 가독성을 해치고 의존성 배열 누락 버그를 유발했다.
- Meta 팀이 도입한 **React Compiler**는 빌드 타임에 AST(추상 구문 트리)를 분석하여 값이 변경되었을 때만 필요한 부분만 선택적으로 리렌더링하도록 컴포넌트 코드를 자동으로 최적화한다.
- **v1.0 안정판 (2025-10-07)**: "React Compiler is a build-time tool that optimizes your React app through automatic memoization."
  - 설치: `npm install --save-dev --save-exact babel-plugin-react-compiler@latest`. 버전을 고정하라고 권장하는 이유는 "changing memoization may under rare circumstances cause unexpected behavior."이기 때문이다.
  - 호환: "React Compiler is compatible with React 17 and up." React 19 미만이면 `react-compiler-runtime` 의존성과 target 설정이 필요하다.
  - 기본 활성화 현황: Expo SDK 54 이상은 기본으로 켜져 있고, Vite(`create-vite`)와 Next.js(`create-next-app`)는 컴파일러가 켜진 템플릿을 고르는 옵트인 방식이다.

---

## 4. React 19.x 릴리스 이력과 보안 공지 (`react.dev/blog` 기준)

| 버전·글 (발표일) | 핵심 변경 | 원문 근거 |
|---|---|---|
| **19.0** (2024-12-05) | Actions, `use`, `ref` Prop화, `<Context>` 직접 렌더링 등 (3절) | "React v19" 글 |
| **19.1** | 별도 블로그 글 없음. GitHub `CHANGELOG.md` 참고 | "not every React release deserves its own blog post" |
| **19.2** (2025-10-01) | `<Activity />`, `useEffectEvent`, `cacheSignal`, Chrome DevTools Performance Tracks, React DOM Partial Pre-rendering, `eslint-plugin-react-hooks` v6(flat config 기본) | "`<Activity>` lets you break your app into "activities" that can be controlled and prioritized." |
| **React Compiler v1.0** (2025-10-07) | 컴파일러 첫 안정판 (3.6절) | "We are releasing the compiler's first stable release today." |
| **React Foundation** (2026-02-24) | React·React Native·JSX 소유권이 Linux Foundation 산하 React Foundation으로 이전. 창립 플래티넘 멤버는 Amazon, Callstack, Expo, Huawei, Meta, Microsoft, Software Mansion, Vercel | "React's technical governance will always be independent from the React Foundation board." |
| **19.3** (2026-09-09) | `<ViewTransition>`와 `addTransitionType`, Suspense 연계 애니메이션, Fragment Refs, `use(browser())`로 서버 렌더링 제외, Trusted Types 지원, Server Components에서 `<Context>` 렌더링 | "The new `<ViewTransition>` component lets you animate elements as they enter, exit, move, or resize using the browser's View Transition API." |

### 4.1 19.3 동작 변경 주의점 (Changelog 항목)
- Strict Mode가 hydration 중에도 Effect를 두 번 호출한다. 클라이언트 렌더링 루트와 동작이 같아졌다.
- `resize` 이벤트로 인한 업데이트는 다음 프레임까지 배치된다.
- Transition끼리 독립적으로 렌더링되어, 느린 Transition이 관계없는 Transition을 붙잡지 않는다.

### 4.2 React Server Components 보안 공지 (2025-12)
RSC를 쓰는 앱(Next.js, React Router, Waku, `@parcel/rsc`, `@vitejs/plugin-rsc`, rwsdk 등)은 아래 버전 이상으로 올려야 한다. 대상 패키지는 `react-server-dom-webpack`, `react-server-dom-parcel`, `react-server-dom-turbopack`이다.
- **2025-12-03 원격 코드 실행**: "There is an unauthenticated remote code execution vulnerability in React Server Components." CVE-2025-55182, CVSS 10.0.
- **2025-12-11 후속 취약점**: 서비스 거부(CVE-2025-55184, CVE-2025-67779, CVE-2026-23864, CVSS 7.5)와 소스 코드 노출(CVE-2025-55183, CVSS 5.3).
- **안전한 버전**: "Versions 19.0.4, 19.1.5, 19.2.4 are safe." 앞선 공지의 19.0.3, 19.1.4, 19.2.3은 불완전한 수정이다.
- 영향 범위: "If your app's React code does not use a server, your app is not affected by this vulnerability." 순수 클라이언트 앱과 React Native 앱 코드는 이 공지의 대상 패키지를 쓰지 않는다 (판단: 공지가 나열한 영향 패키지·프레임워크 목록에 근거).

---

## 5. 기존 도서 서술과의 차이 및 주의점 (Version Drift)

| 항목 | 기존 도서 서술 (2021–2024년 출간 도서들) | React 19 공식 문서 기준 (`react.dev`) |
|---|---|---|
| **Ref 전달** | `forwardRef` 필수 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]], [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]) | `props.ref`로 직접 수신. `forwardRef`는 Deprecated 수순 |
| **Context Provider** | `<Context.Provider>` 필수 사용 | `<Context value={...}>` 직접 렌더링 지원 |
| **폼 및 비동기 상태** | `useState` + `loading`/`error` 수동 관리 및 `handleSubmit` 이벤트 제어 | **`useActionState`**, `useFormStatus`, `useOptimistic` 기반 선언적 Actions |
| **조건부 훅 호출** | 모든 Hook은 조건문 내부 호출 절대 불가 ([[2021-Designing React Hooks the Right Way]]) | 일반 훅은 여전히 금지되나, **`use(Resource)`는 조건문/루프 내부 호출 허용** |
| **성능 메모이제이션** | `useMemo`, `useCallback`, `React.memo` 수동 도배 ([[2024-React in Depth]]) | **React Compiler** 도입으로 수동 메모이제이션 지양, 자동 최적화 지향 |
| **렌더링 패러다임** | 클라이언트 SPA 중심 번들링 | **React Server Components (RSC)** 및 스트리밍 HTML 기본 연계 |

---

## 6. 파생 및 연관 위키 페이지

- **코어 개념**: [[React Fiber and Reconciliation]], [[React Hooks and Component Design Patterns]], [[Virtual DOM and Reconciliation from Scratch]], [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]]
- **엔티티**: [[React]], [[Model Context Protocol (MCP)]]
- **종합 분석**: [[React and React Native Internal Implementation Deep Dive]], [[Web React vs React Native Shared Core and Differences]]
