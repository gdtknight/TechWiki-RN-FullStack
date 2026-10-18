---
type: concept
title: "Agent Memory and Knowledge"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
tags:
  - memory
  - rag
  - graphrag
  - nonparametric-learning
  - ai-agents
created: 2026-10-18
updated: 2026-10-18
---

# Agent Memory and Knowledge

## 정의: 지식(Knowledge)과 메모리(Memory)의 구분

[[2025-Building Applications with AI Agents]] (Ch 6, p.137)는 에이전트의 컨텍스트를 풍부하게 만드는 두 축으로 **Knowledge**와 **Memory**를 엄격히 구분한다:

- **Knowledge (외부 도메인 지식)**:
  - 모델 가중치(Weights)나 현재 대화 밖의 **검증 가능한 사실·도메인 문서**(기술 명세서, 정책 문서, 제품 카탈로그, 코드베이스, 기술 서적 등)를 생성 시점에 끌어오는 것.
  - 주로 RAG(Retrieval-Augmented Generation), **GraphRAG**, 또는 **LLM Wiki** 형태로 구현된다.
- **Memory (에이전트 경험 및 상태 기억)**:
  - 에이전트 **자신의 과거 이력**(이전 사용자 대화, 도구 호출 결과, 실패/성공 경험, 사용자 선호도, 중간 작업 상태)을 포착하고 유지하는 것.

---

## 메모리의 계층 구조

1. **단기 메모리 (Short-Term / Working Memory, p.47, p.138)**
   - 현재 세션이나 태스크 실행 중에 유지되는 컨텍스트 윈도우 내부 상태(최근 메시지 버퍼, 현재 플랜 상태).
2. **장기 메모리 (Long-Term Memory, p.48, p.141)**
   - 세션을 넘어 영속화되는 기억 계층.
   - **시맨틱 경험 메모리 (Semantic Experience Memory, p.145)**: 과거 상호작용과 해결 사례를 임베딩하여 벡터 스토어에 저장해 두고, 유사한 쿼리가 들어왔을 때 컨텍스트 윈도우의 일부 슬롯에 과거의 성공/실패 경험을 주입해 적응형 행동을 유도한다.

---

## 학습과의 연결: 비모수 학습 vs 모수 학습

Ch 7 (p.157–184)에서는 메모리의 축적을 에이전트의 **학습(Learning)** 관점에서 확장한다:

- **비모수 학습 (Nonparametric Learning, p.157)**:
  - 모델의 가중치(파라미터)를 재학습하지 않고, **메모리 축적·리플렉션(Reflexion)·프롬프트/위키 갱신**을 통해 에이전트의 작업 수행 능력을 지속적으로 향상시키는 방식.
  - 비용이 낮고 즉각 반영되며 설명 가능성이 높다.
- **모수 학습 (Parametric Learning: Fine-Tuning, p.168)**:
  - SFT(지도 미세조정), PEFT/LoRA, RLHF/DPO 등을 통해 도메인 지식이나 행동 양식을 모델 가중치 자체에 내재화하는 방식.

> [!IMPORTANT]
> 프로덕션 시스템에서는 먼저 [[Context Engineering]]과 **비모수 학습(메모리 및 지식 베이스 고도화)** 으로 문제를 해결하고, 지연 시간 단축이나 특정 출력 스타일 고정이 꼭 필요한 병목 구간에만 모수 학습(Fine-Tuning)을 적용하는 것이 효과적이다.

---

## 관련 개념 및 엔티티

- **상위 개념**: [[AI Agent Architecture]]
- **연관 분석**: [[RAG vs GraphRAG vs LLM Wiki]], [[Context Engineering]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (Ch 6 p.137–156, Ch 7 p.157–184)
