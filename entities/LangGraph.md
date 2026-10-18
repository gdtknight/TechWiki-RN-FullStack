---
type: entity
title: "LangGraph"
entity_type: "framework"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
tags:
  - langgraph
  - langchain
  - agent-framework
  - orchestration
created: 2026-10-18
updated: 2026-10-18
---

# LangGraph

## 개요

**LangGraph**는 LangChain 생태계 위에 구축된 **상태 기반 그래프(Stateful Graph) 에이전트 오케스트레이션 프레임워크**로, 순환(Cycles/Loops), 조건부 분기(Conditional Branching), 영속적 상태 관리(Persistence/Checkpointing), Human-in-the-loop 제어 흐름을 명시적인 그래프 구조로 모델링한다 ([[2025-Building Applications with AI Agents]], Ch 1 p.35, Ch 4 p.94).

---

## 주요 에이전트 프레임워크 비교

[[2025-Building Applications with AI Agents]] (Ch 1, p.35–36)에서 소개하는 대표적 에이전트 프레임워크들의 포지셔닝 비교:

| 프레임워크 | 핵심 추상화 모델 | 강점 및 주요 특징 | 적합한 유스케이스 |
|---|---|---|---|
| **[[LangGraph]]** | 상태 머신 / 방향 그래프 (Nodes & Edges) | 루프 제어, 세밀한 상태 전이, 높은 제어력, Human-in-the-loop 체크포인팅 | 프로덕션 워크플로우-에이전트 하이브리드, 정교한 [[Context Engineering]] |
| **AutoGen** | 대화형 멀티에이전트 (Conversational Agents) | 에이전트 간 대화/토론 기반 협업, 코드 실행 루프 내장 | 멀티에이전트 브레인스토밍, 자율 코딩 및 문제 해결 |
| **CrewAI** | 역할 기반 팀 (Role-based Crew) | 직관적인 역할(Role)·목표(Goal)·백스토리 정의, 빠른 프로토타이핑 | 역할 분담이 명확한 콘텐츠/리서치 파이프라인 |
| **OpenAI Agents SDK** | 경량 Primitives (Agents, Handoffs, Guardrails) | 최소한의 보일러플레이트, 에이전트 간 핸드오프와 가드레일 기본 제공 | 빠른 에이전트 애플리케이션 구축 및 간결한 멀티에이전트 위임 |

---

## 관련 링크

- **연관 개념**: [[AI Agent Architecture]], [[Multi-Agent Orchestration]], [[Context Engineering]]
- **연관 엔티티**: [[Model Context Protocol (MCP)]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (p.35–36, p.94)
