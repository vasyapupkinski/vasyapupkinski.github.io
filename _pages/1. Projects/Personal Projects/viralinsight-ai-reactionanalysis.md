---
layout: page
title: "ViralInsight - 실시간 바이럴 모니터링 및 여론 분석 플랫폼"
category: "1. Projects / Personal Projects"
status: "Planned"
order: 20
date: 2026-05-04
---

# ViralInsight - 실시간 바이럴 모니터링 및 여론 분석 플랫폼

소셜 플랫폼의 실시간 반응 데이터를 수집 및 분석하여 여론의 흐름과 이슈를 즉각적으로 파악하고 모니터링하는 플랫폼으로, **향후 개발 예정인 프로젝트**입니다.

## 프로젝트 개요 (Overview)
- **목표:** Reddit, YouTube 등 주요 소셜 미디어 플랫폼의 신규 피드를 추적하여 실시간 반응과 여론의 흐름을 분석.
- **문제 정의:** 기존 모니터링 도구들은 단순 키워드 빈도에 의존하여 실제 여론의 깊이나 논쟁의 본질을 파악하기 어려움.
- **해결 방안:** 직접 파인튜닝한 **SBV-LLM(Core Engine)**을 활용하여 댓글의 비정형 텍스트에서 감정 지수와 논쟁성 지수를 정밀하게 추출하고 시각화.

## 핵심 기능 (Core Features)
- **실시간 반응 수집 (Real-time Stream):** Reddit PRAW 및 YouTube Data API를 통해 타겟 채널 및 키워드에 대한 최신 반응 데이터를 초 단위로 수집.
- **SBV-LLM 여론 분석 (Sentiment & Context Analysis):** 단순 긍/부정을 넘어, 댓글 내의 논쟁성 지수, 화제성 전이 지수 등을 정교하게 수치화하여 데이터화.
- **키워드 스파이크 감지 (Reactive Monitoring):** 특정 브랜드명이나 모니터링 타겟 키워드의 출현 빈도가 급증하거나 여론이 급격히 악화될 경우 실시간 경고 알림 발송.
- **반응 가속도 시각화 (Reaction Velocity Dashboard):** 시간에 따른 반응의 증가 속도와 확산 범위를 대시보드를 통해 실시간으로 모니터링.

## 아키텍처 및 파이프라인 (Architecture)
- **Data Collection:** PRAW(Reddit API) 및 YouTube Data API를 연동하여 실시간 피드 및 댓글 스트림 수집.
- **Processing / AI:** Redis 큐와 Celery 워커를 활용하여 비동기 데이터 처리 파이프라인 구성. **SBV-LLM**을 활용하여 텍스트 내 심층 감정 및 컨텍스트 정보를 정밀 추출.
- **Serving / Frontend:** Streamlit 기반 실시간 대시보드를 구축하여 여론 현황 시각화 및 텔레그램 봇(Telegram Bot API)을 통한 즉각적인 알림 전송.

## 기술 스택 (Tech Stack)
- **Backend & Pipeline:** Python 3.11+, Celery
- **AI/ML:** Gemini 2.5 Flash API (Feature Extractor), SBV-LLM (Fine-tuned Engine)
- **Data & Infra:** Redis, SQLite / PostgreSQL
- **Frontend & Integration:** Streamlit, Telegram Bot API

## 엔지니어링 주안점 (Engineering Focus)
- **LLM 기반 정밀 분석:** 단순 키워드 매칭의 한계를 극복하기 위해 LLM을 활용한 정밀한 컨텍스트 분석 및 데이터 정형화(JSON Extractor Core 활용) 수행.
- **실시간 데이터 파이프라인:** 수천 건의 실시간 스트림 데이터를 지연 없이 처리하기 위해 Redis 기반의 분산 태스크 큐 최적화.
- **API Rate Limit 제어:** 외부 API의 호출 제한을 준수하면서도 실시간성을 유지하기 위한 지능형 트래픽 제어 시스템 구현.
- **확장성 있는 데이터 스키마:** 다양한 소셜 미디어의 반응 데이터를 통합적으로 분석할 수 있는 유연한 데이터 모델 설계.
