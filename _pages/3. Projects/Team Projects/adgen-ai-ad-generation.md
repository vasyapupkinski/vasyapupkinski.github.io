---
layout: page
title: "AdGen AI: 오케스트레이션을 통한 고가용성 생성 파이프라인 구축 사례 연구"
category: "3. Projects / Team Projects"
order: 3
---

# AdGen AI: 오케스트레이션을 통한 고가용성 생성 파이프라인 구축 사례 연구

> **"AI의 핵심은 모델 자체보다, 이를 유기적으로 연결하여 실전 가치를 만드는 시스템(Harness)에 있다"**

![생성 결과물 전시장](/assets/images/projects/adgen/showcase.png)
*FLUX.1-Fill과 지능형 레이아웃 엔진이 결합되어 생성된 다양한 카테고리의 실제 광고물 예시*

---

## TL;DR

| 항목 | 내용 |
|---|---|
| **프로젝트** | 제품 사진 1장으로 기획부터 최종 광고 이미지까지 자동 생성하는 End-to-End AI 서비스 |
| **나의 역할** | **팀장 — AI Core System 아키텍처 설계 및 오케스트레이션 총괄** |
| **핵심 성과** | 5단계 이미지 파이프라인 자동화, VRAM 최적화로 생성 속도 **77% 단축**, 77-Token 컨덴서 및 지능형 디자인 컴포저 구축 |
| **최종 스택** | FLUX.1-Fill (NF4), BiRefNet, Real-ESRGAN, T5-Tokenizer, FastAPI, Redis, Celery |

---

### 모델의 한계를 넘는 '공학적 흐름(Flow)'과 '자원 효율성'의 탐구
최근 생성형 AI(Generative AI)의 발전으로 고품질 이미지를 만드는 것은 쉬워졌으나, 이를 **'상용 가용성'**을 갖춘 서비스로 만드는 것은 별개의 문제였습니다. 특히 24GB VRAM이라는 한정된 GPU 자원 내에서 거대 모델들을 지연 없이 서빙해야 하는 환경은 설계 초기부터 **'구조적 오케스트레이션'**과 **'메모리 효율화'**를 동시에 고민하게 만들었습니다.

저는 단순히 프롬프트 한 줄을 고치는 것을 넘어, **"프론트엔드나 상위 레이어의 어떤 요청이든 어떻게든 모델이 돌아가게 만들고(Availability), 그 처리 시간을 극한으로 단축하며(Latency), 설계 초기부터 메모리 문제를 철저히 투명하게 관리한다(Resource)"**는 AI 코어 엔지니어로서의 핵심 마인드셋을 바탕으로 프로젝트에 임했습니다. 메모리 기반의 자원 핸들링이 선행되지 않은 AI 서비스는 사상누각이라는 전제 하에 멀티에이전트 하네스(Harness)를 구현해 보려는 공학적 시도를 단행했습니다. 모델의 불확실성을 가드레일로 통제하고, 자원의 한계치 안에서 최적의 성능을 끌어내는 '안정적인 시스템'을 구축하는 과정에서의 깨달음을 공유합니다.

---

## 2. 개발 및 분석 스택

| Category | Technology | Usage |
|----------|-----------|-------|
| **AI Engine** | FLUX.1-Fill (NF4) | Inpainting 기반 지능적 배경 생성 및 제품 합성 |
| **Vision AI** | BiRefNet / ISNet | 객체 세그멘테이션 (누끼) 및 기하학적 분석 |
| **Optimization** | VRAMHandler (Singleton) | GPU 메모리(24GB VRAM) 점유 최적화 및 로딩 속도 가속 |
| **Orchestration** | Director-Manager Pattern | 5단계 파이프라인 생명주기 제어 및 상태 관리 |
| **Infrastructure** | Redis / Celery | 고부하 이미지 생성 작업의 비동기 분산 처리 |
| **Serving** | FastAPI / Next.js | 비동기 API 서빙 및 캔버스 기반 인터랙티브 편집기 |

---

## 3. 팀 구성

| 이름 | 역할 |
|:---|:---|
| **이승완 (팀장)** | **AI Core System 아키텍처 설계 및 구현**. 5-Stage 제작 파이프라인 개발, VRAMHandler 기반 GPU 최적화 전략 수립, 계층형 오케스트레이션(Director) 구축, 물리 기반 레이아웃 가드레일 설계 |
| 박시찬 | **프이프라인 통합 및 서비스 전반 구현**. Next.js 기반 UI/UX 구축, Konva.js 캔버스 편집기 및 실시간 피드백 루프 개발, 프론트-백엔드 통합 연동 |
| 신아름 | 프로젝트 시각화 자료 및 발표 자료(PPT) 제작 지원 |

---

## 3. System Architecture & Pipeline: "The Orchestrated Engine"

프로젝트의 핵심은 단순히 이미지를 생성하는 것이 아니라, 기획부터 렌더링까지 이어지는 **'상용 수준의 제작 공정'**을 구축하는 것이었습니다.

### Architecture Overview

![전체 시스템 아키텍처](/assets/images/projects/adgen/architecture.png)
*Next.js 프론트엔드부터 비동기 Celery 워커, Singleton 기반 VRAMHandler까지 이어지는 전체 시스템 구조*

### 5-Stage Image Synthesis Pipeline
개별 이미지 생성은 다음의 5단계를 거쳐 정밀하게 통제됩니다.

![실제 사용 시나리오 및 파이프라인 흐름](/assets/images/projects/adgen/pipeline_flow.png)
*사용자의 정성적 요청(프롬프트)이 시스템 가드레일을 거쳐 최종 엔진 체인까지 전달되는 흐름*

1.  **Segmenter (BiRefNet)**: 제품 객체 추출 및 기하학적 분석.
2.  **FluxEngine (Generative)**: NF4 양자화된 FLUX.1을 통한 배경 생성 및 제품 합성.
3.  **Upscale (Real-ESRGAN)**: 저해상도(NF4) 생성물을 4K 수준으로 품질 보정.
4.  **ColorMaster**: 배경과 제품 간의 통계적 색감 동기화 및 톤 보정.
5.  **AdDesigner**: 기획된 좌표계 위에서 텍스트 및 디자인 레이어 벡터 렌더링.

### Director-Centric Orchestration
전체 워크플로우는 `Director & State Manager`에 의해 제어됩니다.

![호출 구조 및 2단계 프롬프트 전략](/assets/images/projects/adgen/call_strategy.png)
*Planner(전역 기획)와 Designer(상세 설계)로 역할을 분리하여 환각을 억제하는 에이전트 오케스트레이션*

*   **기획과 생성의 물리적 분리**: 생성 이전에 '디자인 명세서(Manifest)'를 선발행하여, 전체 프로세스를 다시 돌리지 않고 **특정 페이지나 레이어만 재생성**할 수 있는 실무형 구조를 구현했습니다.
*   **단계별 계측(Observability)**: 모든 공정에 `PerformanceTimer`를 도입하여, 병목 구간을 정량적으로 계측하고 최적화 우선순위를 잡았습니다.

---

## 4. My Role & Technical Highlights: "AI Core Lead"

저는 프로젝트의 **AI 코어 설계 및 구현**을 전담하며, 제한된 자원 내에서 고품질 서비스를 안정적으로 서빙하기 위한 핵심 엔지니어링 과제들을 해결했습니다.

### [Action 1] 24GB VRAM 자원 임계점 돌파 (Evolution & Residency)
*   **[Situation / Problem] 자원의 한계**: 24GB라는 한정된 자원에서 거대 모델들을 상주시킬 때 발생하는 OOM(Out of Memory)과 모델 전환 시의 극심한 지연을 극복해야 했습니다.
*   **[Action] 전략적 피벗(Strategic Pivot)**: 4-bit/NF4 양자화 및 CPU Offloading 실험을 거쳐 물리적 자원의 한계를 규명하고, 기획 LLM은 비점유형 API로 전환, 확보된 VRAM을 이미지 엔진의 **'상주(Residency)'**에 100% 집중시켰습니다.
*   **[Result]**: 후속 요청 로딩 시간 0초 달성 및 생성 속도 **77% 개선(300s → 70s)**.

### [Action 2] AI의 시각 인지 한계 극복 (Semantic-to-Geometric Mapping)
*   **[Situation / Problem] 공간 지각 부재**: LLM이 정밀한 픽셀 좌표를 산출하지 못하는 'Spatial Awareness' 부재로 인해 텍스트와 객체의 구도가 깨지는 문제가 발생했습니다.
*   **[Action] 하네스(Harness) 공법 적용**: AI에게는 "우하단 강조" 등 **정성적 의도(Semantic)**만 결정하게 하고, 실제 **섬세한 좌표 조정(Geometric)**은 시스템의 디자인 블루프린트가 담당하도록 가드레일을 구축했습니다.

### [Action 3] 브랜드 일관성 및 환각 억제 (Agentic Orchestration)
*   **[Situation / Problem] 맥락 소실**: 상세페이지 전체의 브랜드 감성(Tone & Manner)이 세션 간에 흔들리거나 환각(Hallucination)이 발생하는 리스크가 존재했습니다.
*   **[Action] 계층형 오케스트레이션 설계**: `ManagerAgent`가 프로젝트 전체 컨셉을 기억(Recall)하고, `CreativeDirector`가 각 이미지별 상세 지시서를 작성하여 정보 소실을 차단했습니다.

### [Action 4] 데이터 무결성 및 인프라 (Storage Auditing)
*   **[Situation / Problem] 스토리지 낭비**: 생성형 AI 서비스 특성상 대량 이미지 생성 시 발생하는 스토리지 부하와 중복 데이터 문제가 대두되었습니다.
*   **[Action] 지능형 지형 관리**: **SHA-256 Deduplication**을 통한 픽셀 데이터 해시 비교 시스템과 에피소드 메모리 기능을 구축했습니다.

---
## 5. Technical Deep-Dive — 지표 이면의 공학적 디테일

### 5.1 Performance Observability (성능 계측 및 최적화 우선순위)
*   **[Situation / Problem] 정량적 지표 부재**: 단순히 시스템이 "느리다"는 정성적 판단만으로는 정확한 최적화 포인트를 찾기 어려웠습니다.
*   **[Action]**: 모든 파이프라인 단계에 `PerformanceTimer`를 삽입하여 실행 시간을 로깅하고, 5단계 생성 공정의 병목 구간을 시각화했습니다.
*   **[Result]**: 모델 로딩이 전체의 60%를 차지한다는 계측 결과를 얻었고, 이를 통해 `VRAM Residency` 전략이라는 근본적인 해결책을 도출할 수 있었습니다.

### 5.2 77-Token Prompt Condenser & T5 Tokenizer
*   **[Situation / Problem] 컨텍스트 소실**: FLUX.1 (T5) 인코더의 **77토큰 제한**을 초과할 경우 뒷부분의 결정적인 묘사들이 무시되어 결과물 품질이 급감하는 문제가 있었습니다.
*   **[Action]**: 매니저 에이전트 내부에 `T5Tokenizer`를 로컬 로드하여 실시간 토큰 수를 계산하고, 초과 시 LLM이 핵심 키워드 위주로 프롬프트를 **자율 압축(Condensation)**하도록 설계했습니다.
*   **[Result]**: 긴 사용자 요구사항도 77토큰 이내의 고농축 프롬프트로 변환되어 생성 결과물의 정합성을 획득했습니다.

### 5.3 Layout-Aware Vector Composition Engine
*   **[Situation / Problem] 레이아웃 붕괴**: AI가 생성한 배경 위에 시각적 요소들이 겹치거나 부자연스러운 구도로 배치되는 현상이 발견되었습니다.
*   **[Action]**: `ShapeComposer`를 통해 전용 **디자인 블루프린트**를 구축하고, 강조 요소가 제품의 좌표(`px`, `py`)를 수학적으로 추적하는 **지능적 앵커링** 로직을 적용했습니다.

### 5.4 Context & Semantic Isolation (맥락 및 의미론적 격리)
*   **[Situation / Problem] 인스트럭션 혼선**: 광고 카피에 배경 묘사가 섞이는 등 정보 간 간섭으로 인한 환각 현상이 발생했습니다.
*   **[Action] 에피소드 메모리 격리**: 사용자의 과거 데이터와 현재 세션의 인스트럭션을 독립적으로 관리하는 격리된 추론 환경을 제공했습니다.

### 5.5 Agentic Control & Harness Engineering (프롬프트의 수치 제어)

![Planner 및 Designer 프롬프트 전략](/assets/images/projects/adgen/prompt_strategy.png)
*Planner와 Designer 에이전트의 실제 프롬프트 구조와 Aesthetic/Physical 지능 제어 핵심 원문*

단순한 텍스트 생성을 넘어, LLM 프롬프트가 시스템의 정량적 수치를 직접 제어하는 **'Agentic Control'** 구조를 구축했습니다.

*   **[Action: Aesthetic Intelligence]**: LLM이 분석한 감성에 맞춰 색보정 수치를 소수점 단위로 결정하고, 이를 `ColorMaster` 파라미터로 실시간 매핑했습니다.
*   **[Action: Geometric translation]**: 정성적 의도를 실시간 픽셀 좌표로 변환하되, 제품이 캔버스 밖으로 이탈하지 않도록 **Safe Margin** 및 **Physical Clamping** 가드레일을 적용했습니다.
*   **[Result: Harness Engineering]**: 비정형 응답이나 오타를 정규표현식으로 정제하는 **JSON Sanitizer** 계층을 구축하여 시스템의 안정성을 확보했습니다.

---

## 6. Project Demo: "The Generation in Action"

상용 수준의 생성 속도와 레이어 편집 기능을 시연하는 실제 구동 영상입니다.

<div style="display: flex; flex-direction: column; gap: 20px;">
    <video src="/assets/images/projects/adgen/New151-web.mp4" controls width="100%" style="border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.3);"></video>
    <video src="/assets/images/projects/adgen/New121-web.mp4" controls width="100%" style="border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.3);"></video>
    <video src="/assets/images/projects/adgen/New131-web.mp4" controls width="100%" style="border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.3);"></video>
</div>

---

## 7. 맺음말 및 프로젝트 회고: "시스템적 사고가 신뢰를 만든다"

AdGen 프로젝트의 AI 코어 시스템을 구축하며 얻은 가장 큰 교훈은 **"단일 모델의 천재성보다, 이를 엮는 시스템의 견고함이 사용자의 신뢰를 만든다"**는 점입니다.

완벽한 멀티에이전트 시스템을 구현하려던 시도는 때로 자원의 벽에 부딪히기도 했으나, 그 과정에서 FLUX와 같은 거대 모델을 24GB VRAM 안에서 어떻게 상주시킬지, LLM의 창의적 환각을 어떻게 수학적인 가드레일로 통제할지 고민하며 **'시스템 하네스 엔지니어링'**의 본질을 경험할 수 있었습니다.

**"화려한 이미지 이면에는, 보이지 않는 VRAM 점유율과 1픽셀의 좌표를 지키기 위한 엔지니어의 치열한 최적화 근거가 숨어있다."**

저는 이 프로젝트를 통해 단순히 기술을 구현하는 것을 넘어, 기술의 비용과 안정성 사이에서 최선의 아키텍처를 선택할 수 있는 **AI Core System Engineer**로서의 가능성을 확인했습니다.

---

## 8. 프로젝트 구조

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

## 9. Lessons Learned

### 기술적 교훈
1.  **"병목은 로직이 아니라 로딩에 있다"** — 생성 자체보다 모델 로딩에 더 많은 시간이 걸린다는 점을 규명하고, `vram_handler`를 통한 상주(Residency) 전략으로 속도를 77% 개선한 것이 가장 큰 성과였습니다.
2.  **"양자화(Quantization) 모델의 실무적 적용"** — NF4 양자화 모델 적용(Application)을 통해 고품질을 유지하면서도 VRAM 효율을 4배 이상 확보하며, 한정된 자원에서의 서비스 확장성을 확인했습니다.
3.  **"수학적 계산만으로는 '미학적 감각'을 대체할 수 없다"** — 캔버스 좌표와 텍스트 박스 크기를 수학적으로 치밀하게 계산하여 최적의 위치를 정하도록 설계했으나, 실제 인간의 눈에 비치는 '섬세한 배치'에는 도달하지 못하는 태생적 한계를 확인했습니다. 이는 에이전트가 시각 데이터(Vision)를 직접 인지하지 못하고 수치에만 의존하기 때문이며, 시간과 자원이 더 허락되었다면 비전 모델을 결합하여 '눈'을 달아주었어야 했다는 엔지니어링적 아쉬움과 깨달음을 남겼습니다.

### 리더십 및 설계 인사이트
4.  **"가이드라인은 수학적이어야 한다"** — LLM에게 "예쁘게 배치해줘"라고 하는 대신, `Scale Guard`와 `Safe Margin` 같은 물리적 제약 조건을 코드로 직접 구현했을 때 비로소 서비스의 일관된 품질이 보장됨을 배웠습니다.
5.  **"엔지니어링은 한계를 인정하는 것에서 시작된다"** — 로컬화를 위한 극한의 최적화 과정은 실패가 아닌, **물리적 자원의 임계점**을 명확히 규명하는 검증 과정이었습니다. "최적화로 알뜰하게 쓸 수 있을지언정, 물리적 총량을 이길 순 없다"는 교훈은 저에게 유연하고 현실적인 아키텍처를 설계하는 안목을 주었습니다.
6.  **"완성보다 중요한 것은 끝까지 해내는 '팀의 호흡'이다"** — 프로젝트 중반, 팀원의 절반이 이탈하는 절망적인 상황을 겪었습니다. 남은 2명이서 감당하기엔 불가능해 보여 포기하고 싶었으나, 남은 팀원과 '미완성이라도 끝까지 최선을 다해보자'는 진솔한 대화를 통해 동력을 회복했습니다. 결국 마감일까지 기대 이상의 결과물과 이미지 퀄리티를 도출해 냈으며, 이를 통해 극한의 상황에서도 서로를 믿는 팀워크가 기술적 능력보다 더 큰 성과를 만든다는 소중한 교훈을 얻었습니다. (끝까지 함께해 준 팀원에게 다시 한번 감사를 전합니다.)
7.  **"AI 코어 엔지니어의 숙명: 가용성과 효율성의 동시 확보"** — 타 파트의 어떠한 요구사항도 기술적 불가능으로 남기지 않고 어떻게든 '작동'하게 만드는 가용성(Availability) 확보 마인드와, 이를 뒷받침하기 위해 초기 설계부터 메모리 점유 및 생성 시간을 극한까지 관리하는 최적화 능력이 AI 시스템 구축의 본질임을 체득했습니다.

---

## 10. Future Roadmap: 시각적 피드백 루프의 완성

현재 시스템은 텍스트 파라미터 기반의 레이아웃 산출 방식으로 인해, 인간의 감각에 준하는 '섬세한 미세 조정'에는 물리적 한계가 존재했습니다. **"에이전트에게 눈(Vision)이 없어 수학적 수치에만 의존한다"는 태생적 제약**을 정면으로 돌파하기 위해 다음과 같은 차기 아키텍처를 구상하고 있습니다.

*   **LMM (Vision) 기반의 Visual Audit**: 생성된 광고 결과물을 멀티모달 모델(LMM)이 직접 분석하고 수정을 제안하는 **'시각적 피드백 루프'** 구축.
*   **Adaptive Layout Discovery**: 정해진 블루프린트를 넘어, 비전 에이전트가 배경 이미지의 여백과 구도를 실시간 인지하여 제품 배치를 동적으로 최적화하는 지능형 배치 엔진 고도화.
*   **Goal**: 모델에게 '눈'을 달아줌으로써, 공학적 가드레일과 미학적 감각이 공존하는 궁극의 디자인 자동화 시스템 완성을 지향합니다.

---

## 관련 링크

- [AdGen 프로젝트 리포지토리](https://github.com/vasyapupkinski/codeitteam5_SMB_GenMarketingContents)