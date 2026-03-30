---
layout: page
title: "[Mission 16] Model Optimization (ONNX/Quantization)"
category: "2. Milestones / Missions"
order: 16
---

## 미션 개요
딥러닝 모델의 추론 속도를 혁신적으로 개선하기 위해 **ONNX 변환** 및 **INT8 양자화(Quantization)**를 적용하고 실제 가속 성능을 측정한 최적화 프로젝트입니다.

## 주요 기술 스택
- **Engine**: `ONNX Runtime`, `TensorRT` (Target-Specific Acceleration)
- **Format**: `INT8 QDQ` (Quantize-Dequantize) 최적화
- **Tool**: `PyTorch to ONNX conversion`

## 최적화 및 벤치마크 성과
- **추론 속도 9.65배 향상**: PyTorch FP32 베이스라인(0.53ms) 대비 **0.05ms** 까지 레이턴시를 단축하여 실시간 서비스 가용성 확보.
- **성능 유지율 98.89%**: 강도 높은 양자화 적용에도 불구하고 정확도 손실을 최소화하여 모델 신뢰성 보존.
- **다이나믹 배치 지원**: 실시간 동적 입력 처리를 위한 **Dynamic Batching** 파이프라인 구현.

## 관련 링크
- [GitHub Repository](https://github.com/vasyapupkinski/codeit_mission_16_AI-Model-Optimization-Inference)