---
type: entity
title: "TanStack Query and Zustand"
entity_type: "framework"
category: "H. Information Systems/React"
sources:
  - "[[2024-React in Depth]]"
  - "[[2026-React Application Architecture for Production Second Edition]]"
  - "[[2025-Full Stack JavaScript Strategies]]"
tags:
  - tanstack-query
  - react-query
  - zustand
  - redux-toolkit
  - state-management
  - data-fetching
  - react
  - react-native
  - valtio
created: 2026-10-19
updated: 2026-10-23
---

# TanStack Query and Zustand

## 개요

모던 [[React]] 및 [[React Native]] 애플리케이션 아키텍처에서 과거 단일 Redux 스토어가 떠안았던 책임을 **① 원격 서버 상태 캐싱(`TanStack Query` / 구 `React Query`)** 과 **② 초경량 클라이언트 전역 상태(`Zustand` 또는 `Redux Toolkit`)** 로 분리하는 현대적 표준 조합이다 ([[2024-React in Depth]], Ch 8 p.239–282, Ch 9 p.283–318; [[2026-React Application Architecture for Production Second Edition]], Ch 5 p.148–161, Ch 6 p.169–179).

두 라이브러리 모두 브라우저 DOM에 의존하지 않는 순수 React 훅 기반이므로 **웹과 React Native 모바일 앱 양쪽에서 100% 동일한 코드로 동작**한다 ([[Web React vs React Native Shared Core and Differences]]).

---

## 1. TanStack Query (Server State & Reactive Caching)

[[2024-React in Depth]] (Ch 9, p.284–317)와 [[2026-React Application Architecture for Production Second Edition]] (Ch 5, p.148–161)에 따르면, `useEffect` + `fetch` + `useState`로 원격 API 데이터를 직접 관리할 때 발생하는 경쟁 상태(Race Condition), 중복 네트워크 요청, 캐시 무효화(Invalidation) 복잡성을 해결하기 위해 도입된다:

- **Query Keys 조직화 (`2026-React Arch 2e` p.150)**: `['todos', { filter, page }]`와 같은 계층적 배열 키를 기준으로 서버 응답을 인메모리 캐싱하고 자동 중복 제거(Deduplication) 및 백그라운드 리페칭(`staleTime`, `gcTime`)을 수행한다.
- **`useQuery` & `useMutation`**:
  - 데이터 조회는 `useQuery`로 선언하고(`isLoading`, `isError`, `data` 자동 제공), 데이터 변경(POST/PUT/DELETE)은 `useMutation`으로 실행한 뒤 `onSuccess`에서 `queryClient.invalidateQueries()`를 호출해 연관 화면 데이터를 자동으로 최신화한다 (`2024-React in Depth` p.309).
- **Optimistic Updates (낙관적 업데이트, `2026-React Arch 2e` p.262)**: 모바일처럼 네트워크 지연이 있는 환경에서 서버 응답이 오기 전에 UI 캐시를 먼저 갱신하고, 실패 시 자동 롤백하여 즉각적인 UX를 제공한다.

---

## 2. 클라이언트 상태 관리 도구 비교: Zustand vs Redux Toolkit vs Context

서버 데이터를 `TanStack Query`로 분리하고 남은 **순수 클라이언트 전역 상태**(테마, UI 필터, 오프라인 설정, 멀티스텝 위저드 등)를 다루는 4대 도구 비교 ([[2024-React in Depth]], Ch 8 p.242–281):

| 항목 | **Zustand** (`2024-React in Depth` p.267) | **Redux Toolkit (RTK)** (p.261) | ** 순수 Context + `useReducer`** (p.245) | **XState** (p.271) |
|---|---|---|---|---|
| **핵심 철학** | **`create()` 단일 함수로 생성하는 초경량 훅 기반 스토어** | 고전 Redux의 단방향 아키텍처를 유지하되 `createSlice`와 Immer로 보일러플레이트 최소화 | 외부 의존성 없는 React 내장 API 활용 | 유한 상태 머신(Finite State Machine) 및 Statecharts 기반 엄격한 전이 |
| **Provider 래핑 필요 여부** | **불필요** (`<Provider>` 없이 어디서든 훅 호출 가능) | `<Provider store={store}>` 필수 | `<Context.Provider>` 필수 | Context 또는 훅 결합 |
| **부분 구독(Selector) 및 리렌더링 최적화** | **`useStore(s => s.count)` 셀렉터 기본 내장** (선택한 필드가 바뀔 때만 리렌더링) | `useSelector` 기본 내장 | **기본 미지원** (Context 값 일부만 바뀌어도 모든 Consumer가 리렌더링되므로 Context 분할 필요) | `useSelector` 지원 |
| **React Native 적합성** | **최상** (번들 크기가 극소량이며 `persist` 미들웨어에 `AsyncStorage`만 연결하면 즉시 영속화 완성) | 대규모 엔터프라이즈 팀 및 기존 Redux 코드베이스에 적합 | 변경 빈도가 낮은 다크모드/로케일/인증 정보에 적합 | 복잡한 미디어 플레이어, 결제/인증 플로우, 제스처 상태 머신에 적합 |

---

## 3. 관점 비교: [[2025-Full Stack JavaScript Strategies]] (Ch 15–16, 20)

### 3.1 외부 상태 관리의 세 접근 (p.212)

| 접근 | 책의 설명 | 예 (책) | 장점 | 단점 |
|---|---|---|---|---|
| **reducer** | "The reducer approach is the one that has a centralized source of truth." 모든 컴포넌트가 중앙에 액션을 보냄 | Redux, Zustand | 중앙 저장소, DevTools | 새 용어와 개념이 많음 |
| **atom** | 상태를 작은 단위로 쪼개 훅으로 다루고, 작은 단위에서 상태를 파생 | Recoil, Jotai | React 기능과 잘 맞고 `useState`와 비슷함 | 상태를 그래프로 생각해야 해서 주니어에게 어려울 수 있음 |
| **mutable** | 프록시로 직접 읽고 쓰는 가변 데이터 | MobX, Valtio | 의존성 자동 갱신, 리렌더 감소 | 언제 어떻게 바뀌는지 추적하기 어려움 |

책은 예제에 Valtio를 쓰지만, 너무 이른 도입을 경계한다: "Implementing a state-management tool too early can slow development and add more complexity than needed." (p.216)

**Valtio 예제의 오류**: 책은 `useSnapshot`이 돌려준 스냅샷에 값을 쓴다 (`orderSnap.orders = ...`, p.214–215). Valtio 문서: "Snapshots are read-only to render the JSX from their consistent view of the data. Mutations, and also any reads in callbacks that make mutations, need to be made via the proxy" ([*useSnapshot*](https://valtio.dev/docs/api/basic/useSnapshot)).

### 3.2 Context는 성능 도구가 아니다 (p.211)

책에 실린 Ethan Brown의 의견: "The problem is that context doesn't inherently do anything at all to improve performance and can even result in much worse performance if you're not considering how often the state in question is changing." 알맞은 예는 테마·언어·다크 모드·현재 사용자처럼 드물게 바뀌는 값이고, 나쁜 예는 표의 정렬·필터처럼 자주 바뀌는 값이다. 2절 표의 "Context: 부분 구독 기본 미지원" 행과 같은 이야기다.

### 3.3 TanStack Query 캐시 설정 (p.271)

책의 예: `staleTime: 1800000`(30분), `gcTime: 3600000`(1시간), `refetchInterval: 3600000`. 책의 주석은 `gcTime`을 "data stays in cache for 1 hour ... and is garbage collected afterwards"라고 설명한다.

공식 문서로 보면 두 값의 기준 시점이 다르다:
- `staleTime`: "A Query that has a `staleTime` set is considered **fresh** until that `staleTime` has elapsed." (*Important Defaults*)
- `gcTime`: 쿼리를 쓰는 인스턴스가 **모두 사라진 뒤에** 타이머가 시작된다. "Since there are no more active instances of this query, a garbage collection timeout is set using `gcTime` to delete and garbage collect the query (defaults to **5 minutes**)." (*Caching Examples*)
- `refetchInterval`: "which is independent of the `staleTime` setting" (*Important Defaults*)

그러니 책의 주석은 "**사용하는 컴포넌트가 모두 사라진 뒤** 1시간"으로 읽어야 한다 (판단: 위 문장에서).

### 3.4 서버 데이터를 다른 스토어에 복사하기 (p.222–223)

책의 예제는 `useQuery`로 받은 데이터를 `useEffect`에서 Valtio 스토어로 다시 복사한다 (`orderStore.orders = orderData`). TanStack Query 문서는 역할을 이렇게 나눈다: "TanStack Query is a **server-state** library" / "Redux, MobX, Zustand, etc. are **client-state** libraries that _can be used to store asynchronous data, albeit inefficiently when compared to a tool like TanStack Query_" ([*Does TanStack Query replace Redux, MobX or other global state managers?*](https://tanstack.com/query/latest/docs/framework/react/guides/does-this-replace-client-state)). 같은 데이터를 두 곳에 두면 기준이 둘이 되므로, 서버 데이터는 쿼리 결과를 바로 쓰고 클라이언트 상태만 스토어에 두는 편이 이 페이지 개요의 분리 원칙(서버 상태는 TanStack Query, 클라이언트 상태는 Zustand 등)과 맞다 (판단). Effect로 값을 옮기는 비용은 [[React Component Lifecycle and Effects]]의 "You Might Not Need an Effect" 설명 참고.

### 3.5 데이터 요청 라이브러리 고르는 기준 (p.218)

캐시 관리, DevTools, 재시도, 오류 처리, 쿼리·뮤테이션, 지원 프로토콜, API 정의, 프레임워크 통합. 여기에 번들 크기·커뮤니티·문서·예제. 이미 Redux를 쓰면 RTK Query, 캐시·재요청을 기본으로 원하면 TanStack Query나 SWR (p.218). GraphQL이면 urql이나 Apollo Client (p.219).

---

## 관련 링크

- **연관 개념**: [[Offline-First and Mobile State Management]], [[React Hooks and Component Design Patterns]], [[Web React vs React Native Shared Core and Differences]]
- **연관 엔티티**: [[React]], [[React Native]]
- **출처 문헌**: [[2024-React in Depth]] (Ch 8–9, p.239–318), [[2026-React Application Architecture for Production Second Edition]] (Ch 5–6, p.136–194), [[2025-Full Stack JavaScript Strategies]] (Ch 15–16 p.207–227, Ch 20 p.270–271), TanStack Query [*Important Defaults*](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults), [*Caching Examples*](https://tanstack.com/query/latest/docs/framework/react/guides/caching)
