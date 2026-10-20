---
type: concept
title: "Multi-Agent Orchestration"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
tags:
  - multi-agent
  - orchestration
  - swarm
  - actor-model
  - distributed-systems
created: 2026-10-18
updated: 2026-10-18
---

# Multi-Agent Orchestration

## 언제 단일 에이전트에서 멀티에이전트로 전환하는가?

소프트웨어 공학에서 모든 코드를 단일 파일이나 거대한 모놀리스 서버에 넣지 않듯, 에이전트 시스템 역시 **연결된 도구(Tools)의 개수가 많아지고 해결해야 할 문제의 도메인 범위가 넓어질수록** 단일 에이전트의 도구 선택 정확도와 컨텍스트 일관성이 급격히 떨어진다 ([[2025-Building Applications with AI Agents]], Ch 8 p.185).

이때 시스템을 **독립적으로 검증·테스트·통합·재사용 가능한 소규모 전문 에이전트들로 분할**하면 전체 성능과 신뢰성을 높일 수 있다.

---

## 주요 멀티에이전트 아키텍처 패턴

### 1. 중앙집중형 / 계층형 (Supervisor / Hierarchical Pattern)
- 상위 감독자(Supervisor/Orchestrator) 에이전트가 사용자 요청을 분해하여 하위 전문 에이전트(Researcher, Coder, Reviewer 등)에게 위임하고 결과를 취합한다.
- 흐름 제어와 감사(Audit)가 명확하지만 중앙 오케스트레이터가 단일 장애점(SPOF)이나 병목이 될 수 있다.

### 2. 탈중앙화 스웜 아키텍처 (Swarm Architecture, p.200)
- 중앙 감독자 없이 각 에이전트가 로컬 정책(Local policies)과 이웃 에이전트와의 신호 교환만으로 자기조직화(Self-organization)하는 패턴이다.
- **장점**:
  - **확장성(Scalability)**: 느슨한 결합(Loose coupling) 덕분에 조율 오버헤드 없이 수백–수천 개 에이전트로 확장 가능.
  - **강건성(Robustness)**: 단일 장애점(SPOF)이 없어 개별 에이전트가 실패해도 전체 시스템이 유지됨.

### 3. 분산 시스템 인프라와의 결합 (p.214–223)
멀티에이전트 시스템은 본질적으로 분산 시스템이므로 기존 분산 컴퓨팅 인프라 패턴이 그대로 적용된다:
- **메시지 브로커 & 이벤트 버스 (Message Brokers and Event Buses, p.214)**: 에이전트 간 비동기 통신 및 디커플링.
- **액터 프레임워크 (Actor Frameworks, p.217)**: **Ray**, **Orleans**, **Akka** 등을 활용해 상태를 가진 에이전트(Actor)의 생명주기와 메시지 패싱을 대규모로 관리.
- **Meta-Agent Search (MAS, p.206, p.339)**: 다양한 에이전트 아키텍처·컴포넌트·구성을 자동으로 탐색하고 평가해 최적의 에이전트 구성을 찾아내는 기법.

---

## 관련 개념 및 엔티티

- **상위 개념**: [[AI Agent Architecture]]
- **연관 개념**: [[Context Engineering]]
- **구현 프레임워크**: [[LangGraph]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (Ch 2 p.54, Ch 5 p.111, Ch 8 p.185–224)
