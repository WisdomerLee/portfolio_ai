# MCP RAG Bridge System Architecture

## 1. Portfolio Summary Architecture

```mermaid
flowchart LR
    Agent["AI Agent / MCP Host"] --> MCP["FastMCP Server<br/>RB Agent MCP Bridge"]

    MCP --> Tools["MCP Tools<br/>ask_rag_agent<br/>query stages<br/>feedback / FAQ"]
    MCP --> Resources["MCP Resources<br/>categories / documents<br/>query-options / logs"]
    MCP --> Prompts["MCP Prompts<br/>guided RAG workflows"]

    Tools --> Guard["ToolGuard<br/>rate limit / timeout"]
    Guard --> Session["QuerySessionStore<br/>thread_id state"]

    Session --> FAQ["FAQ Shortcut"]
    FAQ -->|miss| Search["Vector Search<br/>Qdrant / pgvector"]
    Search --> Rerank["Rerank"]
    Rerank --> Generate["LLM Generation<br/>OpenAI-compatible"]

    Generate --> Answer["Answer + Sources"]
    Answer --> Logs["QueryLog<br/>memory or Postgres"]

    Tools --> RBAgent["RB Agent API"]
    Tools --> Files["File Upload / Parser"]
    Tools --> Feedback["Feedback / FAQ Store"]

    Search --> VectorDB["Vector DB"]
    Generate --> LLM["LLM Server"]
    Files --> Parser["Parser Server"]
    Feedback --> DB["Postgres or Memory Store"]
```

## 2. Runtime Wiring

```mermaid
flowchart TD
    Start["python -m src.mcp_server"]
    Settings["get_settings<br/>settings.json + env"]
    Container["create_container"]
    Token["RBAgentTokenManager"]
    Guard["ToolGuard"]
    RAGClient["RBAgentRagClient"]
    Search["DirectVectorSearchAdapter"]
    Generator["DirectVLLMGenerationAdapter"]
    Reranker["ScoreBasedRerankerAdapter"]
    Parser["PassthroughParserAdapter"]
    Session["QuerySessionStore"]
    Stage["QueryStageService"]
    QueryService["QueryCapabilityService"]
    App["FastMCP App"]
    Registrar["ToolRegistrar.register_all"]
    Transport{"transport"}

    Start --> Settings
    Settings --> Container
    Container --> Token
    Container --> Guard
    Container --> RAGClient
    Container --> Search
    Container --> Generator
    Container --> Reranker
    Container --> Parser
    Container --> Session
    Container --> Stage
    Container --> QueryService
    QueryService --> Guard
    QueryService --> Stage
    Stage --> Search
    Stage --> Reranker
    Stage --> Parser
    Stage --> Generator
    Container --> App
    App --> Registrar
    App --> Transport
    Transport -->|stdio| Stdio["stdio"]
    Transport -->|sse| SSE["SSE /sse"]
```

## 3. Query Flow: ask_rag_agent

```mermaid
sequenceDiagram
    autonumber
    participant Host as MCP Host
    participant Tool as ask_rag_agent
    participant Guard as ToolGuard
    participant Prep as QueryExecutionPrepService
    participant Session as QuerySessionStore
    participant Log as QueryLogService
    participant Stage as QueryStageService
    participant FAQ as FAQ Shortcut
    participant Search as Vector Search
    participant Rerank as Reranker
    participant Gen as LLM Generation

    Host->>Tool: ask_rag_agent(message, thread_id, stream)
    Tool->>Guard: check + record_request
    Tool->>Prep: prepare(args)
    Prep-->>Tool: normalized args + initial state
    Tool->>Session: create_or_replace(initial state)
    Tool->>Log: save_prepared

    alt stream=false
        Tool->>Stage: execute_non_stream_query_flow
        Stage->>FAQ: faq_shortcut_lookup
        alt FAQ matched
            FAQ-->>Stage: shortcut answer
        else FAQ miss
            Stage->>Gen: plan_query
            Stage->>Search: search_relevant_chunks
            Search-->>Stage: chunks
            Stage->>Rerank: rerank_relevant_chunks
            Rerank-->>Stage: top chunks
            Stage->>Gen: generate_rag_answer
            Gen-->>Stage: answer
        end
        Stage-->>Tool: result
        Tool->>Log: finalize
        Tool-->>Host: formatted text response
    else stream=true
        Tool->>Stage: execute_stream_query_flow
        Stage-->>Tool: start / token / final / error events
        Tool->>Log: finalize
        Tool-->>Host: accumulated formatted response
    end
```

## 4. MCP Exposure Catalog

```mermaid
flowchart TB
    FastMCP["FastMCP"] --> Registrar["ToolRegistrar"]

    Registrar --> Tools["Tools"]
    Registrar --> Resources["Resources"]
    Registrar --> Prompts["Prompts"]
    Registrar -.descriptor only.-> Skills["Skills"]

    Tools --> Rag["tool_catalog.rag"]
    Tools --> Stages["tool_catalog.query_stages"]
    Tools --> Workflows["tool_catalog.query_workflows"]
    Tools --> Feedback["tool_catalog.query_feedback_faq"]

    Resources --> Categories["rag://categories"]
    Resources --> Pipelines["rag://chunk-pipelines"]
    Resources --> Documents["rag://documents"]
    Resources --> Options["rag://query-options"]
    Resources --> Logs["rag://query-logs"]

    Prompts --> ByCategory["rag-search-by-category"]
    Prompts --> Explore["rag-explore-then-ask"]
```

## 5. RAG Search and Generation

```mermaid
flowchart TB
    Query["User Query"] --> Plan["Query Plan"]
    Plan --> Embed["Embedding Server"]
    Embed --> Backend{"Vector Backend"}
    Backend -->|qdrant| Qdrant["Qdrant"]
    Backend -->|pgvector| PgVector["Postgres + pgvector"]
    Qdrant --> Chunks["QueryChunkScoreDTO"]
    PgVector --> Chunks
    Chunks --> Rerank{"Reranker available?"}
    Rerank -->|yes| RemoteRerank["Remote Rerank"]
    Rerank -->|no/fail| ScoreFallback["Score fallback"]
    RemoteRerank --> Context["Top context chunks"]
    ScoreFallback --> Context
    Context --> Prompt["Build LLM messages"]
    Prompt --> LLM["OpenAI-compatible LLM"]
    LLM --> Answer["Answer + Sources"]
```

## 6. Canonical vs Monolith Reduced

```mermaid
flowchart TB
    Contract["MCP Public Contract<br/>Tools / Resources / Prompts"]

    subgraph Canonical["Canonical<br/>src/mcp_server"]
        Main["main.py"]
        Container["container.py"]
        Registrar["registrar.py"]
        Catalog["tool/resource/prompt catalog"]
        Services["services"]
        Adapters["clients / adapters"]
    end

    subgraph Monolith["Reduced Monolith<br/>src/monolith_reduced_main.py"]
        Single["single-file FastMCP app"]
        Smoke["smoke / parity path"]
    end

    Contract --> Canonical
    Contract --> Monolith
    Canonical --> Main
    Main --> Container
    Main --> Registrar
    Registrar --> Catalog
    Container --> Services
    Container --> Adapters
    Monolith --> Single
    Single --> Smoke
    Canonical -.no source sharing.-> Monolith
    Monolith -.contract-compatible.-> Canonical
```

## 7. Design Notes

- `src/mcp_server`는 canonical 구조이며 신규 기능은 이 경로에 우선 반영합니다.
- `src/monolith_reduced_main.py`는 독립적인 축약 구현이며 smoke test와 contract parity 확인 용도입니다.
- Tool/Resource/Prompt는 catalog module 단위로 명시 등록합니다.
- `QueryStageService`는 분해형 query primitive의 핵심 orchestration 계층입니다.
- `ask_rag_agent`는 formatted text를 반환하고, 분해형 tool은 dict payload를 반환합니다.
- Query log, feedback/FAQ, file metadata, query options는 memory 또는 shared Postgres backend로 교체 가능합니다.

## 8. Improvement Ideas

- public MCP tool contract 확정 후 documents/admin catalog 확장
- skill catalog를 runtime workflow descriptor로 활용
- adapter registry 기반 다중 agent routing 확장
- query evaluation / feedback loop 자동화
- production-grade session persistence 도입
