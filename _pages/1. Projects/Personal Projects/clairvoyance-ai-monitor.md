---
layout: page
title: "Project Clairvoyance - 글로벌 인텔리전스 통찰 시스템"
category: "1. Projects / Personal Projects"
status: "Planned"
order: 30
date: 2026-05-03
---

# Project Clairvoyance - 글로벌 인텔리전스 통찰 시스템

전 세계의 실시간 오픈소스 인텔리전스(OSINT) 및 다종의 데이터를 수집, 교차 분석하여 거시적 인사이트와 예측을 제공하는 글로벌 모니터링 에이전트 시스템으로, **향후 개발 예정인 프로젝트**입니다.

## 프로젝트 개요 (Overview)
- **목표:** 전 세계 무료 데이터 소스(자연재해, 경제, 지정학적 이슈 등)를 통합하여, 상호 독립적인 데이터 포인트에서 유의미한 상관관계를 추론해 내는 정보 체계 구축.
- **문제 정의:** 기존의 AI 모니터링 툴은 뉴스 원문을 단순 요약(Summarization)하는 수준에 그쳐, 이종 데이터 간의 인과관계를 파악하거나 복합적인 판단을 내리지 못함.
- **해결 방안:** 수집, 구조화, 교차 분석, 맥락화, 판단, 예지로 이어지는 6-Layer Intelligence Pipeline을 구축하고, 전문화된 멀티 에이전트를 통해 데이터 간 Signal Convergence(신호 수렴)를 탐지.

## 핵심 기능 (Core Features)
- **다중 소스 교차 분석 (Signal Convergence):** 뉴스, 항공기 트래픽, 인터넷 장애 빈도, 자연재해 알림 등 서로 다른 도메인의 데이터가 지리적/시간적으로 일치할 경우 중요 이벤트로 격상.
- **계층적 브리핑 자동 생성:** 일일 위기 탐지 속보, 주간 추세 분석, 월간 궤적 평가 등 Map-Reduce 방식의 시간축 기반 리포팅 자동화.
- **양방향 대화형 인터페이스:** Telegram 봇을 연동하여 특정 지역이나 이벤트에 대한 실시간 AI 질의응답 및 알림 수신 지원.
- **자율 웹 검색 및 심층 열람 (Agentic Retrieval):** DuckDuckGo, Crawl4AI, ScrapeGraphAI를 MCP 규격으로 연동하여 에이전트가 팩트체크를 위해 스스로 기사 원문을 획득 및 구조화.
- **비전 AI 기반 문서 해독:** OSINT 채널 특성상 자주 유출되는 전장 지도, 통계 인포그래픽, 외국어 문서를 Vision LLM(Qwen-VL)으로 정밀 해석.

## 아키텍처 및 파이프라인 (Architecture)
- **Data Collection:** Celery Beat 기반 정기 스케줄러를 활용하여 GDELT, USGS, Cloudflare Radar, Telegram OSINT 등 15개 이상의 무료 데이터 소스 주기적 수집.
- **Processing / AI:** LangChain 기반 멀티 에이전트(감시자, 분석관, 판단관, 예언자) 아키텍처. 작업 복잡도에 따라 Gemini Flash(수집/추출)와 Groq Llama 70B(심층 분석)로 동적 라우팅 수행.
- **Serving / Frontend:** WebSocket 기반의 대시보드 및 Telegram Bot API를 통해 최종 인텔리전스 배포.

## 기술 스택 (Tech Stack)
- **Backend:** FastAPI, Python 3.11+
- **AI/ML:** LangChain, OpenAI API, Groq, Ollama, Qwen-VL
- **Data & Infra:** Celery, Redis, PostgreSQL, NetworkX (Knowledge Graph)
- **Integration:** FastMCP, Telethon, python-telegram-bot

## 엔지니어링 주안점 (Engineering Focus)
- **LLM 라우팅 최적화 (Smart Router):** 비용 제로(0원) 파이프라인 유지를 위해 단일 모델에 의존하지 않고, 작업 난이도에 따라 가장 효율적인 무료 API 또는 Local 모델로 요청을 라우팅.
- **다중 계층 복구 체계 (Multi-layered Fallback):** 외부 의존성이 높은 글로벌 API 호출 실패 시, Circuit Breaker 발동 및 Ollama 기반 로컬 캐시 모델로 즉시 전환(Zero Downtime) 보장.
- **데이터 신뢰성 검증 체계:** 무분별한 텔레그램 속보나 가짜뉴스를 걸러내기 위해, 단일 출처 정보는 보류하고 교차 검증(공식 언론 또는 2차 데이터)이 완료된 건만 신뢰도 격상 알고리즘 적용.
- **데이터 정규화 및 스키마 검증:** 15개 이상의 파편화된 외부 데이터 소스를 통합하기 위해 Pydantic을 활용한 엄격한 스키마 검증(Quality Gate) 파이프라인 구축.