---
type: source
title: "2026-Build AI-Enhanced Web Apps"
category: "H. Information Systems/Web Infrastructure"
tags: [ai-engineering, react, nextjs, typescript, vercel-ai-sdk, langchain-js, rag, prompt-engineering, mcp, streaming]
authors: ["Theo Despoudis"]
publisher: "Manning Publications"
year: 2026
pdf_path: "/Users/yoshin/Documents/MarginNote4/H. Information Systems/Web Infrastructure/2026-Build AI-Enhanced Web Apps-Theo Despoudis-Manning Publications.pdf"
status: ingested
sources: []
created: 2026-10-19
updated: 2026-10-19
---

# 2026-Build AI-Enhanced Web Apps

## 1. 핵심 테제 (Core Thesis)
[[React]] 및 Next.js([[TypeScript]]) 기반의 모던 웹 애플리케이션에 생성형 AI(LLM)를 네이티브하게 통합하는 풀스택 AI 엔지니어링 실전서(총 394페이지)다. 단순 API 호출을 넘어 **Vercel AI SDK를 활용한 실시간 토큰 스트리밍과 Generative UI(`useChat`, `streamText`)**, **대화 상태 및 컨텍스트 윈도우 관리**, **LangChain.js 기반 멀티스텝 워크플로우와 RAG(Retrieval-Augmented Generation)**, **LLM 앱 전용 평가·테스팅·보안(Prompt Injection 방어, Rate Limiting)**, 그리고 **[[Model Context Protocol (MCP)]] 통합**까지 프로덕션 AI 웹/모바일 앱 구축의 전 과정을 다룬다.

---

## 2. 챕터별 상세 요약 (Chapter-by-Chapter Summary)

### Part 1: 기본 생성형 AI 웹 앱 구축 (Building Basic Generative AI Web Apps, p.23–148)
- **Ch 1–2 Using Generative AI & Building Your First App (p.25–75)**: 웹 애플리케이션에서 LLM의 역할(요약, 생성, 추출, 대화형 인터페이스)과 토큰·컨텍스트 윈도우·온도(Temperature) 등 핵심 파라미터를 이해하고, Next.js App Router와 OpenAI API를 연동한 첫 AI 웹 앱을 구축한다.
- **Ch 3 Connecting AI Models with the Vercel AI SDK (p.76–109)**:
  - 다양한 LLM 프로바이더(OpenAI, Anthropic, Google Gemini 등)를 단일 인터페이스로 추상화하는 **Vercel AI SDK** 아키텍처를 다룬다.
  - 서버 라우트 핸들러의 `streamText` / `generateObject`와 클라이언트 React 훅인 **`useChat`**, **`useCompletion`**, **`useObject`** 를 결합해, 별도의 복잡한 SSE(Server-Sent Events) 파서 작성 없이도 **실시간 스트리밍 응답과 스키마 검증된 구조화 출력(Zod 기반), 그리고 도구 호출(Tool Calling) 기반 Generative UI**를 구현한다.
- **Ch 4 Managing Conversation and State in Your Application (p.110–148)**: 멀티턴 대화에서 메시지 히스토리 상태 관리, 영속화(Persistence), 그리고 컨텍스트 윈도우 한계를 초과하지 않기 위한 **슬라이딩 윈도우(Sliding Window) 및 대화 요약(Conversation Summarization) 메모리 전략**([[Agent Memory and Knowledge]]와 연결)을 구현한다.

### Part 2: 고급 생성형 AI 기법과 배포 (Advanced Generative AI Techniques and Deployment, p.149–324)
- **Ch 5 Prompt Engineering in Web Applications (p.151–187)**: 시스템 프롬프트 설계, Few-shot 프롬프팅, Chain-of-Thought(CoT), 프롬프트 템플릿 버전 관리 및 동적 변수 주입 패턴을 다룬다([[Context Engineering]]과 연결).
- **Ch 6 Building AI Workflows with LangChain.js (p.188–218)**: **LangChain.js**([[LangGraph]] 생태계)와 LCEL(LangChain Expression Language)을 활용해 프롬프트-모델-출력 파서(Output Parser)를 체이닝하고, 분기·병렬 실행·폴백(Fallback)이 포함된 복합 AI 워크플로우를 구축한다.
- **Ch 7 Document Summarization and RAG with LangChain.js (p.219–253)**:
  - 대용량 문서를 처리하기 위한 Map-Reduce / Refine 요약 전략을 구현한다.
  - 문서 로더(Document Loaders) -> 텍스트 청킹(`RecursiveCharacterTextSplitter`) -> 임베딩(Embeddings) -> 벡터 스토어(Vector Store) 검색 -> 컨텍스트 증강 생성으로 이어지는 **웹 기반 [[Agent Memory and Knowledge|RAG (Retrieval-Augmented Generation)]] 파이프라인**([[RAG vs GraphRAG vs LLM Wiki]] 비교 참조)을 완성한다.
- **Ch 8–9 Testing, Debugging, Deployment, and Security (p.254–324)**:
  - **테스팅 및 디버깅(Ch 8)**: 비결정론적인 LLM 출력을 검증하기 위한 결정론적 모킹(Mocking), 시맨틱 유사도 검증, **[[AI Agent Architecture|Agent Evaluation (LLM-as-a-Judge)]]**, 그리고 트레이싱 도구를 다룬다.
  - **배포 및 보안(Ch 9)**: **프롬프트 인젝션(Prompt Injection) 방어**, 입출력 가드레일(Guardrails), API 키 보호, 사용자별 토큰 사용량 제한(Rate Limiting & Cost Control), 엣지 런타임(Edge Runtime) 스트리밍 배포를 다룬다.

### Part 3: 실전 핸즈온 프로젝트 (Hands-On Projects, p.325–350)
- **Ch 10 Building an AI Interview Assistant (p.327–338)**: 실시간 음성/텍스트 입력, 직무별 맞춤형 질문 생성, 답변 평가 및 구조화된 피드백 리포트를 제공하는 AI 면접 코치 앱을 구현한다.
- **Ch 11 Building an AI RAG Agent (p.339–350)**: 사용자 업로드 문서를 인덱싱하고 질문 의도에 따라 검색 도구(Retriever Tool)를 자율적으로 호출해 출처(Citations)와 함께 답변하는 에이전틱 RAG 웹 앱을 구축한다.

### Part 4: 고급 통합과 AI의 미래 (Advanced Integrations and the Future of AI, p.351–374)
- **Ch 12 Integrating Web Apps with the Model Context Protocol (MCP) (p.353–374)**:
  - 각 웹 앱이 외부 데이터 소스나 도구마다 파편화된 커스텀 연동 코드를 작성하던 문제를 해결하는 표준 프로토콜인 **[[Model Context Protocol (MCP)]]** 의 호스트-클라이언트-서버 아키텍처를 설명한다.
  - TypeScript MCP SDK를 사용해 웹 애플리케이션 백엔드에서 MCP 서버의 Resources, Prompts, Tools를 탐색·호출하고, 이를 React 프론트엔드의 채팅/에이전트 UI와 결합하는 방법을 상세히 제시한다.

---

## 3. 교차 참조: 핵심 개념 및 엔티티 (Cross-References)

### 관련 개념 (Concepts)
- [[AI-Enhanced React Applications and Specification-Driven Development]] — Vercel AI SDK(`useChat`, `streamText`), LangChain.js RAG, MCP 웹 통합 아키텍처 종합
- [[Agent Memory and Knowledge]] — Ch 4의 대화 메모리 전략 및 Ch 7, 11의 LangChain.js 기반 문서 청킹·임베딩·벡터 검색(RAG) 파이프라인
- [[Context Engineering]] — Ch 4, 5의 대화 메모리 관리 및 프롬프트 엔지니어링
- [[AI Agent Architecture]] — Ch 8–9의 에이전트 평가(LLM-as-a-Judge), 가드레일 및 보안

### 관련 엔티티 (Entities)
- [[React]]
- [[TypeScript]]
- [[Model Context Protocol (MCP)]] — Ch 12의 TypeScript 웹 애플리케이션 MCP 클라이언트/서버 연동
- [[LangGraph]] — LangChain.js 워크플로우 및 에이전트 생태계

### 함께 읽으면 좋은 소스 (Related Sources)
- [[2026-Full-Stack React TypeScript and Node 2nd Edition]] — Ch 15의 `CLAUDE.md` 기반 스펙 주도 AI 코딩 워크플로우
- [[2026-React Application Architecture for Production Second Edition]] — Ch 13의 AI 코드 생성 시대를 위한 프로덕션 React 아키텍처
- [[2025-Building Applications with AI Agents]] — 에이전트 아키텍처, 도구 호출, MCP의 백엔드/시스템 관점 원론
