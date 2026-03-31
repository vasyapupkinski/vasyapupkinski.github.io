---
layout: page
title: "입찰 공고 분석 Agentic RAG"
category: "3. Projects"
order: 2
---

## 프로젝트 개요
수천 페이지에 달하는 복잡한 입찰 공고문에서 핵심 요건을 정밀 추출하고 질의응답을 제공하는 고성능 RAG(Retrieval-Augmented Generation) 시스템입니다. **데이터 무결성**과 **신뢰성 있는 출처(Source) 명시**를 최우선으로 설계했습니다.

## 주요 기술 스택
- **Parsing**: `Upstage Layout Analysis OCR`, `Header Chunking` (Context Preservation)
- **Search & Retrieval**: `Qdrant`, `BM25`, `Hybrid Search`, `Hybrid Reranking`
- **Orchestration**: `LangGraph Agentic RAG`, `GPT-5-mini`
- **Optimization**: `Kiwipiepy` (한국어 형태소 분석 최적화)

---

## 핵심 성과 및 전략 (Technical Story)

### 1. 구조적 파싱 및 계층적 문맥 보존
비정형 HWP/PDF 문서의 복잡한 표(Table)와 계층 구조를 보존하기 위한 데이터 엔지니어링에 몰입했습니다.
- **Upstage 구조화 파싱**: 단순 텍스트 추출이 아닌, 문서의 시각적 관계를 보존하여 계층적 문맥(Context)을 완벽하게 활용.
- **텍스트 이원화 전략**: 검색용(Summary)과 생성용(Raw) 데이터셋을 분리 관리하여 검색 품질과 답변 정확도를 동시에 확보.

### 2. 고정밀 정보 추출 및 할루시네이션(Hallucination) 방지
시스템의 신뢰도를 서비스 수준으로 끌어올리기 위한 **정밀 필터링** 로직을 구축했습니다.
- **메타데이터 우선 참조 설계**: 핵심 수치 정보(예산, 날짜 등)에 대해 메타데이터를 우선 참조하도록 설계하여 **수치 오류율 0%** 달성.
- **투명한 근거 제시**: 답변의 모든 근거를 출처(Source)와 함께 시각적으로 제시하여 시스템의 투명성과 객관성 확보.

### 3. Agentic Workflow 및 하이브리드 최적화
단순 검색을 넘어, 검색 실패와 품질을 스스로 판단하는 에이전트 구조를 도입했습니다.
- **LangGraph 기반 자율 폴백**: 검색 결과가 부적절할 경우 질문을 재구성(Query Transformation)하거나 폴백 로직을 실행하여 복잡한 질의에 대한 대응력 제고.
- **하이브리드 검색 & 재채점**: 키워드(BM25)와 의미론적(Vector) 검색을 결합하고 RRF(Reciprocal Rank Fusion)로 순위를 재조정하여 정밀한 정보 탐색 구현.

---

## 관련 링크
- [GitHub 전용 리포지토리](https://github.com/vasyapupkinski/codeitteam6_midproject_Bid-Analysis-RAG-System)