---
type: source
title: "React in Depth"
authors:
  - "Morten Barklund"
publisher: "Manning Publications"
year: 2024
category: "H. Information Systems/React"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2024-React in Depth-Morten Barklund-Manning Publications.pdf"
status: ingested
tags:
  - react
  - component-patterns
  - performance
  - typescript
  - zustand
  - redux-toolkit
  - xstate
  - tanstack-query
sources: ["[[2024-React in Depth]]"]
created: 2026-10-19
updated: 2026-10-19
---

# React in Depth (2024)

> **서지 정보**: Morten Barklund | Manning Publications (2024) | 434 Pages
> **분류**: `H. Information Systems / React`
> **원천 PDF**: [2024-React in Depth-Morten Barklund-Manning Publications.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/React/2024-React%20in%20Depth-Morten%20Barklund-Manning%20Publications.pdf)

---

## 핵심 요약 (Core Thesis)

기초 문법을 막 뗀 주니어 개발자가 **시니어 수준의 React/React Native 아키텍트**로 도약하기 위해 반드시 마스터해야 할 실전 엔지니어링 기법을 집대성한 Manning 심화서다.

특히 이 책의 전반부–중반부(Ch 2–9)에서 다루는 **고급 컴포넌트 합성 패턴(Provider, Composite, Summary)**, **리렌더링 최적화와 의존성 배열(Dependency Arrays)의 원리**, **TypeScript 제네릭과 React Hooks의 결합**, 그리고 동일한 목표 추적 앱을 **순수 Context → Redux Toolkit(RTK) → [[TanStack Query and Zustand|Zustand]] → XState → [[TanStack Query and Zustand|TanStack Query]]** 로 차례로 리팩토링하며 비교하는 8–9장은 **[[React Native]] 모바일 개발에 100% 그대로 적용되는 핵심 자산**이다 ([[Web React vs React Native Shared Core and Differences]] 참조).

---

## 챕터별 핵심 내용 요약

### 1. 고급 컴포넌트 패턴과 렌더링 최적화 (Ch 1 – Ch 4)
- **Ch 1. Developer's guide to the React Ecosystem (p.25–42)**
  - 모던 React 기술 스택(빌드 툴, 스타일링, 상태 관리, 데이터 페칭, 테스팅) 지도.
- **Ch 2. Advanced component patterns (p.43–79)**
  - **[[React Hooks and Component Design Patterns]] 핵심 챕터**:
    - **Provider Pattern (p.44)**: 다중 Context 분리와 커스텀 소비 훅을 통해 관심사를 분리하고 불필요한 리렌더링을 방지하는 패턴.
    - **Composite Pattern (Compound Components, p.55)**: `<RadioGroup>`과 `<RadioGroup.Option>`처럼 부모-자식 컴포넌트가 암묵적 Context로 상태를 공유하며 선언적 API를 제공하는 패턴 (모바일 커스텀 UI 컴포넌트 설계에 필수).
    - **Summary Pattern (p.68)**: 복잡한 폼이나 위저드 상태를 요약·집계하는 구조.
- **Ch 3. Optimizing React performance (p.80–108)**
  - **컴포넌트가 리렌더링되는 3가지 유일한 이유 (p.81)**: ① 마운트 시점, ② 부모 컴포넌트가 리렌더링될 때, ③ 컴포넌트 내부의 훅이 리렌더링을 플래그했을 때.
  - 컴포넌트 트리 구조 개선(상태 아래로 내리기/State Colocation, `children` prop으로 감싸기)과 `memo()`, `useMemo`, `useCallback` 및 참조 동일성(Referential Equality)을 고려한 의존성 배열 설계(p.101).
- **Ch 4. Better code maintenance with developer tooling (p.109–142)**
  - ESLint(`eslint-plugin-react-hooks`), Prettier, React Developer Tools Profiler를 활용한 렌더링 병목 추적.

### 2. TypeScript 마스터리와 스타일링 (Ch 5 – Ch 7)
- **Ch 5 – Ch 6. TypeScript & Mastering TypeScript with React (p.143–204)**
  - Discriminated Unions(판별 유니온), 제네릭(Generics), 유틸리티 타입(`ComponentPropsWithoutRef`, `ReactNode` vs `JSX.Element`) 및 모든 React Hooks(`useState`, `useReducer`, `useRef`, `createContext`)의 엄격한 타이핑과 제네릭 페이지네이션 컴포넌트 구현.
- **Ch 7. CSS in JavaScript (p.205–238)**
  - 일반 CSS, CSS Modules, `styled-components` 등의 트레이드오프 비교.

### 3. 모던 클라이언트 상태 관리와 서버 캐싱 (Ch 8 – Ch 9)
- **Ch 8. Data management in React (p.239–282)**
  - 동일한 애플리케이션의 데이터 레이어를 5가지 방식으로 구현하며 장단점을 직접 비교 ([[TanStack Query and Zustand]], [[Offline-First and Mobile State Management]]):
    1. 순수 `useState` + Context (전체 트리 리렌더링 문제)
    2. `useReducer` + `useContextSelector` + Immer
    3. **Redux Toolkit (RTK, p.261)**: 고전 Redux의 보일러플레이트를 제거한 슬라이스(`createSlice`) 기반 상태 관리.
    4. **Zustand (p.267)**: Context Provider조차 필요 없이 단일 `create()` 함수로 훅을 생성해 어디서나 구독·선택(Selector)하는 초경량 스토어.
    5. **XState (p.271)**: 유한 상태 머신(Finite State Machine) 기반의 엄격한 상태 전이 제어.
- **Ch 9. Remote data and reactive caching (p.283–318)**
  - 수동 `fetch` + `useEffect` 방식의 경쟁 상태(Race Condition)와 캐시 일관성 문제를 짚고, **[[TanStack Query and Zustand|TanStack Query (React Query)]]** (`useQuery`, `useMutation`, `invalidateQueries`, 낙관적 업데이트)를 통한 반응형 원격 데이터 캐싱 아키텍처 구축.

### 4. 테스팅과 웹 프레임워크 (Ch 10 – Ch 12)
- **Ch 10. Unit-testing React (p.319–350)**: Vitest + React Testing Library(RTL)를 이용한 정적/인터랙티브/의존성 컴포넌트 단위 테스트.
- **Ch 11 – Ch 12. Website frameworks & Remix Project (p.351–398)**: Next.js, Remix, Gatsby, Astro 비교 및 Remix 기반 풀스택 경비 추적 앱 구현.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[React Hooks and Component Design Patterns]] — 렌더링 3대 조건, Provider/Composite/Summary 패턴, 의존성 배열과 메모이제이션, TypeScript 제네릭 훅 (Ch 2–3, Ch 5–6)
- [[Web React vs React Native Shared Core and Differences]] — 웹과 모바일에서 100% 공유되는 TypeScript 훅, 컴포넌트 패턴, 상태 관리 아키텍처
- [[Offline-First and Mobile State Management]] — 고전 Redux에서 RTK, Zustand, TanStack Query로의 상태 관리 진화 (Ch 8–9)

### Entities
- [[TanStack Query and Zustand]] — 모던 React/RN 생태계의 서버 상태 캐싱(TanStack Query) 및 클라이언트 스토어(Zustand, RTK) 표준 (Ch 8–9)
- [[React]] — 모던 React 생태계 기술 스택과 테스팅 전략 (Ch 1, Ch 10)
