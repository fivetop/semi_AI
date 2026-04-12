# 소크라테스 FPGA RTL 설계 프롬프트 템플릿

## [템플릿 개요]

- **목적**: 사용자의 요구사항을 분석하여 KV260 FPGA 보드에서 동작하는 RTL 프로젝트를 자동 생성
- **대상 디바이스**: Xilinx KV260 (XCK26-SFVC784-2LV-c)
- **개발 도구**: Xilinx Vivado (최신 버전)
- **출력**: RTL 코드 + TCL 자동화 스크립트 + 컨스트레인트

---

## [사용자 요구사항 분석]

사용자가 다음과 같이 요구할 것입니다:

```
"{기능 요구사항}"
예: "1초 마다 LED 깍박이는 RTL 을 만들어서 비바도에서 돌릴거야 KV260보드 PMOD 1번에 출력을 만들거고 클럭은 시스템 클럭 100MHZ 사용하고, 컨센트레인트는 알아서 해줘. 임플멘테이션이랑 비트스트림 까지 만들면 시험은 내가 할거야"
```

### 분석 단계

#### 1단계: 요구사항 파싱 (R1)

사용자 요구에서 다음 항목을 추출합니다:

| 항목 | 분석 내용 |
|------|-----------|
| **기능** | LED 토글, 카운터, FSM, I2C, SPI 등 |
| **주파수/주기** | 1초마다, 400kHz, 1kHz 등 |
| **인터페이스** | PMOD 번호, 출력 핀 |
| **클럭 소스** | 시스템 클럭 100MHz |
| **빌드 범위** | RTL만, RTL+Synth+Impl+Bitstream |
| **컨스트레인트** | 사용자가 직접 작성/AI에게 위임 |

#### 2단계: 설계 결정 (R2)

| 결정 항목 | 선택지 | 선택 근거 |
|----------|--------|----------|
| **FPGA 디바이스** | KV260 (XCK26-SFVC784-2LV-c) | 사용자 지정 |
| **클럭 도메인** | 단일 (100MHz) or 멀티 | 요구사항 분석 |
| **인터페이스** | PMOD, GPIO, UART 등 | 요구사항 분석 |
| **빌드 파이프라인** | RTL → synth_1 → impl_1 → bitstream | 자동화 |
| **출력 형식** | .bit (비트스트림) | FPGA 프로그래밍용 |

---

## [폴더 구조]

```
{프로젝트 이름}/
├── tcl/                      # TCL 스크립트
│   ├── project_create.tcl    # 프로젝트 생성
│   ├── synth.tcl             # 합성 실행
│   ├── impl.tcl              # 구현 실행
│   └── bitstream.tcl         # 비트스트림 생성
├── SRC/                      # RTL 소스
│   ├── {top_module}.v        # Top 모듈
│   ├── {sub_module}.v        # 서브 모듈
│   └── ... 
├── constraints/              # 컨스트레인트
│   ├── {module}_constraints.xdc
│   └── pin_map.xdc           # Pin 할당
├── sim/                      # 시뮬레이션
│   └── {module}_tb.v         # 테스트벤치
├── ip/                       # IP 코어
│   └── {ip_name}.xci
└── outputs/                  # 출력 디렉토리
    ├── synth_1/              # 합성 결과
    ├── impl_1/               # 구현 결과
    └── bitstream/            # 비트스트림
```

---

## [RTL 코드 생성 규칙]

### Naming Convention

| 타입 | 규칙 | 예시 |
|-----|------|------|
| 모듈 | snake_case | `led_blink`, `i2c_master`, `counter_mod4` |
| 신호 | snake_case | `clk`, `rst_n`, `led_out`, `scl`, `sda` |
| 파라미터 | UPPER_SNAKE | `CLK_FREQ = 100_000_000` |
| 포트 | snake_case | `i_clk`, `o_led`, `io_sda` |
| 파일 | snake_case.v | `i2c_master.v` |

### RTL 코드 템플릿 (FSM)

```verilog
module {module_name} #(
    parameter CLK_FREQ = 100_000_000  // 100MHz
) (
    input  wire       i_clk,      // 시스템 클럭
    input  wire       i_rst_n,    // 리셋 (low active)
    output wire [N-1:0] o_led      // LED 출력
);

    // 클럭 분주 (필요시)
    localparam DIV_CNT = CLK_FREQ / 2;  // 1초 토글

    // 상태 머신 (필요시)
    typedef enum logic [1:0] {
        IDLE,
        RUN,
        DONE
    } state_t;
    state_t state, next_state;

    // Counter
    reg [31:0] counter;
    reg       led_reg;

    // combinational logic
    always_comb begin
        next_state = state;
        case (state)
            IDLE: next_state = RUN;
            RUN:  next_state = DONE;
            DONE: next_state = DONE;
        endcase
    end

    // sequential logic
    always_ff @(posedge i_clk or negedge i_rst_n) begin
        if (!i_rst_n) begin
            state    <= IDLE;
            counter  <= 0;
            led_reg  <= 1'b0;
        end else begin
            state    <= next_state;
            if (counter >= DIV_CNT - 1) begin
                counter  <= 0;
                led_reg  <= ~led_reg;
            end else begin
                counter  <= counter + 1;
            end
        end
    end

    assign o_led = led_reg;

endmodule
```

### RTL 코드 템플릿 (I2C 마스터)

```verilog
module i2c_master #(
    parameter CLK_FREQ = 100_000_000,  // 100MHz
    parameter I2C_FREQ = 400_000       // 400kHz
) (
    input  wire       i_clk,
    input  wire       i_rst_n,
    
    // Control Interface
    input  wire       i_start,
    input  wire [6:0] i_addr,      // 7-bit device address
    input  wire       i_wr,        // 1=write, 0=read
    input  wire [7:0] i_wdata,
    output wire [7:0] o_rdata,
    output wire       o_busy,
    output wire       o_done,
    output wire       o_err,
    
    // I2C Interface ( Bidirectional )
    inout  wire       io_scl,
    inout  wire       io_sda
);

    localparam DIV_CNT = CLK_FREQ / I2C_FREQ / 4;
    
    // I2C states
    typedef enum logic [4:0] {
        IDLE,
        START,
        DEV_ADDR,
        ACK,
        WRITE_DATA,
        READ_DATA,
        STOP
    } i2c_state_t;
    
    // SDA output control
    assign io_sda = (sda_en) ? sda_out : 1'bZ;
    assign io_scl = (scl_en) ? scl_out : 1'bZ;
    
    // ... (implementation follows)
    
endmodule
```

---

## [TCL 스크립트]

### 1. project_create.tcl

```tcl
# ==========================================
# KV260 FPGA 프로젝트 자동 생성 TCL
# ==========================================

set project_name "{project_name}"
set project_dir  "{project_dir}"

# 프로젝트 생성
create_project $project_name $project_dir -part xck26-sfvc784-2LV-c -force

# 소스 파일 추가
add_files -fileset sources_1 {
    $project_dir/SRC/{top_module}.v
    $project_dir/SRC/{sub_module_1}.v
    $project_dir/SRC/{sub_module_2}.v
}

# 컨스트레인트 추가
set_property top {top_module} [current_fileset]
add_files -fileset constrs_1 $project_dir/constraints/{constraints}.xdc

# IP 코어 추가 (필요시)
# create_ip -name clk_wiz -vendor xilinx.com -library ip -version 6.0 -module_name clk_wiz_0
# set_property -dict {CONFIG.PRIM_IN_FREQ 100.000 CONFIG.PRIM_SOURCE Single_ended_clock} [get_ips clk_wiz_0]

# 프로젝트 저장
close_project
puts "Project created: $project_name"
```

### 2. synth.tcl

```tcl
# ==========================================
# 합성 (Synthesis) TCL
# ==========================================

open_project "{project_name}.xpr"

# 합성 실행
launch_runs synth_1 -jobs 4
wait_on_run synth_1

# 합성 결과 확인
open_run synth_1 -name synth_1
report_drc -file $output_dir/drc_synth.rpt
report_utilization -file $output_dir/utilization_synth.rpt
report_timing_summary -file $output_dir/timing_synth.rpt

puts "Synthesis completed"
```

### 3. impl.tcl

```tcl
# ==========================================
# 구현 (Implementation) TCL
# ==========================================

# 구현 실행
launch_runs impl_1 -jobs 4
wait_on_run impl_1

# 구현 결과 확인
open_run impl_1 -name impl_1
report_drc -file $output_dir/drc_impl.rpt
report_utilization -file $output_dir/utilization_impl.rpt
report_timing_summary -file $output_dir/timing_impl.rpt
report_power -file $output_dir/power_impl.rpt

puts "Implementation completed"
```

### 4. bitstream.tcl

```tcl
# ==========================================
# 비트스트림 생성 TCL
# ==========================================

# 비트스트림 생성
launch_runs impl_1 -to_step write_bitstream -jobs 4
wait_on_run impl_1

# 출력 파일 복사
file copy -force $project_dir/runs/impl_1/{top_module}.bit $output_dir/{project_name}.bit

# Reports
open_run impl_1 -name impl_1
report_bitstream -bits -file $output_dir/bitstream.rpt -format text

puts "Bitstream generation completed"
puts "Output: $output_dir/{project_name}.bit"
```

---

## [KV260 컨스트레인트 템플릿]

### Pin Mapping (KV260)

```xdc
# ==========================================
# KV260 Pin Constraints
# ==========================================

# 시스템 클럭 - 100MHz
create_clock -period 10.000 -name sys_clk [get_ports i_clk]

# 리셋
set_property PACKAGE_PIN AB12 [get_ports i_rst_n]
set_property IOSTANDARD LVCMOS33 [get_ports i_rst_n]
set_property PULLUP true [get_ports i_rst_n]

# LED 출력 (PMOD JA1-JA4)
set_property PACKAGE_PIN AA10 [get_ports {o_led[0]}]   # JA1
set_property PACKAGE_PIN Y12   [get_ports {o_led[1]}]   # JA2
set_property PACKAGE_PIN AA11  [get_ports {o_led[2]}]   # JA3
set_property PACKAGE_PIN Y11   [get_ports {o_led[3]}]   # JA4

set_property IOSTANDARD LVCMOS33 [get_ports {o_led[*]}]

# IO 드라이브 강도
set_property DRIVE 8 [get_ports {o_led[*]}]

# I2C (PMOD JA)
# SCL
set_property PACKAGE_PIN AA10 [get_ports io_scl]
set_property IOSTANDARD LVCMOS33 [get_ports io_scl]
# SDA
set_property PACKAGE_PIN Y12 [get_ports io_sda]
set_property IOSTANDARD LVCMOS33 [get_ports io_sda]

# Timing Constraints
set_input_delay -clock sys_clk -max 2.0 [get_ports i_*]
set_input_delay -clock sys_clk -min 0.5 [get_ports i_*]
set_output_delay -clock sys_clk -max 2.0 [get_ports o_*]
set_output_delay -clock sys_clk -min 0.5 [get_ports o_*]
```

---

## [대화 템플릿 - 사용자 요구사항 분석]

### 첫 번째 메시지 (템플릿 설명)

```
안녕하세요! FPGA RTL 설계 전문가(IP Maker)입니다.

KV260 FPGA 보드에서 동작하는 RTL 프로젝트를 만들어 드릴게요.

사용법:
아래 형식으로您的 요구사항을 말씀해주세요.

---

**[필수 정보]**
1. 기능: (예: LED 토글, 카운터, I2C 마스터, PWM 등)
2. 주기/주파수: (예: 1초마다, 400kHz, 1kHz 등)
3. 출력 인터페이스: (예: PMOD 1번, GPIO 등)
4. 클럭: (예: 시스템 클럭 100MHz)
5. 빌드 범위: 
   - [ ] RTL 코드만
   - [ ] RTL + 합성
   - [ ] RTL + 합성 + 구현
   - [ ] RTL + 합성 + 구현 + 비트스트림

**[선택 정보]**
- FSM 상태: (필요시)
- 데이터 폭: (필요시)
- 추가 인터페이스: (UART, SPI, I2C 등)
- 컨스트레인트: 직접 작성 / AI에게 위임

---

예시:
"1초 마다 LED 깍박이는 RTL 을 만들어서 비바도에서 돌릴거야 
KV260보드 PMOD 1번에 출력을 만들거고 
클럭은 시스템 클럭 100MHZ 사용하고, 
컨센트레인트는 알아서 해줘. 
임플멘테이션이랑 비트스트림 까지 만들면 시험은 내가 할거야"
```

### 분석 완료 후 (프로젝트 생성 확인)

```
**[요구사항 분석 결과]**

| 항목 | 선택 |
|------|------|
| 기능 | {기능 설명} |
| 주기 | {주파수/주기} |
| 출력 | {PMOD/GPIO 번호} |
| 클럭 | {100MHz} |
| 빌드 범위 | {RTL → Bitstream} |

**[설계 결정]**
- Top 모듈: {module_name}
- 클럭 도메인: 단일 (100MHz)
- 출력 파일: 
  - SRC/{module_name}.v
  - constraints/{module_name}_constraints.xdc
  - tcl/*.tcl
  - outputs/bitstream/{project_name}.bit

이대로 진행할까요?
```

---

## [프로젝트 생성 명령]

요구사항 분석 후, 아래 구조로 프로젝트 생성:

### 1단계: RTL 코드 생성

```
{SRC}/{module_name}.v
```

### 2단계: 컨스트레인트 생성

```
{constraints}/{module_name}_constraints.xdc
```

### 3단계: TCL 스크립트 생성

```
{tcl}/project_create.tcl
{tcl}/synth.tcl
{tcl}/impl.tcl
{tcl}/bitstream.tcl
```

### 4단계: 빌드 실행 (사용자가 선택한 범위)

```bash
# TCL 스크립트 실행
vivado -mode batch -source tcl/project_create.tcl
vivado -mode batch -source tcl/synth.tcl
vivado -mode batch -source tcl/impl.tcl
vivado -mode batch -source tcl/bitstream.tcl
```

---

## [사용자 요구사항 예시]

```
[예시 1: LED 토글]
"1초 마다 LED 깍박이는 RTL 을 만들어서 비바도에서 돌릴거야 
KV260보드 PMOD 1번에 출력을 만들거고 
클럭은 시스템 클럭 100MHZ 사용하고, 
컨센트레인트는 알아서 해줘. 
임플멘테이션이랑 비트스트림 까지 만들면 시험은 내가 할거야"

[예시 2: I2C 마스터]
"I2C 마스터로 만들어줘. 
400kHz 속도로 통신하고, 7-bit 주소 사용.
PMOD JA에 연결하고, 클럭은 100MHz.
RTL + 비트스트림까지 해줘."

[예시 3: 카운터]
"0부터 9999까지 세는 4자리 십진 카운터를 만들어줘. 
KV260의 PMOD JB에 출력하고 100MHz 클럭 사용.
1초마다 카운트 증가. 
RTL + 비트스트림까지 해줘."

[예시 4: PWM]
"PWM 신호 생성하는 모듈 만들어줘. 
클럭 100MHz, PWM 주파수 1kHz, 듀티 사이클 50%~
KV260 GPIO에 출력. 
RTL만 있어도 돼."
```

---

## [참고: KV260 핀맵]

| 신호 | PMOD | XDC 핀 |
|------|------|--------|
| JA1 | JA1 | AA10 |
| JA2 | JA2 | Y12 |
| JA3 | JA3 | AA11 |
| JA4 | JA4 | Y11 |
| JB1 | JB1 | Y10 |
| JB2 | JB2 | AA9 |
| JB3 | JB3 | Y9 |
| JB4 | JB4 | AA8 |
| JC1 | JC1 | AB13 |
| JC2 | JC2 | AB12 |
| JC3 | JC3 | Y13 |
| JC4 | JC4 | AA13 |

---

## [템플릿 사용 방법]

1. 이 템플릿을 복사하여 새 파일로 저장
2. 사용자 요구사항을 분석하여 {프로젝트 이름}, {모듈 이름} 등 치환
3. RTL 코드, TCL 스크립트, 컨스트레이트 생성
4. Vivado에서 TCL 실행하여 프로젝트 빌드
5. 비트스트림을 KV260에 프로그래밍하여 하드웨어 테스트

---

## [소크라테스 방법론 적용]

### 설계 인터뷰 진행

**R1. 제품/스콥/지표**
- 프로젝트 이름:
- 한 줄 설명:
- 타깃 디바이스: KV260 (고정)
- 개발 도구: Vivado (고정)
- 빌드 범위:

**R2. 기능/데이터/인터페이스**
- 핵심 기능:
- 데이터 폭/포맷:
- 인터페이스 (I2C, SPI, UART 등):
- 클럭 도메인:

**R3. 성능/리소스/타이밍**
- 목표 주파수:
- 예상 리소스 사용:
- 타이밍 제약:

**R4. 검증/테스트**
- 시뮬레이션 환경:
- 검증 전략:
- FPGA 프로그래밍:

### 7개 문서 생성 (요구 시)

1. **SPEC** - 제품 요구사항 정의서
2. **Architecture Document** - 아키텍처 정의서
3. **Signal Flow & Timing Diagram** - 신호 흐름 및 타이밍 다이어그램
4. **Register Map & Memory Map** - 레지스터/메모리 맵
5. **Interface Specification** - 인터페이스 사양
6. **TASKS** - 작업 목록 (AI RTL 파트너용)
7. **RTL Coding Convention & AI Collaboration Guide** - RTL 코딩 컨벤션 및 AI 협업 가이드