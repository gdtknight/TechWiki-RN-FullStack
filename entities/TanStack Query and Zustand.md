---
type: entity
title: "TanStack Query and Zustand"
entity_type: "framework"
category: "H. Information Systems/React"
sources:
  - "[[2024-React in Depth]]"
  - "[[2026-React Application Architecture for Production Second Edition]]"
tags:
  - tanstack-query
  - react-query
  - zustand
  - redux-toolkit
  - state-management
  - data-fetching
  - react
  - react-native
created: 2026-10-19
updated: 2026-10-19
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

## 관련 링크

- **연관 개념**: [[Offline-First and Mobile State Management]], [[React Hooks and Component Design Patterns]], [[Web React vs React Native Shared Core and Differences]]
- **연관 엔티티**: [[React]], [[React Native]]
- **출처 문헌**: [[2024-React in Depth]] (Ch 8–9, p.239–318), [[2026-React Application Architecture for Production Second Edition]] (Ch 5–6, p.136–194)
