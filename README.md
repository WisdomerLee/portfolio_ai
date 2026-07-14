# AI Application / Agent Portfolio

Python 기반 AI Application Engineer 포트폴리오입니다.  
LLM/RAG Agent, 시계열 파운데이션 모델, AI-OCR, Hybrid Symbolic RL, FastMCP 기반 MCP 서버 PoC를 중심으로 정리했습니다.

## Positioning

LLM/RAG Agent, 시계열 AI 모델, MCP Tool 연동, AI-OCR 파이프라인을 서비스 구조로 연결하고 검증하는 AI Application Engineer입니다.

개발 과정에서는 AI Coding Assistant를 활용하되, 요구사항 정의, 시스템 아키텍처 설계, 코드 리뷰, 테스트, 성능 검증, 한계 분석을 통해 PoC를 유지보수 가능한 구조로 정리하는 데 집중했습니다.

## Core Projects

| Project | Summary | Key Technologies |
|---|---|---|
| [Self-Correcting RAG](projects/self-correcting-rag/README.md) | LangGraph 기반 자가 교정형 RAG PoC | LangGraph, RAG, LLM, Agent Workflow |
| [Time Series AI Platform](projects/time-series-ai-platform/README.md) | Moirai/Chronos/TimeCopilot 기반 시계열 분석 및 추론 최적화 PoC | PyTorch, Moirai, Chronos, DeepSpeed, TimeCopilot |
| [AI-OCR Pipeline](projects/ai-ocr-pipeline/README.md) | PaddleOCR + VLM 기반 문서 구조화 추출 및 검증 파이프라인 | PaddleOCR, VLM, FastAPI, Validation |
| [Hybrid Symbolic RL](projects/hybrid-symbolic-rl/README.md) | LLM Symbolic Agent + SAC 기반 전력시장 입찰 최적화 PoC | Reinforcement Learning, SAC, Unsloth, LLM |
| [MCP RAG Bridge](projects/mcp-rag-bridge/README.md) | RB Agent RAG API를 MCP Tool/Resource/Prompt로 노출하는 FastMCP 브리지 | FastMCP, MCP, RAG, Tool Calling |

## Detailed Architecture Documents

- [Architecture Index](docs/architecture_index.md)
- [Portfolio Overview](docs/portfolio_overview.md)
- [Resume Project Summary](docs/resume_project_summary.md)

## Repository Structure

```text
portfolio_ai/
├── README.md
├── docs/
│   ├── portfolio_overview.md
│   ├── resume_project_summary.md
│   └── architecture_index.md
└── projects/
    ├── self-correcting-rag/
    ├── time-series-ai-platform/
    ├── ai-ocr-pipeline/
    ├── hybrid-symbolic-rl/
    └── mcp-rag-bridge/
```

## Documentation Policy

이 저장소는 공개 포트폴리오용으로 작성되었습니다. 내부 서버 주소, 실제 데이터 경로, API 키, 고객사 세부 정보, 비공개 모델 체크포인트 경로는 포함하지 않습니다.

각 프로젝트는 다음 기준으로 정리합니다.

1. 문제 정의
2. 목표
3. 시스템 아키텍처
4. 본인 역할
5. 주요 구현 내용
6. 결과와 한계
7. 개선 방향

## 시연 영상
### ai-ocr-pipeline
<video src="https://github.com/user-attachments/assets/8a19c4ec-1672-4a88-a9b5-07f159220946"></video>
### time-series-ai-platform
<video src="https://github.com/user-attachments/assets/8b91d9f1-7fc5-4b29-82f4-dc1f99ac65d8"></video>
<video src="https://github.com/user-attachments/assets/91b37a4a-a449-4235-8fd0-b598cf334e75"></video>
