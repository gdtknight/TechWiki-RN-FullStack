---
type: source
title: "Build a Frontend Web Framework (From Scratch)"
authors:
  - "Ángel Sola Orbaiceta"
publisher: "Manning Publications"
year: 2024
category: "H. Information Systems/Full Stack"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/Full Stack/2024-Build a Frontend Web Framework (From Scratch)-Ángel Sola Orbaiceta-Manning Publications.pdf"
status: ingested
tags:
  - javascript
  - virtual-dom
  - reconciliation
  - diffing-algorithm
  - frontend-framework
  - scheduler
sources: ["[[2024-Build a Frontend Web Framework (From Scratch)]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Build a Frontend Web Framework (From Scratch) (2024)

> **서지 정보**: Ángel Sola Orbaiceta | Manning Publications (2024) | 385 Pages
> **분류**: `H. Information Systems / Full Stack`
> **원천 PDF**: [2024-Build a Frontend Web Framework (From Scratch)-Ángel Sola Orbaiceta-Manning Publications.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/Full%20Stack/2024-Build%20a%20Frontend%20Web%20Framework%20(From%20Scratch)-%C3%81ngel%20Sola%20Orbaiceta-Manning%20Publications.pdf)

---

## 핵심 요약 (Core Thesis)

"프런트엔드 프레임워크([[React]], Vue 등)는 마법이 아니다." 외부 라이브러리 없이 **순수 [[JavaScript (ECMAScript)]]만으로 Virtual DOM 생성기(`h`), 마운트/언마운트 엔진, 상태 관리 디스패처, 두 가상 트리를 비교해 최소한의 DOM 패치만 수행하는 Reconciliation(재조정) Diffing 알고리즘, Key 기반 리스트 최적화, 그리고 마이크로태스크 기반 비동기 생명주기 스케줄러까지 직접 코딩해 NPM 패키지로 배포**하는 실습 명저다.

[[2024-Fluent React]]가 개념적으로 설명한 [[React Fiber and Reconciliation|Reconciliation]]이 실제로 어떤 배열/객체 비교 코드(`objectsDiff`, `arraysDiffSequence`, `patchDOM`)로 구현되는지 100% 투명하게 밝혀준다 ([[Virtual DOM and Reconciliation from Scratch]] 참조).

---

## 챕터별 핵심 내용 요약

### Part 1 – Part 2: 바닐라 JS의 한계와 Virtual DOM & Reconciliation 엔진 구현 (Ch 1 – Ch 8)
- **Ch 1 – Ch 2. 왜 프레임워크가 필요한가? (p.26–63)**
  - 순수 Vanilla JS(`document.createElement`, 명령형 이벤트 핸들러)로 Todo 앱을 만들 때 상태 변경과 DOM 조작 코드가 뒤엉키는 구조적 한계 체험.
- **Ch 3 – Ch 4. Virtual DOM 렌더링, 마운팅, 파괴 (p.66–108)**
  - **[[Virtual DOM and Reconciliation from Scratch]] 기초**:
    - `h(tag, props, children)`, `hString(text)`, `hFragment(vNodes)` 함수를 통해 인메모리 가상 노드(`DOM_TYPES.ELEMENT`, `TEXT`, `FRAGMENT`) 트리 생성(Ch 3).
    - 가상 노드를 실제 물리 DOM 노드로 변환해 화면에 붙이고(`mountDOM`) 실제 DOM 참조(`el`)와 이벤트 리스너를 저장한 뒤, 언마운트 시 메모리 누수 없이 제거(`destroyDOM`)하는 구현(Ch 4).
- **Ch 5 – Ch 6. 상태 관리 생명주기와 첫 버전 배포 (p.109–144)**
  - 커맨드-리듀서(Command-Reducer) 기반의 `Dispatcher` 클래스를 설계하고, 상태가 바뀔 때마다 전체 앱을 언마운트 후 재마운트하는 단순 렌더러를 먼저 만들어 문제점(포커스 상실, 스크롤 초기화, 성능 저하)을 확인.
- **Ch 7 – Ch 8. Reconciliation 알고리즘: Virtual Tree Diffing & DOM Patching (p.145–227)**
  - **핵심 하이라이트**: 전체 DOM을 부수지 않고 이전 가상 트리(`oldVDom`)와 새 가상 트리(`newVDom`)를 비교해 변경된 부분만 수술적으로 패치하는 **`patchDOM`** 엔진 완성:
    - `objectsDiff` & `patchAttrs` / `patchStyle` / `patchEvents`: 속성, 클래스, 인라인 스타일, 이벤트 리스너의 추가·삭제·변경분만 추출.
    - **`arraysDiffSequence` (p.165–183)**: 자식 노드 배열에서 `NOOP`(유지), `ADD`(추가), `REMOVE`(삭제), `MOVE`(이동) 연산 시퀀스를 도출해 최소한의 `insertBefore` / `removeChild`만 수행.

### Part 3: 상태 저장 컴포넌트, Keyed Lists, 비동기 스케줄러 (Ch 9 – Ch 14)
- **Ch 9 – Ch 11. Stateful Components, Props, Events (p.230–301)**
  - 독자적인 로컬 상태를 가지는 컴포넌트 클래스(`defineComponent`), 내부 메서드 바인딩, 부모→자식 Props 전달 및 자식→부모 커스텀 이벤트(`emit`) 통신 구현.
- **Ch 12. Keyed Lists (p.302–321)**
  - 왜 [[React]]와 [[React Native]](`FlatList`)에서 리스트 렌더링 시 고유한 **`key` prop**이 필수적인지, `key`가 없을 때와 있을 때 `arraysDiffSequence`의 DOM 노드 재사용 및 상태 보존 차이가 어떻게 발생하는지 코드로 증명.
- **Ch 13 – Ch 14. Component Lifecycle Hooks, Scheduler & Async Testing (p.322–353)**
  - `onMounted`, `onUnmounted` 생명주기 훅이 메인 렌더링 스레드를 블로킹하지 않도록 **`queueMicrotask` 기반 작업 스케줄러(Jobs Queue)** 로 비동기 실행하는 아키텍처([[JavaScript Event Loop and Asynchronous Patterns]]) 및 비동기 컴포넌트 단위 테스트.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Virtual DOM and Reconciliation from Scratch]] — `h()` 가상 노드, `mountDOM`, `arraysDiffSequence` Key 기반 리스트 Diffing, `patchDOM`의 실제 구현 알고리즘 (Ch 3–4, Ch 7–8, Ch 12)
- [[React Fiber and Reconciliation]] — React 가상 DOM 재조정 엔진의 기저 원리 비교
- [[JavaScript Event Loop and Asynchronous Patterns]] — `queueMicrotask`를 이용한 프레임워크 생명주기 스케줄러 구현 (Ch 13)

### Entities
- [[JavaScript (ECMAScript)]] — 순수 바닐라 JS만으로 모던 선언적 UI 프레임워크를 구축하는 실전 기법
- [[React]] — 컴포넌트, Props, State, Key, 생명주기 훅의 내부 동작 원리 체화
