---
type: source
title: "Fluent React"
authors:
  - "Tejas Kumar"
publisher: "O'Reilly Media"
year: 2024
category: "H. Information Systems/React"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2024-Fluent React-Tejas Kumar-OReilly Media.pdf"
status: ingested
tags:
  - react
  - virtual-dom
  - fiber
  - reconciliation
  - concurrent-react
  - render-lanes
  - react-server-components
  - ssr
sources: ["[[2024-Fluent React]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Fluent React (2024)

> **서지 정보**: Tejas Kumar | O'Reilly Media (2024) | 337 Pages
> **분류**: `H. Information Systems / React`
> **원천 PDF**: [2024-Fluent React-Tejas Kumar-OReilly Media.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/React/2024-Fluent%20React-Tejas%20Kumar-OReilly%20Media.pdf)

---

## 핵심 요약 (Core Thesis)

단순히 "React API를 어떻게 쓰는가"를 넘어, **"React가 내부적으로 어떻게 작동하는가(Under the Hood)"** 를 가장 깊이 있게 해부하는 O'Reilly 심화서다.

명령형 UI 조작의 역사적 한계에서 출발하여, JSX 컴파일 원리, Virtual DOM, **[[React Fiber and Reconciliation|Fiber Reconciler 아키텍처]]** (Stack Reconciler 대비 중단·우선순위 스케줄링이 가능한 이유), **Concurrent React** (`Render Lanes`, `useTransition`, `useDeferredValue`), 서버 사이드 렌더링(SSR/Hydration) 및 **React Server Components (RSC)** 까지 React 코어 엔진의 작동 메커니즘을 명쾌하게 설명한다. 특히 **Reconciler(`react`)와 Host Environment(`react-dom`, `react-native`)의 분리 구조(Ch 4, p.97)** 를 이해하면 왜 동일한 React 코드가 웹 브라우저와 iOS/Android 네이티브 위젯 양쪽에서 동작하는지 근본 원리를 깨우칠 수 있다.

---

## 챕터별 핵심 내용 요약

### Part I: React 코어 메커니즘과 Fiber 아키텍처 (Ch 1 – Ch 5)
- **Ch 1. The Entry-Level Stuff (p.19–56)**
  - jQuery/Backbone 시절 명령형(Imperative) DOM 조작의 상태 동기화 고통과, `v = f(s)` (뷰는 상태의 함수)라는 React의 선언적(Declarative)·불변(Immutable) 모델 탄생 배경.
- **Ch 2. JSX (p.57–70)**
  - Babel/TypeScript 컴파일러가 JSX 구문을 AST(추상 구문 트리)를 거쳐 `React.createElement` (또는 `_jsx` 런타임) 호출 객체로 변환하는 내부 과정.
- **Ch 3. The Virtual DOM (p.71–96)**
  - 실제 브라우저 DOM의 리플로우(Reflow)/리페인트(Repaint) 비용 문제와, 인메모리 경량 청사진으로서의 Virtual DOM이 변경 사항을 배칭(Batching)하고 최소한의 diff만 계산하는 원리 (p.85).
- **Ch 4. Inside Reconciliation (p.97–114)**
  - **[[React Fiber and Reconciliation]] 핵심 챕터**:
    - **호스트 환경 독립성 (p.97)**: Virtual DOM 청사진을 받아 실제 환경(웹 브라우저, 셸, **iOS/Android 네이티브 플랫폼**)에 반영하는 재조정(Reconciliation) 프로세스.
    - **Stack Reconciler vs Fiber Reconciler (p.100–112)**: 동기적 호출 스택에 갇혀 중간에 멈출 수 없었던 구형 Stack Reconciler의 한계를 극복하기 위해 도입된 **Fiber 데이터 구조**(`tag`, `type`, `props`, `stateNode`, 그리고 단일 연결 리스트를 형성하는 `return`, `child`, `sibling` 포인터)와 **Render Phase(비동기/중단 가능) vs Commit Phase(동기 UI 반영)** 의 2단계 모델.
- **Ch 5. Common Questions and Powerful Patterns (p.115–168)**
  - `React.memo`와 `useMemo`의 정확한 사용 시점, 코드 스플리팅(`React.lazy`), `useState` vs `useReducer` 선택 기준, 그리고 **Compound Components**, **Control Props**, **Custom Hooks** 등 실전 디자인 패턴 ([[React Hooks and Component Design Patterns]]).

### Part II: 동시성, 서버 렌더링, 프레임워크와 미래 (Ch 6 – Ch 11)
- **Ch 6. Server-Side React (p.169–202)**
  - CSR의 한계(빈 HTML, 초기 로딩 지연)와 SSR, `renderToString` vs 스트리밍 SSR(`renderToPipeableStream`), 그리고 클라이언트 측 이벤트 연결 과정인 **Hydration**.
- **Ch 7. Concurrent React (p.203–234)**
  - 단일 메인 스레드를 블로킹하지 않기 위한 **스케줄러(Scheduler)** 와 비트마스크 기반 우선순위 큐인 **Render Lanes (p.212)**.
  - 긴급 업데이트(사용자 타이핑)와 전환 업데이트(무거운 검색 결과 렌더링)를 분리하는 `useTransition`(p.216)과 `useDeferredValue`(p.219), 그리고 Tearing(화면 찢어짐) 문제 해결을 위한 `useSyncExternalStore`.
- **Ch 8 – Ch 9. Frameworks & React Server Components (p.235–294)**
  - Next.js, Remix 등 메타 프레임워크의 필요성과, 서버에서만 실행되어 클라이언트 번들 크기를 0으로 줄이는 **React Server Components (RSC)** 및 Server Actions의 작동 원리.
- **Ch 10 – Ch 11. React Alternatives & Conclusion (p.295–326)**
  - **"React Is Not Reactive" (p.308)**: 세밀한 시그널(Fine-grained Signals) 기반인 Solid/Svelte/Vue와 달리, React는 상태 변경 시 컴포넌트 함수를 다시 호출해 Virtual DOM 스냅샷을 비교하는 Pull/Schedule 기반 모델임을 비교하고, 이를 자동 메모이제이션으로 보완하는 **React Forget (React Compiler, p.312)** 의 방향성을 제시.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[React Fiber and Reconciliation]] — Virtual DOM, Stack vs Fiber Reconciler, Fiber 트리 포인터 구조, Render/Commit 단계, Render Lanes 동시성 스케줄링 (Ch 3–4, Ch 7)
- [[React Hooks and Component Design Patterns]] — 메모이제이션(`React.memo`, `useMemo`), `useState` vs `useReducer`, Compound Components 패턴 (Ch 5)
- [[Web React vs React Native Shared Core and Differences]] — Reconciler(`react`)와 호스트 렌더러(`react-dom` vs `react-native` Fabric)의 아키텍처 연결 (Ch 4)

### Entities
- [[React]] — 선언적 UI 철학, JSX 컴파일, 타 프레임워크(Vue, Svelte, Solid)와의 반응성 모델 비교 및 React Compiler (Ch 1–2, Ch 10)

### Syntheses
- [[React Native Learning Roadmap and Evolution (2019-2022)]] — React Native의 JS 스레드 및 Fabric 렌더러를 깊이 이해하기 위한 필독 이론서
