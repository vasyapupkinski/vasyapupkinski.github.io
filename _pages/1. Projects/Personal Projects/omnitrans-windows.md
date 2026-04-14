---
layout: page
title: "OmniTrans Windows: 시스템 전역 실시간 AI 번역 유틸리티"
category: "1. Projects / Personal Projects"
order: 2
---

# OmniTrans Windows
> **C++ + Python + 로컬 LLM 기반의 윈도우 전역 실시간 텍스트 치환 및 번역 엔진**

이 프로젝트는 윈도우 시스템 레벨에서 타이핑과 동시에 AI 번역 및 텍스트 치환을 수행하는 **설치형 유틸리티**입니다.

---

## 프로젝트 비전
그 어떤 소프트웨어나 플랫폼 위에서도 제약 없이 AI 아키텍처의 도움을 받을 수 있는 환경을 구축합니다. 메모장, IDE, 게임 채팅 등 윈도우 전역에서 동작하는 보편적인 AI 번역 도구를 목표로 합니다.

## 핵심 기술 아키텍처 (Planned)
- **C++ System Hooking**: `SetWindowsHookEx` API를 활용한 저수준 키보드 이벤트 캡처 및 전역 디바운싱 알고리즘.
- **Adaptive Hardware Fallback**: 사용자의 하드웨어(GPU/VRAM)를 자동 감지하여 로컬 추론(llama.cpp/ExLlamaV2) 또는 클라우드 API(Gemini/Groq)를 실시간으로 선택하는 지능형 라우팅.
- **High-Speed IPC**: Named Pipe를 이용한 프로세스 간 저지연 통신으로 전체 파이프라인 지연시간 300ms 이하 달성 목표.
- **Modern Input Simulation**: `SendInput` API 기반의 자연스러운 텍스트 자동 치환 기능.

## 구현 목표
- **Zero-Friction UX**: 별도의 복잡한 설정 없이 더블클릭만으로 설치와 로컬 LLM 환경 구성이 완료되는 인앱 자동화.
- **Privacy-First**: 고사양 환경에서 100% 로컬 추론을 제공하여 개인정보와 데이터 보안 유지.
- **Universal Compatibility**: 모든 윈도우 애플리케이션과의 완벽한 호환성 확보 및 장애 시 클립보드 폴백 모드 지원.

---

본 프로젝트는 현재 아키텍처 설계를 완료하고 **구현 예정(Planned)** 상태이며, 시스템 프로그래밍과 최신 LLM 추론 기술의 결합을 테마로 진행될 예정입니다.
