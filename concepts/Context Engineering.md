---
type: concept
title: "Context Engineering"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
tags:
  - context-engineering
  - orchestration
  - prompt-engineering
  - ai-agents
created: 2026-10-18
updated: 2026-10-18
---

# Context Engineering

## 정의 (Definition)

**컨텍스트 엔지니어링(Context Engineering)** 은 에이전트의 실행 계획(Plan) 내 각 단계마다 모델이 최상의 추론과 행동을 수행할 수 있도록 **사용자 메시지, 시스템 지침, 검색된 외부 지식(Knowledge), 이전 단계의 도구 실행 결과, 워크플로우 상태(State)를 동적이고 토큰 효율적인 형태로 조립하여 컨텍스트 윈도우에 구성하는 기술**이다 ([[2025-Building Applications with AI Agents]], p.111, p.134).

---

## Prompt Engineering vs Context Engineering

[[2025-Building Applications with AI Agents]] (p.134)는 두 개념의 차이를 명확히 구분한다:

| 구분 | 프롬프트 엔지니어링 (Prompt Engineering) | 컨텍스트 엔지니어링 (Context Engineering) |
|---|---|---|
| **초점** | 단일 호출에서 모델의 지시문(Instruction) 문구를 어떻게 잘 작성할 것인가 | 다단계 에이전트 루프에서 **매 시점 어떤 정보와 상태를 선택·압축·구조화해 넣을 것인가** |
| **성격** | 정적 텍스트 최적화 (Static instruction crafting) | 동적 상태 조립 및 정보 아키텍처 (Dynamic state & knowledge assembly) |
| **핵심 과제** | 명확한 역할 부여, 출력 포맷 지정, Few-shot 예시 작성 | 컨텍스트 오염(Context Pollution) 방지, 토큰 예산 관리, 계획-실행 간 상태 브리징 |

---

## 에이전트 토폴로지별 컨텍스트 흐름

오케스트레이션 패턴에 따라 컨텍스트 엔지니어링의 핵심 요구사항이 달라진다 ([[2025-Building Applications with AI Agents]], p.127–135):

1. **ReAct (Reasoning + Acting) 에이전트**:
   - 매 추론 사이클마다 `Thought → Action(Tool Call) → Observation(Tool Output)`이 누적된다.
   - 도구 출력이 방대할 경우(예: 긴 로그나 전체 문서) 그대로 컨텍스트에 쌓으면 추론력이 저하되므로, 핵심 결과만 요약·필터링하여 다음 사이클에 주입해야 한다.
2. **Planner-Executor (계획-실행 분리) 에이전트**:
   - Planner가 수립한 구조화된 계획(Plan)과 현재까지의 진행 상태(Completed Steps)를 Executor의 각 하위 스텝 호출 시 깔끔하게 격리된 컨텍스트로 전달해야 한다.
3. **멀티에이전트 핸드오프 (Multi-Agent Handoffs)**:
   - 모든 에이전트가 전체 대화 이력을 공유하면 토큰 낭비와 역할 혼선이 발생한다. 하위 에이전트에는 해당 서브태스크에 필요한 최소한의 배경 정보와 스키마만 전달한다 ([[Multi-Agent Orchestration]]).

---

## 관련 개념 및 엔티티

- **상위 개념**: [[AI Agent Architecture]], [[Multi-Agent Orchestration]]
- **연관 개념**: [[Agent Memory and Knowledge]], [[RAG vs GraphRAG vs LLM Wiki]]
- **관련 프로토콜**: [[Model Context Protocol (MCP)]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (Ch 5, p.111–135)
