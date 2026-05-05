---
layout: page
title: "Devoffs-AI-Jobaggregator"
category: "1. Projects / Personal Projects"
order: 1
---

# Devoffs-AI-Jobaggregator

개발자의 기술 스택과 실무 프로젝트 역량에 최적화된 맞춤형 채용 기회를 제공하는 지능형 채용 데이터 통합 플랫폼입니다.

## 프로젝트 개요 (Overview)
- **목표:** 다수의 채용 플랫폼에 분산된 공고를 수집하고, 개발자 관점에서 유의미한 정보만 정제하여 통합 제공.
- **문제 정의:** 기존 채용 플랫폼의 키워드 검색은 프레임워크 버전, 인프라 환경, 실제 수행 프로젝트 등의 상세 기술 요구사항을 정확히 반영하지 못함.
- **해결 방안:** 크롤링과 LLM 파이프라인을 결합하여 JD(Job Description)의 비정형 텍스트를 구조화된 데이터로 변환하고, 벡터 검색을 통해 이력서 기반 정밀 매칭 수행.

## 핵심 기능 (Core Features)
- **기술 스택 정밀 추출:** JD 본문에서 단순 키워드를 넘어 프레임워크 조합 및 인프라 환경(예: Spring Boot 3.x + Kotlin)을 분리하여 추출.
- **개발자 중심 요약 (AI Summarizer):** 공고 내 핵심 수행 프로젝트, 기술적 도전 과제, 협업 방식을 3줄로 자동 요약.
- **시맨틱 중복 공고 병합:** 원티드, 사람인 등 여러 플랫폼에 중복 게재된 공고를 벡터 유사도 기반으로 식별하여 단일 공고로 통합.
- **대화형 검색 에이전트:** 디스코드/텔레그램 봇을 연동하여 자연어 기반 조건 검색 및 필터링(Slot-filling) 지원.
- **이력서 기반 맞춤 매칭:** 사용자가 업로드한 이력서를 파싱하여 요구 기술 스택과 매칭되는 공고를 실시간 추천.

## 아키텍처 및 파이프라인 (Architecture)
- **Data Collection:** Celery Beat 기반 스케줄러와 Crawl4AI를 활용하여 타겟 플랫폼의 채용 공고를 마크다운 형식으로 정기 수집.
- **Processing / AI:** ScrapeGraphAI와 Local LLM(Ollama)을 연동하여 수집된 마크다운에서 스택 및 요약 데이터를 JSON으로 추출 및 정규화.
- **Serving / Frontend:** FastAPI를 통해 정제된 데이터를 서빙하며, SSE(Server-Sent Events)를 활용하여 AI 분석 과정을 실시간 스트리밍. Redis를 통해 검색 쿼리 및 대화 상태 캐싱.

## 기술 스택 (Tech Stack)
- **Backend:** FastAPI, Python 3.11+
- **AI/ML:** LangChain, Ollama (Qwen, Llama), Gemini 2.5 Flash, Milvus (Vector DB)
- **Data & Infra:** Celery, Redis, PostgreSQL, Elasticsearch, Docker
- **Auth:** Supabase (Google OAuth 2.0)

## 엔지니어링 주안점 (Engineering Focus)
- **파이프라인 안정성:** 타겟 사이트의 레이아웃 변경이나 차단에 대비한 Crawler Circuit Breaker 구현 및 에러 감지 로직 적용.
- **비용 최적화 (Cost Efficiency):** LLM 토큰 소모량을 최소화하기 위해 프롬프트를 압축하고, 복잡도에 따라 Local LLM과 상용 API를 선택적으로 라우팅.
- **응답 지연 시간(Latency) 관리:** 대규모 공고 데이터에 대한 임베딩 벡터 검색 시, 응답 속도를 100ms 이하로 유지하기 위한 캐싱 및 색인 최적화.
- **보안 및 개인정보 보호 (PII Guard):** 이력서 분석 과정에서 이름, 연락처 등 민감 정보(PII)가 외부 API로 노출되지 않도록 로컬 모델에서 자동 마스킹 처리.
