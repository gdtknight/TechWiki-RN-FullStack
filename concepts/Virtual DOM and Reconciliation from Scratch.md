---
type: concept
title: "Virtual DOM and Reconciliation from Scratch"
category: "H. Information Systems/React"
tags: [virtual-dom, reconciliation, diffing-algorithm, frontend-framework, mountdom, patchdom, hooks-internals]
sources:
  - "[[2024-Build a Frontend Web Framework (From Scratch)]]"
  - "[[2021-Designing React Hooks the Right Way]]"
  - "[[2024-Fluent React]]"
created: 2026-10-19
updated: 2026-10-19
---

# Virtual DOM and Reconciliation from Scratch

## 1. 개요 (Overview)
모던 선언형 UI 프레임워크([[React]], Vue 등)는 개발자가 상태($S$)의 함수로 UI($V = f(S)$)를 선언하면, 런타임이 이전 가상 노드 트리($V_{old}$)와 새 가상 노드 트리($V_{new}$)의 차이(Diff)를 계산해 최소한의 실제 DOM 연산만 수행한다. [[2024-Build a Frontend Web Framework (From Scratch)]] (Ch 3–8, 12–13)와 [[2021-Designing React Hooks the Right Way]] (Ch 2–3)를 교차 종합하여, **가상 DOM 생성(`h`) -> 마운트(`mountDOM`) -> 재조정 및 패치(`patchDOM`) -> 컴포넌트 훅 상태 슬롯(Hook State Slots)** 의 밑바닥(From Scratch) 구현 원리를 정리한다.

---

## 2. Virtual DOM 노드 표현 (`h`, `hString`, `hFragment`)
[[2024-Build a Frontend Web Framework (From Scratch)]] (Ch 3, p.49–77)에 따르면, 가상 DOM 트리는 세 가지 기본 VNode 타입(`DOM_TYPES`)으로 구성된다.

1. **Text 노드 (`DOM_TYPES.TEXT`)**: `hString(str)`으로 생성되며 `{ type: 'text', value: str }` 구조를 가진다.
2. **Element 노드 (`DOM_TYPES.ELEMENT`)**: `h(tag, props, children)` 하이퍼스크립트 함수로 생성되며 `{ tag, props, children, type: 'element' }` 구조를 가진다. 이때 `null`이나 `undefined` 자식은 필터링되고, 원시 문자열 자식은 자동으로 Text 노드로 변환된다.
3. **Fragment 노드 (`DOM_TYPES.FRAGMENT`)**: `hFragment(vNodes)`로 생성되며, 실제 DOM에 불필요한 래퍼 요소를 추가하지 않고 여러 자식 노드를 그룹화한다.
4. **Component 노드 (`DOM_TYPES.COMPONENT`)**: 클래스 또는 함수형 컴포넌트를 나타내며(Ch 9, p.220), 자신만의 내부 상태와 생명주기를 캡슐화한다.

---

## 3. 마운트(`mountDOM`)와 언마운트(`destroyDOM`)
가상 노드를 처음 브라우저 화면에 그리는 `mountDOM(vdom, parentEl, index)` 과정(Ch 4, p.78–111)은 두 가지 핵심 불변식(Invariant)을 유지한다:

1. **실제 DOM 참조 저장 (`vdom.el = element`)**: 생성된 실제 DOM 노드(`Text` 또는 `HTMLElement`)의 참조를 반드시 VNode의 `el` 속성에 저장해야 한다. 그래야 이후 업데이트(`patchDOM`)나 언마운트(`destroyDOM`) 시 DOM 트리를 다시 탐색하지 않고 $O(1)$에 접근할 수 있다.
2. **이벤트 리스너 참조 저장 (`vdom.listeners`)**: `addEventListeners`로 등록한 핸들러 참조를 `vdom.listeners`에 보관하여, 언마운트 시 `removeEventListener`로 메모리 누수를 완벽히 방지한다.

---

## 4. 재조정(Reconciliation) 및 Diffing 알고리즘 (`patchDOM`)
상태가 변경되어 새 가상 트리(`newVdom`)가 생성되면, 프레임워크는 `patchDOM(oldVdom, newVdom, parentEl)`을 실행해 최소 변경 집합만 실제 DOM에 반영한다([[2024-Build a Frontend Web Framework (From Scratch)]] Ch 7–8, p.146–219).

### 4.1 노드 동등성 검사 (`areNodesEqual`)
- 두 VNode의 `type`이 다르면 전혀 다른 노드로 간주한다.
- 둘 다 `ELEMENT`인 경우 `tag` 이름과 **`key` 속성(`props.key`)** 이 모두 같아야 동일한 노드로 판정한다(Ch 12, p.295).
- 노드가 다르면 기존 노드를 `destroyDOM`으로 제거하고 같은 인덱스 위치에 새 노드를 `mountDOM`으로 교체한다. 노드가 같다면 `newVdom.el = oldVdom.el`로 실제 DOM 참조를 이어받은 뒤 세부 패치를 진행한다.

### 4.2 객체 및 속성 비교 (`objectsDiff`)
이전 `props`와 새 `props` 객체를 비교하여 `added`, `removed`, `updated` 키 배열을 산출한다(Ch 7.3, p.154):
- 일반 어트리뷰트와 `class`(배열/문자열), `style` 객체, 이벤트 리스너(`on`)를 각각 분리하여 추가·제거·갱신된 항목만 DOM API(`setAttribute`, `removeAttribute`, `style.setProperty`)로 수정한다.

### 4.3 자식 배열 시퀀스 비교 (`arraysDiffSequence` & `key` Prop)
자식 노드 배열(`children`)을 재조정할 때 단순 인덱스 비교를 수행하면 맨 앞에 요소가 하나만 추가되어도 뒤따르는 모든 형제 노드를 다시 그리는 심각한 성능 저하가 발생한다.
- **`arraysDiffSequence(oldArray, newArray, equalsFn)` (Ch 7.5, p.164–182)**: 두 배열을 순회하며 원래 인덱스를 추적하는 래퍼(`ArrayWithOriginalIndices`)를 사용해 **`NOOP`(유지), `ADD`(추가), `REMOVE`(삭제), `MOVE`(이동)** 의 4가지 연산 시퀀스를 생성한다.
- **`key` Prop의 결정적 역할 (Ch 12, p.295–334)**: `key`가 없으면 동일한 `<li>` 태그들을 구별할 수 없어 내부 텍스트와 상태를 덮어쓰는 버그가 발생하지만, 고유한 `key`를 부여하면 `arraysDiffSequence`가 정확히 어떤 DOM 노드가 이동(`MOVE`)했는지 식별하여 `parentEl.insertBefore(el, refEl)`만으로 기존 DOM 노드와 내부 컴포넌트 상태를 그대로 보존한 채 위치만 옮긴다.

---

## 5. 함수형 컴포넌트와 Hook 상태 슬롯의 연결
[[2021-Designing React Hooks the Right Way]] (Ch 2–3, p.26–74)와 [[React Hooks and Component Design Patterns]]에서 밝히듯, 함수형 컴포넌트가 재조정 과정에서 매번 다시 호출되더라도 상태를 유지할 수 있는 이유는 **VNode(React에서는 Fiber 노드) 인스턴스 내부에 연결된 훅 상태 배열(또는 연결 리스트)과 호출 인덱스(`cursor`)** 덕분이다:
- `areNodesEqual`을 통해 동일 위치·동일 타입(및 `key`)의 컴포넌트로 판정되면, 기존 컴포넌트 인스턴스에 저장된 `states` 배열과 `effects` 배열이 새 렌더링으로 인계된다.
- 반대로 조건부 렌더링으로 컴포넌트 타입이 바뀌거나 `key`가 변경되면 기존 컴포넌트 인스턴스가 `destroyDOM`되면서 클린업 함수(`useEffect` 반환 함수)가 실행되고 훅 상태 슬롯도 완전히 초기화된다.
- **공식 문서 근거 (`react.dev/learn/preserving-and-resetting-state`)**: React 공식 문서는 이를 **"동일한 트리 위치의 동일한 컴포넌트는 상태를 보존하고, 다른 타입이거나 다른 `key`가 부여되면 서브트리의 상태를 즉시 초기화(Reset)한다"** 는 원칙으로 정의하며, 폼 입력값을 사용자별로 초기화할 때 `useEffect` 대신 `<Chat key={to.id} />`처럼 의도적으로 `key`를 변경하는 패턴을 권장한다.

---

## 6. 관련 문서 (See Also)
- [[React Fiber and Reconciliation]] — 동기식 재귀 `patchDOM`을 작업 분할이 가능한 비동기 연결 리스트(Fiber) 아키텍처로 발전시킨 React 16+ 내부 구조
- [[React Hooks and Component Design Patterns]] — 함수형 컴포넌트의 훅 상태 슬롯 메커니즘과 리렌더링 최적화
- [[2024-Build a Frontend Web Framework (From Scratch)]]
- [[2021-Designing React Hooks the Right Way]]
