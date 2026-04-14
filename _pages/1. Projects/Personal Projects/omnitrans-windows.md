---
layout: page
title: "OmniTrans Windows: 시스템 전역 실시간 AI 번역 유틸리티"
category: "1. Projects / Personal Projects"
order: 2
---

# 🌐 OmniTrans Windows
> **C++ + Python + 로컬 LLM 기반의 윈도우 전역 실시간 텍스트 치환 및 번역 엔진**

현재 **Phase 1: Keyboard Hook & Core Engine** 단계 진행 중입니다.

---

## 📐 핵심 아키텍처
어떤 환경에서도 동작하는 **Adaptive Hardware Fallback** 구조를 통해 사양에 구애받지 않는 최적의 번역 경험을 제공합니다.

- **C++ System Core**: `SetWindowsHookEx`를 활용한 저수준 키보드 후킹 및 `SendInput` API 기반의 자연스러운 텍스트 치환.
- **Python AI Backend**: 하드웨어를 자동 진단(NVIDIA GPU/VRAM)하여 로컬 추론(llama.cpp/ExLlamaV2) 또는 클라우드 API(Gemini/Groq)로 처리 경로를 실시간 결정.
- **IPC Protocol**: Named Pipe를 이용한 고속 프로세스 간 통신으로 입력-번역-치환 전체 파이프라인 지연시간 300ms 이하 달성 목표.
- **Modern Packaging**: PyInstaller와 Inno Setup을 결합하여 추가 설치 없이 실행 가능한 `.exe` 배포 패키지 구성.

## 🛡️ Harness Engineering 포인트
- **System Hook Fallback**: 보안 소프트웨어 등에 의해 후킹이 차단될 경우, 자동으로 클립보드 감시 모드(Clipboard Polling)로 전환하여 서비스 연속성 유지.
- **Resource Guard**: VRAM 사용량을 실시간 모니터링하여 85% 초과 시 자동으로 경량 모델로 다운그레이드하거나 API 모드로 전환하는 자원 보호 로직 탑재.
- **Quality Gate**: LLM의 환각 현상(Hallucination)이나 서술형 답변을 필터링하여 순수 번역문만 텍스트 입력부로 전달하는 정합성 레이어.

---

## 🗓️ 현재 진행 상태 (Roadmap)
- [x] **Phase 0**: C++ ↔ Python IPC 프로토콜 및 에코 테스트 완료
- [\/] **Phase 1**: 전역 키보드 후킹 및 디바운싱 알고리즘 구현 (진행 중)
- [ ] **Phase 2**: 하드웨어 가속 자동 감지 및 스마트 라우터
- [ ] **Phase 3**: 로컬 LLM(ExLlamaV2) 및 클라우드 API 통합 번역 엔진
- [ ] **Phase 4**: 인앱 모델 자동 다운로더 및 온보딩 UI
