---
layout: page
title: "Clairvoyance AI Monitor: 글로벌 지능형 모니터링 에이전트"
category: "1. Projects / Personal Projects"
order: 1
---

# Clairvoyance AI Monitor
> **FastAPI + Celery + Redis + LLM + Harness Engineering의 정수를 담은 인텔리전스 시스템**

이 프로젝트는 단순한 데이터 수집기를 넘어, **그동안 학습한 모든 백엔드 및 AI 엔지니어링 기술을 집약시켜 구축할 예정인 프로젝트**입니다. 

---

## 프로젝트 비전
글로벌 OSINT(Open Source Intelligence) 데이터와 실시간 이벤트(분쟁, 금융, 자연재해 등)를 통합 수집하고, AI 에이전트가 이를 분석하여 사용자에게 의미 있는 인사이트를 제공하는 **프로덕션급 모니터링 시스템**을 지향합니다.

## 핵심 기술 아키텍처 (Planned)
- **High-Performance Backend**: **FastAPI**와 **Celery**, **Redis**를 활용한 비동기 분기 처리 및 대규모 데이터 파이프라인.
- **S-Tier AI Engine**: **LangChain** 기반의 에이전틱 워크플로우, Structured Output 및 Self-Correction 루프 탑재.
- **Harness-First Engineering**: 
    - **Observability**: Prometheus와 Grafana를 통한 실시간 시스템 메트릭 및 LLM 토큰 추적.
    - **Resilience**: Circuit Breaker와 Exponential Backoff를 통한 외부 API 의존성 격리.
    - **Security**: PII(개인정보) 자동 마스킹 및 데이터 가드레일 설계.

## 구현 목표
- **Telegram-Centric UX**: 텔레그램 봇을 통한 실시간 브리핑 및 양방향 AI 질의응답.
- **Adaptive Caching**: Redis 기반의 2-Tier 캐싱으로 응답 속도 최적화.
- **Global Data Freshness**: 전 세계 다양한 도메인의 데이터를 초단위로 수집하고 분석하는 신뢰성 있는 파이프라인 구축.

---

본 프로젝트는 현재 상세 설계 단계를 마치고 **구현 예정(Planned)** 상태이며, 모든 개발 과정은 Harness Engineering 원칙에 따라 투명하게 기록될 예정입니다.
