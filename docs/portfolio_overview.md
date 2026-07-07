# Portfolio Overview

## 한 줄 소개

LLM/RAG Agent, 시계열 파운데이션 모델, AI-OCR, MCP Tool 연동, Hybrid Symbolic RL을 서비스 구조로 연결하고 검증하는 Python 기반 AI Application Engineer 포트폴리오입니다.

## 핵심 역량

### LLM / RAG / Agent

- LangGraph 기반 Agentic RAG 구조 설계
- Self-Correction Loop를 활용한 RAG 환각 저감 구조 검토
- MCP Tool, Resource, Prompt 기반 외부 API 도구화
- LLM 기반 query planning, RAG generation, feedback/FAQ flow 설계

### Time Series AI

- Moirai, Chronos, TimeCopilot 기반 시계열 분석 PoC
- 대형 시계열 파운데이션 모델 추론 병목 분석
- DeepSpeed 기반 데이터 분산 처리 및 추론 최적화 시도

### AI-OCR

- PaddleOCR와 VLM을 결합한 문서 구조화 추출 파이프라인 설계
- OCR evidence, bbox 복원, 합계 검증, artifact 저장 구조 검토
- 검증 가능한 AI-OCR 결과와 디버깅 가능한 artifact 설계

### Reinforcement Learning / Simulation

- LLM + RL 기반 전력시장 입찰 최적화 PoC
- 초기 GRPO 코드 생성형 접근의 한계 분석
- Hybrid Symbolic RL 구조로 LLM과 SAC 역할 분리
- Two-Settlement 시장 환경, DA/RT 분리 학습, requested/executed action 추적 구조 설계

### AI-assisted Development

- 요구사항 정의 → 시스템 설계 → 구현 계획 → 코드 생성 → 코드 리뷰 → 테스트 → 성능 검증 흐름 관리
- CUPID 관점으로 Tool 책임, 입출력 계약, 예측 가능성, 도메인 기반 구조 검토

## 프로젝트 요약

| Project | Type | Core Value |
|---|---|---|
| Self-Correcting RAG | LLM/RAG Agent | RAG 답변 검증과 재생성 루프 설계 |
| Time Series AI Platform | Time Series AI / MLOps | 시계열 모델 실행, 비교, 분석, 최적화 구조 설계 |
| AI-OCR Pipeline | OCR / VLM | OCR evidence와 VLM 구조화 추출 결합 |
| Hybrid Symbolic RL | RL / Simulation | LLM은 전략 해석, SAC는 수치 최적화로 역할 분리 |
| MCP RAG Bridge | MCP / Agent Tooling | 기존 RAG API를 MCP Tool/Resource/Prompt로 노출 |

## 문서 구성 원칙

이 저장소는 세 단계로 관리합니다.

1. 최상위 README: 전체 포지셔닝과 프로젝트 목록
2. 프로젝트별 README: 문제, 목표, 역할, 결과 요약
3. 프로젝트별 docs: Mermaid 기반 상세 시스템 아키텍처와 기술 설계

## 공개 문서화 기준

- 내부 서버 주소, API 키, 실제 데이터 경로는 기록하지 않습니다.
- 내부 모델 checkpoint 경로는 일반화합니다.
- 구체적인 비공개 고객사 정보는 제거합니다.
- 핵심 설계 의도, 구조, 한계, 개선 방향은 공개 가능한 수준으로 정리합니다.
