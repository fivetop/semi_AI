# FPGA Vivado 프로젝트 템플릿 생성 자동화

## 개요
oh-my-opencode를 사용하여 Xilinx Vivado FPGA 프로젝트의 자동 생성 프롬프트.

## 프로젝트 구조 분석 완료

### 현재 템플릿 구성
```
prj_ojs/
├── src/
│   ├── led_blink.v          # Verilog RTL 모듈
│   └── constraints/
│       └── kv260_pin.xdc    # Pin 할당 제약조건
└── scripts/
    ├── 00_run_all.tcl       # 전체 빌드 실행
    ├── 01_create_project.tcl  # 프로젝트 생성 (xck26-sfvc784-2LV-c)
    ├── 02_create_bd.tcl    # Block Design (Zynq PS + RTL)
    ├── 03_run_synthesis.tcl # 합성 (synth_1, 8 jobs)
    └── 04_run_implementation.tcl # 구현 + 비트스트림 (4 jobs)
```

### 빌드 파이프라인
1. **프로젝트 생성** → Vivado 프로젝트 파일 (.xpr) 생성
2. **Block Design** → Zynq UltraScale+ PS + Proc System Reset + 사용자 RTL 연결
3. **합성** → RTL → 넷리스트 변환
4. **구현** → 넷리스트 → 배치배선 → 비트스트림 (.bit)

---

## 자동화 프롬프트 템플릿

oh-my-opencode에서 이 템플릿을 사용하여 사용자에게 다음과 같은 자동화를 제공:

```markdown
## 🎯 FPGA 프로젝트 템플릿 생성기

**대상 보드**: KV260 (Xilinx Kria Starter Kit, xck26-sfvc784-2LV-c)

### 생성 옵션:

**1. 모듈 선택:**
- [ ] LED Blink (기본)
- [ ] PWM 제어
- [ ] UART收发信
- [ ] 커스텀 Verilog 모듈

**2. 빌드 단계:**
- [ ] 프로젝트 생성만
- [ ] 합성까지
- [ ] 전체 (구현 + 비트스트림)

**3. 추가 옵션:**
- [ ] 시뮬레이션 포함
- [ ] IP Integrator 블록 디자인
- [ ] 커스텀 제약조건
```

### 생성될 파일 구조:
```
${PROJECT_NAME}/
├── src/
│   ├── ${MODULE_NAME}.v
│   └── constraints/
│       └── ${BOARD}_pin.xdc
└── scripts/
    ├── 00_run_all.tcl
    ├── 01_create_project.tcl
    ├── 02_create_bd.tcl
    ├── 03_run_synthesis.tcl
    └── 04_run_implementation.tcl
```

---

## oh-my-opencode 통합 시나리오

### 스킬 설계:
- **스킬 이름**: `fpga-vivado-template`
- **트리거**: "FPGA 프로젝트 생성", "Vivado 템플릿", "KV260 프로젝트"

### 기능:
1. 사용자에게 보드 선택 요청 (KV260, ZedBoard, Pynq 등)
2. 모듈 유형 선택 (LED, PWM, UART, 커스텀)
3. 파라미터 입력 (클럭 주파수, LED 개수 등)
4. 템플릿 생성 및 Tcl 스크립트 실행

---

이 프롬프트를 oh-my-opencode 스킬로 구현할까요, 아니면 별도의 자동화 스크립트로 만들까요?