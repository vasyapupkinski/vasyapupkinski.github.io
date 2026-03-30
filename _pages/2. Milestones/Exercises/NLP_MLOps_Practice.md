---
layout: page
title: "Part 3 & 4: NLP & MLOps & RAG 실습"
category: "2. Milestones / Exercises"
order: 2
---

## 실습 개요
텍스트 파싱부터 최신 LLM 아키텍처, 그리고 이를 실서비스에 배포하기 위한 MLOps 인프라 구축까지의 통합 실습 기록입니다.

## Part 3. 자연어 처리(NLP) 및 생성형 AI
*   **01_NLP_and_Transformers**: 텍스트 데이터 전처리, 임베딩, BERT/GPT 등 트랜스포머 아키텍처 학습.
*   **02_LLM_FineTuning**: `klue/bert-base`, `Gemma` 모델 기반의 **PEFT(LoRA/QLoRA)** 및 프롬프트 엔지니어링 실습.
*   **03_RAG_and_Agents**: **LangGraph**를 이용한 조건부 라우팅(Conditional Routing) 및 **Claude 3(via AWS Bedrock)** 연동 멀티모달 에이전트 설계.

## Part 4. AI 서비스 배포 및 MLOps
*   **04_MLOps_and_Serving**:
    *   **모델 서빙**: **FastAPI** 기반 API 엔드포인트 설계 및 웹앱 프로토타입 개발.
    *   **최적화**: **ONNX**, Symmetric/Asymmetric **INT8 양자화** 실습.
    *   **인프라**: **vLLM (PagedAttention)**, **Triton Inference Server** 기반 고성능 서빙 및 **Docker** 컨테이너화.

## 주요 사용 기술
- `Python`, `HuggingFace`, `LangChain`, `LangGraph`
- `vLLM`, `NVIDIA Triton`, `Docker`, `ONNX`
- `AWS Bedrock (Boto3)`, `Qdrant`, `PostgreSQL`

## 관련 링크
- [GitHub 실습 코드 바로가기](https://github.com/vasyapupkinski/codeit-ai-practice/tree/main/NLP_MLOps%26Serving)