# Self-Correcting RAG System Architecture

## 1. Portfolio Summary Architecture

```mermaid
flowchart LR
    User["User Query"] --> Graph["LangGraph Agent Workflow"]
    Graph --> Retrieve["Retriever"]
    Retrieve --> Context["Retrieved Context"]
    Context --> Generate["Answer Generator"]
    Generate --> Check["Self Check / Critic"]
    Check --> Decision{"Answer grounded?"}
    Decision -->|Yes| Final["Final Answer"]
    Decision -->|No| Revise["Revise Query or Regenerate"]
    Revise --> Retrieve
```

## 2. Detailed Flow

```mermaid
sequenceDiagram
    autonumber
    participant User as User
    participant Graph as LangGraph Workflow
    participant Retriever as Retriever
    participant LLM as LLM Generator
    participant Checker as Self-Correction Checker

    User->>Graph: Ask question
    Graph->>Retriever: Search relevant documents
    Retriever-->>Graph: Retrieved chunks
    Graph->>LLM: Generate answer with context
    LLM-->>Graph: Draft answer
    Graph->>Checker: Check grounding and missing evidence

    alt answer is acceptable
        Checker-->>Graph: pass
        Graph-->>User: final answer
    else answer is weak or unsupported
        Checker-->>Graph: fail with reason
        Graph->>Retriever: re-search or adjust context
        Graph->>LLM: regenerate answer
        LLM-->>Graph: revised answer
        Graph-->>User: final answer after correction
    end
```

## 3. Design Notes

- RAG 응답을 바로 반환하지 않고 검증 단계를 둡니다.
- 검색, 생성, 검증, 재생성 단계를 LangGraph node로 분리합니다.
- 답변 품질 저하 원인은 근거 부족, 문서 불일치, 누락 정보, 질문 의도 오해로 나누어 볼 수 있습니다.
- 이 프로젝트는 환각을 완전히 제거하는 것이 아니라, 환각 가능성을 줄이기 위한 구조적 접근을 검토한 PoC입니다.

## 4. Improvement Ideas

- 평가 질문셋 구축
- Groundedness score 도입
- 답변-근거 문장 alignment 저장
- Human feedback 기반 재학습 또는 FAQ 승격 연동
