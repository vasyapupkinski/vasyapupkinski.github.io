---
layout: page
title: "[Mission 17] Multimodal AI Interface"
category: "2. Milestones / Missions"
order: 17
---

## 미션 개요
다양한 형태의 데이터(상세 이미지, 카메라 캡처 등)를 처리하는 **Multimodal AI**를 구현하고, 사용자 친화적인 대화형 웹 인터페이스를 구축한 프로젝트입니다.

## 주요 기술 스택
- **Model**: `google/vit-base-patch16-224` (Vision Transformer)
- **Framework**: `Streamlit` (Interactive Dashboard)
- **Optimization**: `@st.cache_resource` (모델 로딩 효율화)

## 주요 기능 및 인터페이스
- **멀티모달 입력 시스템**: 파일 업로드(Upload), 실시간 카메라 촬영(Capture), 배치 이미지 선택 기능을 탭 구조로 구현.
- **실시간 데이터 시각화**: 분류 확률(Top-5)을 막대그래프(Bar Chart)로 실시간 렌더링하여 추론 결과의 보정 및 확인 가능.
- **리소스 최적화**: Streamlit의 캐싱 기능을 활용하여 세션 간 모델 중복 로딩을 방지하고 메모리 사용량을 최소화.

## 관련 링크
- [GitHub Repository](https://github.com/vasyapupkinski/codeit_mission_17_Multimodal-AI-Interactive-Interface)
