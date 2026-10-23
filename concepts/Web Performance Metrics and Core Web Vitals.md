---
type: concept
title: "Web Performance Metrics and Core Web Vitals"
category: "H. Information Systems/Web Infrastructure"
sources:
  - "[[2026-Web Performance Engineering in the Age of AI]]"
  - "[[2025-Full Stack JavaScript Strategies]]"
tags:
  - web-performance
  - core-web-vitals
  - lcp
  - inp
  - cls
  - tbt
  - long-tasks
  - rum
created: 2026-10-21
updated: 2026-10-23
---

# Web Performance Metrics and Core Web Vitals

## 정의 (Definition)

**Core Web Vitals**(CWV)는 Google이 정한 사용자 경험 중심의 웹 성능 지표 세트다. 현재 안정 지표는 로딩(LCP), 상호작용(INP), 시각적 안정성(CLS) 세 가지다 (web.dev *Web Vitals*).

[[2026-Web Performance Engineering in the Age of AI]]의 저자는 이 지표를 정의한 Chrome 팀의 일원으로, 세 지표가 "weren't chosen arbitrarily; they correspond to real improvements in user behavior and satisfaction"이라고 설명한다 (p.32). 또 이 지표들은 실험실이 아닌 **실제 사용자 브라우저에서 측정할 수 있다**는 점이 중요하다고 강조한다 (p.32).

---

## 1. 세 가지 핵심 지표

| 지표 | 무엇을 재나 | "좋음" 기준 (web.dev) | 책의 설명 |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | 로딩: 화면에서 가장 큰 콘텐츠가 그려진 시점 | "within 2.5 seconds of when the page first starts loading" | 사용자가 기다리는 핵심 콘텐츠가 보인 순간. 작은 헤더만 떠도 잡히는 FCP와 다름 (p.33) |
| **INP** (Interaction to Next Paint) | 상호작용: 클릭·탭·키 입력부터 **다음 프레임이 그려질 때까지** | "200 milliseconds or less" | 첫 입력만 재던 FID를 대체. 모든 상호작용의 반응성을 봄 (p.34) |
| **CLS** (Cumulative Layout Shift) | 시각적 안정성: 예상치 못한 레이아웃 이동의 누적 점수 | "0.1. or less" (원문 그대로) | 크기를 모르는 이미지·나중에 끼어드는 콘텐츠가 주범 (p.35, p.68–69) |

- 측정 기준: "a good threshold to measure is the 75th percentile of page loads, segmented across mobile and desktop devices." (web.dev)
- INP의 FID 대체: "INP became a stable Core Web Vital metric in 2024" (web.dev)

### 1.1 렌더링 파이프라인과의 연결

| 지표 | 파이프라인에서 주로 영향을 주는 곳 ([[Browser Rendering Pipeline and Frame Lifecycle]]) |
|---|---|
| LCP | 네트워크·렌더링 차단 리소스(CSS, 동기 스크립트)·큰 이미지 (책 p.89–90, p.95) |
| INP | **메인 스레드를 오래 붙잡는 작업**. 이벤트 처리기 → 상태 갱신 → 렌더링 → 페인트까지가 모두 INP 시간에 들어감 (판단: INP 정의 "until the next frame is painted"에서) |
| CLS | 레이아웃 단계에서 자리를 미리 잡지 않은 요소 (p.96) |

---

## 2. 보조 지표

| 지표 | 정의 (책 p.35) |
|---|---|
| **TBT** (Total Blocking Time) | "A lab-only metric summing how long tasks >50 ms run between FCP and TTI." 메인 스레드가 얼마나 바쁜지의 대리 지표 |
| **TTI** (Time to Interactive) | 페이지가 안정적으로 반응할 만큼 한가해지는 시점. TTI가 LCP보다 한참 늦으면 "보이지만 못 누르는" 상태가 김 |
| **Speed Index** | 화면이 시각적으로 얼마나 빨리 채워지는지 |

**긴 작업(long task)**: web.dev "Any task that takes longer than 50 milliseconds is a long task." 해결책은 작업을 나누고 메인 스레드에 **양보(yield)** 하는 것이다: "When you yield to the main thread, you're giving it an opportunity to handle more important tasks" / "`scheduler.yield()` is an API specifically designed for yielding to the main thread in the browser." (web.dev *Optimize long tasks*) 책도 같은 이유로 양보를 강조한다 (p.98).

---

## 3. 실험실 측정과 현장 측정

| | 실험실 (Lab) | 현장 (Field) |
|---|---|---|
| 도구 | Lighthouse, DevTools Performance 패널, WebPageTest | CrUX(Chrome 사용자 경험 보고서), Web Vitals 라이브러리 등 RUM |
| 장점 | 조건이 일정해 원인 진단에 좋음 | 실제 사용자의 기기·네트워크를 반영 |
| 한계 | 실제 사용자보다 좋은 기기를 가정할 수 있음 | 원인을 짚기 어려움 |

책: "Savvy performance engineers use lab tools for diagnostics and field tools for validation." (p.32) 실험실 LCP가 2.0초여도 현장에서는 느린 휴대폰 때문에 4초일 수 있다는 예를 든다 (p.32).

---

## 4. React 코드에서 자주 나오는 지표 문제

[[2026-Web Performance Engineering in the Age of AI]] Ch 4는 AI가 생성한 React 코드를 예로 들지만, 사람이 쓴 코드에도 그대로 해당한다 (판단).

| 지표 | 흔한 원인 (책) | 대응 (책) |
|---|---|---|
| CLS | `width`/`height` 없는 `<img>`; `useEffect`에서 데이터를 받아 목록을 한꺼번에 렌더링 (p.68–69) | 이미지 크기·`aspect-ratio`, 컨테이너 `min-height`, 스켈레톤 화면 (p.70) |
| LCP | 작은 기능에 무거운 라이브러리 전체를 가져옴; 비필수 작업을 첫 렌더링에 실행 (p.70) | 코드 분할(`React.lazy`, 동적 `import()`), 상호작용·가시성 기반 지연 로딩 (p.70–72) |
| INP | 입력할 때마다 큰 목록을 렌더 경로에서 필터링·정렬 (p.72) | 디바운스, 웹 워커, `useMemo`·`React.memo`, 작업 나누기 (p.72–73) |

---

## 5. React Native와의 관계

Core Web Vitals는 **웹 브라우저** 지표다. [[React Native]] 앱은 브라우저 렌더링 파이프라인을 쓰지 않으므로 CWV를 그대로 적용하지 않는다 (판단: [[React Native Architecture (Bridge to New Architecture)]]). 다만 "메인 스레드를 오래 붙잡으면 입력 반응이 늦어진다"는 원리는 같다. React Native의 렌더링과 스레드는 [[React Native Rendering and App Lifecycle]] 참고.

React Native 공식 문서는 같은 프레임 예산("at most 16.67ms")을 쓰되, **JS 프레임 레이트**와 **UI 프레임 레이트**를 따로 본다. 예: 무거운 다시 렌더가 200 ms 걸리면 "result in 12 frames being dropped", 반면 JS 스레드가 막혀도 `ScrollView`는 "lives on the main thread"라서 스크롤된다 (reactnative.dev *Performance Overview*). 입문 설명: [[렌더링 첫걸음 07 - React Native는 화면을 어떻게 그릴까]] 4절.

### 5.1 INP의 세 구간

web.dev: "the input delay the time before any callback for an interaction is handled, the processing duration the time for all the callbacks to execute, and the presentation delay the time after the callbacks have been executed until the frame is presented on the user's screen." (*Interaction to Next Paint (INP)*) React의 렌더·커밋은 처리기가 끝난 뒤 실행되므로 주로 표시 지연에 들어간다 (판단: react.dev "React processes state updates after event handlers have finished running."과 [[렌더링 첫걸음 09 - 버튼 하나를 누르면]] 2.1절 실험).

---

## 6. 관점 비교 / 출처별 차이

| 주제 | [[2025-Full Stack JavaScript Strategies]] (Ch 20) | 현재 기준 |
|---|---|---|
| 상호작용 지표 | "Some of the main metrics to start with are LCP, first input delay (FID), and cumulative layout shift (CLS)." FID 기준 "100 ms or less", 300 ms 넘으면 개선 필요 (p.262) | web.dev (2024-01-31): "INP will officially become a Core Web Vital and replace FID on March 12 of this year, and that FID will be deprecated in this transition." 지금은 1절의 INP(200 ms 이하) |
| LCP·CLS 기준 | LCP 2.5초 이내(4초 넘으면 나쁨), CLS 0.1 미만(0.25 넘으면 개선 필요) (p.262) | 1절의 web.dev "좋음" 기준과 거의 같음 (CLS를 책은 "less than 0.1", web.dev는 "0.1. or less"로 씀) |
| 측정 환경 | 로컬 Lighthouse 결과는 압축·최소화가 안 된 개발 빌드라 운영과 크게 다를 수 있으니, 로컬에서 **운영 빌드**를 만들어 측정하라 (p.263) | 3절의 실험실·현장 구분과 같은 취지 (판단) |
| CI 자동화 | Lighthouse npm 패키지로 JSON 보고서를 만들고, 기준을 넘으면 파이프라인을 실패시킴 (p.265–266). sitespeed.io로 시간에 따른 추이를 봄 (p.266) | — |

책의 개선 항목(번들 분석, 캐시, `lazy`·`Suspense` 스켈레톤, 프리페치, 이미지·폰트)은 4절 표와 겹친다. 스켈레톤으로 CLS를 줄이는 이유: "Remember to keep at least the height and width of the loading component about the same as the real component." (p.273)

---

## 관련 개념 및 엔티티

- **입문 레슨**: [[렌더링 첫걸음 00 - 로드맵]]
- **상위/연관 개념**: [[Browser Rendering Pipeline and Frame Lifecycle]], [[JavaScript Event Loop and Asynchronous Patterns]], [[React Component Lifecycle and Effects]], [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]]
- **구현 기술/엔티티**: [[Web Browser Architecture (Chromium Blink)]], [[React]]
- **출처 문헌**: [[2026-Web Performance Engineering in the Age of AI]] (Ch 2, 4, 5), [[2025-Full Stack JavaScript Strategies]] (Ch 20), web.dev [*Interaction to Next Paint becomes a Core Web Vital on March 12*](https://web.dev/blog/inp-cwv-march-12), web.dev [*Web Vitals*](https://web.dev/articles/vitals), web.dev [*Optimize long tasks*](https://web.dev/articles/optimize-long-tasks), web.dev [*Interaction to Next Paint (INP)*](https://web.dev/articles/inp), reactnative.dev [*Performance Overview*](https://reactnative.dev/docs/performance)
