# Marbles: Interactive Live Electronics System for Acoustic Bass Drum

> **Author:** 김동휘 (B.M. in Classical Composition & Computer Music)  
> **Release Date:** 2025/12  
> **Structure:** 4 Major Parts (A-B-C-D), Total 121 Measures  
> **Environment:** Max/MSP 8.x, Tape & Live Electronics Hybrid System  

---

## 1. Abstract & System Overview
본 프로젝트 **'Marbles'**는 베이스 드럼(Bass Drum)과 라이브 일렉트로닉스(Live Electronics)를 위한 총 121마디 규모의 작품입니다. 구슬이 흐르는 시각적·기하학적 움직임에서 유추한 비선형적 에너지와 예측 불가능한 흐름을 음악적 매개변수(Parameter)로 변환하는 것을 목표로 합니다.

본 작품은 크게 **A - B - C - D의 4개 부분**으로 구성되어 있으며, 전반적으로 고정 미디어(Tape)와 실시간 음향 처리(Live Electronics)가 결합된 하이브리드 편성입니다. 특히 **주요 발전부인 [Part C]에서는 Max/MSP를 통한 실시간 인터랙티브 제어 시스템이 집중적으로 활성화**되며, 연주자가 사용하는 다양한 말렛(Mallet)과 도구(탬버린, 비즈 등)의 익스텐디드 테크닉(Extended Techniques)에 따라 이펙터 아키텍처가 동적으로 전환됩니다.

---

## 2. System Architecture & Modular Design

시스템은 유지보수와 확장이 용이하도록 메인 실행 패치(`Marbles_Main.maxpat`)와 `patchers/` 폴더 내의 독립된 하위 추상화 패치(Abstraction Patches)로 완전히 모듈화(Modular Architecture)되어 있습니다.

### 📁 Repository Structure
```
├── README.md                  <- 본 기술 보고서 및 시스템 명세서
├── Document/
│   └── Marbles_Score.pdf      <- 연주 노테이션 및 배치표가 포함된 총보 (PDF)
├── MaxMSP/
│   ├── Marbles_Main.maxpat    <- 전체 시스템을 총괄하는 메인 실행 패치
│   └── patchers/              <- 독립된 서브 모듈로 분리된 하위 패치 폴더 (.maxpat)
│       ├── mic.maxpat         <- Input Audio Signal Preprocessing & Envelope Follower
│       ├── counting_events.maxpat <- State Machine for Part C's sub-sections (A, B, C, D)
│       ├── teeth_filter.maxpat    <- Comb-Filtering Matrix Engine (Teeth~)
│       ├── vocoder_filter.maxpat  <- Formant & Pulse Synthesis Module
│       ├── mix.maxpat         <- Dry/Wet Signal Balance & Multi-channel Spatialization
│       └── midi.maxpat        <- Hardware Controller Mapping Interface
└── Audio/
    └── Samples/               <- 클릭 및 루프 재생 루틴을 위한 오디오 에셋 (.wav)
```

---

## 3. Interaction & Score Mapping Design

발표 자료 및 악보 명세를 바탕으로 정립한 하이브리드 전자음향 매핑 구조입니다. Max/MSP 실시간 제어가 극대화되는 **[Part C]는 이펙터 결합 특성에 따라 내부적으로 다시 4개의 서브 섹션(Section A, B, C, D)으로 정교하게 분기**됩니다.

| Macro Structure | Part C Sub-Section | Performance & Acoustic Gestures (Mallets & Tools) | Active DSP Module | Technical Parameter Mapping Strategy |
| :--- | :--- | :--- | :--- | :--- |
| **Part A** | - | Hand & Finger Tapping, Circle Scratching on Head | **Tape Base** | 고정 미디어(Tape) 레이어와 연주자의 기초 타격음 동기화 |
| **Part B** | - | Initial Extended Textures | **Tape Base** | 음향적 텍스처 전개 및 파트 C 진입을 위한 상태 준비 |
| **Part C** | **Section A** | Put a tambourine on the drum head, Timpani Mallets | **Vocoder + Delay** | 탬버린 징글(Jingles)의 고주파수 성분을 보코더 필터와 결합하여 금속성 타격음 증폭 |
| | **Section B** | Snare Drum Mallets, Complex rhythms (Tremolo & Fast tuplets) | **Teeth Filter (Comb)** | `teeth~` 오브젝트를 통해 빠른 타격음의 미세 주파수를 빗살 모양 공진 주파수로 통과시켜 전개 |
| | **Section C** | Complex tuplets, Fast notes with **Free Rhythm (Ad lib.)** | **Teeth + Delay** | 연주자에게 리듬적 자유도를 부여함과 동시에, 시간 축에 따른 딜레이 피드백 제어로 음향 에너지 고조 |
| | **Section D** | Fast 7-tuplets, Tremolo, Free Rhythm (**Summation of elements**) | **Vocoder Only** | 앞서 사용된 모든 테크닉과 음악적 세포들이 총집합하는 클라이맥스로, 포먼트 신세시스 극대화 |
| **Part D** | - | Maximum Dynamic Mallet Striking & Decrescendo | **Spatialization Decay** | 잔향 및 공간 확산(Spatialization)의 극대화를 통한 음향적 잔상 구현 및 Fade-out |

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
