# Time Series AI Platform PoC

## Overview

Moirai, Chronos, TimeCopilot 등 시계열 파운데이션 모델을 활용한 시계열 분석 플랫폼 PoC입니다.

이 프로젝트는 시계열 모델 실행, 성능 비교, 그래프 시각화, LLM 기반 자연어 분석, 대형 모델 추론 최적화를 함께 검토했습니다.

## Problem

- 시계열 파운데이션 모델은 실행 구조와 의존성이 복잡함
- 단일 GPU 환경에서 대형 모델 추론 시 메모리 병목이 발생할 수 있음
- 수치 예측 결과만으로는 비전문 사용자가 해석하기 어려움

## Goal

- Moirai/Chronos 계열 모델을 통합 실행하는 분석 흐름 검토
- DeepSpeed 기반 분산 처리와 추론 시간 최적화 시도
- TimeCopilot을 통해 예측 결과를 LLM 자연어 분석과 연결

## My Role

- 시계열 모델 실행 구조 분석
- Moirai 추론 병목 및 GPU 메모리 이슈 분석
- DeepSpeed 기반 데이터 분산 처리 및 모델 분산 시도
- TimeCopilot 기반 LLM 분석 인터페이스 PoC 검토
- 추론 시간, 성능 결과, 한계 문서화

## Tech Stack

- Python
- PyTorch
- DeepSpeed
- Moirai
- Chronos
- TimeCopilot
- Pandas
- Matplotlib / Plotly
- Streamlit
- FastAPI
- Redis / RQ
- MLflow
- A100 GPU

## Core Implementation Logic

- CSV 또는 시계열 입력 데이터를 표준 schema로 정규화
- 모델 alias 기반으로 Moirai/Chronos 등 forecaster를 생성
- Cross-validation을 수행하고 모델별 metric을 계산
- MAPE, MAE, MASE, MSE, cMAPE 등 평가 지표 산출
- 예측 결과를 plot artifact로 변환
- TimeCopilot과 LLM을 연결해 예측 결과를 자연어로 분석
- 대형 모델 추론 시 batch 구성, device mapping, 분산 처리 가능성 검토

## Architecture Pattern

- ForecastPipeline 중심 pipeline architecture
- Model factory pattern
- Metric strategy registry
- Plot strategy registry
- Runtime cache bundle
- Streamlit UI와 FastAPI API의 dual interface 구조
- Redis/RQ 기반 async job orchestration
- MLflow 또는 file backend 기반 observability/cache 확장 구조

## Result

- 모델 분산은 당시 구조적 제약으로 완전 성공하지 못했지만, 데이터 처리 및 실행 파이프라인 개선을 통해 추론 시간을 약 40% 단축했습니다.
- TimeCopilot을 통해 시계열 예측 결과를 그래프와 자연어 설명으로 연결하는 분석 Copilot 가능성을 확인했습니다.
- 시계열 모델 실행, 평가, 시각화, LLM 분석을 하나의 플랫폼 흐름으로 정리했습니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
