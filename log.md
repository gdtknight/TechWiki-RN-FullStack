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

## [2026-10-21] ingest | 공식 문서 Source 3개 최신 릴리스 반영 (RN 0.88 · Expo SDK 58 · React 19.3)
- **근거**: `reactnative.dev/blog` 릴리스 글(0.76, 0.79–0.88, "The New Architecture is Here"), `reactnative.dev/docs/environment-setup`, `expo.dev/changelog` SDK 52–58 글, `react.dev/blog` 글(19.2, Compiler v1.0, React Foundation, 19.3, RSC 보안 공지 2건). 인용문은 모두 원문 문구 그대로 옮김.
- **[[2026-React Native Official Documentation]]**: 버전을 `0.76–0.78+` → `0.76–0.88`로 갱신. 0.79–0.88 릴리스 이력 표 추가. Hermes V1 기본화(0.84)와 JSC 커뮤니티 이관 반영. Bridgeless 절을 공식 원문 기준으로 다시 작성("The bridge remains for backward compatibility", interop layer 유지). "프레임워크 없이는 Brownfield에서만"이라는 과장 서술을 원문 기준으로 교정. GitHub 경로를 `react/react-native-website`로 변경. publisher를 React Foundation으로 변경.
- **[[2026-Expo Official Documentation]]**: 버전을 `SDK 52–53+` → `SDK 52–58`로 갱신. New Architecture 전환 단계 표(SDK 52 신규 프로젝트 기본 → 53 전체 기본 → 54 레거시 지원 마지막 → 55 전용) 추가. SDK 53–58 릴리스 이력 추가. 근거가 확인되지 않은 "모든 코어 모듈을 JSI·TurboModules로 전면 재작성" 서술은 삭제.
- **[[2026-React Official Documentation]]**: 버전을 `React 19` → `19.0–19.3`으로 갱신. React Compiler v1.0 안정판(설치, React 17+ 호환, Expo SDK 54+ 기본 활성화) 반영. 19.2·19.3 신규 API, 19.3 동작 변경 사항, React Foundation 이전, 2025-12 RSC 보안 공지(CVE, 안전 버전 19.0.4, 19.1.5, 19.2.4) 추가. publisher를 React Foundation으로 변경.
- **도서 인용 교정**: [[JavaScript (ECMAScript)]] 엔진 비교표에서 `2022-RN 4e` p.314 인용을 원문(PDF p.314, 인쇄 p.291)과 대조함. 원문에 없는 "0.69 이전까지 기본"과 "Immer"를 삭제하고, 원문이 실제로 다루는 범위(초기 JSCore만 지원, 0.60 Hermes, 0.64 iOS, `react-native-firebase`·`mobx`)로 수정함.
- **연쇄 갱신**: [[React Native Architecture (Bridge to New Architecture)]] 4절 제목, `index.md`(원천 소스 라이브러리 목록 및 공식 문서 Source 3개 요약), `CLAUDE.md`(RN 웹사이트 GitHub 경로).

## [2026-10-21] lint | 공식 문서 갱신의 후속 반영 (Flexbox 개념, React·Expo 엔티티)
- **[[Mobile Layout and Styling with Flexbox]]** 4절을 다시 작성함. 근거: `reactnative.dev/docs/view-style-props`, `layout-props`, 0.71·0.76·0.77 릴리스 글, `react/react-native` CHANGELOG.
  - 교정: `filter` 예시(`blur`·`saturate`)가 iOS에서 동작하지 않음 → iOS는 `brightness`·`opacity`만, `blur`·`dropShadow`는 Android 12 이상으로 명시.
  - 교정: `mixBlendMode` 도입 버전 0.76 → 0.77, Android 10 이상 조건 추가.
  - 교정: `boxShadow`에 Android 9+(outset)·10+(inset) 조건 추가.
  - 교정: 근거를 확인하지 못한 "Yoga 3.0" 언급 삭제.
  - 추가: 0.77의 `isolation`·`outlineSpread`, `backgroundImage` 그라디언트(0.87에서 `experimental_` 접두사 제거, 최초 도입 버전은 미확인), 0.88 `fontVariationSettings`, 0.81 `<SafeAreaView>` 사용 중단.
- **[[React]]**: React Foundation 소유권 이전(2026-02-24), React Compiler v1.0, 19.2·19.3 신규 API, RN별 React 반영 버전(0.80→19.1.0, 0.83→19.2, 0.88→19.3.0) 추가.
- **[[Expo]]**: 런타임 아키텍처 비교 행, SDK 52–58과 RN 버전 대응표, SDK 58 요점 추가. "프레임워크 없는 방식은 예외적인 경우에만" 서술을 공식 문서 문구 기준으로 교정.

## [2026-10-21] query | JavaScript 입문 레슨 시리즈 "JS 첫걸음" 시작 (로드맵 + 레슨 01–02)
- **요청**: 기본 개념이 부족한 상태에서 JavaScript를 체계적으로 공부하고 싶음. 초등학생도 이해할 만큼 쉽고 자세한 설명.
- **형식 결정 (사용자 선택)**: `syntheses/`에 위키 레슨 시리즈로 작성. 로드맵과 레슨 1–2를 먼저 쓰고 검토를 받은 뒤 나머지를 진행.
- **생성된 Synthesis 페이지 (3개)**:
  - [[JS 첫걸음 00 - 로드맵]]: 12개 레슨 구성, 레슨별 근거 장(입문서 PDF 쪽 범위), 마친 뒤 이어서 읽을 위키 페이지.
  - [[JS 첫걸음 01 - 프로그램과 콘솔]]: 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 1 (p.26–41), MDN, Chrome DevTools 문서.
  - [[JS 첫걸음 02 - 변수]]: 근거는 같은 책 Ch 2 (p.43–45), MDN *let*·*const*·*var*·*Grammar and types*·*Lexical grammar*·*Assignment*, Chrome DevTools 80 릴리스 노트.
- **검증**: 레슨의 모든 예제 코드와 에러 메시지를 Node.js v24.19.0에서 실행해 확인함.
- **입문서와 현재 공식 자료의 차이 (각 레슨의 "관점 비교" 절에 기록)**:
  - "interpreted, 컴파일하지 않음" → MDN은 "interpreted (or just-in-time compiled)"
  - "현재 표준은 ES6" → 최신 판은 ECMAScript 2026 (제17판, 2026년 6월)
  - "var has global scope" → MDN은 "function-scoped or globally-scoped"
- **인덱스 갱신**: `index.md` Syntheses 섹션에 3개 페이지 등록 (누적 73개 위키 페이지: Sources 34개, Concepts 24개, Entities 9개, Syntheses 6개).

## [2026-10-21] query | JS 첫걸음 레슨 03 (자료형) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 03 - 자료형]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 2 (p.45–57)와 MDN *JavaScript data types and data structures*·*Template literals*·*NaN*·*Falsy*.
- **검증**: 예제·퀴즈의 코드와 결과를 Node.js v24.19.0에서 모두 실행해 확인함.
- **입문서와의 차이 ("관점 비교" 절에 기록)**:
  - Number의 한계를 2^53−1로 설명 → MDN상 이 값은 정수를 정확하게 다룰 수 있는 범위(`MAX_SAFE_INTEGER`)임
  - p.56의 "빈 문자열, `null`, `undefined`만 `false`" → MDN falsy 목록 9개로 보충
  - 출력 예시의 `Undefined`(대문자 U) 인쇄 오류
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 03 상태를 작성 완료로 변경, 링크 추가), [[JS 첫걸음 02 - 변수]](다음 레슨 링크), `index.md` Syntheses 섹션.
- 참고 출처의 MDN 링크는 클릭 가능한 마크다운 링크로 작성함 (레슨 01–02의 참고 출처도 같은 날 링크 형식으로 바꿈).

## [2026-10-21] query | JS 첫걸음 레슨 04 (연산자) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 04 - 연산자]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 2 (p.57–70)와 MDN *Operator precedence*·*Remainder*·*Increment*·*Strict equality*·*Logical AND*·*Logical OR*·*NaN*·*Less than*·*Window: prompt()*.
- **검증**: 예제·퀴즈·"자주 하는 실수" 코드를 Node.js v24.19.0에서 모두 실행해 확인함 (`8 <= 20 <= 13` → `true` 포함).
- **입문서와의 차이 ("관점 비교" 절에 기록)**:
  - p.63 우선순위 표에서 `**`를 전위 `++`보다 위에 둠 → MDN 기준 전위 연산자(14)가 거듭제곱(13)보다 먼저
  - p.62 후위 `++`를 "나중에 실행된다"고 설명 → MDN: 즉시 증가하고 증가 전 값을 돌려줌
  - p.67–68 `&&`·`||`가 불리언을 돌려준다고 설명 → MDN: 피연산자 값 중 하나를 그대로 돌려줌
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 04 작성 완료), [[JS 첫걸음 03 - 자료형]](다음 레슨 링크), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 05 (조건문) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 05 - 조건문]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 4 (p.96–111)와 MDN *if...else*·*switch*·*Conditional (ternary) operator*·*Empty statement*.
- **검증**: 예제·퀴즈·"자주 하는 실수" 코드를 Node.js v24.19.0에서 모두 실행해 확인함. 입문서 p.98에 인쇄된 `==` 코드도 실행해 설명과 다른 결과(`else` 쪽 실행)가 나오는 것을 확인함.
- **입문서와의 차이 ("관점 비교" 절에 기록)**:
  - p.98 "흔한 실수" 예제의 인쇄 오류 (본문은 `=` 하나를 설명, 코드는 `==`)
  - p.98의 "이퀄 두 개(`==`)를 쓰라"는 권고 → 이 시리즈는 `===`
  - p.101 삼항 연산자로 실행 코드를 고르는 예 → 이 시리즈는 값 고르기에만 씀 (판단)
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 05 작성 완료), [[JS 첫걸음 04 - 연산자]](다음 레슨 링크), `index.md` Syntheses 섹션.

## [2026-10-21] lint | 전체 76개 페이지 점검 (JS 첫걸음 레슨 00–05 추가 이후)
- **링크 무결성**: 깨진 위키링크 0건, 고아 페이지 0건, 아웃바운드 링크 없는 페이지 0건. 표 열 수 불일치 0건.
- **Index 동기화**: 실제 파일 76개(Sources 34, Concepts 24, Entities 9, Syntheses 9)와 `index.md`가 100% 일치하고, 섹션 배치 오류도 없음.
- **프론트매터**: 공통 필수 필드 누락 0건, `pdf_path`·`epub_path` 경로 모두 실존. 물결표(`~`) 본문 위반 0건.
- **레슨 간 절 참조**: "레슨 NN의 N.N절" 형태의 참조 11건이 모두 실제 제목과 일치함.
- **모순/근거 부족 서술 교정 ([[JavaScript Core Syntax and Modern ES6+ Fundamentals]])**:
  - 2.2절 "Falsy는 딱 6가지" → 실무 6종과 MDN 전체 목록 9개(`-0`, `0n`, `document.all` 포함)로 구분. [[JS 첫걸음 03 - 자료형]] 6.4절과 같은 기준으로 맞춤.
  - 2.2절 React Native 크래시 서술 → 공식 문서로 확인되는 범위("you must wrap all the text nodes inside of a `<Text>` component", "will raise exception")로 한정함. 정확한 에러 문구는 미확인으로 표시하고 확인 방법을 적음.
  - 1.4절 표의 `const` "95% 이상 사용" → 출처 없는 수치라서 삭제하고 "(판단)"으로 표시.
- **양방향 연결 보강**: [[JS 첫걸음 00 - 로드맵]]으로 들어오는 링크를 [[JavaScript Core Syntax and Modern ES6+ Fundamentals]](선행 학습), [[JavaScript (ECMAScript)]](입문 레슨), [[2021-JavaScript from Beginner to Professional]](이 책을 바탕으로 한 레슨 목록과 각 레슨의 관점 비교 요약)에 추가함.
- **미생성 엔티티 후보 (언급 파일 수)**: Node.js(27), Express(13), Next.js(13), Redux(11), GraphQL(11), Hermes(11), Kubernetes(10), MongoDB(8), OpenTelemetry(8), Zod(7), Prisma(7), Docker(7).
- **이전 점검에서 넘어온 미해결 항목**: [[React Native Learning Roadmap and Evolution (2019-2022)]] 제목·범위 불일치(이름 변경 제안), 도서 Source 31개의 `source_type` 필드 누락(기본값 `book`이라 규칙 위반은 아님).
- **다음 Ingest 추천**: JS 첫걸음 레슨 06–11은 이미 투입된 [[2021-JavaScript from Beginner to Professional]]·[[2015-인사이드 자바스크립트]]로 충분함. 위키 확장용 추천은 이전 점검과 같음 (MarginNote4 `Node.js/2025-Node.js Projects`, `API Design/2021-GraphQL in Action`, `Testing/2022-Full Stack Testing`, `PostgreSQL/2026-Just Use Postgres`. 제목 기준이며 목차는 미확인).

## [2026-10-21] query | JS 첫걸음 레슨 06 (반복문) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 06 - 반복문]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 5 (p.112–140)와 MDN *while*·*do...while*·*for*·*break*·*continue*·*for...in*.
- **범위 결정**: `for...of`(p.126)와 `for...in`(p.128)은 배열·객체를 배운 뒤에 다루도록 레슨 08·09로 넘기고, 이 레슨에서는 미리 보기만 둠.
- **검증**: 예제·퀴즈·"자주 하는 실수" 코드를 Node.js v24.19.0에서 모두 실행해 확인함. 무한 루프 예(p.135, `while` 안의 `continue`)는 `perl` alarm으로 2초 뒤 강제 종료해 `1`만 출력하고 멈추지 않는 것을 확인함.
- **입문서와의 차이 ("관점 비교" 절에 기록)**:
  - p.113 `while` 예제의 인쇄된 출력이 `1`부터 시작함 (실제는 `0`–`9`)
  - p.127 "`for...of`는 무한 루프에 빠지지 않는다" → 반복 도중 같은 배열에 `push`하면 끝나지 않는 것을 실행으로 확인함
  - p.129 "`for...in` 순서는 보장되지 않는다" → MDN: 최신 명세에서 순서가 정해져 있음
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 06 작성 완료), [[JS 첫걸음 05 - 조건문]](다음 레슨 링크), [[2021-JavaScript from Beginner to Professional]](레슨 목록), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 07 (함수) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 07 - 함수]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 6 (p.142–155, p.163–169)과 MDN *Functions*(가이드)·*Parameter*·*Default parameters*·*return*·*Arrow function expressions*·*Callback function*.
- **범위 결정**: 스코프·IIFE·중첩 함수(p.155–166)는 레슨 10으로, spread·rest(p.150–152)는 레슨 08로 넘김. 로드맵의 레슨 08·10 근거 쪽수도 갱신함.
- **검증**: 예제·퀴즈·"자주 하는 실수" 코드를 Node.js v24.19.0에서 모두 실행해 확인함 (`return` 줄바꿈 → `undefined`, `doTwice(cheer())` → `TypeError`, 함수 표현식 호이스팅 → `ReferenceError` 포함).
- **입문서와의 차이 ("관점 비교" 절에 기록)**:
  - p.167 이름 없는 함수 단독 선언 → 실행하면 `SyntaxError: Function statements require a function name`
  - p.163 "멈추지 않는다"는 무한 재귀 → 실제로는 `RangeError: Maximum call stack size exceeded`로 멈춤
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 07 작성 완료), [[JS 첫걸음 06 - 반복문]](다음 레슨 링크), [[2021-JavaScript from Beginner to Professional]](레슨 목록), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 08 (배열) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 08 - 배열]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 3 (p.72–85), Ch 5 (p.123–127), Ch 6 (p.150–152), Ch 8 (p.201–206)과 MDN *Array*·*at*·*push*·*includes*·*find*·*forEach*·*map*·*filter*·*sort*·*Spread syntax*·*for...of*·*const*.
- **검증**: 예제·퀴즈·"자주 하는 실수" 코드를 Node.js v24.19.0에서 모두 실행해 확인함.
- **입문서와의 차이 ("관점 비교" 절에 기록)**:
  - p.83 "`sort()`는 숫자를 작은 것부터 정렬" → 기본 정렬은 글자 순서. `[40, 1, 5, 200].sort()` → `[ 1, 200, 40, 5 ]`
  - p.76 `cars[-1] = "Fiat"`이 "생겨난다" → 칸이 아니라 별도 속성이 되며 `length`는 그대로
- **작성 중 교정**: `pop`이 뺀 값을 돌려준다는 서술에 p.80을 근거로 붙였으나 원문에 해당 문장이 없어, 실행 결과를 근거로 바꿈.
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 08 작성 완료), [[JS 첫걸음 07 - 함수]](다음 레슨 링크), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 09 (객체) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 09 - 객체]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 3 (p.72, p.86–94), Ch 5 (p.127–131)와 MDN *Working with objects*·*Object initializer*·*Object.entries()*·*for...in*·*Optional chaining*.
- **입문서 범위 밖에서 추가한 내용**: 메서드 기초(입문서는 Ch 7로 미룸), `{ ...obj }` 복사와 객체 `===` 비교, `TypeError: Cannot read properties of undefined`와 `?.`. 근거는 MDN과 실행 결과.
- **검증**: 예제·퀴즈·"자주 하는 실수" 코드를 Node.js v24.19.0에서 모두 실행해 확인함.
- **입문서와의 차이**: p.86 `typeof` 출력 예시의 `Object`(대문자 O) 인쇄 오류 → 실제 `object`.
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 09 작성 완료), [[JS 첫걸음 08 - 배열]](다음 레슨 링크), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 10 (스코프와 클로저) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 10 - 스코프와 클로저]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 6 (p.155–162, p.165–166, p.172), [[2015-인사이드 자바스크립트]] Ch 5 (PDF p.148, p.156–169), MDN *Scope*·*Closures*·*Strict mode*·*var*.
- **검증**: 예제·퀴즈 코드를 Node.js v24.19.0에서 모두 실행해 확인함 (`var` 반복 클로저 `4 4 4` → `let`이면 `1 2 3`, 엄격 모드에서 선언 없는 대입 → `ReferenceError` 포함). 매개변수 예제 이름은 브라우저 전역 `name`과 겹치지 않게 `petName`으로 정함.
- **출처와의 차이 ("관점 비교" 절에 기록)**:
  - [[2015-인사이드 자바스크립트]] p.148 "오직 함수만이 유효 범위" → ES6 이후 `let`·`const`는 블록 스코프
  - 같은 책 p.169 반복문 클로저를 즉시 실행 함수로 해결 → `let`만으로 해결됨
  - 입문서 p.161 선언 없는 대입 → 엄격 모드·모듈에서는 `ReferenceError`
  - 입문서 p.172 장 요약의 `var` 설명이 같은 장 본문(p.157–158)과 맞지 않음
- **작성 중 교정**: [[2015-인사이드 자바스크립트]] "함수의 캡슐화" 절 인용 쪽을 p.163–165로 적었다가 원문 대조 후 PDF p.164–166으로 고침.
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 10 작성 완료), [[JS 첫걸음 09 - 객체]](다음 레슨 링크), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 11 (비동기 기초) 작성
- **생성된 Synthesis 페이지**: [[JS 첫걸음 11 - 비동기 기초]]. 근거는 [[2021-JavaScript from Beginner to Professional]] Ch 13 (p.370–386)과 MDN *setTimeout()*·*Promise*·*async function*·*await*·*Using the Fetch API*·*JavaScript execution model*.
- **범위 결정**: Microtask/Macrotask 구분과 `Promise.all`은 다루지 않고 [[JavaScript Event Loop and Asynchronous Patterns]]로 안내함. `fetch`는 MDN 예제의 모양만 소개함 (네트워크가 필요해 실행 검증 대상에서 제외).
- **검증**: `fetch` 외의 예제·퀴즈 코드를 Node.js v24.19.0에서 모두 실행해 확인함 (`await` 없이 출력 → `Promise { <pending> }`, 비 async 함수 안의 `await` → `SyntaxError`, 잡지 않은 rejection → 종료 코드 1 포함).
- **입문서와의 차이 ("관점 비교" 절에 기록)**: p.380 "이벤트 루프가 실제 일을 하는 실행자이며 콜 스택을 감시한다"는 설명 → MDN 실행 모델 기준으로 정리. p.378 "`await`는 async 함수에서만" → 모듈 최상위에서도 가능.
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 11 작성 완료), [[JS 첫걸음 10 - 스코프와 클로저]](다음 레슨 링크), `index.md` Syntheses 섹션.

## [2026-10-21] query | JS 첫걸음 레슨 12 (모듈과 다음 단계) 작성 — 시리즈 완료
- **생성된 Synthesis 페이지**: [[JS 첫걸음 12 - 모듈과 다음 단계]]. 근거는 MDN *JavaScript modules*·*export*·*await*·*Scope*, Node.js *ECMAScript modules* 문서, [[2021-JavaScript from Beginner to Professional]] Ch 1 (p.34–35), Ch 15 (p.428–430, p.440–445). 입문서에 모듈 장이 없어 공식 문서를 주 근거로 씀.
- **검증**: `.mjs` 파일로 `export`/`import`, 기본 내보내기, `as`, 모듈 최상위 `await`, 모듈 엄격 모드를 Node.js v24.19.0에서 실행해 확인함. 실수 예 3건(없는 이름, 기본 내보내기에 중괄호, `./` 누락)의 에러 문구도 실행 결과 그대로 실음. `.js` 파일에서 `import`를 써도 Node.js v24에서 에러가 나지 않아 실수 목록에서 뺌.
- **다음 단계 안내**: 시리즈 회고 표와 이 위키에서 이어 읽을 페이지(JS 개념 3개 → TypeScript → React → React Native·Expo → 학습 로드맵)를 정리함.
- **연결 갱신**: [[JS 첫걸음 00 - 로드맵]](레슨 12 작성 완료, 12개 레슨 전체 완료), [[JS 첫걸음 11 - 비동기 기초]](다음 레슨 링크), [[2021-JavaScript from Beginner to Professional]](레슨 01–12 목록과 관점 비교 요약), `index.md` Syntheses 섹션.
