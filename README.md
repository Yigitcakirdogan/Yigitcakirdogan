<div align="center">

# Yiğit Çakırdoğan
### Embedded Systems & Firmware Engineer
**ARM Cortex-M (STM32) • FreeRTOS • Bare-Metal Drivers • Industrial Protocols (CAN / Modbus) • Industrial IoT (ESP32)**

[![GitHub Followers](https://img.shields.io/github/followers/Yigitcakirdogan?style=flat-square&logo=github&label=Follow)](https://github.com/Yigitcakirdogan)
![Focus](https://img.shields.io/badge/Focus-Deterministic%20Firmware%20%26%20RTOS-00599C?style=flat-square&logo=c)
![Architecture](https://img.shields.io/badge/Target-ARM%20Cortex--M4%2FM7%20%7C%20ESP32-E7352C?style=flat-square&logo=arm)
![CI](https://img.shields.io/badge/Verification-Host%20Unit%20Tests%20%26%20CI-2EA44F?style=flat-square&logo=githubactions)

</div>

---

## Engineering Profile

I design and implement **deterministic, resource-constrained embedded software** for microcontrollers and industrial nodes. My engineering approach prioritizes **predictable real-time execution**, **zero-copy peripheral DMA pipelines**, **hardware-decoupled driver architectures**, and **automated host-based verification**.

- **Bare-Metal & CMSIS Firmware:** Direct register-level peripheral bring-up (`RCC`, `GPIO`, `DMA`, `USART`, `SPI`, `I2C`, `TIM/PWM`, `ADC`, `IWDG`), custom GNU LD linker scripts, startup vector tables, and HardFault diagnostics on **STM32 (ARM Cortex-M4F/M7)**.
- **Real-Time Operating Systems (FreeRTOS):** Rate-monotonic task partitioning, static memory allocation (`xTaskCreateStatic`), lock-free ISR-to-task synchronization, priority-inheritance mutexes, and multi-task hardware watchdog supervision.
- **Industrial & Automotive Fieldbuses:** **CAN 2.0B (ISO 11898)** frame filtering/packing, **Modbus RTU (RS-485)** slave/master state machines with table-driven CRC-16, and deterministic binary command protocols.
- **Connected Industrial Edge & Control:** **ESP32 (ESP-IDF)** telemetry nodes with finite state machine (FSM) network resilience, 1D Kalman/median sensor conditioning, and discrete-time PID control algorithms.

---

## Reference Firmware Architecture

```mermaid
flowchart LR
    subgraph HW["Hardware & Peripherals (STM32 / ESP32)"]
        ADC["ADC + Circular DMA"]
        UART["USART + DMA + IDLE ISR"]
        CAN["bxCAN / ISO 11898"]
        I2C_SPI["SPI / I2C Bus Recovery"]
    end

    subgraph HAL["Zero-Copy HAL & Lock-Free Primitives"]
        RB["Lock-Free SPSC Ring Buffer"]
        DRV["Register-Level CMSIS Drivers"]
        FILT["Kalman & Median DSP Filter"]
    end

    subgraph RTOS["Deterministic FreeRTOS Application Layer"]
        GW["CAN <-> Modbus RTU Gateway"]
        CLI["Zero-Heap Diagnostic CLI"]
        FSM["Telemetry & OTA State Machine"]
        WDT["Multi-Task Watchdog Supervisor"]
    end

    ADC --> FILT --> GW
    UART --> RB --> CLI
    CAN --> DRV --> GW
    I2C_SPI --> DRV --> FSM
    WDT -. "Deadman Check-In (IWDG)" .-> GW & CLI & FSM
```

---

## Featured Engineering Repositories

| Repository | Architecture & Technical Highlights | Verification |
| :--- | :--- | :---: |
| **[`stm32-dma-ringbuffer-uart-cli`](https://github.com/Yigitcakirdogan/stm32-dma-ringbuffer-uart-cli)** | **Zero-Copy UART DMA + Lock-Free SPSC Ring Buffer & CLI**<br>Uses `DMA1` Circular Mode + `USART_SR_IDLE` interrupt for variable-length packet reception with near-zero CPU overhead, paired with a static-memory diagnostic shell. | ![CI](https://github.com/Yigitcakirdogan/stm32-dma-ringbuffer-uart-cli/actions/workflows/ci.yml/badge.svg) |
| **[`freertos-canbus-modbus-gateway`](https://github.com/Yigitcakirdogan/freertos-canbus-modbus-gateway)** | **FreeRTOS CAN 2.0B to Modbus RTU Industrial Gateway**<br>Bridges automotive/industrial CAN frames into RS-485 Modbus Holding/Input registers (`FC03`, `FC04`, `FC06`, `FC10`) with table-driven CRC-16 and task-level watchdog supervision. | ![CI](https://github.com/Yigitcakirdogan/freertos-canbus-modbus-gateway/actions/workflows/ci.yml/badge.svg) |
| **[`stm32f4-baremetal-hal-drivers`](https://github.com/Yigitcakirdogan/stm32f4-baremetal-hal-drivers)** | **Register-Level STM32F407 Drivers & Custom Linker Script**<br>Pure CMSIS peripheral drivers (`GPIO`, `SPI`, `I2C` with 9-clock bus lockup recovery, `IWDG`), `Reset_Handler` startup in C, and `CCM-RAM` linker script—zero vendor HAL bloat. | ![CI](https://github.com/Yigitcakirdogan/stm32f4-baremetal-hal-drivers/actions/workflows/ci.yml/badge.svg) |
| **[`esp32-industrial-iot-telemetry-node`](https://github.com/Yigitcakirdogan/esp32-industrial-iot-telemetry-node)** | **Fault-Tolerant Industrial IoT Edge Node**<br>Deterministic connection FSM with exponential backoff, 1D Kalman + Median-of-5 sensor filtering, CRC32 binary/JSON telemetry codec, and offline store-and-forward ring buffering. | ![CI](https://github.com/Yigitcakirdogan/esp32-industrial-iot-telemetry-node/actions/workflows/ci.yml/badge.svg) |
| **[`yay-kutle-sonum-sistemi`](https://github.com/Yigitcakirdogan/yay-kutle-sonum-sistemi)** | **Discrete-Time Control & Mass-Spring-Damper Simulation**<br>Finite-difference discretization of $m\ddot{x} + c\dot{x} + kx = F(t)$ paired with an anti-windup, derivative-on-measurement Embedded C PID controller and Python step-response analysis. | ![Control](https://img.shields.io/badge/Control-PID%20%26%20Discretization-blue) |

---

## Technical Stack & Tooling

| Domain | Technologies & Standards |
| :--- | :--- |
| **Languages & Standards** | `C (C99 / C11)`, `C++ (C++17 Embedded Subset)`, `Python 3`, `ARM Thumb-2 Assembly`, `GNU LD Linker Scripts` |
| **Microcontrollers & SoCs** | `STM32F4 / STM32F1 / STM32H7 (ARM Cortex-M3/M4F/M7)`, `ESP32 / ESP32-S3 (Xtensa / RISC-V)` |
| **RTOS & Concurrency** | `FreeRTOS (Static Allocation, Queues, Direct-to-Task Notifications, Mutexes)`, `Lock-Free Ring Buffers`, `DMA Double/Circular Buffering` |
| **Communication Protocols** | `CAN 2.0A/B (ISO 11898)`, `Modbus RTU (RS-485)`, `SPI`, `I2C (SMBus)`, `UART / RS-232`, `MQTT / TLS`, `BLE` |
| **Build, Test & Quality** | `CMake`, `GNU Arm Embedded Toolchain (arm-none-eabi-gcc)`, `CTest`, `Cppcheck Static Analysis`, `GitHub Actions CI/CD`, `GDB / OpenOCD` |
| **Hardware & Lab Debugging** | `Saleae Logic Analyzer`, `DSO Oscilloscope (Protocol Decoding & Timing Profiling)`, `ST-Link / J-Link SWD`, `Schematic Reading` |

---

## Core Firmware Design Principles

1. **Zero Dynamic Allocation After Init:** All queues, ring buffers, protocol contexts, and RTOS task control blocks (`StaticTask_t`) are statically allocated to eliminate heap fragmentation and guarantee bounded execution times.
2. **Interrupt Service Routines (ISRs) Stay Minimal:** ISRs only acknowledge hardware flags, update lock-free indices, or defer processing via `xQueueSendFromISR()` / DMA transfer-complete callbacks.
3. **Hardware Abstraction via Dependency Injection:** Peripheral drivers expose clean operations structs (`vtable`/ops pointers), enabling full host-side unit testing (`x86_64 GCC`) on every commit before flashing target silicon.
4. **Defensive Embedded Coding:** Hardware loops include explicit tick timeouts, I2C masters implement physical bus-unstick recovery, and Independent Watchdogs (`IWDG`) are gated by a multi-task deadman supervisor.
