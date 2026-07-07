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

## Tech Stack

- Python
- LangGraph
- LangChain
- FastAPI
- Vector DB
- LLM API

## Core Implementation Logic

- User query를 입력받아 retriever에서 관련 문서 chunk를 검색
- 검색된 context를 기반으로 LLM answer generator가 초안 답변 생성
- Self-check node가 답변의 근거 적합성, 누락 여부, 재검색 필요성을 판단
- 답변이 부적합하면 query/context를 보정해 재검색 또는 재생성 수행
- 최종 답변은 근거 문서와 함께 반환되도록 구성

## Architecture Pattern

- LangGraph 기반 node/edge workflow
- Retriever / Generator / Checker / Regenerator 책임 분리
- Self-Correction Loop
- Agentic RAG pipeline
- 검색 단계와 생성 단계의 느슨한 결합
- 검증 실패 시 재시도 가능한 feedback loop 구조

## Result

- 단순 질의응답형 RAG가 아니라, 답변 검증과 재생성을 포함한 Agentic RAG 구조를 PoC로 정리했습니다.
- 검색, 생성, 검증, 교정 단계를 분리하여 디버깅과 유지보수가 쉬운 구조로 설계했습니다.
- RAG 환각을 완전히 제거하는 것이 아니라, 구조적으로 줄이기 위한 설계 접근을 정리했습니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
