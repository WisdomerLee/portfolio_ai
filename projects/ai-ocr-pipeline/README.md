# AI-OCR Pipeline PoC

## Overview

PaddleOCR와 VLM을 결합해 문서에서 구조화된 정보를 추출하고, OCR evidence와 validation artifact를 남기는 AI-OCR 파이프라인 PoC입니다.

단순 OCR 결과를 반환하는 것이 아니라, VLM이 추출한 필드와 OCR line evidence를 연결해 raw text, bbox, confidence, corrected 여부를 추적할 수 있도록 설계했습니다.

## Problem

- OCR 결과만으로는 추출 필드의 근거를 추적하기 어려움
- VLM 추출 결과는 구조화에는 강하지만 원문 근거와 bbox 추적이 약할 수 있음
- 문서 합계, line item, discount 등 업무 검증 규칙이 필요함
- 실패 시 원인 분석을 위한 artifact가 필요함

## Goal

- PDF/Image → OCR evidence → VLM extraction → bbox restore → validation → artifact 저장 흐름 구성
- OCR line id와 VLM field ocrIds를 연결해 검증 가능한 결과 생성
- 합계 검증 및 trace/raw/result artifact 저장 구조 설계

## My Role

- AI-OCR 파이프라인 구조 분석 및 문서화
- OCR evidence, bbox 복원, validation rule chain 구조 정리
- artifact 저장 구조와 디버깅 가능성 검토
- 포트폴리오용 시스템 아키텍처 Mermaid 정리

## Tech Stack

- Python
- FastAPI
- PaddleOCR
- PyMuPDF
- Pillow / OpenCV
- VLM / Vision LLM API
- SQLAlchemy
- SQLite / PostgreSQL
- Playwright

## Core Implementation Logic

- PDF를 page image로 렌더링하고 이미지 단위로 전처리
- 회전 보정, split/crop, enhancement 등 OCR 전처리 수행
- PaddleOCR로 line-level text, score, bbox, polygon 추출
- OCR line을 `ocrId`, text, bbox, confidence 형태의 evidence로 정규화
- VLM에 원본 이미지와 OCR evidence를 함께 전달해 구조화된 JSON 추출
- VLM 결과의 `ocrIds`를 OCR line index와 연결해 raw text, bbox, confidence 복원
- line item, discount, total amount, current/past due 등 validation rule chain 수행
- result, trace, raw payload를 artifact로 저장해 실패 원인 분석 가능하게 구성

## Architecture Pattern

- OCR engine layer와 VLM extraction layer 분리
- Evidence-driven extraction pattern
- OCR line index 기반 bbox restoration
- Validation rule chain
- Artifact file store + DB index 분리
- Batch job / SSE progress pattern
- Human review와 ERP integration을 고려한 workflow boundary

## Result

- OCR 결과와 VLM 추출 결과를 evidence 기반으로 연결하는 구조를 정리했습니다.
- 합계 검증, bbox 복원, raw OCR text 비교, trace artifact 저장을 통해 검증 가능한 AI-OCR 결과를 만들 수 있는 구조를 도출했습니다.
- 단순 OCR API가 아니라 디버깅과 운영 검증을 고려한 AI 문서 처리 파이프라인으로 확장했습니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
