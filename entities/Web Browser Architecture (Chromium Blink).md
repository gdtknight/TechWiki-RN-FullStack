---
type: entity
title: "Web Browser Architecture (Chromium Blink)"
entity_type: "system"
category: "H. Information Systems/Web Infrastructure"
sources:
  - "[[2026-Web Performance Engineering in the Age of AI]]"
  - "[[2025-Web Browser Engineering]]"
tags:
  - browser
  - chromium
  - blink
  - renderer-process
  - main-thread
  - compositor-thread
  - task-scheduler
created: 2026-10-21
updated: 2026-10-21
---

# Web Browser Architecture (Chromium Blink)

## 개요

웹 브라우저는 HTML·CSS·JavaScript를 받아 화면을 그리고 사용자 입력에 반응하는 **프로그램이자 실행 환경**이다. 이 페이지는 [[2026-Web Performance Engineering in the Age of AI]] 5장이 사례로 삼는 **Chrome(Chromium, 렌더링 엔진 Blink)** 의 구조를 정리한다. 저자는 Chrome을 고른 이유로 시장 점유율과 문서화된 구조를 들며, "the fundamental concepts apply broadly across modern browsers—though the specific implementation details may vary"라고 덧붙인다 (p.85).

[[React]]가 웹에서 화면을 바꿀 때 실제로 픽셀을 그리는 것은 이 브라우저다. React는 DOM을 바꾸고, 그 뒤의 스타일·레이아웃·페인트·합성은 브라우저의 몫이다 ([[Browser Rendering Pipeline and Frame Lifecycle]]).

---

## 아키텍처 및 특징

### 1. 멀티 프로세스

"Chrome and other modern browsers use a multiprocess architecture for stability and security." (p.86) 한 사이트가 오류로 죽어도 그 탭만 영향을 받는다. 새 사이트로 이동하면 보통 새 **렌더러 프로세스**가 만들어지고, Blink는 이 렌더러 프로세스 안에서 돈다 (p.86).

네트워크는 별도의 **Network Service** 프로세스에서 처리되고, 결과는 렌더러에 비동기로 전달된다 (p.86, p.101).

### 2. 렌더러 프로세스 안의 스레드

| 스레드 | 역할 (p.86, p.92) |
|---|---|
| **메인 스레드** | JavaScript 실행, HTML 파싱·DOM, 스타일·레이아웃 계산, 사용자 이벤트 처리. "The main thread is the orchestrator of the browser's activity." |
| **컴포지터 스레드** | 그래픽 레이어 처리와 부드러운 스크롤. `transform`/`opacity` 애니메이션은 메인 스레드가 바빠도 여기서 계속 돈다 |
| **래스터 스레드** | 그리기 명령을 픽셀로 바꿈. 큰 레이어는 타일로 나눠 병렬 래스터, 주로 GPU와 Skia 사용 |
| **워커 스레드** | 웹 워커·서비스 워커 |

메인 스레드가 병목이 되는 이유: "if it's busy running a large script, it can't simultaneously respond to user input or render updates. This is why many performance techniques focus on moving work off the main thread." (p.86)

[[2025-Web Browser Engineering]]은 같은 구조를 단순화해 직접 구현한다. 메인 스레드(스크립트·스타일·레이아웃·페인트)와 브라우저 스레드(래스터·그리기·스크롤·입력)를 나누고 **commit**으로 표시 목록을 넘긴다. 저자는 이 브라우저 스레드가 실제 브라우저에서는 "often called the compositor thread"라고 설명한다 (p.294).

### 3. 메인 스레드 작업 스케줄러 (Blink scheduler)

- 작업 큐를 여러 개 두고 우선순위를 매긴다. 입력 처리와 애니메이션(`requestAnimationFrame`)은 높음, `setTimeout`·네트워크 콜백은 보통, `requestIdleCallback`·background 작업은 낮음 (p.96–98).
- 실행 중인 작업은 끊지 못한다: "Chrome can't truly preempt the JavaScript: it's single-threaded and can't be interrupted except at awaits or yields." (p.98) 그래서 긴 작업을 나눠 **양보**하는 것이 중요하다.
- 개발자용 API: `scheduler.postTask(fn, { priority: 'user-blocking' | 'user-visible' | 'background' })`, `scheduler.yield()` (p.98).

### 4. RenderingNG

Chrome의 레이아웃·페인트·합성을 재설계한 프로젝트(2021–2022 출시). 레이아웃 모듈 LayoutNG는 바뀐 하위 트리로 레이아웃 범위를 좁히려 하고, 부분 페인트로 바뀐 영역만 다시 그린다 (p.91–92).

---

## 대안 기술과의 비교

| 항목 | 웹 브라우저 (Chromium) | [[React Native]] 앱 |
|---|---|---|
| 화면을 그리는 것 | Blink: DOM → 스타일 → 레이아웃 → 페인트 → 합성 | 각 OS의 네이티브 뷰 (Fabric 렌더러, Yoga 레이아웃) |
| JavaScript가 도는 곳 | 렌더러 프로세스의 메인 스레드 | JS 스레드 (Hermes) |
| 레이아웃 계산 | Blink 레이아웃 (메인 스레드) | Yoga (C++) |
| 더 보기 | [[Browser Rendering Pipeline and Frame Lifecycle]] | [[React Native Rendering and App Lifecycle]], [[React Native Architecture (Bridge to New Architecture)]] |

---

## 관련 링크

- **연관 개념**: [[Browser Rendering Pipeline and Frame Lifecycle]], [[JavaScript Event Loop and Asynchronous Patterns]], [[Web Performance Metrics and Core Web Vitals]], [[Web React vs React Native Shared Core and Differences]]
- **연관 엔티티**: [[JavaScript (ECMAScript)]], [[React]], [[React Native]]
- **출처 문헌**: [[2026-Web Performance Engineering in the Age of AI]] (Ch 5, p.85–108), [[2025-Web Browser Engineering]] (Ch 12–13)
