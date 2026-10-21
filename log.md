# TechWiki-RN-FullStack Activity Log

시간순으로 기록되는 위키 작업 로그(append-only)입니다.
각 항목은 `## [YYYY-MM-DD] <operation> | <title>` 형식을 따르므로 `grep "^## \[" log.md | tail -5` 등으로 최근 이력을 빠르게 조회할 수 있습니다.

---

## [2026-10-18] init | TechWiki Vault & Schema Initialization
- Obsidian iCloud 경로(`/Users/yoshin/Library/Mobile Documents/iCloud~md~obsidian/Documents/TechWiki`)에 볼트 생성 및 `/Users/yoshin/Documents/TechWiki` 심볼릭 링크 연결.
- Git 저장소 초기화 및 디렉토리 구조(`sources/`, `concepts/`, `entities/`, `syntheses/`, `templates/`) 생성.
- 오픈소스 PDF→Markdown 라이브러리 `pymupdf4llm` (PyMuPDF 1.28.2) 설치 및 스키마(`CLAUDE.md`, `AGENTS.md`) 수립.

## [2026-10-18] ingest | Building Applications with AI Agents (Michael Albada, O'Reilly 2025)
- **원천 PDF**: `/Users/yoshin/Documents/MarginNote4/G. Artificial Intelligence/Agents/2025-Building Applications with AI Agents-Michael Albada-O'Reilly Media.pdf` (355p)
- **생성된 Source 페이지**:
  - [[2025-Building Applications with AI Agents]]
- **생성된 Concept 페이지 (4개)**:
  - [[AI Agent Architecture]]
  - [[Context Engineering]]
  - [[Agent Memory and Knowledge]]
  - [[Multi-Agent Orchestration]]
- **생성된 Entity 페이지 (2개)**:
  - [[Model Context Protocol (MCP)]]
  - [[LangGraph]]
- **생성된 Synthesis 페이지 (1개)**:
  - [[RAG vs GraphRAG vs LLM Wiki]]
- **인덱스 갱신**: `index.md`에 총 8개 위키 페이지 등록 완료.

## [2026-10-18] ingest | React Native 도서 3권 통합 Ingest & 학습 로드맵 구축
- **원천 PDF (3권)**:
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/Mobile/2019-React Native in Action-Nader Dabit-Manning.pdf` (321p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/Mobile/2021-Do it! 리액트 네이티브 앱 프로그래밍-전예홍-이지스퍼블리싱.pdf` (856p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2022-React and React Native, Fourth Edition-Adam Boduch,Roy Derks 외-Packt Publishing.pdf` (606p)
- **생성된 Source 페이지 (3개)**:
  - [[2019-React Native in Action]]
  - [[2021-Do it! 리액트 네이티브 앱 프로그래밍]]
  - [[2022-React and React Native, Fourth Edition]]
- **생성된 Concept 페이지 (4개)**:
  - [[React Native Architecture (Bridge to New Architecture)]]
  - [[Mobile Layout and Styling with Flexbox]]
  - [[React Native Gesture and Animation System]]
  - [[Offline-First and Mobile State Management]]
- **생성된 Entity 페이지 (3개)**:
  - [[React Native]]
  - [[Expo]]
  - [[React Navigation]]
- **생성된 Synthesis 페이지 (1개)**:
  - [[React Native Learning Roadmap and Evolution (2019-2022)]]
- **인덱스 갱신**: `index.md`에 신규 11개 페이지(누적 19개 위키 페이지) 등록 완료.

## [2026-10-19] ingest | 모던 React 핵심 도서 4권 추가 Ingest 및 React ↔ React Native 교차 연결
- **원천 PDF (4권)**:
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2023-Do it! 리액트 모던 웹 개발 with 타입스크립트-전예홍-이지스퍼블리싱.pdf` (680p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2024-Fluent React-Tejas Kumar-OReilly Media.pdf` (337p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2024-React in Depth-Morten Barklund-Manning Publications.pdf` (434p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2026-React Application Architecture for Production Second Edition-Alan Alickovic,Anthony Alicea-Packt Publishing.pdf` (374p)
- **생성된 Source 페이지 (4개)**:
  - [[2023-Do it! 리액트 모던 웹 개발 with 타입스크립트]]
  - [[2024-Fluent React]]
  - [[2024-React in Depth]]
  - [[2026-React Application Architecture for Production Second Edition]]
- **생성된 Concept 페이지 (3개)**:
  - [[React Fiber and Reconciliation]]
  - [[React Hooks and Component Design Patterns]]
  - [[Web React vs React Native Shared Core and Differences]]
- **생성된 Entity 페이지 (2개)**:
  - [[React]]
  - [[TanStack Query and Zustand]]
- **갱신된 기존 페이지 (2개)**:
  - [[Offline-First and Mobile State Management]] (TanStack Query, Zustand, RTK 반영)
  - [[React Native Learning Roadmap and Evolution (2019-2022)]] (React 4권 + RN 3권 통합 4단계 로드맵으로 확장)
- **인덱스 갱신**: `index.md`에 신규 9개 페이지(누적 28개 위키 페이지) 등록 완료.

## [2026-10-19] ingest | JavaScript 3권 & TypeScript 3권(총 6권) 추가 Ingest 및 언어–프레임워크 지식 그래프 연결
- **원천 PDF (6권)**:
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2015-인사이드 자바스크립트-송형주,고현준-한빛미디어.pdf` (285p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2021-JavaScript from Beginner to Professional-Laurence Lars Svekis,Maaike van Putten 외-Packt Publishing.pdf` (545p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2024-JavaScript Design Patterns-Hugo Di Francesco-Packt Publishing.pdf` (308p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2021-Mastering TypeScript Fourth Edition-Nathan Rozentals-Packt Publishing.pdf` (539p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition-Theofanis Despoudis-Packt Publishing.pdf` (424p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2026-Clean Code with TypeScript-Rukevwe Ojigbo,Dr. Sanjay Krishna Anbalagan-Packt Publishing.pdf` (438p)
- **생성된 Source 페이지 (6개)**:
  - [[2015-인사이드 자바스크립트]]
  - [[2021-JavaScript from Beginner to Professional]]
  - [[2024-JavaScript Design Patterns]]
  - [[2021-Mastering TypeScript Fourth Edition]]
  - [[2025-TypeScript 5 Design Patterns and Best Practices 2nd Edition]]
  - [[2026-Clean Code with TypeScript]]
- **생성된 Concept 페이지 (4개)**:
  - [[JavaScript Execution Context, Closures, and Prototype]]
  - [[JavaScript Event Loop and Asynchronous Patterns]]
  - [[TypeScript Type System and Advanced Generics]]
  - [[Type-Safe Contract-First and Runtime Boundary Architecture]]
- **생성된 Entity 페이지 (2개)**:
  - [[JavaScript (ECMAScript)]]
  - [[TypeScript]]
- **갱신된 기존 페이지 (1개)**:
  - [[React Native Learning Roadmap and Evolution (2019-2022)]] (JS 3권 + TS 3권 + React 4권 + RN 3권 총 13권 5단계 마스터 로드맵으로 확장)
- **인덱스 갱신**: `index.md`에 신규 12개 페이지(누적 40개 위키 페이지) 등록 완료.

## [2026-10-19] ingest | React / React Native / JavaScript / TypeScript 심화 확장 도서 (TOP 6 전체 — 총 8권) 추가 Ingest
- **원천 PDF (8권)**:
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2021-React Hooks in Action-John Larsen-Manning Publications.pdf` (418p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2021-Designing React Hooks the Right Way-Fang Jin,Sagar Kale-Packt Publishing.pdf` (252p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2026-Full-Stack React TypeScript and Node 2nd Edition-David Choi-Packt Publishing.pdf` (514p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/Web Infrastructure/2024-Build a Frontend Web Framework (From Scratch)-Ángel Sola Orbaiceta-Manning Publications.pdf` (399p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2021-JavaScript Cookbook 3e-Adam D. Scott,Matthew MacDonald,Shelley Powers-O'Reilly Media.pdf` (539p)
  - `/Users/yoshin/Documents/MarginNote4/E. Programming Languages/JavaScript-TypeScript/2022-자바스크립트로 배우는 SICP-해럴드 에이블슨,제럴드 제이 서스먼 외-한빛미디어.pdf` (883p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/React/2025-Advanced Front-End Development-Nitesh Upadhyaya-Apress.pdf` (438p)
  - `/Users/yoshin/Documents/MarginNote4/H. Information Systems/Web Infrastructure/2026-Build AI-Enhanced Web Apps-Theo Despoudis-Manning Publications.pdf` (394p)
- **생성된 Source 페이지 (8개)**:
  - [[2021-React Hooks in Action]]
  - [[2021-Designing React Hooks the Right Way]]
  - [[2026-Full-Stack React TypeScript and Node 2nd Edition]]
  - [[2024-Build a Frontend Web Framework (From Scratch)]]
  - [[2021-JavaScript Cookbook 3e]]
  - [[2022-자바스크립트로 배우는 SICP]]
  - [[2025-Advanced Front-End Development]]
  - [[2026-Build AI-Enhanced Web Apps]]
- **생성된 Concept 페이지 (3개)**:
  - [[Virtual DOM and Reconciliation from Scratch]]
  - [[Full-Stack Authentication, Token Refresh, and Observability]]
  - [[AI-Enhanced React Applications and Specification-Driven Development]]
- **갱신된 기존 페이지 (1개)**:
  - [[React Native Learning Roadmap and Evolution (2019-2022)]] (총 21권 통합 로드맵 및 6대 심화 확장 트랙 추가)
- **인덱스 갱신**: `index.md`에 신규 11개 페이지(누적 51개 위키 페이지: Sources 22개, Concepts 18개, Entities 9개, Syntheses 2개) 등록 완료.

## [2026-10-19] lint | 전체 51개 위키 페이지 링크 무결성 검증 및 공식 문서(React 19 · RN 0.76–0.77 · TS 5.4–5.8 · MDN ES2023–2025) 기반 최신 내용 보강
- **끊어진 위키링크(Broken Links) 및 인바운드 고아 링크 교정 (6개 문서)**:
  - `sources/2022-자바스크립트로 배우는 SICP.md`, `sources/2025-Advanced Front-End Development.md`, `sources/2026-Build AI-Enhanced Web Apps.md`
  - `concepts/Virtual DOM and Reconciliation from Scratch.md`, `concepts/Full-Stack Authentication, Token Refresh, and Observability.md`, `concepts/AI-Enhanced React Applications and Specification-Driven Development.md`
  - 위 문서들 내의 미생성·불일치 링크를 실제 존재하는 51개 위키 페이지 제목으로 100% 정규화하고, 기존 코어 문서들(`React`, `TypeScript`, `React Hooks...`, `JavaScript Execution Context...`, `JavaScript Event Loop...`)에서 신규 페이지들로의 양방향 백링크를 연결.
- **도서 간 시대적 차이 교정 및 공식 문서(Official Docs) 근거 최신 보강 (7개 문서)**:
  - [[React Hooks and Component Design Patterns]] & [[React]]: **React 19 공식 문서(`react.dev`)** 기준 `ref` 일반 Prop 전달(`forwardRef` Deprecated), `<Context>` 직접 Provider 렌더링, 비동기 Actions(`useActionState`, `useFormStatus`, `useOptimistic`) 및 조건부 호출이 가능한 `use(resource)` API 추가.
  - [[Virtual DOM and Reconciliation from Scratch]]: **`react.dev/learn/preserving-and-resetting-state`** 기준 트리 위치와 `key`에 따른 훅 상태 보존·초기화 원칙 보강.
  - [[React Native Architecture (Bridge to New Architecture)]] & [[Mobile Layout and Styling with Flexbox]]: **React Native 0.76–0.77+ 공식 문서(`reactnative.dev`)** 기준 **New Architecture 및 Bridgeless Mode 기본 활성화(Default)**, 웹 정렬 Event Loop 및 `useLayoutEffect` 동기 레이아웃 측정, 그리고 통합 **`boxShadow`**, **`filter`**, Flexbox **`gap`**, **`display: 'contents'`** 스타일링 표준 보강.
  - [[TypeScript Type System and Advanced Generics]] & [[TypeScript]]: **TypeScript 5.4–5.8 공식 문서(`typescriptlang.org`)** 기준 `NoInfer<T>`(TS 5.4), `filter` 자동 타입 서술어 추론(`Inferred Type Predicates`, TS 5.5), 모노레포 병렬 빌드를 위한 `isolatedDeclarations`(TS 5.5), Node.js 네이티브 TS 실행 호환을 위한 `erasableSyntaxOnly`(TS 5.8) 추가.
  - [[JavaScript Execution Context, Closures, and Prototype]], [[JavaScript Event Loop and Asynchronous Patterns]], [[Full-Stack Authentication, Token Refresh, and Observability]]: **MDN Web Docs (`developer.mozilla.org`, ES2023–ES2025)** 기준 불변 배열 메서드(`toSorted`, `toReversed`, `toSpliced`, `with`), 네이티브 깊은 복사(`structuredClone`), 그룹화(`Object.groupBy`), 집합 연산(ES2025 `Set` methods), 그리고 401 토큰 리프레시 뮤텍스를 간결하게 만드는 `Promise.withResolvers()`(ES2024) 보강.

## [2026-10-19] query | 풀스택 개발자 준비를 위한 JavaScript & TypeScript 기초·핵심 문법 심층 보충 (Concept 2개 신설)
- **생성된 Concept 페이지 (2개)**:
  - [[JavaScript Core Syntax and Modern ES6+ Fundamentals]] (`const`/`let`/TDZ, 6대 Falsy와 React/RN `&&` 렌더링 크래시 주의점, `?.` 옵셔널 체이닝과 `??` 널 병합, 화살표 함수 객체 반환 `=> ({})`, 구조 분해 할당과 스프레드 `...` 불변 업데이트, 5대 배열 고차 함수와 `Map`/`Set`, `#private` 클래스, ESM `import`/`export`)
  - [[TypeScript Core Syntax and Full-Stack Type Annotations]] (기본 타입/튜플과 추론 기준, `interface` 선언 병합 vs `type` 유니온, 함수 및 `Promise<T>` 타이핑, 5대 Type Narrowing과 `as` 남용 경고, 8대 필수 유틸리티 타입 `Omit`/`Pick`/`Partial`/`Record`, React Props·이벤트 및 Node.js Express `Request`·Prisma 실전 타이핑 치트시트)
- **갱신된 기존 페이지**:
  - [[JavaScript (ECMAScript)]], [[TypeScript]], [[React Native Learning Roadmap and Evolution (2019-2022)]] (Step 0 최우선 학습 문서로 연결)
- **인덱스 갱신**: `index.md`에 신규 2개 페이지(누적 53개 위키 페이지: Sources 22개, Concepts 20개, Entities 9개, Syntheses 2개) 등록 완료.

## [2026-10-19] lint | 신규 기초 문법·용어 사전 Concept 2개 포함 전체 53개 페이지 교차참조 및 무결성 점검
- **양방향 인바운드/아웃바운드 링크 보강 (7개 문서)**:
  - 신규 생성된 [[JavaScript Core Syntax and Modern ES6+ Fundamentals]] 및 [[TypeScript Core Syntax and Full-Stack Type Annotations]]가 원천 도서([[2021-JavaScript from Beginner to Professional]], [[2021-JavaScript Cookbook 3e]], [[2021-Mastering TypeScript Fourth Edition]], [[2026-Full-Stack React TypeScript and Node 2nd Edition]]) 및 심화 개념([[JavaScript Execution Context, Closures, and Prototype]], [[TypeScript Type System and Advanced Generics]])과 양방향으로 촘촘하게 연결되도록 백링크 추가.
- **포맷 및 취소선(`~`) 규칙 검증**:
  - `index.md` 및 신규 Concept 2개 문서 내 범위 표기에서 물결표(`~`) 0건(전량 엔대시 `–` 사용) 확인 및 전체 53개 파일과 `index.md` 100% 동기화 검증 완료.

## [2026-10-19] schema | Apple Books (iBooks) EPUB 라이브러리를 제2 원천 소스(Raw Source)로 공식 등록
- **갱신된 스키마 및 인덱스 (`CLAUDE.md`, `index.md`)**:
  - Apple Books 로컬 EPUB 패키지 경로(`/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents`, 총 86권 로컬 EPUB) 및 메타데이터 SQLite DB(`/Users/yoshin/Library/Containers/com.apple.iBooksX/Data/Documents/BKLibrary/BKLibrary*.sqlite`)를 `CLAUDE.md` 1계층(Raw Sources)과 `index.md` 원천 라이브러리에 등록.
  - `CLAUDE.md` Section 3.4에 macOS APFS NFD 한글 정규화(`unicodedata.normalize('NFC', ...)`) 및 EPUB 패키지 내부 `.opf`/`toc.ncx`/`.xhtml` 파싱 스크립트를 표준화.

## [2026-10-19] ingest | Apple Books 추천 도서 9권 전체 (Next.js · MongoDB 8.0 · Docker/K8s · GitHub Actions · Observability 2.0 · Beyond Vibe Coding · AI 에이전트 · Prompt Engineering · 개발자 커리어)
- **생성된 Source 페이지 (9개)**:
  - [[2025-비욘드 바이브 코딩 (Beyond Vibe Coding)]] (Addy Osmani, O'Reilly 2025)
  - [[2022-Real-World Next.js]] (Michele Riva, Packt 2022)
  - [[2025-MongoDB 8.0 in Action, Third Edition]] (Arek Borucki, Manning 2025)
  - [[2021-컨테이너 인프라 환경 구축을 위한 쿠버네티스 도커]] (조훈, 심근우, 문성주, 길벗 2021)
  - [[2021-Hands-on GitHub Actions]] (Chaminda Chandrasekara, Pushpa Herath, Apress 2021)
  - [[2026-AI 에이전트를 깊이 이해하기]] (Bojie Li / 역: JeongJaeSoon, 2026)
  - [[2024-Prompt Engineering for LLMs]] (John Berryman, Albert Ziegler, O'Reilly 2024)
  - [[2025-Open Source Observability]] (Peter Corless, Neha Pawar, O'Reilly 2025)
  - [[2023-어떤 개발자가 되고 싶니]] (박동기, 슬로디미디어 2023)
- **생성된 Concept 페이지 (4개)**:
  - [[Next.js Rendering Strategies (SSR, SSG, ISR, and RSC)]]
  - [[NoSQL Document Modeling, Aggregation, and Vector Search]]
  - [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]]
  - [[Beyond Vibe Coding and AI-Augmented Software Engineering]]
- **보강된 기존 페이지 (4개)**:
  - [[Context Engineering]] (KV Cache 친화적 접두사 불변 설계 및 탄력적 스니펫 Elastic Snippets 추가)
  - [[AI Agent Architecture]] (`에이전트 = LLM + 컨텍스트 + 도구` 공식, 하네스 엔지니어링, 사후 학습 SFT/RL 폐루프 추가)
  - [[Full-Stack Authentication, Token Refresh, and Observability]] (분리형 오픈소스 관측성 스택 Observability 2.0, 고카디널리티 분석, Prometheus/Grafana, Next.js SSR 쿠키 인증 추가)
  - [[React Native Learning Roadmap and Evolution (2019-2022)]] (총 30권 통합 로드맵 및 8대 심화 확장 트랙으로 확장)
- **인덱스 갱신**: `index.md`에 신규 13개 페이지(누적 66개 위키 페이지: Sources 31개, Concepts 24개, Entities 9개, Syntheses 2개) 등록 완료.

## [2026-10-19] lint | 전체 66개 페이지 링크·프론트매터·인덱스 동기화 점검 및 Expo 최신화
- **링크 무결성**: 끊어진 위키링크 0건, 고아 페이지 0건, 아웃바운드 없는 페이지 0건 (표 안의 `[[문서\|별칭]]` 이스케이프 링크는 Obsidian 정상 문법으로 확인).
- **Index 동기화**: 실제 파일 66개 ↔ `index.md` 100% 일치. 단, 분류 라벨 2건을 프론트매터·실제 PDF 폴더(`H. Information Systems/Full Stack`)에 맞춰 수정:
  - [[2026-Full-Stack React TypeScript and Node 2nd Edition]] (`React` → `Full Stack`, 공저자 Cihan Yakar 추가)
  - [[2024-Build a Frontend Web Framework (From Scratch)]] (`Web Infrastructure` → `Full Stack`)
- **프론트매터 보정 (19개 Source + 템플릿)**: 공통 필수 필드 `sources`가 빠진 Source 페이지 19개에 자기 참조 `sources: ["[[...]]"]` 추가, `templates/source.md`에도 `sources` 필드 추가. 모든 `pdf_path`/`epub_path` 실존 확인.
- **오래된 서술 교정 (1개)**: [[Expo]]에 "관점 비교 / 출처별 차이" 섹션 추가 — `expo init`/`expo eject`(도서 기준) → `npx create-expo-app` · CNG `npx expo prebuild` · Development Build · EAS(공식 문서 기준), RN 공식 문서의 프레임워크(Expo) 기본 권장 반영.
- **물결표(`~`) 규칙**: 위반 0건.
- **미생성 엔티티 후보 (언급 파일 수)**: Node.js(22), Express(13), Next.js(11), Redux(11), GraphQL(11), Kubernetes(10), MongoDB(8), OpenTelemetry(8), Docker(7), Prisma(7), Zod(7).
- **구조 개선 제안**: [[React Native Learning Roadmap and Evolution (2019-2022)]]는 현재 30권·8개 트랙을 다루므로 제목과 범위가 맞지 않음 → 이름 변경(예: `Full-Stack Engineering Learning Roadmap`) 검토 필요.

## [2026-10-21] query | React / React Native 내부 구현(Under the Hood) 심층 분석 (Synthesis 1개 신설)
- **생성된 Synthesis 페이지 (1개)**:
  - [[React and React Native Internal Implementation Deep Dive]]:
    1. ReactElement vs FiberNode 데이터 구조 비교 (`child`/`sibling`/`return` 단일 연결 리스트, `alternate`, `flags`, `lanes`)
    2. 더블 버퍼링(Double Buffering) 및 DFS 순회(`beginWork` / `completeWork`)와 비동기 협조적 스케줄링(`workLoopConcurrent`)
    3. Hooks의 밑바닥 구현: `memoizedState` 단일 연결 리스트, Dispatcher 분기(`mountState` vs `updateState`), 규칙 위반 시 포인터 파괴 원리
    4. 호스트 렌더러 분리: `react-reconciler` Host Config 인터페이스 계약
    5. 구형 React Native Bridge의 한계(JSON 직렬화/역직렬화 병목, 비동기 지연 및 스크롤 시 흰색 빈 공간 문제)
    6. New Architecture & Bridgeless Mode(RN 0.76+ Default): JSI C++ HostObject 직접 참조, Fabric C++ Shadow Tree 및 Yoga 엔진의 C++ 레벨 동기 레이아웃 계산, TurboModules & Codegen, Hermes AOT 바이트코드 최적화
    7. 웹(`react-dom`) vs 모바일(`react-native`) 렌더링 파이프라인 9대 요소 상세 비교표
- **인덱스 갱신**: `index.md`에 신규 1개 페이지(누적 67개 위키 페이지: Sources 31개, Concepts 24개, Entities 9개, Syntheses 3개) 등록 완료.

## [2026-10-21] ingest | React & React Native 공식 웹 문서(react.dev, reactnative.dev) 정식 Source 등록 및 스키마 확장
- **스키마 확장 (`CLAUDE.md`, `templates/official-docs.md`)**:
  - `CLAUDE.md` 제1계층 Raw Sources에 제3의 원천 소스로 "공식 웹 기술 문서(Official Web Documentation)"를 공식 편입.
  - 프론트매터 규격에 `source_type: official-docs`, `url`, `version` 필드 지원 추가.
  - 신규 템플릿 `templates/official-docs.md` 생성.
- **생성된 Source 페이지 (2개)**:
  - [[2026-React Official Documentation]] (`react.dev` / Meta Open Source — React 19 코어 명세, 컴포넌트 순수성 및 상태 보존/초기화 규칙, `ref` 일반 Prop 전달, 비동기 Actions 폼 훅 3총사, `use()` API, React Server Components 및 React Compiler 자동 최적화)
  - [[2026-React Native Official Documentation]] (`reactnative.dev` / Meta Open Source — RN 0.76–0.78+ 코어 명세, 구형 Bridge 폐기 및 Bridgeless Mode 기본화, JSI, Fabric C++ Shadow Tree & Yoga 동기 레이아웃, TurboModules & Codegen, Hermes AOT 바이트코드, "Use a Framework" Expo & CNG 표준)
- **교차 연결 및 보강된 기존 페이지 (6개)**:
  - [[React and React Native Internal Implementation Deep Dive]] (공식 문서 sources 및 See Also 백링크 추가)
  - [[React Fiber and Reconciliation]] (sources 및 공식 문서 출처 추가)
  - [[React Native Architecture (Bridge to New Architecture)]] (sources 및 공식 문서 출처 추가)
  - [[React]] (sources 및 공식 문서 출처 추가)
  - [[React Native]] (sources 및 공식 문서 출처 추가)
  - [[Expo]] (sources 및 공식 문서 출처 추가)
- **인덱스 갱신**: `index.md`에 신규 2개 페이지(누적 69개 위키 페이지: Sources 33개, Concepts 24개, Entities 9개, Syntheses 3개) 등록 완료.

## [2026-10-21] ingest | Expo 공식 웹 문서 (docs.expo.dev) 정식 Source 등록
- **원천 공식 문서**: `https://docs.expo.dev` (Expo SDK 52–53+ 기준, GitHub: `expo/expo`)
- **생성된 Source 페이지 (1개)**:
  - [[2026-Expo Official Documentation]] (Continuous Native Generation CNG 및 `npx expo prebuild`, Development Builds `expo-dev-client`, Expo Router 파일 기반 라우팅, EAS 클라우드 인프라, Expo Modules API DSL, RN New Architecture 기본 지원)
- **교차 연결 및 보강된 기존 페이지 (3개)**:
  - [[Expo]] (sources 및 출처 문헌에 공식 문서 반영)
  - [[2026-React Native Official Documentation]] (관련 공식 소스로 Expo 문서 상호 연결)
  - `CLAUDE.md`, `index.md` (공식 웹 기술 문서 목록에 `docs.expo.dev` 반영)
- **인덱스 갱신**: `index.md`에 신규 1개 페이지(누적 70개 위키 페이지: Sources 34개, Concepts 24개, Entities 9개, Syntheses 3개) 등록 완료.

## [2026-10-21] schema | 위키 볼트 리네이밍: TechWiki -> TechWiki-RN-FullStack
- **배경 및 목적**:
  - 본 위키의 본래 목적인 **"React Native 기반 iOS/Android 크로스플랫폼 앱 개발"** 및 이를 프로덕션으로 배포하기 위해 확장된 **풀스택(TypeScript, Node.js, Next.js, Prisma, PostgreSQL, Docker/K8s, CI/CD, 관측성, AI 에이전트)** 영역을 명확히 명시.
  - 향후 신설될 Java/Spring/Enterprise 백엔드 전용 위키(`TechWiki-Java-Backend`)와의 명확한 대칭성 확보.
- **경로 및 심볼릭 링크 갱신**:
  - 실제 iCloud 볼트 디렉터리: `/Users/yoshin/Library/Mobile Documents/iCloud~md~obsidian/Documents/TechWiki-RN-FullStack`
  - 로컬 심볼릭 링크: `/Users/yoshin/Documents/TechWiki-RN-FullStack` (기존 `/Users/yoshin/Documents/TechWiki`는 하위 호환성을 위해 새 경로 심볼릭 링크 유지)
- **문서 갱신**: `CLAUDE.md`, `index.md`, `log.md` 내부 경로 및 타이틀 정규화 완료.

## [2026-10-21] lint | 전체 70개 페이지 링크·인덱스·프론트매터 점검 및 RN 레거시 아키텍처 서술 교정
- **링크 무결성**: 실제로 깨진 위키링크 0건, 고아 페이지 0건, 아웃바운드 링크 없는 페이지 0건. 검출된 후보 8건(`[[Prototype]]`, `[[Scope]]`, `[[위키링크]]`)은 모두 인라인 코드나 mermaid 코드 블록 안의 JS 내부 슬롯·예시 표기라서 링크가 아님.
- **Index 동기화**: 실제 파일 70개와 `index.md` 항목이 100% 일치하고, 섹션 배치 오류도 없음.
- **프론트매터**: 공통 필수 필드 누락 0건, `pdf_path`·`epub_path` 경로 모두 실존. 도서 Source 31개에 `source_type` 필드가 없음. 스키마상 기본값이 `book`이라 위반은 아니지만, `templates/source.md`에도 이 필드가 없음 → 일괄 추가할지 보류.
- **물결표(`~`) 규칙**: 본문 위반 0건. 검출 9건은 프론트매터 `epub_path`의 `iCloud~com~apple~iBooks` 경로 문자열.
- **오래된 서술 교정 (2개 문서)**: "RN 0.76에서 구형 Bridge가 완전히 제거되었다"는 과장 서술을 공식 블로그 원문 기준으로 교정함 (0.76은 기본값만 전환, 0.82부터 Legacy Architecture를 켤 수 없음, interop layer는 유지, 0.84부터 레거시 코드 제거 진행).
  - [[React Native Architecture (Bridge to New Architecture)]] (4절에 레거시 제거 타임라인 추가)
  - [[React and React Native Internal Implementation Deep Dive]] (5.2절)
- **버전 노후화 (미교정, 후속 작업 필요)**: 공식 문서 Source 3개의 기준 버전이 현재 릴리스보다 낮음.
  - [[2026-React Native Official Documentation]]: `0.76–0.78+` → 현재 0.88 (2026-10-19)
  - [[2026-Expo Official Documentation]]: SDK `52–53+` → 현재 SDK 58 (2026-10-15, RN 0.88 포함)
  - [[2026-React Official Documentation]]: React 19 → 현재 19.3 (2026-09-09). 이 밖에 React Compiler v1.0 (2025-10-07), React Foundation 출범 공지 (2025-10-07) 등.
- **미생성 엔티티 후보 (언급 파일 수)**: Node.js(22), Express(13), Next.js(13), Redux(11), GraphQL(11), Kubernetes(10), Hermes(9), MongoDB(8), OpenTelemetry(8), Zod(7), Prisma(7), Docker(7).
- **로드맵 범위**: [[React Native Learning Roadmap and Evolution (2019-2022)]]에 공식 문서 Source 3개와 [[2025-Building Applications with AI Agents]]가 포함되지 않음. 제목과 범위 불일치(이름 변경 제안)도 여전히 미반영.
- **다음 Ingest 추천 (MarginNote4, 제목 기준 선별 · 목차 미확인)**: `H. Information Systems/Node.js/2025-Node.js Projects`, `H. Information Systems/API Design/2021-GraphQL in Action`, `D. Software Engineering/Testing/2022-Full Stack Testing`, `F. Data Management/PostgreSQL/2026-Just Use Postgres`.
