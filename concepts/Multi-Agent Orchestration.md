---
type: concept
title: "Multi-Agent Orchestration"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2025-Building Applications with AI Agents]]"
  - "[[2026-하네스 엔지니어링 with 클로드 코드]]"
tags:
  - multi-agent
  - orchestration
  - swarm
  - actor-model
  - distributed-systems
created: 2026-10-18
updated: 2026-10-23
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

### 4. 여섯 가지 팀 패턴 ([[2026-하네스 엔지니어링 with 클로드 코드]] 8장)

원칙 셋 (p.154): 문제의 구조를 먼저 보면 패턴은 따라온다, 실전에서는 단계(Phase)마다 다른 패턴을 섞는다, 계층은 2단계 이내.

| 패턴 | 모양 | 언제 (책) | 주의 (책) |
|---|---|---|---|
| **파이프라인** | 앞 단계 출력이 다음 단계 입력 | 강한 순서 의존 (p.155) | 중간이 느리면 전체 대기, 병렬화 불가 (p.156) |
| **팬아웃·팬인** | 같은 입력을 여러 관점이 동시에, 리더가 통합 | 같은 입력을 다른 관점으로 볼 때 (p.157) | 모든 메시지가 리더를 거치면 순차로 퇴행 (p.157, p.170) |
| **전문가 풀** | 라우터가 N명 중 한 명에게만 보냄 | 입력 유형이 뚜렷이 나뉠 때 (p.159) | 라우터 오분류가 엉뚱한 곳에 수정을 남김. 애매하면 사람에게 (p.158–159) |
| **생성-검증** | 만들고 검사하고 다시 만듦 | 검증 기준이 명확할 때 (p.160) | 재시도 상한(2–3회)과 상한 도달 시 에스컬레이트 필수 (p.159, p.161) |
| **감독자** | 작업 큐에서 워커가 스스로 작업을 가져감(work-stealing) | 작업 수·처리 시간이 들쭉날쭉할 때 (p.163) | 워커를 늘려도 감독자 부담이 비례해 커짐 (p.164) |
| **계층적 위임** | 총괄 → 팀장 → 실무자 | 독립 하위 영역으로 나뉘는 큰 문제 (p.166) | 3단계 이상은 요약·재해석이 누적되어 지연과 의도 손실 (p.165) |

1절의 감독자/계층형은 이 표의 감독자·계층적 위임에, 2절의 스웜은 중앙 감독자가 없다는 점에서 이 표에 직접 대응하는 패턴이 없다 (판단). Claude Code 에이전트 팀에서는 팀원이 다시 팀을 만들 수 없으므로("No nested teams", Claude Code 공식 문서 *Agent teams*), 책은 1단계를 팀으로, 2단계를 서브에이전트로 구성하는 방식을 든다 (p.166). 구현 방법은 [[Claude Code Harness Design (Agents, Skills, and Orchestration)]] 5절.

---

## 관련 개념 및 엔티티

- **상위 개념**: [[AI Agent Architecture]]
- **연관 개념**: [[Context Engineering]], [[Claude Code Harness Design (Agents, Skills, and Orchestration)]]
- **구현 프레임워크**: [[LangGraph]]
- **출처 문헌**: [[2025-Building Applications with AI Agents]] (Ch 2 p.54, Ch 5 p.111, Ch 8 p.185–224), [[2026-하네스 엔지니어링 with 클로드 코드]] (8장 p.152–171)
