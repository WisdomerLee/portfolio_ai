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

## PDF 포트폴리오에 넣을 대표 다이어그램

PDF에는 상세 Mermaid 전체를 넣기보다 프로젝트별 대표 구조도 하나만 넣는 것이 좋습니다.

| Project | PDF용 대표 흐름 |
|---|---|
| Self-Correcting RAG | User Query → Retriever → Generator → Self Check → Regenerate / Final Answer |
| Time Series AI Platform | Data → ForecastPipeline → Moirai/Chronos → Metric/Plot → LLM Analysis |
| AI-OCR Pipeline | PDF/Image → PaddleOCR Evidence → VLM Extraction → bbox Restore → Validation → Artifact |
| Hybrid Symbolic RL | Market State → LLM Guideline → SAC Optimizer → Market Env → Reward → Replay Buffer |
| MCP RAG Bridge | AI Agent → FastMCP Server → Tools/Resources/Prompts → RAG/LLM/Vector DB |

## 이력서에는 이렇게 요약

상세 아키텍처 자체는 이력서에 넣지 않고, 아래처럼 설계 역량 중심으로 요약합니다.

```text
LLM/RAG Agent, AI-OCR, 시계열 AI, MCP Tooling, Hybrid Symbolic RL PoC를 수행하며 시스템 아키텍처 설계, AI 생성 코드 검토, 성능 및 한계 분석을 담당했습니다.
```
