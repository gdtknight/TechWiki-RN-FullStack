---
type: concept
title: "Browser Rendering Pipeline and Frame Lifecycle"
category: "H. Information Systems/Web Infrastructure"
sources:
  - "[[2025-Web Browser Engineering]]"
  - "[[2026-Web Performance Engineering in the Age of AI]]"
tags:
  - browser
  - rendering-pipeline
  - critical-rendering-path
  - layout
  - reflow
  - paint
  - compositing
  - frame-budget
  - requestanimationframe
  - main-thread
created: 2026-10-21
updated: 2026-10-21
---

# Browser Rendering Pipeline and Frame Lifecycle

## 정의 (Definition)

**렌더링 파이프라인**은 브라우저가 HTML·CSS·JavaScript를 화면의 픽셀로 바꾸는 일련의 단계다. MDN: "The Critical Rendering Path is the sequence of steps the browser goes through to convert the HTML, CSS, and JavaScript into pixels on the screen." (*Critical rendering path*)

이 파이프라인은 페이지를 처음 열 때 한 번만 도는 것이 아니다. "This happens once initially, and then again and again for each frame update or user interaction that changes the page." ([[2026-Web Performance Engineering in the Age of AI]] p.91) 그래서 **프레임**(화면 한 장)마다 어떤 단계가 다시 실행되는지, 그 일이 **메인 스레드**에서 얼마나 걸리는지가 부드러운 화면의 핵심이 된다.

입문자용 설명은 [[렌더링 첫걸음 00 - 로드맵|렌더링 첫걸음 시리즈]]에서 볼 수 있다.

---

## 1. 렌더링 파이프라인의 단계

[[2026-Web Performance Engineering in the Age of AI]] 5장(p.87–89)은 Chrome 기준으로 여섯 단계로 정리한다.

| 단계 | 하는 일 | 결과물 |
|---|---|---|
| ① 내비게이션·리소스 가져오기 | DNS 조회, 연결(TCP/QUIC, TLS), HTTP 요청 | HTML 바이트 |
| ② HTML 파싱·DOM 구성 | 바이트를 읽어 노드 트리 생성. 점진적(incremental)으로 진행 | DOM 트리 |
| ③ CSS 파싱·스타일 계산 | CSSOM을 만들고 각 노드에 적용될 스타일을 계산 (DevTools의 "Recalculate Style") | 계산된 스타일 |
| ④ 레이아웃 (reflow) | 각 요소의 크기와 위치 계산 | 레이아웃 트리 (좌표가 붙은 트리) |
| ⑤ 페인트 | "draw a red rectangle here, then draw this text…" 같은 그리기 명령 생성 | 그리기 명령 목록 (display list) |
| ⑥ 합성 (compositing) | 레이어별로 래스터(픽셀화)한 결과를 합쳐 최종 화면 생성 | 프레임 비트맵 |

- 레이아웃 트리: [[2025-Web Browser Engineering]] "layout is about producing a layout tree, whose nodes are layout objects, each associated with an HTML element and each with a size and a position." (p.106)
- 렌더 트리에 들어가는 것: MDN "If there's a `display: none;` set on an element, neither it, nor any of its descendants, are in the render tree." (*Critical rendering path*)
- 레이아웃과 리플로우: MDN "The first time the size and position of each node is determined is called *layout*. Subsequent recalculations of *layout* are called *reflows*." (*How browsers work*)
- 합성: [[2025-Web Browser Engineering]] "caching some rastered images on the GPU and reusing them during later frames" (p.322)

### 1.1 렌더링을 막는 것

[[2026-Web Performance Engineering in the Age of AI]] p.89:
- `defer`/`async` 없는 `<script>`는 **HTML 파서를 멈춘다**. 스크립트가 `document.write`로 HTML을 더 쓸 수 있기 때문이다 (p.88).
- `<link rel="stylesheet">`는 그 아래 콘텐츠의 **렌더링을 막는다**. 스타일이 없는 화면(FOUC)을 보여 주지 않기 위해서다.
- 메인 스레드의 큰 JavaScript 실행(예: 큰 React 하이드레이션)은 입력 처리와 레이아웃·페인트 갱신을 늦춘다.

---

## 2. 프레임과 프레임 예산

### 2.1 60fps와 16.67 ms

- [[2025-Web Browser Engineering]]: "we'd like the browser to render the page exactly as fast as the display hardware can refresh. On most computers, this is 60 times per second, or 16 ms per frame." (p.284)
- MDN: "everything occupying the main thread, including calculating styles, along with reflow and paint, must take the browser less than 16.67ms to accomplish." (*How browsers work*)
- web.dev는 브라우저 자체의 일이 있으므로 개발자 코드는 더 짧게 잡으라고 한다: "all of your work needs to be completed inside 10 milliseconds." (*Rendering performance*)

이 시간을 넘기면 프레임을 놓쳐 화면이 끊긴다. MDN은 이것을 **jank**라고 부른다: "sluggishness in a user interface, usually caused by executing long tasks on the main thread, blocking rendering, or expending too much processor power on background processes." (*Jank*)

### 2.2 바뀐 속성에 따라 다시 도는 단계가 다르다

web.dev *Rendering performance*의 세 경로:

| 바뀐 것 | 다시 도는 단계 | 비용 |
|---|---|---|
| 요소의 크기·위치(기하)에 영향 | Style → **Layout** → Paint → Composite | 가장 큼 |
| 그려지는 모양만 영향 | Style → Paint → Composite | 중간 |
| 합성만 영향 (`transform`, `opacity` — [[2026-Web Performance Engineering in the Age of AI]] p.91) | Style → Composite | 가장 작음 (web.dev가 가장 싼 경로로 소개) |

`height` 애니메이션은 매 프레임 레이아웃을 일으킨다는 예도 같은 책에 있다 (p.91: "if you animate `height`, for example, that triggers layout every frame").

[[2026-Web Performance Engineering in the Age of AI]]도 같은 결론이다: "if you animate `transform` or `opacity`, those can often be done purely by the compositor on the GPU, without redoing the layout for each frame." (p.91) [[2025-Web Browser Engineering]] 13.6절은 투명도 애니메이션이 레이아웃과 래스터를 건너뛰도록 구현한다 (p.336).

### 2.3 레이아웃 스래싱 (layout thrashing)

"If JavaScript repeatedly forces layout calculations in a loop (for example, reading DOM size, then modifying DOM, then reading again), the browser may do multiple style and layout passes where one would suffice." ([[2026-Web Performance Engineering in the Age of AI]] p.95)

[[2025-Web Browser Engineering]]도 같은 문제를 구현 관점에서 보여 준다. 렌더링을 미루는 더티 비트 방식에서는 레이아웃 정보를 **읽어야 하는 순간**(예: 클릭 위치 판정) 미뤄 둔 렌더링을 즉시 해야 한다 (p.286). 12장 요약은 "forced style and layout makes it hard to fully isolate the rendering pipeline from JavaScript"라고 정리한다 (p.308).

---

## 3. 이벤트 루프와 렌더링

### 3.1 작업 하나가 끝나야 그릴 수 있다

- "JavaScript execution in the browser happens within the main thread's event loop. … it runs to completion before the browser can do other work on that thread, like rendering or handling input." ([[2026-Web Performance Engineering in the Age of AI]] p.93)
- 한 번의 이벤트 루프 순회: 작업 하나 실행 → 쌓인 마이크로태스크 전부 처리 → (필요하면) 렌더링 → 다음 작업 (같은 책 p.93: "the browser picks a task from the queue, runs it to completion, then processes all queued microtasks. Finally, it may render an update before starting the next tick with another task.")
- 마이크로태스크가 계속 마이크로태스크를 만들면 렌더링 차례가 오지 않아 화면이 멈춘다 (같은 책 p.93–94).

### 3.2 `requestAnimationFrame`

- MDN: "It requests the browser to call a user-supplied callback function before the next repaint." / "The frequency of calls to the callback function will generally match the display refresh rate." / "`requestAnimationFrame()` is one-shot." (*Window: requestAnimationFrame()*)
- [[2025-Web Browser Engineering]]: "This API allows scripts to run code right before the browser runs its rendering pipeline, making the animation maximally smooth." (p.288)
- `setTimeout`으로 애니메이션을 흉내 내면 렌더링 루프와 박자가 맞지 않아 불필요한 렌더링이 생길 수 있고, `requestAnimationFrame`은 탭이 백그라운드일 때 렌더링 작업을 멈출 수 있다 (같은 책 p.291). MDN도 백그라운드 탭에서 호출이 일시 중지된다고 설명한다.

---

## 4. 메인 스레드와 컴포지터 스레드

[[Web Browser Architecture (Chromium Blink)]] 참고.

| 스레드 | 하는 일 |
|---|---|
| **메인 스레드** | JavaScript 실행, HTML 파싱·DOM, 스타일·레이아웃 계산, 사용자 이벤트 처리 ([[2026-Web Performance Engineering in the Age of AI]] p.86) |
| **컴포지터 스레드** | 레이어 합성, 부드러운 스크롤. `transform`/`opacity` 애니메이션은 메인 스레드가 바빠도 여기서 계속 돈다 (같은 책 p.92) |
| **래스터 스레드** | 그리기 명령을 픽셀로 바꿈. 주로 GPU 사용 (같은 책 p.86, p.92) |

[[2025-Web Browser Engineering]]은 이 구조를 직접 구현한다. 메인 스레드는 스크립트·스타일·레이아웃·페인트를 하고 결과(display list)를 **commit**으로 브라우저 스레드에 넘기며, 브라우저 스레드는 래스터·그리기·스크롤·입력 전달을 한다. 저자는 실제 브라우저에서 이에 해당하는 스레드가 "often called the compositor thread"라고 각주로 밝힌다 (p.294, p.308).

> **React와의 연결**: React의 "커밋(commit)"은 React가 계산한 변경을 DOM에 반영하는 단계이고, 위의 브라우저 내부 commit(메인 스레드 → 컴포지터 스레드)과는 다른 개념이다. React 커밋이 끝난 뒤 브라우저가 스타일·레이아웃·페인트를 하고, 그 페인트 전후로 `useLayoutEffect`와 `useEffect`가 실행된다. 자세한 순서는 [[React Component Lifecycle and Effects]] 참고.

---

## 5. 설계 트레이드오프 및 관점 비교

| 주제 | [[2025-Web Browser Engineering]] | [[2026-Web Performance Engineering in the Age of AI]] / 공식 문서 |
|---|---|---|
| 프레임 목표 | 실제 목표는 60fps(16 ms)라고 설명하지만, 책의 장난감 브라우저는 단순화를 위해 30fps(33 ms)를 목표로 구현함 (p.284) | MDN 16.67 ms, web.dev 개발자 작업 10 ms 이내 |
| 단계 구분 | render(스타일·레이아웃·페인트) → raster → draw로 구현 (p.284) | 6단계(내비게이션–합성) (p.87–89), web.dev는 JavaScript·Style·Layout·Paint·Composite 5단계 |
| 합성 레이어 | 레이어 수를 최적화해야 한다 (p.351) | 레이어를 강제로 만들면 고립에 도움이 되지만 "overusing layers can take a toll on memory" (p.92) |

---

## 관련 개념 및 엔티티

- **입문 레슨**: [[렌더링 첫걸음 00 - 로드맵]]
- **상위/연관 개념**: [[JavaScript Event Loop and Asynchronous Patterns]], [[Web Performance Metrics and Core Web Vitals]], [[React Component Lifecycle and Effects]], [[React Fiber and Reconciliation]], [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]]
- **구현 기술/엔티티**: [[Web Browser Architecture (Chromium Blink)]], [[React]]
- **출처 문헌**: [[2025-Web Browser Engineering]] (Ch 5, 12, 13, 16), [[2026-Web Performance Engineering in the Age of AI]] (Ch 5), MDN [*Critical rendering path*](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path), MDN [*How browsers work*](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/How_browsers_work), MDN [*requestAnimationFrame()*](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame), MDN [*Jank*](https://developer.mozilla.org/en-US/docs/Glossary/Jank), web.dev [*Rendering performance*](https://web.dev/articles/rendering-performance)
