---
type: source
title: "React Hooks in Action"
authors:
  - "John Larsen"
publisher: "Manning Publications"
year: 2021
category: "H. Information Systems/React"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2021-React Hooks in Action-John Larsen-Manning Publications.pdf"
status: ingested
tags:
  - react
  - react-hooks
  - suspense
  - concurrent-react
  - state-management
  - custom-hooks
sources: ["[[2021-React Hooks in Action]]"]
created: 2026-10-19
updated: 2026-10-19
---

# React Hooks in Action (2021)

> **서지 정보**: John Larsen | Manning Publications (2021) | 376 Pages
> **분류**: `H. Information Systems / React`
> **원천 PDF**: [2021-React Hooks in Action-John Larsen-Manning Publications.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/React/2021-React%20Hooks%20in%20Action-John%20Larsen-Manning%20Publications.pdf)

---

## 핵심 요약 (Core Thesis)

회의실 예약 애플리케이션(Bookables)을 처음부터 끝까지 구축해 나가면서, **[[React]]의 내장 Hooks(`useState`, `useReducer`, `useEffect`, `useRef`, `useMemo`, `useCallback`, `useContext`)와 커스텀 훅 설계**, **서드파티 훅(`useQuery`, `useParams`)**, 그리고 **동시성 렌더링을 위한 `Suspense`, `useTransition`, `useDeferredValue`** 까지 단계적으로 마스터하는 Manning 실전서다.

클래스 컴포넌트의 복잡한 생명주기 메서드(`componentDidMount` / `componentDidUpdate` / `componentWillUnmount`)에 흩어져 있던 연관 로직들을 어떻게 **단일 `useEffect`와 커스텀 훅으로 응집(Colocate)** 시키는지 시각적인 다이어그램과 함께 명확히 보여준다 ([[React Hooks and Component Design Patterns]] 참조).

---

## 챕터별 핵심 내용 요약

### Part 1: 코어 React Hooks와 커스텀 훅 마스터리 (Ch 1 – Ch 10, p.22–301)
- **Ch 1. React is evolving (p.24–47)**
  - 클래스 컴포넌트의 3대 문제점(래퍼 지옥/HOC, 생명주기 메서드 간 로직 파편화, `this` 바인딩 혼란)과 함수형 Hooks의 탄생 배경, 그리고 **Hooks의 2대 규칙(최상위에서만 호출, React 함수 내에서만 호출)**.
- **Ch 2 – Ch 3. `useState` & `useReducer`로 컴포넌트 상태 관리 (p.48–112)**
  - 초기값 지연 계산(Lazy initial state `useState(() => compute())`), 이전 상태 기반 업데이터 함수(`setCount(c => c + 1)`), 그리고 복수의 하위 상태가 맞물려 변할 때 리듀서 함수(`useReducer`)로 상태 전이 규칙을 분리하는 기법.
- **Ch 4. Working with side effects (`useEffect`, p.113–138)**
  - 렌더링 후 실행되는 이펙트의 생명주기, 의존성 배열(Dependency Array) 설정 기준, 그리고 이벤트 리스너/타이머/구독 해제를 위한 **클린업(Cleanup) 함수 반환 패턴**.
- **Ch 5. Managing component state with the `useRef` hook (p.139–154)**
  - 두 가지 핵심 용도: ① DOM 엘리먼트(또는 [[React Native]]의 `TextInput`/`FlatList` 인스턴스) 직접 참조, ② **리렌더링을 유발하지 않고 렌더링 간에 값을 유지하는 가변 컨테이너(타이머 ID 등)**.
- **Ch 6 – Ch 8. 애플리케이션 상태 분할, `useMemo` 성능 최적화, `useContext` (p.155–238)**
  - 부모-자식 간 상태 끌어올리기(Lifting State Up) vs 프롭 내리기(Prop Drilling) 균형(Ch 6), 고비용 연산 메모이제이션(`useMemo`)과 함수 참조 유지(`useCallback`, Ch 7), Context Provider를 통한 전역 사용자 상태 공급(Ch 8).
- **Ch 9 – Ch 10. 커스텀 훅 제작(`useFetch`, `useLocalStorage`) 및 서드파티 훅 활용 (p.239–301)**
  - 반복되는 데이터 페칭, 윈도우 크기 감지, 로컬 스토리지 동기화 로직을 **독립적인 커스텀 훅으로 추출**하고, React Router 및 **[[TanStack Query and Zustand|React Query(`useQuery`, `useMutation`)]]** 와 결합하는 방법(Ch 10).

### Part 2: Suspense와 Concurrent React (Ch 11 – Ch 13, p.302–363)
- **Ch 11 – Ch 12. Code splitting & Data fetching with `Suspense` (p.304–347)**
  - `React.lazy`와 `<Suspense fallback={...}>` 및 Error Boundary를 결합해 선언적으로 로딩 상태와 에러 상태를 컴포넌트 외부 경계에서 처리하는 아키텍처 ([[React Fiber and Reconciliation]]).
- **Ch 13. Experimenting with `useTransition`, `useDeferredValue`, and `SuspenseList` (p.348–363)**
  - 느린 네트워크나 무거운 렌더링 전환 시 이전 화면을 유지하면서 `isPending` 인디케이터를 보여주는 `useTransition`과 지연된 값을 소비하는 `useDeferredValue` 실습.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[React Hooks and Component Design Patterns]] — Hooks의 2대 규칙, `useState` 지연 초기화, `useReducer` 상태 전이, `useEffect` 클린업, `useRef` 가변 참조, 커스텀 훅 추출 (Ch 1–10)
- [[React Fiber and Reconciliation]] — `Suspense` 기반 선언적 비동기 경계와 `useTransition` / `useDeferredValue` 동시성 UI (Ch 11–13)

### Entities
- [[React]] — 클래스 컴포넌트에서 함수형 Hooks 및 Concurrent 모드로의 진화
- [[TanStack Query and Zustand]] — 커스텀 `useFetch` 훅의 한계를 해결하는 React Query 도입 (Ch 10)
