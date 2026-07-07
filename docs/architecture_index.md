# Architecture Index

이 문서는 포트폴리오 프로젝트별 상세 시스템 아키텍처 문서의 인덱스입니다.

## Project Architecture Documents

| Project | Architecture Document | Recommended Usage |
|---|---|---|
| Self-Correcting RAG | [system_architecture.md](../projects/self-correcting-rag/docs/system_architecture.md) | Agentic RAG 구조 설명 |
| Time Series AI Platform | [system_architecture.md](../projects/time-series-ai-platform/docs/system_architecture.md) | 시계열 분석 플랫폼과 추론 최적화 설명 |
| AI-OCR Pipeline | [system_architecture.md](../projects/ai-ocr-pipeline/docs/system_architecture.md) | OCR/VLM/Validation pipeline 설명 |
| Hybrid Symbolic RL | [system_architecture.md](../projects/hybrid-symbolic-rl/docs/system_architecture.md) | LLM+RL 구조와 한계 분석 설명 |
| MCP RAG Bridge | [system_architecture.md](../projects/mcp-rag-bridge/docs/system_architecture.md) | FastMCP 기반 MCP Tooling 구조 설명 |

## 프로젝트 별 기능 요약


| Project | 대표 흐름 |
|---|---|
| Self-Correcting RAG | User Query → Retriever → Generator → Self Check → Regenerate / Final Answer |
| Time Series AI Platform | Data → ForecastPipeline → Moirai/Chronos → Metric/Plot → LLM Analysis |
| AI-OCR Pipeline | PDF/Image → PaddleOCR Evidence → VLM Extraction → bbox Restore → Validation → Artifact |
| Hybrid Symbolic RL | Market State → LLM Guideline → SAC Optimizer → Market Env → Reward → Replay Buffer |
| MCP RAG Bridge | AI Agent → FastMCP Server → Tools/Resources/Prompts → RAG/LLM/Vector DB |
