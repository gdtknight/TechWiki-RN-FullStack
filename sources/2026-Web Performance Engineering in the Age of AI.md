---
type: source
source_type: book
title: "Web Performance Engineering in the Age of AI"
authors:
  - "Addy Osmani"
publisher: "O'Reilly Media"
year: 2026
category: "H. Information Systems/Web Infrastructure"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/Web Infrastructure/2026-Web Performance Engineering in the Age of AI-Addy Osmani-O'Reilly Media, Incorporated.pdf"
status: partial
tags:
  - web-performance
  - core-web-vitals
  - lcp
  - inp
  - cls
  - rendering-pipeline
  - main-thread
  - hydration
  - ai-generated-code
sources: ["[[2026-Web Performance Engineering in the Age of AI]]"]
created: 2026-10-21
updated: 2026-10-21
---

# Web Performance Engineering in the Age of AI (2026)

> **서지 정보**: Addy Osmani | O'Reilly Media (2026) | 256 Pages
> **분류**: `H. Information Systems / Web Infrastructure`
> **원천 PDF**: [2026-Web Performance Engineering in the Age of AI-Addy Osmani-O'Reilly Media, Incorporated.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/Web%20Infrastructure/2026-Web%20Performance%20Engineering%20in%20the%20Age%20of%20AI-Addy%20Osmani-O'Reilly%20Media,%20Incorporated.pdf)
> **쪽 표기**: 이 페이지의 `p.XX`는 **PDF 쪽 번호**입니다. 인쇄 쪽과 다릅니다 (예: PDF p.85 = 인쇄 67쪽).
> **투입 범위 (`status: partial`)**: 2장 CWV 절(p.32–35), 4장 앞부분(p.67–74), **5장 전체(p.85–108)**, 7장 앞부분(p.119–123)을 정독함. 나머지 장은 첫 쪽과 목차로 요약함.

---

## 핵심 요약 (Core Thesis)

Chrome 팀에서 Core Web Vitals를 이끈 저자(p.21)가 쓴 웹 성능 실무서다. 성능은 곧 사용자 경험이라는 관점에서, **측정(Core Web Vitals) → 브라우저 내부 이해 → JavaScript 비용 줄이기 → 서드파티 스크립트 관리 → 팀 문화**로 이어진다.

책 전체를 관통하는 문제의식은 **AI가 생성한 코드**다. "These tools currently optimize for correctness (satisfying the prompt's intent) and perhaps style—not for performance." (p.55) AI가 만든 React 컴포넌트에서 자주 생기는 레이아웃 흔들림, 번들 비대, 메인 스레드 막힘을 Core Web Vitals로 진단하고 고치는 법을 보여 준다 (Ch 4).

이 위키에서는 주로 **5장 "Inside the Browser: How Pages Load and Render"** 를 [[Browser Rendering Pipeline and Frame Lifecycle|브라우저 렌더링 파이프라인]]의 근거로, 2장을 [[Web Performance Metrics and Core Web Vitals|성능 지표]]의 근거로 쓴다.

---

## 챕터별 핵심 내용 요약

### Part I. Performance Is User Experience (Ch 1–3)
- **Ch 1. Why Performance Is User Experience (p.21–30)**: 느린 사이트는 사용자 경험의 장애물이다. 다양한 기기·네트워크 조건, 성능 우선 설계.
- **Ch 2. Measuring What Matters (p.31–53)** — CWV 절 정독
  - **실험실 측정**(Lighthouse)과 **현장 측정**(CrUX, RUM)의 차이. "Field data is the true measure of what users experience" (p.32).
  - **LCP**(가장 큰 콘텐츠가 그려지는 시점): 목표 2.5초 미만, 사용자 75번째 백분위 기준 (p.33).
  - **INP**(상호작용부터 다음 프레임이 그려질 때까지): FID를 대체. 75번째 백분위의 상호작용을 200 ms 미만으로 (p.34).
  - **CLS**(예상치 못한 레이아웃 이동): 페이지 로드의 75%에서 0.1 미만 (p.35).
  - 보조 지표 **TBT**: "A lab-only metric summing how long tasks >50 ms run between FCP and TTI." (p.35)
- **Ch 3. AI-Generated Code and the Performance Paradox (p.55–64)**: AI 코드는 정확성에 맞춰져 있고 성능에는 맞춰져 있지 않다 (p.55).

### Part II. Optimizing Web Performance in the Age of AI (Ch 4–6)
- **Ch 4. Optimizing AI-Generated Frontends (p.67–84)** — 앞부분 정독
  - AI가 만든 React 코드의 **CLS 문제**: 크기를 지정하지 않은 `<img>`, `useEffect`에서 데이터를 받아 목록을 한꺼번에 넣어 아래 내용을 밀어내는 패턴 (p.68–69). 해결은 이미지 크기·`min-height`·스켈레톤으로 자리를 미리 잡는 것 (p.70).
  - **LCP 문제**: 작은 기능에 무거운 라이브러리를 통째로 가져오는 번들 비대. 코드 분할(`React.lazy`, 동적 `import()`) (p.70–72).
  - **INP 문제**: "LLMs do not understand the concept of the browser's main thread or the event loop" (p.72). 입력할 때마다 큰 목록을 렌더 경로에서 필터링하는 예. 디바운스, 웹 워커, `useMemo`·`React.memo` (p.72–73).
- **Ch 5. Inside the Browser: How Pages Load and Render (p.85–108)** — 전체 정독
  - **멀티 프로세스 구조**: 페이지마다 렌더러 프로세스, 그 안에 메인 스레드(JavaScript, DOM, 스타일, 레이아웃, 이벤트)·컴포지터 스레드·래스터 스레드·워커 스레드 (p.86).
  - **렌더링 파이프라인 6단계**: 내비게이션과 리소스 가져오기 → HTML 파싱과 DOM 구성 → CSS 파싱과 스타일 계산 → 레이아웃(reflow) → 페인트 → 합성 (p.87–89).
  - 렌더링을 막는 것: `defer`/`async` 없는 `<script>`는 파서를 막고, `<link rel="stylesheet">`는 아래 콘텐츠의 렌더링을 막는다 (p.89).
  - 첫 로드 뒤에도 DOM·스타일이 바뀔 때마다 파이프라인 일부가 다시 돈다. `transform`·`opacity` 애니메이션은 컴포지터에서 처리될 수 있어 레이아웃을 다시 하지 않는다 (p.91).
  - **스크립트와 이벤트 루프**: 작업은 끝까지 실행되며(run-to-completion), 각 작업 뒤에 마이크로태스크를 모두 처리하고, 그다음에 렌더링이 일어날 수 있다. 마이크로태스크가 계속 새 마이크로태스크를 만들면 화면이 멈춘다 (p.93–94).
  - **렌더링 병목**: 네트워크, 파싱·스크립트 실행, 레이아웃·페인트 복잡도, **레이아웃 스래싱**(읽기·쓰기를 반복해 레이아웃을 여러 번 강제), 예약되지 않은 백그라운드 작업, 시각적 안정성 (p.95–96).
  - **메인 스레드 스케줄러**: 입력·애니메이션은 높은 우선순위, `setTimeout`·네트워크 콜백은 보통, idle 콜백은 낮은 우선순위. 실행 중인 작업을 중간에 끊지는 못하므로 **양보(yield)** 가 중요하다. `scheduler.postTask`, `scheduler.yield()` (p.96–98).
  - **SSR과 하이드레이션 비용**: 서버 렌더링은 화면을 빨리 보여 주지만, 하이드레이션 동안 페이지가 입력에 반응하지 못할 수 있다. 부분 하이드레이션(islands), 점진적 하이드레이션, 재개 가능성(Qwik), React 18의 선택적 하이드레이션 (p.98–101).
  - 네트워크 스택(HTTP/2·3, Early Hints, 우선순위), 최적화된 로드의 타임라인 예 (p.101–108).
- **Ch 6. Trade-Offs in Performance Optimizations (p.109–116)**: 한 지표를 개선하면 다른 지표가 나빠질 수 있다. 성능 예산, 언제 최적화를 멈출지.

### Part III. Optimizing JavaScript (Ch 7)
- **Ch 7. The Cost of JavaScript (p.119–131)** — 앞부분 정독
  - JavaScript는 파싱·실행이 CPU에 묶여 있어 저가 기기에서 몇 배 느리다 (p.120).
  - 모바일 네트워크는 특히 지연 시간이 병목이다 (p.121).
  - "Only one JavaScript task runs at a time. If a task doesn't yield, nothing else can run." (p.123)

### Part IV–V. 서드파티 스크립트, 성능 문화, 사례, AI 시대의 미래 (Ch 8–13)
- **Ch 8–10 (p.135–188)**: 서드파티 스크립트의 비용, `async`/`defer`·지연 로딩·파사드 패턴, `requestIdleCallback`을 이용한 예약.
- **Ch 11 (p.191–196)**: 성능을 기능으로 다루는 팀 문화, CI/CD에 성능 검사 넣기.
- **Ch 12 (p.197–221)**: 이커머스·미디어·여행·금융 분야의 성능 개선 사례.
- **Ch 13 (p.223–246)**: 스트리밍 AI 인터페이스와 성능.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Browser Rendering Pipeline and Frame Lifecycle]] — 렌더링 파이프라인 6단계, 렌더링 차단, 컴포지터, 레이아웃 스래싱 (Ch 5)
- [[Web Performance Metrics and Core Web Vitals]] — LCP, INP, CLS, TBT, 실험실과 현장 측정 (Ch 2, 4)
- [[JavaScript Event Loop and Asynchronous Patterns]] — 마이크로태스크 기아, 작업 우선순위, `scheduler.postTask` (Ch 5)
- [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]] — 하이드레이션 비용과 대안 (Ch 5)

### Entities
- [[Web Browser Architecture (Chromium Blink)]] — 렌더러 프로세스와 스레드 구성 (Ch 5)
