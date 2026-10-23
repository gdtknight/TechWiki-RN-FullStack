---
type: source
source_type: book
title: "Web Browser Engineering"
authors:
  - "Pavel Panchekha"
  - "Chris Harrelson"
publisher: "Oxford University Press"
year: 2025
category: "H. Information Systems/Full Stack"
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/Full Stack/2025-Web Browser Engineering-Pavel Panchekha,Chris Harrelson-Oxford University Press, USA.pdf"
status: partial
tags:
  - browser
  - rendering-pipeline
  - layout
  - paint
  - compositing
  - event-loop
  - requestanimationframe
  - threads
sources: ["[[2025-Web Browser Engineering]]"]
created: 2026-10-21
updated: 2026-10-21
---

# Web Browser Engineering (2025)

> **서지 정보**: Pavel Panchekha, Chris Harrelson | Oxford University Press (2025) | 529 Pages
> **분류**: `H. Information Systems / Full Stack`
> **원천 PDF**: [2025-Web Browser Engineering-Pavel Panchekha,Chris Harrelson-Oxford University Press, USA.pdf](file:///Users/yoshin/Documents/MarginNote4/H.%20Information%20Systems/Full%20Stack/2025-Web%20Browser%20Engineering-Pavel%20Panchekha,Chris%20Harrelson-Oxford%20University%20Press,%20USA.pdf)
> **쪽 표기**: 이 페이지의 `p.XX`는 PDF 쪽 번호입니다.
> **투입 범위 (`status: partial`)**: 전 16장의 도입부와 요약 절을 읽어 장별 요약을 작성함. 12장(12.1–12.9, p.276–300)과 13장(13.1–13.3, 13.6, p.316–324, p.336–337)은 본문까지 정독함.

---

## 핵심 요약 (Core Thesis)

Python으로 **작은 웹 브라우저를 처음부터 직접 만들면서** 브라우저의 내부 동작을 배우는 책이다. 1장의 URL 다운로드에서 시작해 화면 그리기, 텍스트 배치, HTML 트리 파싱, 레이아웃 트리, CSS 적용, 링크·폼, JavaScript 실행, 쿠키 보안까지 차례로 구현하고, 5부에서는 현대 브라우저가 빠르고 부드럽게 동작하는 비결인 **시각 효과·작업 스케줄링과 스레드·애니메이션과 합성(Compositing)·접근성·임베디드 콘텐츠·증분 계산(invalidation)** 을 다룬다.

공저자 Chris Harrelson은 Google에서 일하는 브라우저 엔지니어라고 서문에서 밝힌다 (p.16). 이 위키에서는 주로 **[[Browser Rendering Pipeline and Frame Lifecycle|브라우저 렌더링 파이프라인]]** 과 **[[JavaScript Event Loop and Asynchronous Patterns|이벤트 루프]]가 화면 그리기와 맞물리는 방식**의 근거로 쓴다.

---

## 챕터별 핵심 내용 요약

### Part 1–2: 페이지 불러오기 (Introduction, Ch 1–3)
- **i–ii. Browsers and the Web / History of the Web (p.16–35)**: 저자의 브라우저 경험과, 1945년 Vannevar Bush의 memex 구상부터 이어지는 웹의 역사.
- **Ch 1. Downloading Web Pages (p.36–53)**: URL을 scheme·host·path로 나누고, 소켓으로 서버에 연결해 HTTP 요청을 보내고 응답을 상태 줄·헤더·본문으로 나눈다 (1.8 Summary, p.49).
- **Ch 2. Drawing to the Screen (p.54–67)**: 운영체제에 창을 요청하고, 텍스트를 배치해 그리고, 키보드 입력에 반응해 스크롤한다 (2.6 Summary, p.65). 그래픽 툴킷의 `mainloop`가 이벤트를 받아 처리하는 이벤트 루프의 출발점이다 (p.54).
- **Ch 3. Formatting Text (p.68–87)**: 단어 단위 배치, 줄 바꿈, 굵게·기울임, 크기가 다른 텍스트 (3.8 Summary, p.83).

### Part 3: 문서 보기 (Ch 4–7)
- **Ch 4. Constructing an HTML Tree (p.88–105)**: 토큰을 트리로 바꾸는 파서. 저자는 이 트리가 "usually called the DOM tree"라고 각주로 밝힌다 (p.88). 잘못된 HTML의 자동 보정, 트리를 재귀적으로 배치하는 레이아웃 (4.7 Summary, p.101).
- **Ch 5. Laying Out Pages (p.106–121)**: HTML 트리를 **레이아웃 트리**로 바꾼 뒤 각 객체의 크기와 위치를 계산하고 그린다. "In a browser, layout is about producing a layout tree, whose nodes are layout objects, each associated with an HTML element and each with a size and a position." (p.106)
- **Ch 6. Applying Author Styles (p.122–141)**: CSS 파서, `style` 속성과 외부 CSS 파일, 캐스케이드와 상속 (6.8 Summary, p.137).
- **Ch 7. Handling Buttons and Links (p.142–169)**: 단어마다 위치를 기록해 클릭된 링크를 찾고, 탭·주소창·뒤로 가기 버튼을 그린다 (7.8 Summary, p.163).

### Part 4: 애플리케이션 실행 (Ch 8–10)
- **Ch 8. Sending Information to Servers (p.170–191)**: 입력창·버튼 레이아웃, 포커스 처리, HTTP POST 폼 전송 (8.8 Summary, p.187).
- **Ch 9. Running Interactive Scripts (p.192–215)**: DukPy(Duktape 래퍼)로 JavaScript를 실행하고, 페이지 요소 핸들, 속성 읽기, DOM 수정, 이벤트 리스너를 구현한다 (9.10 Summary, p.210).
- **Ch 10. Keeping Data Private (p.216–241)**: 쿠키, 동일 출처 정책, CSRF 대응(nonce, SameSite), XSS 대응(이스케이프, Content-Security-Policy) (10.10 Summary, p.234).

### Part 5: 현대 브라우저 (Ch 11–16) ⭐ 이 위키의 주 근거
- **Ch 11. Adding Visual Effects (p.242–275)**: Skia·SDL로 바꾸고, 빠른 스크롤을 위한 별도 surface(합성), 투명도, `mix-blend-mode`, 둥근 모서리 클리핑 (11.11 Summary, p.269).
- **Ch 12. Scheduling Tasks and Threads (p.276–315)** — 정독
  - 브라우저의 모든 일(페이지 로드, 스크립트 실행, 사용자 입력 응답)을 **작업(task)** 으로 만들어 큐에서 하나씩 실행한다 (12.1, p.276).
  - `setTimeout`은 타이머가 끝나면 콜백을 **작업 큐에 넣고**, JavaScript는 메인 스레드에서만 실행된다 (12.2, p.279). 각주: "JavaScript is not a multithreaded programming language." (p.279)
  - **렌더링 주기**: "we'd like the browser to render the page exactly as fast as the display hardware can refresh. On most computers, this is 60 times per second, or 16 ms per frame." 책의 장난감 브라우저는 30fps(33 ms)를 목표로 한다 (12.4, p.284).
  - **더티 비트(dirty bit)**: 바뀐 것이 없으면 다시 그리지 않는다 (12.5, p.285–288).
  - **`requestAnimationFrame`**: "This API allows scripts to run code right before the browser runs its rendering pipeline, making the animation maximally smooth." (12.6, p.288)
  - `setTimeout`으로 애니메이션을 흉내 내던 방식의 문제와 `requestAnimationFrame`의 장점 (12.6 Go Further, p.291).
  - **두 스레드**: 스크립트 실행·스타일·레이아웃·페인트를 하는 **메인 스레드**와, 래스터·그리기·스크롤·입력 처리를 하는 **브라우저 스레드**(실제 브라우저의 컴포지터 스레드에 해당). 둘은 **commit**으로 표시 목록(display list)을 넘긴다 (12.8–12.9, p.294–300; 12.13 Summary, p.308).
- **Ch 13. Animating and Compositing (p.316–359)** — 핵심 절 정독
  - GPU로 래스터·그리기를 옮기면 훨씬 빨라진다 (13.2, p.318–321).
  - **합성(compositing)**: "caching some rastered images on the GPU and reusing them during later frames" (13.3, p.322). 투명도처럼 자주 바뀌는 시각 효과는 그리기 단계에 두고, 나머지 그림은 합성 레이어로 캐시한다 (p.323–324).
  - **합성 애니메이션**: 투명도 애니메이션은 레이아웃과 래스터를 건너뛰고 일부 단계만 다시 실행한다 (13.6, p.336).
  - 요약: "GPU acceleration is necessary for smooth animations." / "It's important to optimize the number of composited layers." (13.9, p.351)
- **Ch 14. Making Content Accessible (p.360–407)**: 접근성 트리, 다크 모드, 키보드 탐색, 아웃라인 (14.11 Summary, p.399).
- **Ch 15. Supporting Embedded Content (p.408–455)**: 이미지 다운로드·디코드·레이아웃·페인트, iframe (15.10 Summary, p.444).
- **Ch 16. Reusing Previous Computations (p.456–509)**: 레이아웃 트리를 캐시로 보고 **바뀐 부분만 다시 계산**한다. "the cost of a change should be proportional to the size of the change, not the size of the page as a whole." (16.15 Summary, p.498)

### Part 6: 결론
- **A. What Wasn't Covered (p.510–513)**: 고성능 JavaScript 엔진(실행 중 기계어 컴파일, hidden class), WebAssembly, 텍스트·그래픽 렌더링의 복잡성 등.

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Browser Rendering Pipeline and Frame Lifecycle]] — 파싱 → 스타일 → 레이아웃 → 페인트 → 래스터·합성, 프레임 예산, `requestAnimationFrame`, 메인 스레드와 컴포지터 스레드 (Ch 4–6, 12, 13, 16)
- [[JavaScript Event Loop and Asynchronous Patterns]] — 작업 큐와 렌더링 작업의 관계 (Ch 12)

### Entities
- [[Web Browser Architecture (Chromium Blink)]] — 멀티 스레드 렌더링 구조 (Ch 12–13)
