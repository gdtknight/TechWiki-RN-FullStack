---
type: source
title: "Do it! 리액트 모던 웹 개발 with 타입스크립트"
authors:
  - "전예홍"
publisher: "이지스퍼블리싱"
year: 2023
category: "E. Programming Languages/JavaScript-TypeScript"
pdf_path: "/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2023-Do it! 리액트 모던 웹 개발 with 타입스크립트-전예홍-이지스퍼블리싱.pdf"
status: ingested
tags:
  - react
  - typescript
  - virtual-dom
  - react-hooks
  - tailwindcss
  - redux
  - react-router
sources: ["[[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Do it! 리액트 모던 웹 개발 with 타입스크립트 (2023)

> **서지 정보**: 전예홍 | 이지스퍼블리싱 (2023) | 680 Pages
> **분류**: `E. Programming Languages / JavaScript-TypeScript`
> **원천 PDF**: [2023-Do it! 리액트 모던 웹 개발 with 타입스크립트-전예홍-이지스퍼블리싱.pdf](file:///Users/yoshin/Documents/MarginNote4/E.%20Programming%20Languages/JavaScript-TypeScript/2023-Do%20it!%20%EB%A6%AC%EC%95%A1%ED%8A%B8%20%EB%AA%A8%EB%8D%98%20%EC%9B%B9%20%EA%B0%9C%EB%B0%9C%20with%20%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%A0%84%EC%98%88%ED%99%8D-%EC%9D%B4%EC%A7%80%EC%8A%A4%ED%8D%BC%EB%B8%94%EB%A6%AC%EC%8B%B1.pdf)

---

## 핵심 요약 (Core Thesis)

[[2021-Do it! 리액트 네이티브 앱 프로그래밍]]의 저자 전예홍이 집필한 **쌍둥이 구성의 모던 React + TypeScript 웹 개발 기본서**다.

모바일 편([[2021-Do it! 리액트 네이티브 앱 프로그래밍]])과 **챕터 목차·예제 설계·커스텀 훅·백엔드 API 서버 구조가 1:1로 대응**되도록 설계되어 있어, Android/iOS 에뮬레이터 빌드 부담 없이 가벼운 웹 브라우저 환경에서 **가상 DOM([[React Fiber and Reconciliation]]), JSX, Flexbox 레이아웃, [[React Hooks and Component Design Patterns|7대 React Hooks]], Redux 상태 관리**를 먼저 빠르게 체득한 뒤 [[React Native]]로 넘어가는 징검다리 교재로 최적이다 ([[Web React vs React Native Shared Core and Differences]] 참조).

---

## 챕터별 핵심 내용 요약

### 01–02장: 리액트 개발 환경과 동작 원리 (p.11–125)
- **Ch 01. 리액트 개발 준비 (p.11–49)**
  - Node.js, VSCode, TypeScript 템플릿 기반 프로젝트 스캐폴딩 및 Mock 데이터(Chance, Luxon) 유틸리티 구성.
- **Ch 02. 리액트 동작 원리 (p.50–125)**
  - **물리 DOM vs 가상 DOM (p.51–64)**: `React.createElement`로 생성되는 가상 DOM 트리가 `react-dom` 렌더러를 통해 실제 브라우저 DOM으로 변환되는 과정 ([[React Fiber and Reconciliation]]).
  - JSX 구문, 함수형 컴포넌트, `key`와 `children` 속성(p.93), 그리고 브라우저 DOM 이벤트와 React 합성 이벤트(`SyntheticEvent`, 이벤트 버블링, `stopPropagation`, `preventDefault`, p.98–125).

### 03–04장: CSS/Flexbox 스타일링과 리액트 훅 (p.126–371)
- **Ch 03. 컴포넌트 CSS 스타일링 (p.126–250)**
  - 인라인 스타일, CSS 클래스, **TailwindCSS** 및 **daisyUI** 컴포넌트 시스템 구축.
  - CSS 박스 모델과 **Flexbox 레이아웃(p.203–228)**: [[Mobile Layout and Styling with Flexbox]]의 기반이 되는 주축/교차축 정렬 원리를 웹에서 먼저 시각적으로 실습.
- **Ch 04. 함수 컴포넌트와 리액트 훅 (p.251–371)**
  - [[React Hooks and Component Design Patterns]]: 모바일 편(`2021-Do it! RN` Ch 04–05)과 완벽히 동일한 흐름으로 **`useMemo`, `useCallback`, `useState`, `useEffect`, `useLayoutEffect`, `useRef`, `useImperativeHandle`, `useContext`** 의 동작 원리와 커스텀 훅 패턴을 다룬다.

### 05–07장: 리덕스(Trello 클론), 라우터, 풀스택 서버 연동 (p.372–673)
- **Ch 05. 상태 관리와 리덕스 패키지 (p.372–501)**
  - `@reduxjs/toolkit` 및 고전 Redux의 Action, Reducer, Store, 미들웨어(`redux-logger`, `redux-thunk`)를 학습하고, 드래그 앤 드롭(`react-beautiful-dnd`)이 포함된 **Trello 칸반 보드 웹 앱(p.442–501)** 을 구현한다.
- **Ch 06. 리액트 라우터 (p.502–568)**
  - `react-router-dom` v6 기반 SPA 라우팅, `<Outlet>` 중첩 라우팅, 인증 여부에 따른 공개/비공개 라우트(`RequireAuth`) 보호 (모바일의 [[React Navigation]]과 대비됨).
- **Ch 07. 몽고DB와 API 서버 (p.569–673)**
  - MongoDB + Express + JWT 기반 REST API 인증 서버 구축 및 프런트엔드 연동 ([[2021-Do it! 리액트 네이티브 앱 프로그래밍]] Ch 09의 백엔드와 동일 아키텍처).

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[React Fiber and Reconciliation]] — 가상 DOM과 물리 DOM의 차이 및 호스트 렌더러 분리 (Ch 02)
- [[React Hooks and Component Design Patterns]] — 7대 기본 훅과 커스텀 훅, `forwardRef`/`useImperativeHandle` 패턴 (Ch 04)
- [[Web React vs React Native Shared Core and Differences]] — 동일 저자의 웹 편(2023)과 모바일 편(2021) 비교를 통한 공유 코어와 플랫폼 차이 분석
- [[Mobile Layout and Styling with Flexbox]] — 웹 CSS/Tailwind Flexbox와 RN Flexbox 비교 (Ch 03)
- [[Offline-First and Mobile State Management]] — Redux 미들웨어와 JWT 인증 상태 관리 (Ch 05, Ch 07)

### Entities
- [[React]] — 선언적 컴포넌트 아키텍처와 JSX, Virtual DOM 기초 (Ch 01–02)
- [[React Native]] — 웹 React 지식이 모바일 네이티브 앱으로 확장되는 대상 프레임워크

### Syntheses
- [[React Native Learning Roadmap and Evolution (2019-2022)]] — React 기초 선행 학습 및 RN 병행 학습 로드맵 통합
