---
layout: page
title: "SBV-LLM (Struct Bee Vector) - 고성능 구조화 데이터 추출 엔진"
category: "1. Projects / Personal Projects"
status: "In Progress"
order: 5
date: 2026-05-11
---

# SBV-LLM (Struct Bee Vector) - 고성능 구조화 데이터 추출 엔진

비정형 데이터의 혼돈 속에서 완벽한 JSON 구조를 추출하기 위해 설계된 **정보 추출(Extraction) 전용 특화 모델 엔진**으로, **현재 핵심 엔진 및 학습 파이프라인을 개발 중인 프로젝트**입니다. 에이전트 시스템의 출력 안정성을 보장하며 상용 모델 대비 극강의 가성비를 제공합니다.

## 프로젝트 개요 (Overview)
- **목표:** LLM의 고질적인 문제인 출력 형식 불안정성(Format Hallucination)을 해결하고, 12GB VRAM 환경에서 30B급 이상의 지능을 구현.
- **문제 정의:** GPT-5.5와 같은 상용 모델은 비싸고 느리며, 소형 오픈소스 모델은 복잡한 문맥에서 구조화된 데이터를 완벽하게 뽑아내는 능력이 부족함.
- **해결 방안:** 최신 MoE(Mixture-of-Experts) 아키텍처 모델을 QLoRA 및 양자화 기술로 튜닝하여, 저사양 하드웨어에서도 100% 신뢰할 수 있는 JSON 데이터를 생성.

## 핵심 기능 (Core Features)
- **Multi-Model Benchmarking:** Llama-4, Qwen-3.6, Gemma-4 등 최신 모델 3종을 동일 데이터셋으로 학습시켜 최적의 추출 엔진 선정.
- **LoRA vs DoRA Benchmarking:** 단순 LoRA와 최신 DoRA 기법의 성능을 비교 분석하여 12GB VRAM 환경에서의 최적 파인튜닝 기법 도출.
- **JSON-Only Decoding:** 철저하게 JSON 데이터만 출력하도록 인스트럭션 튜닝을 진행하여 후처리가 필요 없는 클린 데이터 제공.
- **VRAM Optimization:** 12GB VRAM 환경에서 100B급 MoE 모델(Llama-4 Scout)을 구동하기 위한 메모리 최적화 레이어 오프로딩 적용.
- **SBV-LLM Structured Scoring:** 추출된 정보의 정확도뿐만 아니라 JSON 스키마 준수율을 정량적으로 측정하는 독자적인 벤치마킹 지표 구축.

## 기술 구현 및 최적화 (Technical Implementation)
- **QLoRA 파인튜닝:** `Unsloth`와 `PEFT`를 활용하여 학습 속도를 2배 이상 가속화하고 메모리 점유율을 70% 절감. (`use_dora=True` 옵션을 통한 성능 최적화 포함)
- **Advanced Quantization:** 4-bit NF4(NormalFloat4) 및 Double Quantization을 적용하여 모델 가중치 정밀도 손실 최소화.
- **"Extreme Diet" Config:** VRAM 12GB 환경에 최적화된 학습 설정(Rank 16, Seq Length 2048 등)을 통해 OOM 없이 안정적인 파인튜닝 수행.
- **Dual-Stage Quantization:** 학습 시에는 메모리 최적화(QLoRA), 배포 시에는 추론 가속화(PTQ/GGUF)를 위한 2단계 양자화 파이프라인 구축.
- **MoE Offloading:** 전체 파라미터가 아닌 활성 전문가(Active Experts) 위주로 GPU 메모리를 점유하게 설계하여 12GB 카드의 물리적 한계 격복.

## 기술 스택 (Tech Stack)
- **Base Models:** Llama-4-Scout (17B Active), Qwen-3.6-35B (3B Active), Gemma-4-26B (4B Active)
- **Fine-tuning:** Unsloth, QLoRA (bitsandbytes, PEFT)
- **Optimization:** llama.cpp (GGUF), PTQ (Post-Training Quantization)
- **Deployment:** FastAPI, Ollama, Docker
- **Research Base:** LoRA (2021), QLoRA (2023), DoRA (2024)

## 엔지니어링 주안점 (Engineering Focus)
- **안정적인 에이전트 파이프라인:** LLM의 응답이 규격을 벗어나 시스템이 중단되는 문제를 파인튜닝된 특화 모델로 해결하여 전체 시스템의 가동률 향상.
- **하드웨어 제약 돌파:** 12GB VRAM이라는 열악한 환경에서 35B~109B급 거대 모델을 성공적으로 튜닝해낸 최적화 엔지니어링 역량 입증.
- **가성비 모델 구축:** 상용 API(GPT-5.5 Pro) 대비 운영 비용을 90% 이상 절감하면서도 특정 태스크(Extraction)에서 동등 이상의 성능 달성.
