---
layout: page
title: "Clairvoyance AI Monitor: 글로벌 지능형 모니터링 에이전트"
category: "1. Projects / Personal Projects"
order: 1
---

# Clairvoyance AI Monitor
> **FastAPI + Celery + Redis + Telegram Bot 기반의 프로덕션급 글로벌 인텔리전스 모니터링 시스템**

현재 **Phase 1: Data Collection Engine** 단계 진행 중입니다.

---

## 핵심 아키텍처
본 프로젝트는 **Harness-First** 원칙에 따라 기능 구현 전 모니터링과 회복력 레이어를 시스템의 핵심으로 설계했습니다.

- **Data Sources**: ACLED(분쟁), GDELT(이벤트), USGS(지진), NASA FIRMS(산불), Yahoo Finance 등 6개 이상의 글로벌 도메인 실시간 수집.
- **Async Pipeline**: Celery와 Redis를 활용한 비동기 분산 처리로 대규모 데이터 스트림 처리.
- **AI Intelligence**: LLM Fallback Chain(OpenAI → Groq → Ollama)을 통한 중단 없는 분석.
- **Harness Layer**: Prometheus와 Grafana를 통한 수집 지연시간 및 LLM 토큰 사용량 실시간 관측.

## Harness Engineering 포인트
- **2-Tier Caching**: Local Memory(L1)와 Redis(L2)를 결합하여 외부 API 부하를 최소화하고 응답 속도를 극대화.
- **Circuit Breaker**: 외부 API 장애 시 서킷을 개방하고 로컬 캐시 데이터를 서빙하여 서비스 가용성 보장.
- **Structured Output**: AI 분석 결과의 JSON 정합성을 100% 보장하는 Self-Correction 루프 탑재.

---

## 현재 진행 상태 (Roadmap)
- [x] **Phase 0**: Harness Foundation & Infra 구축 (Docker, Prometheus, Logging)
- [\/] **Phase 1**: 데이터 수집 엔진 및 Pydantic v2 데이터 정합성 검증 레이어 (진행 중)
- [ ] **Phase 2**: LLM 기반 스마트 요약 및 중복 제거 엔진
- [ ] **Phase 3**: aiogram 3 기반 양방향 텔레그램 인터페이스
- [ ] **Phase 4**: 실시간 웹 대시보드 (WebSocket)
