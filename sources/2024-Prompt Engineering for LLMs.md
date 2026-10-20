---
type: source
title: "Prompt Engineering for LLMs: The Art and Science of Building Large Language Model-Based Applications"
authors: ["John Berryman", "Albert Ziegler"]
publisher: "O'Reilly Media"
year: 2024
category: "Apple Books/AI & Software Engineering"
epub_path: "/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents/Prompt Engineering for LLMs for Epub True.epub"
status: ingested
tags: [prompt-engineering, llm, context-engineering, github-copilot, tokenization, logprobs, rag, evaluation, agents]
sources: ["[[2024-Prompt Engineering for LLMs]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Prompt Engineering for LLMs

## 1. 핵심 테제 (Core Thesis)
**GitHub Copilot**의 초기 핵심 아키텍트인 **John Berryman**과 **Albert Ziegler**는 프롬프트 엔지니어링을 단순한 '자연어 마법 주문 찾기'가 아니라, **자기회귀(Auto-regressive) 문서 완성 엔진인 LLM의 인지 구조에 맞춰 정적·동적 정보 조각(Snippets)을 제한된 토큰 예산 안에 조립하는 엄밀한 소프트웨어 엔지니어링**으로 정의한다. 토큰화(Tokenization), 확률 분포와 로그 확률(`logprobs`), RLHF 정렬 세금(Alignment Tax) 등 모델 내부 원리부터, 화면 해상도에 맞춰 레이아웃이 조절되듯 컨텍스트 한도에 맞춰 우선순위에 따라 정밀하게 신축되는 **탄력적 스니펫(Elastic Snippets)** 조립 기법, 대화형 에이전트와 워크플로우, 오프라인/온라인 평가까지 Copilot 개발 현장의 노하우를 전수한다.

---

## 2. 챕터별 핵심 요약 (Chapter-by-Chapter Summary)

### Part I. Foundations (LLM의 내부 작동 방식과 애플리케이션 루프)
- **Ch 1–2. Introduction & Understanding LLMs**:
  - LLM의 본질은 '질의응답 기계'가 아니라 **'문서 완성기(Document Completer)'** 다.
  - 인간 사고와 LLM 처리의 3가지 결정적 차이: ① 결정론적 토크나이저(BPE)로 인해 글자 단위 연산에 취약함, ② 한 번에 한 토큰씩(One Token at a Time) 자기회귀적으로 생성하므로 앞선 토큰이 뒤이은 사고를 구속함(따라서 결론보다 추론 과정을 먼저 출력하게 해야 함), ③ 패턴과 반복에 강하게 끌림(Few-shot 포맷 일관성의 중요성).
  - Temperature, Top-p 샘플링 확률 분포와 Transformer 어텐션 메커니즘.
- **Ch 3. Moving to Chat**:
  - SFT(지도 미세 조정)와 보상 모델을 거치는 **RLHF** 파이프라인, 환각 억제와 **정렬 세금(Alignment Tax)**, 그리고 기본 완성(Completion) 모델에서 역할 기반 Chat API(**극작(Playwriting)으로서의 프롬프트 엔지니어링**)로의 진화.
- **Ch 4. Designing LLM Applications**:
  - LLM 애플리케이션 루프 해부: 사용자 문제 영역(Problem Space) -> 컨텍스트 수집 및 프롬프트 조립 -> 모델 순전파(Feedforward Pass) -> 출력 파싱 및 검증 -> 사용자 액션 반영.

### Part II. Core Techniques (프롬프트 콘텐츠 수집, 탄력적 조립, 모델 길들이기)
- **Ch 5. Prompt Content**:
  - 정적 콘텐츠(시스템 지침, 페르소나, 고정 퓨샷)와 동적 콘텐츠(현재 열려 있는 파일/커서 주변 코드, RAG 검색 청크, 대화 히스토리, 도구 호출 결과)의 수집 전략.
- **Ch 6. Assembling the Prompt (핵심 챕터)**:
  - 이상적인 프롬프트 문서의 해부학과 마크다운/XML 태그를 활용한 명확한 섹션 구분.
  - **탄력적 스니펫(Elastic Snippets)**: GitHub Copilot에서 고안된 기법으로, 각 정보 조각(스니펫)을 여러 상세도 수준(예: 전체 함수 코드 -> 함수 시그니처만 -> 생략)과 중요도 가중치로 정의해 두고, **토큰 예산(Token Budget) 한도 내에서 가장 가치 있는 스니펫 조합을 동적으로 채워 넣는 알고리즘**.
- **Ch 7. Taming the Model**:
  - 이상적인 완성 결과 유도(Stop sequences, JSON 스키마 강제), 그리고 텍스트 너머의 신호인 **로그 확률(`logprobs`)** 을 활용해 모델의 확신도(Confidence)를 측정하거나 분류기(Classifier)/라우터로 사용하는 고급 기법.

### Part III. An Expert of the Craft (에이전트, 워크플로우, 평가)
- **Ch 8–9. Conversational Agency & LLM Workflows**:
  - 도구 사용(Tool Usage), 추론(Reasoning), 작업 상태 컨텍스트 관리를 결합한 대화형 에이전트 구축(Ch 8).
  - 언제 단일 대화형 에이전트로 충분하고 언제 다단계 결정론적/동적 **LLM 워크플로우(체인, 라우팅, 병렬 서브태스크 분해)** 로 분리해야 하는가(Ch 9).
- **Ch 10–11. Evaluating LLM Applications & Looking Ahead**:
  - GitHub Copilot 사례를 중심으로 한 **오프라인 평가(골든 데이터셋, 루브릭 기반 LLM-as-a-Judge, 회귀 테스트)** 와 **온라인 평가(수락률 Acceptance Rate, 사용자 잔류율, A/B 테스트)** 체계(Ch 10) 및 멀티모달 전망(Ch 11).

---

## 3. 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Context Engineering]] — 정적·동적 콘텐츠 수집, 토큰 예산에 맞춘 **탄력적 스니펫(Elastic Snippets)** 조립, 마크다운/XML 구조화 및 `logprobs` 활용 (Ch 5–7)
- [[Beyond Vibe Coding and AI-Augmented Software Engineering]] — LLM의 문서 완성기 본질과 토큰 단위 자기회귀 생성 특성 이해, 오프라인/온라인 평가 파이프라인 (Ch 2–4, Ch 10)
- [[AI Agent Architecture]] — 대화형 에이전시(도구+추론) vs 다단계 LLM 워크플로우 아키텍처 선택 기준 (Ch 8–9)
