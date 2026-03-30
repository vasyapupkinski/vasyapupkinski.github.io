---
layout: page
title: "공공 입찰 분석 RAG 시스템"
category: "3. Projects"
order: 2
---

## 프로젝트 개요
수천 페이지에 달하는 복잡한 입찰 공고문에서 핵심 요건을 정밀 추출하고 질의응답을 제공하는 고성능 RAG(Retrieval-Augmented Generation) 시스템입니다.

## 주요 기술 스택
- **Search**: `LangChain`, `Upstage Layout Analysis OCR`, `LangGraph`
- **Retrieval**: `Qdrant` (High-Performance Vector Store), `BM25`, `Hybrid Search`
- **LLM**: `GPT-5-mini` (Query Transformation & Generation)
- **Tokenization**: `Kiwipiepy` (한국어 복합명사 처리 및 형태소 분석 최적화)

## 주요 성과 및 기술적 차별화
- **속도와 확장성 확보**: Rust 기반의 **Qdrant 엔진**을 채택하여 Python 기반 스토어 대비 검색 레이턴시 단축 및 대규모 데이터 처리 안정성 확보.
- **핵심 정보 할루시네이션 0% 달성**: Qdrant의 **Payload Filtering**과 메타데이터 우선 참조 설계를 통해 핵심 수치 정보(예산, 날짜)의 정보 추출 정밀도 극대화.
- **한국어 검색 고도화**: `Kiwipiepy` 연동을 통해 법률/행정 전문 용어의 형태소 분석 정밀도를 높여 검색 성능 체감 향상.
- **Agentic Workflow**: LangGraph를 활용하여 검색 실패 시 폴백(Fallback) 및 자율 수정 프로세스를 설계하여 시스템 완성도 제고.

## 관련 링크
- [GitHub 전용 리포지토리](https://github.com/vasyapupkinski/codeitteam6_midproject_Bid-Analysis-RAG-System)