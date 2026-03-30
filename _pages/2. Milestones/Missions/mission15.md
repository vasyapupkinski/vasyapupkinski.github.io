---
layout: page
title: "[Mission 15] MLOps Foundation (Docker/FastAPI)"
category: "2. Milestones / Missions"
order: 15
---

## 미션 개요
AI 모델을 가공하고 서비스화하는 전 과정을 **Docker 컨테이너화**하고, 자동화된 모델 서빙 파이프라인을 구축한 MLOps 기초 프로젝트입니다.

## 아키텍처 및 기술 스택
- **Container**: `Docker`, `Docker-Compose` (Multi-service Orchestration)
- **Framework**: `FastAPI` (Asynchronous Model Serving)
- **Base Image**: `python:3.12-slim` (경량화된 서빙 환경 구축)

## 주요 구현 성과
- **서비스 격리 및 연동**: 모델 학습(`researcher-1`)과 API 서빙(`researcher-2`) 서비스를 분리하여 독립적인 운영 환경 확보.
- **데이터 영속성 및 공유**: **Shared Volume**(`shared`) 구성을 통해 전처리된 데이터와 학습 모델을 서비스 간 안전하게 전송.
- **API 안정성**: **Pydantic** 스키마를 통한 입출력 데이터의 엄격한 유효성 검증으로 서빙 안정성 강화.

## 관련 링크
- [GitHub Repository](https://github.com/vasyapupkinski/codeit_mission_15_MLOps-Docker-Collaborative-Pipeline)