---
layout: page
title: "AdGen — AI 광고 이미지 자동 제작 솔루션"
category: "3. Projects"
order: 3
---

## 프로젝트 개요
제품 사진 한 장으로 상업용 기획 및 광고 이미지를 자동 생성하는 End-to-End AI 광고 제작 파이프라인입니다. 자원 제한 상황에서의 모델 서빙 최적화와 계층형 에이전트 설계에 집중했습니다.

## 주요 기술 스택
- **AI Engine**: `FLUX.1-Fill`, `BiRefNet`, `Real-ESRGAN`
- **Orchestration**: `GPT-5-mini` & `Gemini` 기반의 계층형(Director-Manager) 기획 로직
- **Optimization**: `NF4 Quantization` 및 **VRAMHandler 스테이징 설계**

## 주요 성과 및 기술적 차별화
- **자원 제약 환경 극복**: 고성능 파운데이션 모델(FLUX.1)을 **단일 GPU(VRAM 24GB) 내에서 구현**하기 위해 NF4 양자화 및 지능형 스테이징 메모리 관리 기술 적용.
- **사용자 경험(UX) 최적화**: 파이프라인 최적화를 통해 이미지 생성 프로세스를 **77% 단축(300s → 70s)**하여 실무 대기 시간 대폭 완화.
- **물리 정밀 레이아웃**: Heuristic Guardrail 알고리즘을 통한 제품의 스케일 및 배경 조화도 제어로 생성 이미지의 품질 객관화.
- **완전 자동화 엔지니어링**: 광고 전략 기획부터 고해상도 합성까지의 복잡한 5단계 공정을 단일 워크플로우로 완전 자동화.

## 관련 링크
- [GitHub 전용 리포지토리](https://github.com/vasyapupkinski/codeitteam5_SMB_GenMarketingContents)