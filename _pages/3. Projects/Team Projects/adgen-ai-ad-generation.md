---
layout: page
title: "AdGen AI: 오케스트레이션을 통한 고가용성 생성 파이프라인 구축"
category: "3. Projects / Team Projects"
order: 3
---

# AI 기반 광고 이미지 자동 생성 시스템 (AdGen)

> **"AI의 핵심은 모델이 아니라, 모델들을 엮어 실전 가치를 만드는 시스템(Orchestration)이다"**

---

## TL;DR

| 항목 | 내용 |
|---|---|
| **프로젝트** | 제품 사진 1장으로 기획부터 최종 광고 이미지까지 자동 생성하는 End-to-End AI 서비스 |
| **나의 역할** | **팀장(PM) — AI Core System 구축 및 오케스트레이션 총괄** |
| **핵심 성과** | 5단계 이미지 파이프라인 자동화, VRAM 최적화로 이미지 생성 속도 **77% 단축(300초 → 70초)** 및 OOM(GPU 메모리 부족) 문제 완전 해결 |
| **최종 스택** | FLUX.1-Fill (NF4), BiRefNet, Real-ESRGAN, FastAPI, Redis, Celery, OpenCV, PyTorch |

---

![AdGen 시스템 아키텍처: 사용자 요청부터 최종 광고 이미지 생성까지의 5단계 오케스트레이션 흐름도](/assets/images/projects/adgen/architecture-flow.png)
<br>

---

## 1. 프로젝트 배경 및 기술 철학

### 모델의 한계를 넘는 '공학적 흐름(Flow)'의 설계
최근 생성형 AI(Generative AI)의 발전으로 고품질 이미지를 만드는 것은 쉬워졌으나, 이를 **'상업적 광고 제작 공정'**에 그대로 적용하기에는 한계가 명확했습니다. 모델은 레이아웃의 비틀림(Hallucinated Layout)을 제어하기 어렵고, 고해상도 변환이나 텍스트 합성 등 후처리 공정이 파편화되어 있기 때문입니다.

저는 팀장으로서 단순히 프롬프트 한 줄을 고치는 것을 넘어, **"파편화된 AI 엔진들을 하나의 유기적인 시스템으로 오케스트레이션(Orchestration)하는 것"**을 프로젝트의 핵심 과제로 설정했습니다. AI 코어 시스템 설계자로서, 모델의 불확실성을 공학적 가드레일로 통제하고 실전 서비스가 가능한 수준의 신뢰도를 확보하는 데 집중했습니다.

---

## 2. 개발 및 분석 스택

| Category | Technology | Usage |
|----------|-----------|-------|
| **AI Engine** | FLUX.1-Fill (NF4) | Inpainting 기반 지능적 배경 생성 및 제품 합성 |
| **Vision AI** | BiRefNet / ISNet | 객체 세그멘테이션 (누끼) 및 기하학적 분석 |
| **Orchestration** | Director-Manager Pattern | 5단계 파이프라인 생명주기 제어 및 상태 관리 |
| **Optimization** | VRAMHandler (Singleton) | GPU 메모리 점유 최적화 및 로딩 속도 가속 |
| **Infrastructure** | Redis / Celery | 고부하 이미지 생성 작업의 비동기 분산 처리 |
| **Serving** | FastAPI / Next.js | 비동기 API 서빙 및 캔버스 기반 인터랙티브 편집기 |

---

## 3. 팀 구성

| 이름 | 역할 |
|:---|:---|
| **이승완 (팀장)** | **AI Core System 아키텍처 설계 및 구현**. 5-Stage 제작 파이프라인 개발, VRAM 최적화 전략 수립, 계층형 오케스트레이션(Director) 구축, 물리 기반 레이아웃 가드레일 설계 |
| 박시찬 | **프론트엔드 아키텍처 및 서비스 전반 구현**. Next.js 기반 UI/UX 구축, Konva.js 캔버스 편집기 및 실시간 피드백 루프 개발, 프론트-백엔드 통합 연동 |
| 신아름 | 프로젝트 시각화 자료 및 발표 자료(PPT) 제작 지원 |

---

## 4. Phase 1 — 계층형 오케스트레이션: "5-Stage Pipeline"

단순히 이미지를 생성하는 것을 넘어, 전문 디자이너의 작업 공정을 5단계의 **선형 엔지니어링 파이프라인**으로 구조화했습니다.

1.  **Segmenter (Vision)**: 제품의 배경을 제거하고, 캔버스의 중심 좌표와 기하학적 비율(BBox)을 분석합니다.
2.  **FluxEngine (Generative)**: 분석된 좌표를 바탕으로 AI 배경을 생성하고, 제품을 이질감 없이 녹여냅니다.
3.  **Upscaler (Enhancement)**: 저사양 GPU에서의 효율을 위해 NF4로 생성된 이미지를 4K 수준으로 초고해상도 복원합니다.
4.  **ColorMaster (Post-process)**: 생성된 이미지와 원본 제품의 명도, 대비, 채도를 통계적으로 분석하여 색감을 동기화합니다.
5.  **AdDesigner (Synthesis)**: 텍스트 레이어와 도형 요소를 렌더링하여 최종 상업 광고 결과물을 산출합니다.

> **[Result]**: 이 모든 과정을 단 **1회의 API 요청**으로 완전 자동화하여, 수동 작업 대비 광고 제작 효율을 극대화했습니다.

---

## 5. Phase 2 — 지능적 기획 에이전트: "Creative Director"

"어떤 배경을 만들고, 텍스트는 어디에 배치할 것인가?"라는 창의적 의사결정을 위해 **2단계 LLM 에이전트 시스템**을 구축했습니다.

*   **Planner LLM**: 브랜딩 전략과 전체 톤앤매너, 색상 조합을 결정합니다.
*   **Designer LLM**: Planner의 전략을 실제 픽셀 좌표와 프롬프트로 변환하는 '설계 도면'을 생성합니다.
*   **Failover 전략**: **OpenAI(Primary)와 Gemini(Secondary)**를 이중화하여, 특정 API의 장애 시에도 중단 없는 서비스를 보장하는 안정성을 확보했습니다.

---

## 6. Phase 3 — 리소스 최적화: "VRAMHandler & NF4"

24GB라는 한정된 VRAM(NVIDIA L4)에서 거대 모델들을 상주시킬 때 발생하는 OOM(메모리 부족)과 로딩 지연 문제를 해결하기 위해 **기술적 최적화**를 단행했습니다.

### 6.1 [Challenge] 300초의 벽 — 거대 모델의 로딩 지연
*   **[Problem]**: 매 요청마다 FLUX 모델과 업스케일러를 로드/언로드할 경우, 한 장 생성에 5분(300초) 이상 소요되어 실 서비스가 불가능했습니다.
*   **[Action] VRAMHandler (Singleton)**: GPU 메모리 점거 상태를 전역적으로 추적하는 싱글톤 객체를 구현했습니다. 처음에는 'Strict Swap(상호 배타적 교대)' 방식을 썼으나, 분석 결과 NF4 양자화를 통해 두 모델을 동시에 올릴 수 있음을 발견하고 **'24GB Residency'** 전략으로 선회했습니다.
*   **[Result]**: 후속 요청 시 모델 로딩 시간을 0초로 단축하여, 전체 생성 속도를 **77% 개선(300초 → 70초)**하는 데 성공했습니다.

### 6.2 [Challenge] 메모리 파편화 및 누수 차단
*   **[Problem]**: 반복적인 이미지 생성 시 GPU 캐시가 쌓여 서버가 멈추는 현상이 발생했습니다.
*   **[Action] Deep Clean & Staging**: `gc.collect()`와 `torch.cuda.empty_cache()`를 포함한 3단계 메모리 회수 로직을 파이프라인 단계별로 배치하고, 모델 가중치 로딩 시 **Synchronized Lock**을 적용하여 Race Condition에 따른 메모리 붕괴를 원천 차단했습니다.

---

## 7. Phase 4 — 공학적 가드레일: "Physical Safety Guard"

LLM이 생성하는 레이아웃 좌표는 가끔 비정상적인 값(Hallucination)을 내놓습니다. 이를 방지하기 위해 **휴리스틱 물리 가드레일**을 코어 로직에 삽입했습니다.

*   **Safe Margin (5%)**: 제품 이미지가 캔버스 경계를 넘어 잘리지 않도록 상하좌우 안전 영역을 확보합니다.
*   **Scale Guard (80%)**: 제품이 배경보다 너무 크게 배치되어 기괴해 보이지 않도록 최대 스케일 상한선을 공학적으로 제한합니다.
*   **Layout Safety Cap**: LLM이 제안한 좌표를 이미지 해상도 범위(0.0~1.0) 안으로 강제 클램핑(Clamping)하여 렌더링 오류를 0%로 만들었습니다.

---

## 8. 맺음말 및 프로젝트 회고: "시스템이 신뢰를 만든다"

AdGen 프로젝트의 AI 코어 시스템을 구축하며 얻은 가장 큰 교훈은 **"단일 모델의 천재성보다, 시스템의 견고함이 사용자의 신뢰를 만든다"**는 점입니다.

FLUX와 같은 거대 모델을 24GB VRAM이라는 제약 속에서 어떻게 상주시킬지, LLM의 창의적 환각을 어떻게 수학적인 가드레일로 통제할지를 고민하는 과정은 저에게 **'모델을 넘어선 시스템 엔지니어링'**의 쾌감을 일깨워 주었습니다.

**"화려한 결과물 이면에는, 보이지 않는 VRAM 점유율과 1픽셀의 좌표를 지키기 위한 엔지니어의 치열한 최적화 근거가 숨어있다."**

저는 이 프로젝트를 통해 단순히 기술을 구현하는 것을 넘어, 기술의 비용과 안정성 사이에서 최선의 아키텍처를 선택할 수 있는 **AI Core System Engineer**로서의 정체성을 확립했습니다.

---

## 9. 프로젝트 구조

```text
📦 AdGen-AI-Ad-Generation
├── 📂 app/api_server            # FastAPI 기반 통합 API 레이어
├── 📂 app/frontend              # Next.js 14 & Konva.js 캔버스 편집기
├── 📂 core/orchestrator         # Director Pattern 기반 5-Stage 파이프라인 제어
├── 📂 core/generators           # CreativeDirector (2단계 LLM 기획 엔진)
├── 📂 core/resource_manager     # VRAMHandler (Singleton GPU 최적화)
├── 📂 core/engines/vision       # BiRefNet 세그멘테이션 및 기하학 분석
├── 📂 core/engines/generation   # FLUX.1-Fill 배경 생성 및 렌더링 엔진
└── 📄 README.md
```

---

## 10. Lessons Learned

### 기술적 교훈
1.  **"병목은 로직이 아니라 로딩에 있다"** — 생성 자체보다 모델 로딩에 더 많은 시간이 걸린다는 점을 규명하고, `vram_handler`를 통한 상주(Residency) 전략으로 속도를 77% 개선한 것이 가장 큰 성과였습니다.
2.  **"양자화(Quantization)는 필수 선택지다"** — NF4 양자화를 통해 고품질을 유지하면서도 VRAM 효율을 4배 이상 확보하며, 한정된 자원에서의 서비스 가능성을 확인했습니다.

### 리더십 및 설계 인사이트
3.  **"가이드라인은 수학적이어야 한다"** — LLM에게 "예쁘게 배치해줘"라고 하는 대신, `Scale Guard`와 `Safe Margin` 같은 물리적 제약 조건을 코드로 직접 구현했을 때 비로소 서비스의 일관된 품질이 보장됨을 배웠습니다.
4.  **"Failover 없는 서빙은 도박이다"** — 주력 모델의 API 장애 시 즉각적으로 Gemini로 폴백되는 이중화 구조를 통해, AI 서비스의 비즈니스 연속성을 확보하는 경험을 했습니다.

---

## 관련 링크

- [AdGen 프로젝트 리포지토리](https://github.com/vasyapupkinski/codeitteam5_SMB_GenMarketingContents)