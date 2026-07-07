# Marbles: Interactive Live Electronics System for Acoustic Bass Drum

> **Author:** 김동휘 (B.M. in Classical Composition & Computer Music)  
> **Release Date:** 2025/12  
> **Environment:** Max/MSP 8.x, Stereo to 5-Channel Spatialization System  

---

## 1. Abstract & System Overview
본 프로젝트 **'Marbles'**는 베이스 드럼의 어쿠스틱 오디오 신호를 실시간으로 캡처하고, 이를 디지털 오디오 신호 처리(DSP) 모듈과 매핑하여 전자 음향으로 확장하는 **실시간 음향 상호작용 시스템(Interactive Live Electronics System)**입니다. 

구슬이 흐르는 기하학적 움직임과 비선형적 에너지를 음악적 매개변수(Parameter)로 변환하는 것을 목표로 하며, 연주자의 물리적 익스텐디드 테크닉(Extended Techniques)에 따라 이펙터 아키텍처가 동적으로 제어되도록 설계되었습니다. 곡은 총 4개의 섹션(Section A, B, C, D)으로 구성되어 각기 다른 신호 처리 파이프라인을 통과합니다.

---

## 2. System Architecture (Signal & Control Flow)

시스템은 오디오 입력 피처 추출, 이벤트 제어 매트릭스, 그리고 두 가지 주요 DSP 필터 뱅크(Teeth, Vocoder) 및 5채널 공간화 출력 모듈로 구성됩니다.

```
[Acoustic Bass Drum] 
       │ (Mic Input)
       ▼
 ┌───────────┐      ┌──────────────────────────────┐
 │  p mic    │ ───> │     Control Event Matrix     │ (Manual/Procedural Triggers)
 └───────────┘      └──────────────────────────────┘
       │                           │
       ▼ (Dry Signal)              ▼ (Control Data)
 ┌─────────────────────────────────────────────────┐
 │               DSP Filter Banks                  │
 │  - Teeth Filter (Comb-Filtering Matrix)         │
 │  - Vocoder Filter (Formant & Pulse Synthesis)   │
 └─────────────────────────────────────────────────┘
       │
       ▼ (Wet Signal Mix)
 ┌─────────────────────────────────────────────────┐
 │ dac~ 1 2 3 4 5 (Multi-channel Spatialization)   │
 └─────────────────────────────────────────────────┘
```

### 2.1 Audio Input & Event Control
* **`p mic` Module:** 베이스 드럼의 어쿠스틱 신호를 실시간 입력받아 프리앰프 게인 조절 및 인벨로프 추적을 위한 기초 신호 처리를 수행합니다.
* **`p COUNTING_EVENTS` Module:** 연주 상의 타격 횟수 및 비선형적 리듬 궤적을 카운팅하여 오디오 신호 처리 모듈의 프리셋 변환을 트리거합니다.

### 2.2 DSP Modules
* **Teeth Filter (`teeth~` Matrix):** `teeth~ 15 1 0.25 0. 0.25` 오브젝트를 기반으로 한 피드백 컴 필터 유닛입니다. 드럼 표면을 구슬이나 손으로 긁는 회전성 마찰음(Scratching)의 미세한 주파수 성분을 빗살 모양의 공진 주파수로 통과시켜, 금속성 질감과 배음 구조의 변화를 실시간으로 모델링합니다.
* **Vocoder Filter Module:** `noise threshold`, `filter Q`, `pulse width`, `pulse amplitude` 등의 파라미터를 동적으로 제어하는 포먼트 신세시스 블록입니다. 말렛(Mallet) 타격 시 발생하는 과도 응답(Transient Response) 데이터와 노이즈 임계값을 결합하여 파형의 폭과 필터의 Q값을 실시간 매핑 제어합니다.

### 2.3 Spatialization & Output
* **`dac~ 1 2 3 4 5` Matrix:** 최종 오디오 신호는 드라이(Dry) 소스와 4개의 채널별 웨트(Wet) 소스가 유기적으로 믹싱되어 5채널 독립 출력 구조를 가집니다. 이는 무대 위 베이스 드럼의 물리적 위치와 객석을 둘러싼 멀티채널 스피커 환경 사이의 음향적 공간 확산(Sound Projection)을 제어합니다.

---

## 3. Interaction & Score Mapping Design

연주자의 피지컬 제스처(Acoustic Input)와 디지털 시스템(Max/MSP) 간의 대구조(A-B-C-D) 및 Section C 내 세부 소구조(a-b-c-d) 매핑 메커니즘은 다음과 같습니다.

| Major Section | Sub Section | Score Notation (Acoustic Gesture) | Active DSP Module | Parameter Mapping Strategy |
| :--- | :--- | :--- | :--- | :--- |
| **Section A** | - | Tap the drum using hand & finger, Circle scratching | **Tape On / Click On** Base | 초기 타격 인벨로프 추출 및 기본 오디오 루프 동기화 |
| **Section B** | - | Put a tambourine on the bass drum, Medium mallet | **Vocoder + Delay** | 탬버린 징글(Jingles)의 고주파 성분을 보코더 필터 Q값과 연동하여 금속성 타격음 증폭 |
| **Section C** | **c-a** | Snare drum mallet, Complex rhythmic structures | **Teeth Filter (Comb)** | 타격 속도(accel./rit.)에 따른 컴 필터의 지연 시간(Delay Time) 비선형 매핑 |
| | **c-b** | Dynamic Mallet Interlocking | **Teeth + Feedback** | 피드백 게인 조절을 통한 음향적 에너지 및 공진성의 점진적 고조 |
| | **c-c** | Micro-frictional gestures on drum head | **Teeth + Vocoder Crossfade**| 마찰 신호의 주파수 천이에 따른 두 필터 간의 실시간 크로스페이드 |
| | **c-d** | Put a string of beads on the bass drum, Drum stick | **Vocoder Filter (Pulse)** | 비즈(Beads)의 불규칙한 미세 마찰 신호를 노이즈 임계값(`noise threshold`)과 대조하여 고밀도 펄스 트리거 |
| **Section D** | - | Maximum Dynamic Mallet Striking & Fade-out | **Spatialization Decay** | 잔향 및 5채널 공간 확산 매트릭스의 극대화를 통한 음향적 잔상 구현 |

---

## 📁 Repository Structure
```
├── README.md                  <- This technical report
├── Document/
│   └── Marbles_Score.pdf       <- Full score with performance notes
├── MaxMSP/
│   ├── Marbles_Main.maxpat    <- Main performance patch
│   └── patchers/              <- Subpatches (p mic, p mix, etc.)
└── Audio/
    └── Samples/               <- Audio assets for click and tape routines
```
