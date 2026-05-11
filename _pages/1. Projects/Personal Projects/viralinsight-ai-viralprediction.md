---
layout: page
title: "ViralInsight - 바이럴 조기 감지 및 트렌드 분석 플랫폼"
category: "1. Projects / Personal Projects"
status: "Planned"
order: 20
date: 2026-05-04
---

# ViralInsight - 바이럴 조기 감지 및 트렌드 분석 플랫폼

소셜 플랫폼의 초기 반응 패턴을 실시간으로 수집 및 분석하여 콘텐츠의 바이럴 확산 가능성을 조기에 감지하고 예측하는 플랫폼으로, **향후 개발 예정인 프로젝트**입니다.

## 프로젝트 개요 (Overview)
- **목표:** Reddit, YouTube 등 주요 소셜 미디어 플랫폼의 신규 게시물 트래픽을 추적하여 바이럴 가능성이 높은 콘텐츠를 선제적으로 식별.
- **문제 정의:** 기존 트렌드 분석 도구들은 이미 확산이 완료된 사후 데이터(Lagging Indicator)만을 제공하므로, 마케터 및 크리에이터가 트렌드에 선제적으로 대응하기 어려움.
- **해결 방안:** 콘텐츠 게시 직후의 시간차 스냅샷 데이터와 댓글의 비정형 텍스트를 결합한 하이브리드 머신러닝 아키텍처(XGBoost + LLM)를 구축하여 확산 궤적을 예측.

## 핵심 기능 (Core Features)
- **초기 임계값 필터링 (Threshold Filtering):** 실시간으로 유입되는 대규모 게시물 중, 게시 후 5분 내 특정 기준을 통과한 상위 1%의 유의미한 콘텐츠만 선별하여 추적 파이프라인에 등록.
- **시간차 스냅샷 추적 (Time-lapse Snapshot):** 단발성 수집이 아닌 t=0, t=15, t=30 시점의 데이터를 연속 수집하여 조회수 및 댓글의 증가 가속도를 정량적 피처로 계산.
- **하이브리드 AI 예측 (Hybrid Prediction):** LLM을 활용하여 비정형 데이터(댓글)에서 논쟁성 지수 및 감정 지수를 추출하고, 이를 시계열 메트릭과 결합하여 XGBoost 앙상블 모델을 통해 바이럴 확률 산출.
- **키워드 스파이크 감지 (Reactive Monitoring):** 특정 브랜드명이나 모니터링 타겟 키워드의 출현 빈도가 이동평균 대비 급증할 경우, 실시간 경고 알림 발송.

## 아키텍처 및 파이프라인 (Architecture)
- **Data Collection:** PRAW(Reddit API) 및 YouTube Data API를 연동하여 타겟 서브레딧과 키워드의 실시간 피드 수집.
- **Processing / AI:** Redis 큐와 Celery 워커를 활용하여 비동기 지연 재조회(Delayed Retry) 파이프라인 구성. 직접 파인튜닝한 **SBV-LLM(Core Engine)**을 활용하여 댓글 내 논쟁성/감정 지수를 정밀 수치화한 후 XGBoost 추론 수행.
- **Serving / Frontend:** Streamlit 기반 대시보드를 구축하여 예측 랭킹 시각화 및 텔레그램 봇(Telegram Bot API)을 통한 즉각적인 알림 전송.

## 기술 스택 (Tech Stack)
- **Backend & Pipeline:** Python 3.11+, Celery
- **AI/ML:** XGBoost, LightGBM, Gemini 2.5 Flash API (Feature Extractor)
- **Data & Infra:** Redis, SQLite / PostgreSQL
- **Frontend & Integration:** Streamlit, Telegram Bot API

## 엔지니어링 주안점 (Engineering Focus)
- **시그널 수렴 및 피처 엔지니어링:** 단순 LLM API 호출에 의존하지 않고, LLM을 데이터 전처리(Feature Extractor) 용도로 제한한 후 정통 시계열 예측 모델(XGBoost)에 결합하는 실무적인 ML 아키텍처 설계.
- **합성 데이터(Synthetic Data) 전략:** 초기 학습 데이터 부족 문제를 해결하기 위해 시뮬레이션 데이터를 활용하여 전체 파이프라인을 선행 구축하고, 실제 데이터 수집 시 Seamless하게 전환할 수 있는 구조 적용.
- **큐 관리 및 비동기 스케줄링:** 수만 건의 게시물에 대한 시간차 추적 요청을 처리하기 위해 Redis 기반의 분산 태스크 큐를 최적화하여 서버 부하 및 메모리 누수 방지.
- **API Rate Limit 제어:** 외부 API의 호출 제한(Rate Limit)을 우회 및 준수하기 위한 토큰 버킷(Token Bucket) 기반의 트래픽 제어 시스템 구현.
