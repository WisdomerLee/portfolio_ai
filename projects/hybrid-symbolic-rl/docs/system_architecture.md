# Hybrid Symbolic RL System Architecture

## 1. Portfolio Summary Architecture

```mermaid
flowchart LR
    Data["Market Time Series<br/>DA/RT price + generation"] --> Env["Two-Settlement Market Env<br/>15min step"]
    Forecaster["SparseTSF / Dummy Forecaster"] --> Env

    Env --> State["MarketState<br/>numeric features + guidance"]
    State --> LLM["LLM Symbolic Agent<br/>market analysis"]
    LLM --> Guide["Strategy Guideline<br/>text + embedding"]

    State --> SAC["Hierarchical SAC Optimizer"]
    Guide --> SAC

    SAC --> DA["DA Action<br/>reservation_ratio<br/>hourly update"]
    SAC --> RT["RT Action<br/>ess_power_ratio<br/>15min update"]

    DA --> Env
    RT --> Env

    Env --> Reward["Reward<br/>Profit - Baseline"]
    Env --> Trace["Requested vs Executed Action<br/>SoC boundary mismatch"]

    Reward --> Replay["DA/RT Replay Buffers"]
    Trace --> Replay
    Replay --> SAC

    Reward --> Eval["RLEvaluator<br/>ROI / Reward / KL / Stability"]
    Eval --> Artifacts["Plots / Logs / Checkpoints"]
```

## 2. Hybrid Symbolic RL Flow

```mermaid
sequenceDiagram
    autonumber
    participant Train as train.py
    participant Env as TwoSettlementMarketEnv
    participant Orch as BiddingOrchestrator
    participant Sym as SymbolicAgent
    participant HiOpt as HierarchicalOptimizer
    participant DA as DASACOptimizer
    participant RT as RTSACOptimizer
    participant Eval as RLEvaluator

    Train->>Env: build environment
    Train->>Sym: build LLM or dummy symbolic agent
    Train->>HiOpt: build hierarchical SAC optimizer
    Train->>Orch: compose env + symbolic + optimizer

    loop episode
        Orch->>Env: reset
        loop 15min step
            Orch->>Env: get_state
            alt is_new_hour
                Orch->>Sym: generate market guideline
                Sym-->>Orch: SymbolicGuideline + embedding
                Orch->>Env: set_guideline
                Orch->>DA: optimize_da
                DA-->>Orch: reservation_ratio
            else q1~q3
                Orch->>Orch: reuse hourly DA plan
            end
            Orch->>RT: optimize_rt
            RT-->>Orch: ess_power_ratio
            Orch->>Env: step(Action)
            Env-->>Orch: next_state, reward, info
            Orch->>HiOpt: record requested/executed action
            Orch->>RT: update_rt(step reward)
            alt q4 / hour boundary
                Orch->>DA: update_da(hourly reward)
            end
        end
        Orch-->>Train: EpisodeMetrics
        Train->>Eval: log metrics
    end
```

## 3. Initial GRPO Code Generation Attempt

```mermaid
flowchart TB
    Prompt["Prompt<br/>write def strategy(state)"]
    LLM["Unsloth LLM"]
    Completion["Generated Code Completion"]
    Extract["extract_strategy_function"]
    Syntax["Syntax Check<br/>ast.parse + compile"]
    Safety["Safety Check<br/>forbidden modules / no cheating"]
    Sandbox["Locked-down Function"]
    Env["MarketBiddingEnv"]
    Reward["strategy_performance<br/>Profit - Baseline"]
    GRPO["GRPOTrainer"]
    Fallback["fallback_policy"]

    Prompt --> LLM
    LLM --> Completion
    Completion --> Extract
    Extract --> Syntax
    Syntax --> Safety
    Safety --> Sandbox
    Sandbox --> Env
    Env --> Reward
    Reward --> GRPO
    GRPO --> LLM

    Extract -->|fail| Fallback
    Syntax -->|fail| Fallback
    Safety -->|fail| Fallback
    Sandbox -->|runtime fail| Fallback
    Fallback --> Env
```

## 4. Requested vs Executed Action Tracking

```mermaid
flowchart LR
    Policy["SAC Policy"] --> Requested["Requested Action"]
    Requested --> Env["Environment Constraint Projection"]
    SoC["SoC / Capacity / Efficiency / Output Limits"] --> Env
    Env --> Executed["Executed Action"]
    Env --> Reward["Reward"]
    Env --> Info["abs_action_diff<br/>soc_boundary_hit"]
    Requested --> Replay["Replay Buffer"]
    Executed --> Replay
    Reward --> Replay
    Info --> Diagnostics["Mismatch Diagnostics"]
    Replay --> Update["Actor / Critic Update<br/>uses requested action"]
```

## 5. Design Notes

- 초기 GRPO 방식은 LLM이 직접 Python 전략 함수를 생성했기 때문에 문법 오류, 실행 오류, 안전성 검사가 필요했습니다.
- 개선된 구조에서는 LLM을 코드 생성자가 아니라 시장 해석과 전략 가이드 생성자로 제한했습니다.
- DA와 RT는 시간 해상도와 reward scale이 다르므로 별도 SAC optimizer와 replay buffer로 분리했습니다.
- 환경 제약 때문에 requested action과 실제 executed action이 달라질 수 있으므로 이를 별도로 기록했습니다.

## 6. Limitations

- 실제 LLM mode 전체 학습은 GPU 인식 환경에 의존합니다.
- 실제 시장 데이터, DA 계획 고정 제약, 발전소 운영 제약을 더 정교하게 반영해야 합니다.
- Reward scale과 DA/RT optimizer hyperparameter는 추가 튜닝이 필요합니다.
