---
type: source
title: "Building Applications with AI Agents"
authors:
  - "Michael Albada"
publisher: "O'Reilly Media"
year: 2025
category: "G. Artificial Intelligence/Agents"
pdf_path: "/Users/yoshin/Documents/MarginNote4/G. Artificial Intelligence/Agents/2025-Building Applications with AI Agents-Michael Albada-O'Reilly Media.pdf"
status: ingested
tags:
  - ai-agents
  - agentic-systems
  - orchestration
  - context-engineering
  - memory
  - graphrag
  - multi-agent
sources: ["[[2025-Building Applications with AI Agents]]"]
created: 2026-10-18
updated: 2026-10-18
---

# Building Applications with AI Agents (2025)

> **서지 정보**: Michael Albada | O'Reilly Media (2025) | 355 Pages
> **분류**: `G. Artificial Intelligence / Agents`
> **원천 PDF**: [2025-Building Applications with AI Agents-Michael Albada-O'Reilly Media.pdf](file:///Users/yoshin/Documents/MarginNote4/G.%20Artificial%20Intelligence/Agents/2025-Building%20Applications%20with%20AI%20Agents-Michael%20Albada-O'Reilly%20Media.pdf)

---

## 핵심 요약 (Core Thesis)

사전학습(Pretraining) 혁명으로 파운데이션 모델의 추론 능력이 비약적으로 향상되면서, 단일 프롬프트-응답 패러다임을 넘어 **목표를 향해 스스로 계획하고 도구를 호출하며 상태를 유지하는 에이전트 시스템([[AI Agent Architecture]])** 이 소프트웨어 엔지니어링의 새로운 표준 아키텍처로 자리 잡고 있다.

저자 Michael Albada는 프로덕션 수준의 에이전트 시스템을 구축하기 위해 필요한 **4대 핵심 구성요소(Model, Tools, Memory, Orchestration)** 와 함께, UX 설계(Autonomy Slider), [[Context Engineering]], 지식/메모리 확장([[Agent Memory and Knowledge]]), 단일 에이전트에서 멀티에이전트로의 진화([[Multi-Agent Orchestration]]), 그리고 검증·모니터링·보안·인간-에이전트 협업까지 엔드투엔드 엔지니어링 원칙을 체계적으로 제시한다.

---

## 챕터별 핵심 내용 요약

### Part I: 기초와 시스템 설계 (Ch 1 – Ch 3)
- **Ch 1. Introduction to Agents (p.23–37)**
  - **에이전트 정의**: 환경을 인식하고, 목표 달성을 위해 의사결정을 내리며, 도구 호출을 통해 외부 환경에 작용하는 자율/반자율 시스템.
  - **결정론적 워크플로우(Workflows) vs 에이전트(Agents) (p.30)**: 사전에 고정된 경로를 따르는 워크플로우와 달리, 에이전트는 런타임에 실행 경로와 도구 호출 순서를 동적으로 결정한다.
  - 주요 프레임워크 비교: [[LangGraph]], AutoGen, CrewAI, OpenAI Agents SDK (p.35).
- **Ch 2. Designing Agent Systems (p.39–61)**
  - 에이전트 시스템의 4대 핵심 축: **모델 선택(Model Selection)**, **도구(Tools)**, **메모리([[Agent Memory and Knowledge]])**, **오케스트레이션([[Multi-Agent Orchestration]])** (p.42–49).
  - 도구 연동 표준으로서의 [[Model Context Protocol (MCP)]] 소개 (p.48).
  - 4대 설계 트레이드오프: 속도/정확도(Performance), 확장성(Scalability), 신뢰성(Reliability), 비용(Cost) (p.49–53).
- **Ch 3. User Experience Design for Agentic Systems (p.63–90)**
  - 텍스트·GUI·음성·비디오 멀티모달 인터랙션과 **자율성 슬라이더(The Autonomy Slider, p.77)**: 작업의 위험도와 사용자의 신뢰 수준에 따라 에이전트의 자율성 범위를 조절하는 UX 패턴.
  - 동기(Synchronous) vs 비동기(Asynchronous) 에이전트 경험 설계 및 불확실성/신뢰도 전달 방식 (p.80–88).

### Part II: 도구, 오케스트레이션, 메모리, 학습 (Ch 4 – Ch 7)
- **Ch 4. Tool Use (p.93–110)**
  - 로컬 도구, API 기반 도구, [[Model Context Protocol (MCP)]] 기반 도구 설계 및 자동화된 도구 개발(Automated Tool Development).
- **Ch 5. Orchestration (p.111–135)**
  - 에이전트 실행 토폴로지(ReAct, Planner-Executor, DAG 등)와 도구 선택/실행 전략.
  - **[[Context Engineering]] (p.134)**: 단순 프롬프트 작성을 넘어, 매 스텝마다 사용자 의도·검색된 지식·워크플로우 상태·도구 실행 결과를 토큰 효율적으로 조립하여 모델 컨텍스트 윈도우에 주입하는 엔지니어링.
- **Ch 6. Knowledge and Memory (p.137–156)**
  - **지식(Knowledge)과 메모리(Memory)의 구분 (p.137)**: 외부 사실/도메인 문서를 가져오는 Knowledge(RAG)와 에이전트 자체의 상호작용 이력·경험을 축적하는 Memory(단기/장기/시맨틱 경험 메모리).
  - 복합 추론과 관계 탐색을 위한 **GraphRAG** 아키텍처 (p.145) — [[RAG vs GraphRAG vs LLM Wiki]] 참조.
- **Ch 7. Learning in Agentic Systems (p.157–184)**
  - 가중치 수정 없이 프롬프트·메모리·리플렉션(Reflexion)·경험 축적으로 성능을 높이는 **비모수 학습(Nonparametric Learning)** 과 SFT/RLHF/PEFT/DPO 기반의 **모수 학습(Parametric Learning)** 비교.

### Part III: 멀티에이전트와 프로덕션 운영 (Ch 8 – Ch 13)
- **Ch 8. From One Agent to Many (p.185–224)**
  - 모놀리스에서 마이크로서비스로 분리하듯, 도구 수와 문제 영역이 커질 때 단일 에이전트를 멀티에이전트로 분할하는 기준 (p.185).
  - 중앙집중형 감독자(Supervisor) 패턴 vs 탈중앙화 자율 조직인 **스웜(Swarm) 아키텍처 (p.200)**.
  - 메시지 브로커/이벤트 버스(p.214), 액터 프레임워크(Ray, Orleans, Akka, p.217), Meta-Agent Search(p.206).
- **Ch 9 – Ch 11. Validation, Monitoring, Improvement Loops (p.227–290)**
  - 컴포넌트 단위 평가와 엔드투엔드 평가(Ch 9), OpenTelemetry(OTel) 계측과 분포 이동(Distribution Shift) 감지(Ch 10), 피드백 파이프라인과 실험 루프(Ch 11).
- **Ch 12. Protecting Agentic Systems (p.293–318)**
  - 프롬프트 인젝션, 멀티에이전트 메모리 포이즈닝(Memory Poisoning), 권한 상승 등 에이전트 고유의 보안 위협과 방어 계층(Guardrails, 샌드박싱, 최소 권한 원칙).
- **Ch 13. Human-Agent Collaboration (p.319–334)**
  - 인간의 역할 진화: 직접 실행자(Executor)에서 감독자/거버너(Governor)로의 전환과 점진적 위임(Progressive Delegation, p.319).

---

## 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[AI Agent Architecture]] — 에이전트 4대 구성요소, 워크플로우 vs 에이전트, 자율성 슬라이더, 설계 트레이드오프
- [[Context Engineering]] — 프롬프트 엔지니어링을 넘어선 동적 컨텍스트 조립 및 상태 전달
- [[Agent Memory and Knowledge]] — 단기/장기 메모리, 시맨틱 경험 메모리, RAG/GraphRAG, 비모수/모수 학습
- [[Multi-Agent Orchestration]] — 단일 vs 멀티에이전트 분리 기준, Supervisor vs Swarm, 액터 모델 및 이벤트 버스

### Entities
- [[Model Context Protocol (MCP)]] — 외부 지식·도구·상태를 모델 프롬프트에 주입하기 위한 표준 프로토콜
- [[LangGraph]] — 상태 기반 그래프 오케스트레이션 프레임워크 (vs AutoGen, CrewAI, OpenAI Agents SDK)

### Syntheses
- [[RAG vs GraphRAG vs LLM Wiki]] — 이 책의 Ch 6(Knowledge & Memory)와 LLM Wiki 패러다임의 구조적 비교 분석
