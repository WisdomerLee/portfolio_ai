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

## Key Technologies

- Python
- Reinforcement Learning
- SAC
- GRPO
- Unsloth
- TRL
- PyTorch
- LLM
- Energy Market Simulation

## Result

- 초기 코드 생성형 RL 방식의 한계를 확인했습니다.
- 이후 LLM은 전략 가이드라인을 생성하고, RL은 실제 수치 행동을 최적화하는 Hybrid Symbolic RL 구조가 더 안정적이라는 결론을 도출했습니다.
- 15분 단위 Two-Settlement 환경, 1시간 DA 업데이트, 15분 RT 업데이트, DA/RT buffer 분리, requested/executed action 추적 구조를 정리했습니다.

## Detailed Documents

- [System Architecture](docs/system_architecture.md)
