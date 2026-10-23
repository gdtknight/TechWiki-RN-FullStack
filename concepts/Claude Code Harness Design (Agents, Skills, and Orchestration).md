---
type: concept
title: "Claude Code Harness Design (Agents, Skills, and Orchestration)"
category: "G. Artificial Intelligence/Agents"
sources:
  - "[[2026-하네스 엔지니어링 with 클로드 코드]]"
tags:
  - harness-engineering
  - claude-code
  - subagents
  - skills
  - agent-teams
  - generator-verifier
  - guardrails
created: 2026-10-23
updated: 2026-10-23
---

# Claude Code Harness Design (Agents, Skills, and Orchestration)

## 정의 (Definition)

**하네스**는 모델 바깥에서 에이전트가 일하는 환경이다. [[2026-하네스 엔지니어링 with 클로드 코드]]: "하네스란 모델의 가중치를 바꾸지 않고, 모델 바깥의 권한·도구·검증·상태·관측을 설계해 에이전트가 일하게 하는 환경입니다." (p.34) 범위는 "하네스 엔지니어링 ⊇ 컨텍스트 엔지니어링 ⊇ 프롬프트 엔지니어링"이다 (p.35).

이 페이지는 그 하네스를 **Claude Code에서 파일로 만드는 방법**을 다룬다. 책의 설계 원칙을 따르되, 파일 형식과 도구 동작은 2026-10-23 기준 Claude Code 공식 문서로 맞췄다 (책과 다른 점은 [[2026-하네스 엔지니어링 with 클로드 코드]]의 대조표). 하네스라는 개념의 다른 정의와 비교는 [[AI Agent Architecture]].

---

## 1. 왜 프롬프트만으로는 부족한가

책: "프롬프트는 지시이지 강제가 아닙니다. 확률적 시스템에서 지시는 확률적으로만 지켜집니다." (p.36) 같은 규칙을 다른 층에 걸어 두라고 한다: 도구 허용 목록에서 위험한 도구를 빼거나, 실행 전 검증 훅에서 막거나, 리뷰 에이전트의 승인 없이는 쓰기 권한을 주지 않는다 (p.36).

Claude Code 공식 문서에도 같은 층이 있다:
- **도구 제한**: 서브에이전트의 `tools`(허용 목록)와 `disallowedTools`(거부 목록). "If both are set, `disallowedTools` is applied first, then `tools` is resolved against the remaining pool. A tool listed in both is removed." (*Create custom subagents*)
- **훅**: 서브에이전트 프론트매터에 `PreToolUse`·`PostToolUse` 같은 훅을 걸 수 있다 (*Create custom subagents*, "Hooks in subagent frontmatter").

---

## 2. 세 요소: 누가 · 어떻게 · 언제 누구와

책의 핵심 문장: "하네스는 누가(Agent), 어떻게(Skill), 언제·누구와(Orchestrator)라는 세 가지 요소로 나뉘며, 이들은 독립적으로 설계되고 실행 시점에만 맞물린다." (p.58)

| 요소 | 책의 정의 | Claude Code에서의 위치 (공식 문서) |
|---|---|---|
| **에이전트** (누가) | 역할 설정·판단 기준·협업 규칙. "역할 계약서" (p.60, p.65) | `.claude/agents/<name>.md` (프로젝트), `~/.claude/agents/` (사용자) 등 다섯 위치, 이름이 같으면 우선순위가 높은 쪽 |
| **스킬** (어떻게) | 워크플로·트리거·절차적 지식 (p.60, p.65) | `.claude/skills/<skill-name>/SKILL.md` (디렉터리 안의 `SKILL.md`) |
| **오케스트레이터** (언제·누구와) | 팀 구성·작업 의존성·단계 전환. 파일 하나로 고정되지 않고 세션 런타임이 실행 (p.61, p.65) | 스킬 본문의 절차, 또는 에이전트 팀(공유 작업 목록과 메시지) |

**섞였을 때의 문제** (p.59): 재사용 불가, 병렬 불가, 누락 반복(자기 검증 실패), 컨텍스트 폭발. 판단 질문 하나: "이건 역할과 판단 기준의 문제인가, 절차와 워크플로의 문제인가, 팀 구성과 실행 순서의 문제인가?" (p.65)

---

## 3. 에이전트 정의

### 3.1 본문 7개 섹션 (책 p.73–77)

| 섹션 | 무엇을 정하나 |
|---|---|
| 핵심 역할 | 하는 일 1–3개. 하지 않는 일도 함께 경계 지음 |
| 작업 원칙 | 두 선택지가 충돌할 때 어느 쪽을 고를지. 이유를 담고, 이 에이전트만의 특성을 드러냄 |
| 입출력 프로토콜 | 산출물 경로와 형식. "에이전트 간 API 계약" |
| 팀 통신 프로토콜 | 누구에게, 무엇을, 언제 메시지를 보낼지 (팀일 때만) |
| 에러 핸들링 | 실패 시나리오와 **최대 재시도 횟수** (종료 조건) |
| 협업 | 상류·하류 에이전트 |
| 품질 자체 검증 | 제출 전 체크리스트 |

### 3.2 프론트매터에서 자주 쓰는 필드 (공식 문서 기준)

| 필드 | 공식 설명 (*Create custom subagents*) | 고르는 기준 (책) |
|---|---|---|
| `name` | 필수. "The filename doesn't have to match." | — |
| `description` | 필수. "When Claude should delegate to this subagent" | "무엇을 하는가" + "구체적으로 무엇을 다루는가" (p.80) |
| `tools` / `disallowedTools` | 허용 목록 / 거부 목록. `tools`를 생략하면 사용 가능한 도구를 모두 상속 | **부작용 범위**. 읽기 전용 검토자에게는 Write·Edit를 주지 않음 (p.82–83) |
| `model` | `sonnet`, `opus`, `haiku`, `fable`, 전체 모델 ID, `inherit`. 생략하면 정해진 순서로 결정 | **필요한 추론 깊이**. 쓰기 권한 여부와는 무관 (p.80–81) |

`model`을 생략했을 때의 순서 (공식 문서): 호출할 때 넘긴 `model` 인자 → 프론트매터 → `CLAUDE_CODE_SUBAGENT_MODEL` 환경 변수 → 메인 대화의 모델. 책은 결과를 재현하려면 모델을 명시하라고 권한다 (p.46, p.53).

### 3.3 가드레일은 자연어가 아니라 도구 목록으로

책: "본문의 '## 작업 원칙'에 'Write 금지', '민감 파일 접근 금지'라고 써두는 건 프롬프트일 뿐입니다." (p.77) `tools`에서 빼면 그 도구가 아예 보이지 않는다 (p.83). 공식 예시도 같은 방식이다: `tools: Read, Grep, Glob, Bash`면 "The subagent can't edit files, write files, or use any MCP tools" (*Create custom subagents*).

---

## 4. 스킬 설계

### 4.1 description이 호출을 결정한다

공식 문서: "In a regular session, skill descriptions are loaded into context so Claude knows what's available, but full skill content only loads when invoked." (*Skills*) 그래서 description이 모호하면 본문은 읽히지도 않는다. 책의 3단계 공식 (p.95–96):

1. **동사 나열**: 스킬이 실제로 하는 동작 (읽기, 추출, 병합 …)
2. **트리거 상황**: 사용자가 실제로 쓸 표현 (한국어·영어 모두)
3. **경계 조건**: 언제는 이 스킬이 아닌지

길이 제한 (공식 문서): description과 `when_to_use`를 합친 텍스트는 "truncated at 1,536 characters in the skill listing", 목록 전체 예산은 "The budget scales at 1% of the model's context window." 예산을 넘으면 "Claude Code drops descriptions starting with the skills you invoke least" (*Skills*). 스킬이 많을수록 짧고 구체적으로 써야 하는 이유다.

### 4.2 Progressive Disclosure

본문은 핵심만 두고 상세는 참조 파일로 뺀다. 공식 문서: "This keeps `SKILL.md` focused on the essentials while letting Claude access detailed reference material only when needed." (*Skills*, "Add supporting files") 책은 3층으로 정리한다: description(호출할지) → `SKILL.md` 본문(어떻게 수행할지) → references(특정 조건에서만) (p.98). 분리 신호는 본문이 500줄에 가까워질 때, 도메인 분기가 생길 때, 조건부 상세가 생길 때다 (p.98).

### 4.3 규칙보다 이유 (Why-First)

책: 규칙만 읽은 모델은 규칙과 정확히 맞는 상황에서만 적용하지만, 이유를 알면 새 상황에서도 판단할 수 있다 (p.100). 예: "ALWAYS use pdfplumber" 대신 "PyPDF2는 테이블의 행/열 구조를 보존하지 못하기 때문"을 함께 쓴다 (p.100).

### 4.4 `allowed-tools`는 제한이 아니라 허가

공식 문서: `allowed-tools`는 "grants permission for the listed tools during the turn that invokes the skill" 이고, "It does not restrict which tools are available: every tool remains callable". 도구를 빼려면 `disallowed-tools`를 쓴다 (*Skills*, "Pre-approve tools for a skill"). 책은 이 필드를 권한 제한 수단으로 설명하므로 (p.49) 주의한다. 또 이 필드는 작업 공간 신뢰와 상관없이 적용된다: "A skill can grant itself broad tool access, so review the `allowed-tools` of skills checked into a repository before you run Claude Code there." (같은 문서)

---

## 5. 오케스트레이션: 서브에이전트 vs 에이전트 팀

### 5.1 두 실행 모드 (공식 문서)

| | 서브에이전트 | 에이전트 팀 |
|---|---|---|
| 컨텍스트 | "Own context window; results return to the caller" | "Own context window; fully independent" |
| 통신 | 호출한 쪽에 결과 반환 | "Teammates message each other directly" |
| 조율 | "Main agent manages all work" | 메시지 + 공유 작업 목록 |
| 상태 | 정식 기능 | "Agent teams are experimental and disabled by default." (`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`로 켬) |

공식 문서는 팀을 만들기 전에 가벼운 방법을 먼저 보라고 한다: "Before you set up a team, check whether a lighter option does the job." 순차 작업, 같은 파일 수정, 의존성이 많은 일에는 "a single session or subagents are more effective." (*Agent teams*)

### 5.2 팀의 구성 요소와 제약 (공식 문서)

- **팀 만들기**: 팀이 켜져 있으면 Agent 도구 호출에 `name`을 주는 것이 팀원 생성이다. 2.1.178부터 별도의 `TeamCreate`·`TeamDelete` 도구는 없다 (CHANGELOG).
- **구성**: 팀 리더(메인 세션), 팀원, 공유 작업 목록, 메일함. 작업 의존성은 자동 관리된다: "when a teammate completes a task that other tasks depend on, it unblocks the dependent tasks without any action from you."
- **크기**: "Start with 3-5 teammates for most workflows." / "Three focused teammates often outperform five scattered ones."
- **제약**: 세션당 팀 하나, 팀원은 다시 팀원을 만들 수 없음("No nested teams"), 리더는 바뀌지 않음.

### 5.3 설계 원칙 (책)

- **리더는 우체국이 아니다**: 모든 메시지가 리더를 거치면 병렬로 시작한 작업이 다시 순차가 된다 (p.111–112). 팀원끼리 직접 메시지를 보내고, 리더는 알림이 올 때만 반응하는 이벤트 루프처럼 둔다 (p.113). 에이전트 파일의 "팀 통신 프로토콜"에 수평 통신을 적지 않으면 팀원이 리더에게만 보고하는 습관으로 돌아간다 (p.119).
- **무엇으로 전달할까** (p.121): 작고 즉시 쓸 정보는 메시지, 소유자와 상태가 있는 일은 작업 목록, 큰 산출물이나 단계 사이 자료는 파일(`_workspace/`).
- **경계면 검증** (p.129–130): 두 팀원이 각자는 맞아도 메시지 형식이 다르면 합치면 깨진다. 생산자와 소비자의 명세를 **동시에 대조하며** 읽고, 주요 단계 직후에 확인 지점을 둔다.
- **관측** (p.128): 도구 호출 한 번을 JSONL 한 줄로 `PostToolUse` 훅이 기록하면 "언제부터 품질이 떨어졌는가", "어떤 팀원이 문제를 일으키는가", "비용이 어디서 발생하는가"에 답할 수 있다.

---

## 6. 생성-검증과 종료 조건

가장 작은 하네스는 만드는 에이전트와 검사하는 에이전트 둘이다. 책: "'두 명이 한 번씩 보는 것'이 '한 명이 두 번 보는 것'보다 나은 이유가 바로 여기 있습니다." (p.44) 이유는 [[AI Agent Architecture]]에서 말하는 단일 에이전트의 자기 검증 한계와 같다.

필수 규칙 두 가지 (p.47):
1. **판정 기준을 객관적으로**: 형식 이탈·사실 오류일 때만 REDO. 실무에서는 부분 수정용 FIX를 더한 PASS/FIX/REDO 3단계도 쓴다 (p.48, p.74).
2. **재시도 상한**: 보통 2–3회, 넘으면 실패를 숨기지 말고 사람에게 넘긴다 (p.159, p.161).

---

## 관련 개념 및 엔티티

- **상위/연관 개념**: [[AI Agent Architecture]], [[Multi-Agent Orchestration]], [[Context Engineering]], [[Beyond Vibe Coding and AI-Augmented Software Engineering]]
- **구현 기술/엔티티**: [[Model Context Protocol (MCP)]]
- **출처 문헌**: [[2026-하네스 엔지니어링 with 클로드 코드]] (1–6장, 8장), Claude Code 공식 문서 [*Create custom subagents*](https://code.claude.com/docs/en/sub-agents), [*Extend Claude with skills*](https://code.claude.com/docs/en/skills), [*Orchestrate teams of Claude Code sessions*](https://code.claude.com/docs/en/agent-teams), [CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
