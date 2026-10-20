---
type: source
title: "Open Source Observability: A Practical Guide — Disaggregating the Observability Stack for Scale, Cost, and Flexibility"
authors: ["Peter Corless", "Neha Pawar"]
publisher: "O'Reilly Media"
year: 2025
category: "Apple Books/DevOps & Cloud"
epub_path: "/Users/yoshin/Library/Mobile Documents/iCloud~com~apple~iBooks/Documents/Open Source Observability A Practical Guide for .epub"
status: ingested
tags: [observability, opentelemetry, prometheus, grafana, kafka,clickhouse, apache-pinot, high-cardinality, devops, ai-observability]
sources: ["[[2025-Open Source Observability]]"]
created: 2026-10-19
updated: 2026-10-19
---

# Open Source Observability: A Practical Guide

## 1. 핵심 테제 (Core Thesis)
Charity Majors가 서문을 쓴 이 책에서 **Peter Corless**와 **Neha Pawar**는 에이전트·스토리지·대시보드가 단일 상용 벤더에 종속된 고비용 모놀리식 모니터링(Observability 1.0)의 한계를 지적하고, 수집·스트리밍·저장·시각화 계층을 오픈소스 표준으로 분리(Disaggregation)하는 **분리형 관측성 스택(Disaggregated Observability Stack, Observability 2.0)** 을 제시한다. 특히 **OpenTelemetry(OTel)** 로 계측을 표준화하고, **Kafka** 등 스트리밍 계층과 고카디널리티(High Cardinality)·고차원(High Dimensionality) 실시간 OLAP 스토리지(Apache Pinot, ClickHouse 등), 그리고 **Grafana** 시각화를 조합하여 데이터 주권과 대규모 비용 효율성을 동시에 확보하는 아키텍처를 다룬다.

---

## 2. 챕터별 핵심 요약 (Chapter-by-Chapter Summary)

- **Ch 1–2. The Evolution of Observability & Key Benefits of a Disaggregated Stack**:
  - 전통적인 모놀리식 상용 APM/관측성 스택의 문제점: 벤더 종속(Vendor Lock-in), 메트릭·로그·트레이스 사일로(Silo) 분리로 인한 데이터 중복 과금, 카디널리티 급증 시 폭발하는 비용.
  - **분리형 스택(Disaggregated Stack)의 3대 이점**: ① 계층별 최적 도구 선택의 유연성, ② 텔레메트리 데이터를 제품 분석·보안·AI 학습에 재사용하는 데이터 주권(Data Autonomy), ③ 스토리지·컴퓨팅 분리를 통한 대규모 비용 절감.
- **Ch 3. Core Layers of a Disaggregated Stack**:
  - 분리형 관측성 아키텍처의 3계층:
    1. **수집 및 계측 계층(Instrumentation & Collection)**: OpenTelemetry SDK/Collector, Fluent Bit, Vector.
    2. **스토리지 및 쿼리 엔진 계층(Storage & Query)**: Prometheus(시계열), Elasticsearch/OpenSearch, 또는 메트릭·로그·트레이스를 통합 처리하는 실시간 분석 DB(Apache Pinot, ClickHouse).
    3. **시각화·분석·자동화 계층(Visualization & Analysis)**: Grafana, Kibana, Superset 및 이상 탐지 자동화.
- **Ch 4. Implementing the Collection Layer**:
  - 메트릭(Metrics), 로그(Logs), 트레이스(Traces)의 데이터 볼륨과 다양성 관리.
  - **OpenTelemetry Collector**와 **Apache Kafka / Pulsar** 같은 스트리밍 시스템을 결합해 텔레메트리 폭주 시 배압(Backpressure)을 흡수하고, 스트림 처리(Flink 등)로 민감 정보(PII) 마스킹·사전 집계·필터링을 수행하는 파이프라인.
- **Ch 5. Optimizing Storage and Query for Performance**:
  - 모던 마이크로서비스·컨테이너 환경의 최대 난제인 **고카디널리티(High Cardinality: 수억 개의 `userId`, `requestId`, `podId`, `traceId`)** 와 **고차원(High Dimensionality: 수백 개의 JSON 속성/컨텍스트 필드)** 쿼리 성능 최적화.
  - 역색인(Inverted Index), 스타 트리(Star-Tree) 인덱스, JSON 인덱스 및 컬럼형 압축(ZSTD, LZ4) 기법.
- **Ch 6. Best Practices for Visualization**:
  - **Grafana**를 중심으로 한 통합 대시보드 설계 원칙, 메트릭 이상 징후에서 관련 트레이스와 로그로 즉시 드릴다운(Drill-down)하는 상관관계 분석 워크플로우.
- **Ch 7. Future Trends in Observability**:
  - 시맨틱 컨벤션과 텔레메트리 스키마를 코드로 관리하는 Rust 기반 **OTel Weaver**.
  - **관측성을 위한 AI(AI for Observability: 근본 원인 분석 RCA 자동화, 이상 탐지)** 와 **AI를 위한 관측성(Observability for AI / LLM Observability: 토큰 사용량, 프롬프트/응답 지연 시간, 환각 및 에이전트 궤적 추적)** 의 양방향 결합.

---

## 3. 연결된 위키 페이지 (Extracted Nodes)

### Concepts
- [[Full-Stack Authentication, Token Refresh, and Observability]] — 모놀리식 APM을 넘어선 분리형 오픈소스 관측성 스택(OpenTelemetry + Kafka + 고카디널리티 스토리지 + Grafana) 및 AI/LLM 관측성 (Ch 1–7)
- [[Container Infrastructure, Kubernetes, and CI-CD Pipelines]] — 쿠버네티스/컨테이너 환경에서의 고카디널리티 텔레메트리 수집 및 Prometheus/Grafana 연동 (Ch 3–6)
- [[AI Agent Architecture]] — 프로덕션 AI 에이전트 및 LLM 파이프라인의 관측 가능성(Observability for AI) (Ch 7)
