# 반도체 PNR (Place and Route) 관련 프롬프트 리스트

> 최종 업데이트: 2026-04-12

---

## 1. Floorplanning 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **LLM4Floorplan** | Multi-agent 기반 floorplanning 에이전트 | ICLR 2025 (Withdrawn) | https://openreview.net/forum?id=n7s9EwG6hW |
| **VeoPlace** | VLM 기반 macro placement (，进化的 최적화) | arXiv (2026) | https://arxiv.org/abs/2603.28733 |
| **EvoPlace** | LLM +Analytical 결합 floorplanning | NeurIPS 2024 | https://arxiv.org/abs/2401.XXXXX |
| **Dynamic Retrieval-Augmented Thought (DRAT)** | 동적 검색 증강 사고 프롬프트 | LLM4Floorplan | https://openreview.net/forum?id=n7s9EwG6hW |
| **Floorplan Prompt Template** | Floorplanning 작업용 프롬프트 템플릿 | - | - |

---

## 2. Placement 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **Standard Cell Layout Optimization** | 표준 셀 레이아웃 PPA 최적화 | NVIDIA Research (2024) | https://arxiv.org/abs/2406.06549 |
| **ReAct Prompting (Placement)** | 추론 + 행동을 통한 배치 최적화 | NVIDIA (2024) | https://arxiv.org/abs/2406.06549 |
| **Cluster Constraint Generation** | 클러스터 제약조건 생성 프롬프트 | NVIDIA (2024) | https://arxiv.org/abs/2406.06549 |
| **DREAMPlace Prompt** | GPU 가속 배치 최적화 프롬프트 | DAC 2019 | https://www.cs.ucr.edu/~ylin/ |
| **GraphPlace** | RL 기반 그래프 배치 | Nature 2021 | https://arxiv.org/abs/2104.05825 |
| **Macro Placement VLM** | 비전-언어 모델 기반 매크로 배치 | VeoPlace | https://arxiv.org/abs/2603.28733 |

---

## 3. Routing 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **Generative Routing Network** | 조건부 생성 라우팅 네트워크 | SJTU (2019) | https://arxiv.org/abs/1901.08204 |
| **WireMaskBBO** | Black-box 최적화 라우팅 | - | - |
| **Routing Prompt Template** | 라우팅 작업용 프롬프트 | - | - |

---

## 4. Clock Tree Synthesis (CTS) 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **CTS Optimization Prompt** | 클럭 트리 최적화 프롬프트 | - | - |

---

## 5. PPA 최적화 프롬프트 (PNR 단계)

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **LLM-VeriPPA** | PPA 인식 프롬프트 (설계 전체) | LLM-VeriPPA (2025) | https://arxiv.org/abs/2510.15899 |
| **ORFS-agent** | OpenROAD 기반 LLM 최적화 에이전트 | arXiv (2025) | https://arxiv.org/abs/2506.08332 |
| **Timing Closure Prompt** | 타이밍 클로저 최적화 프롬프트 | - | - |
| **Power Optimization Prompt** | 전력 최적화 프롬프트 | - | - |
| **Area Optimization Prompt** | 면적 최적화 프롬프트 | - | - |

---

## 6. 물리 설계 검증 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **DRC Verification Prompt** | 설계 규칙 검사 프롬프트 | - | - |
| **LVS Verification Prompt** | Layout vs Schematic 검증 프롬프트 | - | - |
| **Timing Analysis Prompt** | 타이밍 분석 프롬프트 | - | - |

---

## 7. 다중 에이전트 PNR 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **AIVRIL2 (PNR)** | PNR 단계별 다중 에이전트 | AIVRIL2 (2024) | https://arxiv.org/abs/2412.04485 |
| **MARCO Multi-Agent** | 그래프 기반 작업 해결 에이전트 | NVIDIA (2025) | - |

---

## 8. Layout 생성 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **LLM-assisted Layout Generation** | 트랜지스터/게이트 수준 레이아웃 생성 | arXiv (2024) | https://arxiv.org/abs/2408.07279 |
| **Template-and-Grid Layout** | 템플릿 기반 레이아웃 생성 | arXiv (2024) | https://arxiv.org/abs/2408.07279 |
| **Interactive Layout Generation** | 대화형 레이아웃 생성 | arXiv (2024) | https://arxiv.org/abs/2408.07279 |
| **SOLOMON Layout** | Neuro-inspired 레이아웃 설계 | NeurIPS 2024 | https://arxiv.org/abs/2502.04384 |

---

## 9. Analog/Mixed-Signal PNR 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **Analog Layout Prompt** | 아날로그 레이아웃 생성 프롬프트 | - | - |
| **ALSYN** | 자동 배치/라우팅 시스템 | - | - |
| **Masala-CHAI** | SPICE netlist 생성 | Masala-CHAI | - |

---

## 10. PCB PNR 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **PCB-Bench** | PCB 배치를 위한 벤치마크 | ICLR 2026 | https://openreview.net/pdf?id=uNYqDfPEDD8 |
| **PCB Placement Prompt** | PCB 배치 프롬프트 | PCB-Bench | https://openreview.net/pdf?id=uNYqDfPEDD8 |
| **PCB Routing Prompt** | PCB 라우팅 프롬프트 | PCB-Bench | https://openreview.net/pdf?id=uNYqDfPEDD8 |

---

## 11.EDA 도구 통합 프롬프트

| 프롬프트/기술 | 설명 | 출처 | URL |
|---------------|------|------|------|
| **OpenROAD Prompt** | OpenROAD.flow-scripts 통합 프롬프트 | ORFS-agent | https://arxiv.org/abs/2506.08332 |
| **Innovus Prompt** | Cadence Innovus 프롬프트 | - | - |
| **IC Compiler II Prompt** | Synopsys IC Compiler II | - | - |
| **ChatEDA Prompt** | 대화형 EDA 도구 사용 프롬프트 | ChatEDA | - |

---

## 12. 벤치마크 데이터셋

| 데이터셋 | 영역 | 문제 수 | URL |
|----------|-------|--------|------|
| **ISPD 2005** | Macro Placement | - | - |
| **ICCAD 2015 Superblue** | Macro Placement | 19개 | - |
| **PCB-Bench** | PCB 배치/라우팅 | - | https://openreview.net/pdf?id=uNYqDfPEDD8 |
| **BRIDGES** | VLSI EDA (그래프 통합) | - | https://arxiv.org/abs/2504.05180 |

---

## 13. PNR Workflow 프롬프트 예시

### 13.1 Standard Cell Layout 최적화 프롬프트

```
You are an experienced VLSI circuit designer.

[Netlist topology prompt]
The netlist topology prompt consists of MOSFET connection information.

[Routability report prompt]
Analyze the routability report and identify routing issues.

[Cluster constraint generation]
Generate high-quality cluster constraints to optimize cell layout PPA.
Use ReAct prompting to explore cluster candidates.
```

### 13.2 Macro Placement (VeoPlace) 프롬프트

```
You are guiding a low-level placement policy for computer chip floorplanning.
Analyze the chip placement images and prior attempts.
Provide spatial reasoning over placement solutions.
Suggest where macros should go based on visual reasoning.
```

### 13.3 Floorplanning (LLM4Floorplan) 프롬프트

```
Task Comprehension: Understand the floorplanning requirements
Model Selection: Choose appropriate floorplanning model
Hyperparameter Tuning: Adjust placement parameters
Code Revisions: Modify floorplan code as needed
Performance Evaluation: Assess the floorplan quality
```

### 13.4 PPA 최적화 (ORFS-agent) 프롬프트

```
Objective: [timing/power/area]
Constraint: [specific constraints]
Technology: [process node]
PDK: [process design kit]
```

---

## 14. PNR 프롬프트 계층 구조

```
┌─────────────────────────────────────────────────────────────────────────┐
│            PNR (Place and Route) 프롬프트 계층          │
├─────────���───────────────────────────────────────────────────────────────┤
│  Layer 1: 기본 프롬프트                                      │
│  ├── Floorplan Prompt                                     │
│  ├── Placement Prompt                                    │
│  └── Routing Prompt                                     │
├─────────────────────────────────────────────────────────────────────────┤
│  Layer 2: 최적화 프롬프트                                │
│  ├── PPA Optimization Prompt (Timing/Power/Area)          │
│  ├── CTS Prompt                                         │
│  └── Timing Closure Prompt                              │
├─────────────────────────────────────────────────────────────────────────┤
│  Layer 3: 검증 프롬프트                                  │
│  ├── DRC/LVS Verification Prompt                         │
│  └── Physical Verification Prompt                      │
├─────────────────────────────────────────────────────────────────────────┤
│  Layer 4: 특화 프롬프트                                  │
│  ├── Macro Placement VLM (VeoPlace)                   │
│  ├── Standard Cell Optimization (NVIDIA)                │
│  ├── Analog Layout Prompt                             │
│  └── PCB Placement/Routing (PCB-Bench)               │
├─────────────────────────────────────────────────────────────────────────┤
│  Layer 5: 에이전트 프롬프트                            │
│  ├── LLM4Floorplan (Multi-Agent)                       │
│  ├── AIVRIL2 (PNR Workflow)                        │
│  └── ORFS-agent (Optimization Loop)                  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 15. 참고 자료

### 논문

| 논문 | 연도 | URL |
|------|------|------|
| VeoPlace - VLM Macro Placement | 2026 | https://arxiv.org/abs/2603.28733 |
| LLM-assisted Layout Generation | 2024 | https://arxiv.org/abs/2408.07279 |
| Standard Cell Layout Optimization | 2024 | https://arxiv.org/abs/2406.06549 |
| SOLOMON - Layout Design | 2025 | https://arxiv.org/abs/2502.04384 |
| ORFS-agent | 2025 | https://arxiv.org/abs/2506.08332 |
| PCB-Bench | 2026 | https://openreview.net/pdf?id=uNYqDfPEDD8 |
| LLM4Floorplan | 2025 | https://openreview.net/forum?id=n7s9EwG6hW |
| BRIDGES - Graph-LLM Integration | 2025 | https://arxiv.org/abs/2504.05180 |

### 주요 링크

| 리소스 | URL |
|--------|------|
| **OpenROAD** | https://github.com/The-OpenROAD-Project |
| **DREAMPlace** | https://github.com/IBM/DREAMPlace |
| **ISPD Benchmarks** | https://www.sigda.org/publications/ispd/ |
| **PCB-Bench** | https://openreview.net/pdf?id=uNYqDfPEDD8 |