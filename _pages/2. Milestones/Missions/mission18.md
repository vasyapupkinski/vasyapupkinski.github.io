---
layout: page
title: "[Mission 18] Fullstack Movie AI Platform"
category: "2. Milestones / Missions"
order: 18
---

## 미션 개요
실시간 영화 감성 분석 및 자동 요약을 제공하는 **풀스택 AI 플랫폼**으로, 프론트엔드부터 백엔드, 클라우드 DB까지 전체적인 서비스 아키텍처를 설계한 프로젝트입니다.

## 통합 기술 스택
- **AI Backend**: `FastAPI`, `SQLAlchemy`, `koelectra-base-v3` (Sentiment), `Qwen2.5-7B` (Summarization)
- **Frontend**: `React 18`, `Vite` (Modern JS Bundling)
- **Database**: `Supabase` (PostgreSQL), `Supabase Auth` (사용자 인증)

## 시스템 아키텍처 및 로직
- **다층적 AI 파이프라인**: 
  - **감성 분석**: `koelectra-base-v3`로 리뷰 평점(0.0~1.0) 및 긍/부정 지표 자동 산출.
  - **자동 요약**: `Qwen2.5-7B`를 활용하여 2건 이상의 누적 리뷰에 대해 비판적/종합적 요약 생성.
- **안정적인 데이터 관리**: **Supabase**를 연동하여 실제 서비스 환경에서의 유저 데이터 동기화 및 실시간 CRUD 구현.

## 관련 링크
- [GitHub Repository](https://github.com/vasyapupkinski/codeit_mission_18_Fullstack-Movie-AI-Analytics)