---
layout: page
title: "AI 기반 광고 이미지 생성 서비스 (AdGen)"
category: "Team Projects"
order: 3
---

## 프로젝트 개요
제품 사진 한 장으로 상업용 기획 및 광고 이미지를 자동 생성하는 End-to-End AI 광고 제작 파이프라인입니다. 자원 제한 상황에서의 **모델 서빙 최적화**와 **계층형 에이전트(Orchestration) 설계**를 통해 실무 수준의 완성도를 확보했습니다.

## 주요 기술 스택
- **AI Core**: `FLUX.1-Fill`, `Diffusers`, `BiRefNet`, `Real-ESRGAN`
- **Orchestration**: `Director-Manager 계층 구조`, `OpenAI API`, `Gemini (Failover)`
- **Optimization**: `NF4 Quantization`, `VRAMHandler & Staging 설계`, `Celery`, `Redis`

---

## 핵심 성과 및 전략 (Technical Story)

### 1. 계층형 오케스트레이션 및 문맥 격리 설계
팀원 이탈이라는 돌발 상황 속에서도 전체 5단계 파이프라인을 독자적으로 완수하며 관리형 에이전트 구조를 설계했습니다.
- **Director-Manager 계층 구조**: 기획(Director)과 상세 디자인(Manager) 로직을 분리하여 자율 디자인 품질 제어 및 정밀도 향상.
- **Failover 시스템**: OpenAI API 장애 시 **Gemini API**로 자동 폴백(Fallback)되는 이중화 구조를 구축하여 시스템 안정성 확보.

### 2. GPU 자원 제약 환경 극복 및 서빙 최적화
단일 GPU(VRAM 24GB) 내에서 고성능 모델을 안정적으로 서빙하기 위한 메모리 엔지니어링에 집중했습니다.
- **VRAMHandler & Staging**: NF4 양자화와 더불어, 모델 생명 주기를 관리하는 스테이징 설계를 통해 **OOM(Out Of Memory) 방지**.
- **Cold Start 최적화**: 서빙 파이프라인 최적화를 통해 이미지 생성 시간을 **77% 단축 (300s → 70s)**하여 실무 대기 시간 대폭 완화.

### 3. Heuristic Layout Guardrail 알고리즘
AI가 생성하는 오브젝트의 환각(Hallucinated Coordinates)을 방지하기 위해 물리적 안전장치를 도입했습니다.
- **자동 정밀 배치**: 휴리스틱 알고리즘을 기반으로 제품의 스케일, 세이프 마진(Safe Margin)을 제어하여 제품 이미지가 캔버스를 침범하거나 왜곡되는 **오류율 0%** 달성.

---

## 관련 링크
- [GitHub 전용 리포지토리](https://github.com/vasyapupkinski/codeitteam5_SMB_GenMarketingContents)
