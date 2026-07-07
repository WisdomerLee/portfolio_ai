# Hybrid Symbolic RL PoC

## Overview

전력시장 입찰 수익 최적화를 목표로 LLM과 강화학습을 결합한 PoC입니다.

초기에는 LLM이 `strategy(state)` Python 코드를 직접 생성하고, 해당 코드를 샌드박스에서 실행해 GRPO로 학습하는 구조를 시도했습니다. 이후 코드 안정성, reward 설계, 시장 제약 반영의 한계를 확인하고, LLM은 시장 해석과 전략 가이드라인을 생성하고 SAC 기반 Numerical Optimizer가 실제 수치 행동을 결정하는 Hybrid Symbolic RL 구조로 재설계했습니다.

## Problem

- 전력시장 입찰은 가격, 발전량, ESS 상태, DA/RT 정산 구조가 얽힌 의사결정 문제임
- LLM이 직접 코드를 생성하는 방식은 문법 오류, 보안, 실행 안정성 문제가 큼
- 단일 RL optimizer가 DA 예약과 RT ESS 제어를 함께 학습하면 reward scale과 시간 해상도가 섞임

## Goal

- LLM은 시장 상황을 해석하고 정성적 가이드라인만 생성
- SAC는 가이드라인 임베딩과 시장 상태를 기반으로 수치 행동 결정
- DA 예약과 RT ESS 제어를 분리한 hierarchical optimizer 구조 검토
- requested action과 실제 executed action mismatch를 추적

## My Role

- 초기 GRPO 코드 생성형 RL 구조 실험 및 한계 분석
- Market Bidding Environment / Two-Settlement 환경 구조 검토
- Reward = Profit - Baseline 회계 정합성 검토
- LLM Symbolic Agent + SAC Numerical Optimizer 역할 분리 구조 설계
- requested/executed action tracking 구조 문서화

## Tech Stack

- Python
- PyTorch
- Unsloth
- TRL
- SAC implementation
- Gymnasium-style environment
- Pandas
- NumPy
- Matplotlib
- LLM API / local LLM runtime

## Core Implementation Logic

- 초기 접근에서는 LLM이 `strategy(state)` 함수를 생성하고, 해당 코드를 sandbox에서 실행
- 코드 추출, syntax check, forbidden module check, locked-down execution을 거쳐 전략 함수 검증
- GRPO reward function으로 function validity, no-cheating, simulation performance를 분리 평가
- 개선 구조에서는 LLM이 수치 action이 아니라 market regime과 action guideline을 생성
- guideline을 embedding하여 RL state의 추가 관측 정보로 주입
- SAC optimizer가 DA reservation ratio와 RT ESS power ratio를 결정
- 15분 단위 market step에서 reward = profit - baseline을 계산
- requested action과 environment constraint 이후 executed action을 함께 기록

## Architecture Pattern

- Initial GRPO code-generation RL pattern
- Hybrid Symbolic RL pattern
- LLM Symbolic Agent + Numerical RL Optimizer 역할 분리
- Hierarchical DA/RT SAC optimizer
- Two-Settlement Market Environment
- Replay buffer 분리: DA hour-level transition / RT 15min transition
- Requested vs Executed Action tracking
- Reward accounting and diagnostics

## Result

- 초기 코드 생성형 RL 방식의 한계를 확인했습니다.
- 이후 LLM은 전략 가이드라인을 생성하고, RL은 실제 수치 행동을 최적화하는 Hybrid Symbolic RL 구조가 더 안정적이라는 결론을 도출했습니다.
- 15분 단위 Two-Settlement 환경, 1시간 DA 업데이트, 15분 RT 업데이트, DA/RT buffer 분리, requested/executed action 추적 구조를 정리했습니다.
- PoC의 핵심 성과는 높은 수익률 자체보다, LLM과 RL의 역할을 분리해야 한다는 구조적 결론을 도출한 점입니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
