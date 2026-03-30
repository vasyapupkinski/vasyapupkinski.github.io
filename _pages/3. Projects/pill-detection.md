---
layout: page
title: "알약 객체 검출 및 데이터 신뢰성 오디팅"
category: "3. Projects"
order: 1
---

## 프로젝트 개요
비정상적인 모델 성능 지표(mAP 0.99)의 근본 원인을 데이터 기저에서 파헤치고, 통계적 오디팅을 통해 AI 서비스의 실무 신뢰도를 확보한 Data-Centric 프로젝트입니다.

## 주요 기술 스택
- **Computer Vision**: `YOLOv8n` (Object Detection)
- **Data Centric**: 다각적 임계 통계 EDA(환경/객체/분포), 실루엣 분석(-0.49), Pseudo-labeling
- **Optimization**: 7:1.5:1.5 계층적 분할(Stratified Split) 기반 데이터 릭(Leak) 방지

## 주요 성과 및 기술적 고찰
- **성능 지표의 허구성 입증**: 초기 mAP 0.986 달성에도 불구하고, **"과연 이 점수가 지능적 학습의 결과인가?"**라는 비판적 의구심으로 오디팅을 실시하여 '각인(Imprint)' 미세 특징에 대한 과적합 현상을 통계적으로 증명.
- **데이터 무결성 복원**: Pseudo-labeling 파이프라인을 구축하여 오염된 데이터셋 내 840개 라벨 오류를 100% 전수 복구.
- **Data-Centric 전략 리딩**: 단순 모델 튜닝 경쟁에서 벗어나, 데이터 품질 가시화 및 정제 플랫폼으로서의 프로젝트 방향을 주도적으로 피벗(Pivot).

## 관련 링크
- [GitHub 전용 리포지토리](https://github.com/vasyapupkinski/codeitteam7_Pill-Detection-AI-Diagnostic-Optimization)