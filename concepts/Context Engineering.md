---
type: concept
title: "Context Engineering"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
  - "[[2026-AI 에이전트를 깊이 이해하기]]"
  - "[[2024-Prompt Engineering for LLMs]]"
  - "[[2025-비욘드 바이브 코딩 (Beyond Vibe Coding)]]"
tags:
  - context-engineering
  - orchestration
  - prompt-engineering
  - kv-cache
  - elastic-snippets
  - ai-agents
created: 2026-10-18
updated: 2026-10-19
---

# Context Engineering

## 정의 (Definition)

**컨텍스트 엔지니어링(Context Engineering)** 은 에이전트의 실행 계획(Plan) 내 각 단계마다 모델이 최상의 추론과 행동을 수행할 수 있도록 **사용자 메시지, 시스템 지침, 검색된 외부 지식(Knowledge), 이전 단계의 도구 실행 결과, 워크플로우 상태(State)를 동적이고 토큰 효율적인 형태로 조립하여 컨텍스트 윈도우에 구성하는 기술**이다 ([[2025-Building Applications with AI Agents]], p.111, p.134; [[2026-AI 에이전트를 깊이 이해하기]] Ch 3).

---

## Prompt Engineering vs Context Engineering

[[2025-Building Applications with AI Agents]] (p.134)와 [[2026-AI 에이전트를 깊이 이해하기]] (Ch 2–3)는 두 개념의 차이를 명확히 구분한다:

| 구분 | 프롬프트 엔지니어링 (Prompt Engineering) | 컨텍스트 엔지니어링 (Context Engineering) |
|---|---|---|
| **초점** | 단일 호출에서 모델의 지시문(Instruction) 문구를 어떻게 잘 작성할 것인가 | 다단계 에이전트 루프에서 **매 시점 어떤 정보와 상태를 선택·압축·구조화해 넣을 것인가** |
| **성격** | 정적 텍스트 최적화 (Static instruction crafting) | 동적 상태 조립 및 정보 아키텍처 (Dynamic state & knowledge assembly) |
| **핵심 과제** | 명확한 역할 부여, 출력 포맷 지정, Few-shot 예시 작성 | **KV Cache(Prompt Cache) 적중률 극대화**, 탄력적 스니펫 조립, 컨텍스트 오염 방지 및 압축 |

---

## 핵심 아키텍처 기법 (Core Engineering Techniques)

### 1. KV Cache 친화적인 접두사 불변(Append-Only Prefix) 설계 (`2026-AI 에이전트를 깊이 이해하기` `Ch 3`)
- 자기회귀 Transformer 모델의 **KV Cache(및 상용 API의 Prompt Cache)** 는 이전 턴의 토큰 시퀀스 접두사(Prefix)가 완전히 동일할 때만 재사용되며, **앞부분의 단 1토큰이라도 수정·삭제되면 그 뒤의 모든 캐시가 무효화(Cache Invalidation)** 된다.
- **실전 설계 원칙**:
  1. **시스템 프롬프트와 도구 정의(Tool Schemas) 고정**: 매 턴 바뀌는 현재 시각(`new Date()`)이나 동적 상태값을 시스템 프롬프트 상단에 넣지 않는다.
  2. **Append-Only 메시지 루프**: 이전 턴의 메시지를 중간에서 수정하지 않고 끝에 추가만 한다.
  3. **에이전트 상태 표시줄(Status Bar) 후미 배치**: 현재 작업 진행률, 할 일 목록(`todo`), 열린 파일 상태 등 매 스텝 변하는 메타 정보는 컨텍스트의 **가장 마지막(최신 User/Tool 메시지 끝)** 에 부착하여 앞단 수만 토큰의 KV Cache 적중률을 90% 이상 유지한다.

### 2. 탄력적 스니펫(Elastic Snippets)과 토큰 예산 최적화 (`2024-Prompt Engineering for LLMs` `Ch 5–6`)
- GitHub Copilot 설계자인 John Berryman과 Albert Ziegler가 제시한 **탄력적 스니펫(Elastic Snippets)** 패턴은 반응형 웹 디자인처럼 **제한된 토큰 예산(Token Budget) 안에 가장 가치 있는 정보 조각을 동적으로 맞춰 넣는 알고리즘**이다:
  - 각 컨텍스트 후보(예: 현재 파일, 임포트된 타입 정의, 관련 RAG 문서, 이전 대화)를 여러 해상도(전체 본문 -> 요약/시그니처만 -> 생략)와 우선순위 점수로 정의한다.
  - 프롬프트 조립기(Assembler)가 토큰 한도에 도달할 때까지 우선순위가 높은 스니펫부터 고해상도로 채워 넣고, 덜 중요한 스니펫은 축약 버전을 선택한다.

### 3. 컨텍스트 압축(Context Compression)과 동적 스킬 로딩 (`2026-AI 에이전트를 깊이 이해하기` `Ch 3, Ch 5`)
- **도구 출력 가지치기 및 궤적 요약**: 수십 턴이 이어지는 코딩/리서치 에이전트에서 수천 줄짜리 터미널 로그나 파일 전문이 누적되면 추론력이 급감(Lost-in-the-Middle)한다. 오래된 턴의 상세 도구 출력은 핵심 요약본으로 대체(Compaction)한다.
- **온디맨드 스킬(Skills) 주입**: 수백 개의 도메인 규칙과 도구 가이드를 처음부터 시스템 프롬프트에 다 넣지 않고, 짧은 요약 목록만 보여준 뒤 에이전트가 필요할 때 해당 `SKILL.md`를 읽어 컨텍스트에 동적으로 적재한다.

---

## 에이전트 토폴로지별 컨텍스트 흐름

오케스트레이션 패턴에 따라 컨텍스트 엔지니어링의 핵심 요구사항이 달라진다 ([[2025-Building Applications with AI Agents]], p.127–135):

1. **ReAct (Reasoning + Acting) 에이전트**:
   - 매 추론 사이클마다 `Thought → Action(Tool Call) → Observation(Tool Output)`이 누적된다.
   - 도구 출력이 방대할 경우 핵심 결과만 요약·필터링하여 다음 사이클에 주입해야 한다.
2. **Planner-Executor (계획-실행 분리) 에이전트**:
   - Planner가 수립한 구조화된 계획(Plan)과 현재까지의 진행 상태(Completed Steps)를 Executor의 각 하위 스텝 호출 시 깔끔하게 격리된 컨텍스트로 전달해야 한다.
3. **멀티에이전트 핸드오프 (Multi-Agent Handoffs)**:
   - 모든 에이전트가 전체 대화 이력을 공유하면 토큰 낭비와 역할 혼선이 발생한다. 하위 에이전트에는 해당 서브태스크에 필요한 최소한의 배경 정보와 스키마만 전달한다 ([[Multi-Agent Orchestration]]; [[2026-AI 에이전트를 깊이 이해하기]] Ch 11).

---

## 관련 개념 및 엔티티

- **상위 개념**: [[AI Agent Architecture]], [[Multi-Agent Orchestration]], [[Beyond Vibe Coding and AI-Augmented Software Engineering]]
- **연관 개념**: [[Agent Memory and Knowledge]], [[RAG vs GraphRAG vs LLM Wiki]], [[AI-Enhanced React Applications and Specification-Driven Development]]
- **관련 프로토콜**: [[Model Context Protocol (MCP)]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (Ch 5), [[2026-AI 에이전트를 깊이 이해하기]] (Ch 3, Ch 5), [[2024-Prompt Engineering for LLMs]] (Ch 5–7), [[2025-비욘드 바이브 코딩 (Beyond Vibe Coding)]] (Ch 2–3)
