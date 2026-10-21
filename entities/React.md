---
type: entity
title: "React"
entity_type: "framework"
category: "H. Information Systems/React"
sources:
  - "[[2024-Fluent React]]"
  - "[[2024-React in Depth]]"
  - "[[2026-React Application Architecture for Production Second Edition]]"
  - "[[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]"
  - "[[2022-React and React Native, Fourth Edition]]"
  - "[[2026-React Official Documentation]]"
tags:
  - react
  - ui-library
  - declarative-ui
  - virtual-dom
  - fiber
  - meta
created: 2026-10-19
updated: 2026-10-21
---

# React

## 개요

**React**는 Meta(구 Facebook)가 개발한 **선언적(Declarative)·컴포넌트 기반 JavaScript UI 라이브러리**로, 애플리케이션의 UI를 상태(State)에 대한 순수 함수(`UI = f(state)`)로 모델링한다 ([[2024-Fluent React]], Ch 1 p.19–54; [[2022-React and React Native, Fourth Edition]], Ch 1 p.27).

2026-02-24부터는 Meta가 아니라 Linux Foundation 산하의 **React Foundation**이 React와 [[React Native]]를 소유한다. 기술 거버넌스는 재단 이사회와 독립적으로 운영된다 ("React's technical governance will always be independent from the React Foundation board.") ([[2026-React Official Documentation]] 4절).

핵심 알고리즘인 [[React Fiber and Reconciliation|가상 DOM 및 Fiber Reconciler]](`react` 패키지)를 실제 화면을 그리는 호스트 렌더러(`react-dom`, `react-native`, `react-three-fiber` 등)와 엄격히 분리한 아키텍처 덕분에, 웹 브라우저뿐만 아니라 [[React Native]]를 통한 iOS/Android 모바일 네이티브 앱 개발의 공통 기반 역할을 수행한다 ([[Web React vs React Native Shared Core and Differences]]).

---

## 핵심 아키텍처 특징

1. **선언적 UI와 단방향 데이터 흐름 (Unidirectional Data Flow)**:
   - 개발자가 DOM 노드를 직접 찾아 수정하는 명령형(Imperative) 방식 대신, 상태(State)가 변하면 React가 새로운 Virtual DOM 스냅샷을 생성하고 이전 트리와 비교(Reconciliation)하여 변경된 부분만 업데이트한다 ([[2024-Fluent React]], p.44–54).
2. **함수형 컴포넌트와 Hooks 생태계**:
   - 클래스 컴포넌트의 복잡한 생명주기 메서드와 `this` 바인딩 문제를 해결하고, [[React Hooks and Component Design Patterns|Hooks(`useState`, `useEffect`, 커스텀 훅)]]를 통해 상태 로직을 재사용 가능한 함수 단위로 합성한다 ([[2024-React in Depth]], Ch 2–3).
3. **Concurrent React, React 19 Actions & React Compiler**:
   - React 18부터 도입된 동시성 렌더러(Render Lanes, `useTransition`, `Suspense`)에 이어, **React 19 공식 문서(`react.dev`)** 에서는 비동기 데이터 변이와 폼 처리를 위한 **Actions(`useActionState`, `useFormStatus`, `useOptimistic`)**, 조건부 Promise/Context 읽기 API인 **`use()`**, `forwardRef` 없는 **`ref` 일반 Prop 전달**, **`<Context>` 직접 Provider 렌더링**, 그리고 빌드 타임 자동 메모이제이션을 수행하는 **React Compiler**로 진화했다 ([[2024-Fluent React]], Ch 7 p.203, Ch 10 p.312; [[2026-Full-Stack React TypeScript and Node 2nd Edition]], Ch 5 p.164–191).
   - **19.x 후속 릴리스** ([[2026-React Official Documentation]] 4절):
     - React Compiler는 2025-10-07 v1.0 안정판이 되었다 (React 17 이상 호환, Expo SDK 54 이상은 기본 활성화).
     - 19.2(2025-10-01): `<Activity />`("lets you break your app into "activities" that can be controlled and prioritized")와 `useEffectEvent` 추가. `useEffectEvent`는 외부 시스템의 "이벤트"를 앱 코드에 알리는 Effect 패턴을 위한 API다.
     - 19.3(2026-09-09): `<ViewTransition>`, Fragment Refs, Trusted Types 지원 추가.
   - **React Native 반영 시점**: 0.80에 React 19.1.0, 0.83에 19.2, 0.88에 19.3.0이 들어갔다 ([[2026-React Native Official Documentation]] 5절).

---

## 타 프런트엔드 프레임워크와의 반응성(Reactivity) 비교

[[2024-Fluent React]] (Ch 10 "React Alternatives", p.295–315)에서 분석하는 아키텍처 철학 비교:

| 항목 | **[[React]]** (Web & [[React Native]]) | Solid / Qwik (Fine-grained Signals) | Svelte | Vue.js 3 |
|---|---|---|---|---|
| **업데이트 감지 모델** | **Value-based / Pull-Schedule 모델** (`setState` 호출 시 컴포넌트 함수를 재실행하여 Virtual DOM Fiber 비교) | **Fine-grained Reactive Signals** (컴포넌트 함수는 단 1회만 실행되고, Signal을 구독하는 텍스트/속성 노드만 직접 갱신) | **컴파일러 기반 반응성** (빌드 타임에 상태 할당문을 DOM 업데이트 코드로 직접 변환, 런타임 VDOM 없음) | **Proxy 기반 반응성(`ref`, `reactive`) + Virtual DOM 하이브리드** |
| **핵심 강점** | 압도적인 생태계, **[[React Native]]를 통한 웹·모바일 완벽 통합**, Fiber 기반 동시성 스케줄링과 호스트 환경 독립성 | 가상 DOM 오버헤드 제로, 수동 `useMemo`/`useCallback` 불필요 | 간결한 보일러플레이트와 작은 번들 크기 | 낮은 진입 장벽과 양방향/단방향 유연성 |
| **주의할 트레이드오프** | 부모 리렌더링이 자식으로 전파되므로 상태 코로케이션과 메모이제이션 설계 이해 필요 (`2024-React in Depth` Ch 3) | React 대비 서드파티 생태계 및 모바일 네이티브 확장성 규모가 작음 | 대규모 상태 조합 시 특수 문법 제약 | 템플릿 문법과 JSX 간 이원화 |

---

## 관련 링크

- **연관 개념**: [[Virtual DOM and Reconciliation from Scratch]], [[React Fiber and Reconciliation]], [[React Hooks and Component Design Patterns]], [[Web React vs React Native Shared Core and Differences]], [[Offline-First and Mobile State Management]], [[Full-Stack Authentication, Token Refresh, and Observability]], [[AI-Enhanced React Applications and Specification-Driven Development]]
- **연관 엔티티**: [[React Native]], [[TanStack Query and Zustand]], [[Expo]]
- **출처 문헌**: [[2024-Fluent React]], [[2024-React in Depth]], [[2021-React Hooks in Action]], [[2021-Designing React Hooks the Right Way]], [[2024-Build a Frontend Web Framework (From Scratch)]], [[2025-Advanced Front-End Development]], [[2026-Full-Stack React TypeScript and Node 2nd Edition]], [[2026-React Application Architecture for Production Second Edition]], [[2026-Build AI-Enhanced Web Apps]], [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]], [[2022-React and React Native, Fourth Edition]], [[2026-React Official Documentation]]
