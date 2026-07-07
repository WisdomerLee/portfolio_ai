# MCP RAG Bridge PoC

## Overview

기존 RB Agent RAG API를 MCP Tool, Resource, Prompt 형태로 노출하는 FastMCP 기반 브리지 서버 PoC입니다.

AI Agent 또는 MCP Host가 RAG 질의, 문서 검색, 파일 업로드, 분해형 query stage, feedback/FAQ 기능을 표준 MCP 인터페이스로 호출할 수 있도록 설계했습니다.

## Problem

- 기존 RAG API를 AI Agent가 직접 호출하기에는 도구 계약과 실행 흐름이 명확하지 않음
- end-to-end RAG 질의뿐 아니라 search, rerank, generate 같은 분해형 도구가 필요함
- Tool, Resource, Prompt를 분리해 Agent가 상황에 따라 사용할 수 있는 구조가 필요함
- 인증, rate limit, streaming, query log, feedback/FAQ 관리가 함께 필요함

## Goal

- FastMCP 기반 MCP 서버 구축
- RB Agent RAG API를 MCP Tool로 노출
- `rag://categories`, `rag://documents`, `rag://query-options`, `rag://query-logs` 등 MCP Resource 제공
- RAG workflow를 안내하는 MCP Prompt 제공
- query session, file upload, feedback/FAQ, query log를 adapter 기반으로 분리

## My Role

- MCP 서버 시스템 아키텍처 분석 및 문서화
- Tool/Resource/Prompt catalog 구조 정리
- Query primitive flow와 composite flow 구조 정리
- FastMCP 기반 API bridge 설계 관점 검토
- CUPID 관점으로 Tool 책임, 입출력 계약, 예측 가능성, 도메인 기반 구조 검토

## Tech Stack

- Python
- FastMCP
- MCP
- httpx
- Pydantic
- Starlette-compatible custom routes
- Qdrant / pgvector
- PostgreSQL
- OpenAI-compatible LLM endpoint
- SSE / stdio transport

## Core Implementation Logic

- 기존 RAG API를 MCP Tool로 감싸 AI Agent가 표준 인터페이스로 호출하도록 구성
- `ask_rag_agent`는 end-to-end RAG Tool로 제공
- `prepare_query_inputs`, `plan_query_context`, `search_relevant_chunks`, `rerank_relevant_chunks`, `generate_rag_answer`는 분해형 query primitive로 제공
- `rag://categories`, `rag://documents`, `rag://query-options`, `rag://query-logs`를 MCP Resource로 노출
- `rag-search-by-category`, `rag-explore-then-ask`를 MCP Prompt로 제공
- thread_id 기반 query session state를 유지
- FAQ shortcut을 먼저 확인하고 실패하면 search → rerank → generation으로 진행
- query log, feedback, FAQ, file metadata를 memory 또는 shared Postgres backend로 저장
- stream mode에서는 start/token/final/error 이벤트를 정규화

## Architecture Pattern

- Tool / Resource / Prompt exposure catalog pattern
- FastMCP bridge architecture
- Container-based dependency wiring
- Adapter boundary pattern
- QueryCapabilityService + QueryStageService 분리
- Thread-based session state pattern
- FAQ-first RAG workflow
- Direct vector search + rerank + generation pipeline
- Memory/Postgres swappable store pattern
- Canonical implementation과 monolith reduced parity 구조

## Result

- 기존 RAG API를 MCP Tool, Resource, Prompt로 노출하는 브리지 구조를 정리했습니다.
- `ask_rag_agent` 같은 end-to-end tool뿐 아니라 `prepare_query_inputs`, `plan_query_context`, `search_relevant_chunks`, `rerank_relevant_chunks`, `generate_rag_answer` 같은 분해형 tool 구조를 정리했습니다.
- query log, feedback/FAQ, file metadata, vector search, generation adapter를 교체 가능한 경계로 분리했습니다.
- AI Agent가 기존 API를 직접 이해하지 않아도 MCP contract를 통해 RAG 기능을 사용할 수 있는 구조를 만들었습니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
