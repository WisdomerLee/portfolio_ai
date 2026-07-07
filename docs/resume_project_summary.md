# Resume Project Summary

이 문서는 점핏/사람인/이력서에 옮기기 쉬운 프로젝트 요약입니다.

## 1. LangGraph 기반 자가 교정형 RAG 서비스 PoC

- RAG 응답의 환각을 줄이기 위해 검색 → 답변 생성 → 검증 → 재생성 흐름의 Self-Correction Loop를 설계했습니다.
- LangGraph 기반 Agent Workflow를 구성하여 검색, 생성, 검증 단계를 분리했습니다.
- 단순 RAG가 아닌 Agentic RAG 구조로 PoC를 구현하고, 답변 품질을 구조적으로 검토하는 방식을 정리했습니다.

## 2. 시계열 파운데이션 모델 Moirai 추론 최적화

- 단일 A100 GPU에서 메모리 부족으로 추론이 어려운 Moirai 모델에 대해 DeepSpeed 기반 분산 처리 가능성을 검토했습니다.
- 모델 분산은 구조적 제약으로 완전 성공하지 못했으나, 데이터 처리 및 실행 파이프라인 개선을 통해 추론 시간을 약 40% 단축했습니다.
- GPU 메모리 병목, 추론 로그, 실행 시간 기반 성능 분석을 수행했습니다.

## 3. TimeCopilot 기반 시계열 분석 통합 PoC

- Moirai, Chronos 등 시계열 모델을 TimeCopilot과 연동하여 예측 결과를 LLM 기반 자연어 분석과 연결했습니다.
- 그래프와 자연어 설명을 함께 제공하는 시계열 분석 Copilot 구조를 검토했습니다.
- 시계열 모델 결과를 사용자 친화적인 분석 서비스로 확장하는 방향성을 확인했습니다.

## 4. AI-OCR 문서 처리 파이프라인 PoC

- PaddleOCR와 VLM을 결합해 문서 구조화 추출 PoC를 수행했습니다.
- OCR line evidence를 VLM 추출 결과의 ocrIds와 연결하여 bbox, raw text, confidence를 복원하는 구조를 설계했습니다.
- 합계 검증, validation artifact, trace/raw/result 저장 구조를 통해 디버깅 가능한 AI-OCR 파이프라인을 구성했습니다.

## 5. LLM·강화학습 기반 발전소 수익 최적화 시뮬레이션 PoC

- 발전량 기반 전력 판매 수익 극대화를 목표로 LLM+강화학습 구조를 실험했습니다.
- 초기에는 LLM이 Python 전략 코드를 직접 생성하고 GRPO로 학습하는 방식을 시도했으나, 코드 안정성, reward 설계, 전력시장 제약 반영에 한계를 확인했습니다.
- 이후 LLM은 시장 해석과 전략 가이드라인을 생성하고, SAC 기반 RL optimizer가 실제 수치 행동을 결정하는 Hybrid Symbolic RL 구조로 재설계했습니다.

## 6. FastMCP 기반 MCP RAG Bridge PoC

- 기존 RB Agent RAG API를 MCP Tool, Resource, Prompt 형태로 노출하는 FastMCP 기반 브리지 서버를 구현했습니다.
- `ask_rag_agent`, 파일 업로드, 문서 검색, 분해형 query stage, feedback/FAQ tool을 MCP 인터페이스로 제공했습니다.
- CUPID 관점으로 Tool 책임, 입출력 계약, session state, adapter boundary를 정리했습니다.

## 이력서 상단 요약 예시

```text
Python 기반 AI Application Engineer로서 LLM/RAG Agent, 시계열 파운데이션 모델, AI-OCR, MCP Tooling, Hybrid Symbolic RL PoC를 수행했습니다. LangGraph 기반 자가 교정형 RAG, PaddleOCR+VLM 기반 문서 추출, DeepSpeed 기반 Moirai 추론 최적화, FastMCP 기반 MCP 서버 구조를 설계하고 검증했습니다. AI Coding Assistant를 활용하되 요구사항 정의, 아키텍처 설계, 코드 검토, 테스트, 성능 분석을 통해 PoC를 유지보수 가능한 구조로 정리하는 데 집중했습니다.
```
