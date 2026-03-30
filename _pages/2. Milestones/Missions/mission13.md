---
layout: page
title: "[Mission 13] Efficient Tuning (LoRA/PEFT)"
category: "2. Milestones / Missions"
order: 13
---

## 미션 개요
대규모 언어 모델(LLM)을 한정된 자원으로 효과적으로 미세 조정(Fine-tuning)하기 위해 **LoRA(Low-Rank Adaptation)** 기법을 적용한 프로젝트입니다.

## 주요 기술 스택
- **Model**: `beomi/KcELECTRA-base` (Korean Sentiment Analysis)
- **Library**: `HuggingFace PEFT`, `Transformers`, `PyTorch`
- **Technique**: LoRA (`r=8`, `lora_alpha=16`, `dropout=0.1`)

## 생산성 및 성능 성과
- **파라미터 효율성**: 전체 파라미터 중 단 **0.8%** 만 학습하여 Full Fine-tuning 대비 **92.39%** 의 높은 정확도 유지.
- **저장 공간 최적화**: 모델 스토리지 용량을 **99.2% 절감** (416MB → 3.4MB) 하여 배포 효율성 극대화.
- **불균형 데이터 대응**: **Weighted Smoothing Trainer** 구현을 통해 부족한 클래스 데이터의 학습 가중치를 조정하여 불균형 문제 해결.

## 관련 링크
- [GitHub Repository](https://github.com/vasyapupkinski/codeit_mission_13_LLM-Efficient-Tuning-LoRA)