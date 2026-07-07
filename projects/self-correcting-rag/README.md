# Self-Correcting RAG Service PoC

## Overview

LangGraph를 활용해 RAG 응답의 환각 가능성을 줄이기 위한 자가 교정형 RAG PoC입니다.

일반적인 RAG는 검색 문서를 기반으로 답변하더라도, LLM이 근거와 다른 내용을 생성하거나 필요한 정보를 누락할 수 있습니다. 이 프로젝트는 검색, 생성, 검증, 재생성 단계를 분리하여 답변 품질을 구조적으로 검토하는 Agentic RAG 흐름을 설계했습니다.

## Problem

- 검색된 문서와 답변이 불일치할 수 있음
- LLM이 근거 없는 내용을 생성할 수 있음
- 단순 RAG 구조에서는 답변 검증과 재생성 기준이 명확하지 않음

## Goal

- 검색 → 답변 생성 → 답변 검증 → 재검색/재생성 흐름 구성
- LangGraph 기반 Agent Workflow로 단계별 책임 분리
- 환각 가능성이 있는 응답을 교정하는 Self-Correction Loop 검토

## My Role

- RAG Agent Workflow 설계
- Self-Correction Loop 구조 검토
- 검색/생성/검증 단계 분리
- AI 생성 코드의 구조 검토 및 오류 수정
- 포트폴리오용 시스템 아키텍처 문서화

## Key Technologies

- Python
- LangGraph
- LangChain
- RAG
- LLM
- Vector DB
- FastAPI

## Result

- 단순 질의응답형 RAG가 아니라, 답변 검증과 재생성을 포함한 Agentic RAG 구조를 PoC로 정리했습니다.
- 검색, 생성, 검증, 교정 단계를 분리하여 디버깅과 유지보수가 쉬운 구조로 설계했습니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
