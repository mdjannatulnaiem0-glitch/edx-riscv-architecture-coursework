edx-riscv-architecture-coursework 

Coursework, lab assignments, and technical notes from the professional RISC-V architecture certification course on edX. Focused on RV32I ISA, register allocation, and microarchitecture concepts.

edX RISC-V Architecture & ISA Coursework

This repository contains my personal academic notes, assembly lab exercises, and certification progress for the **RISC-V Fundamentals/Architecture** course hosted on edX.

Course Learning Objectives
- Understanding the **RV32I (Base Integer Instruction Set)** and modular extensions.
- Analyzing hardware-software interfaces, assembly execution pipelines, and instruction decoding.
- Designing low-level logic structures including Control Units, ALUs, and Register Files.
- Simulating and verifying instructions using open-source visual simulators (e.g., Ripes/Venus).

# 🍳 Custom RISC-V (RV32I) ASIC Core for Automated Food Processing

An advanced, application-specific integrated circuit (ASIC) design based on the open-source RISC-V (RV32I) ISA, custom-tailored to control a fully automated multi-stage culinary appliance (Specifically optimized for "Khichuri" cooking automation). This repository contains the structural Verilog/Chisel RTL design, custom ISA extensions, gate-level logic implementations, and bare-metal Assembly firmware.

---

## 🧠 Architectural Overview

This project bypasses generic microcontrollers to build a dedicated, hardware-level state machine. The microarchitecture leverages a streamlined execution pipeline tightly coupled with dedicated sensory registers to make precise real-time thermodynamic decisions.

### 1. 🗃️ Register File Mapping (Internal x0 - x31)
The core utilizes the standard 32 general-purpose registers, dynamically mapped for physical parameter tracking without relying on external RAM overhead:
* **`x0` (Zero Register):** Hardwired to `0` for instantaneous conditional flag resets.
* **`x5` (`REG_CURRENT_TEMP`):** Holds the real-time digitized binary data streamed from the thermocouple sensor.
* **`x6` (`REG_TARGET_TEMP`):** Statically or dynamically loaded with the boiling/simmering thresholds (e.g., 100°C binary equivalent).
* **`x7` (`REG_TIMER_COUNT`):** Dedicated countdown register decrementing at every clock cycle for precise stage duration tracking.
* **`x8` (`REG_SYS_STATE`):** Tracks hardware execution states (`2'b00`: Pre-heating, `2'b01`: Boiling/Agitation, `2'b10`: Simmering/Dam-Cooking).

### 2. 🔢 Custom Arithmetic Logic Unit (ALU) & Gate-Level Logic
The ALU is optimized for structural efficiency using fundamental combinational and sequential logic gates:
* **Hardware Comparator:** Built using optimized gate-level XOR and AND matrices to execute rapid `Branch If Less Than (BLT)` operations by checking if `x5` (Current Temp) has intersected with `x6` (Target Temp).
* **Combinational Safety Interlocks:** 
  * **AND Array:** Implements a hardware-enforced fail-safe macro: `[Vessel_Detected (1)] AND [Fluid_Level_Safe (1)] = Enable_Actuator (1)`.
  * **OR Array:** Handles immediate hazard triaging: `[Over_Temperature (1)] OR [Timer_Expired (1)] = Emergency_Shutdown (1)`.
* **Sequential Storage:** High-density D-Flip-Flop matrices constitute the underlying architecture of the 32 registers, safeguarding data integrity against clock jitter.

### 3. 🖥️ Bare-Metal Assembly Firmware Implementation
The automated processing sequence is governed by optimized bare-metal RISC-V Assembly instructions executing directly on the hardware:

```assembly
# RISC-V (RV32I) Core Culinary Automation Firmware

LI x6, 100            # Load immediate threshold temperature (100°C) into x6
LI x7, 1200           # Load immediate countdown timer (1200 seconds / 20 mins) into x7

POLL_SENSOR:
    IN x5, TEMP_ADDR  # Stream real-time data from Sensor I/O mapped address to x5
    BLT x5, x6, MOD_H # If x5 < x6 (Temp < 100°C), jump to High-Heat Modification
    JAL x1, PHASE_TWO # If threshold reached, branch to Phase Two (Simmering & Agitation)

MOD_H:
    LI x9, 1          # Set High bit in x9
    OUT HEAT_CTRL, x9 # Assert digital output pin high via logic gates to drive the actuator
    JAL x0, POLL_SENSOR # Absolute jump back to re-evaluate the thermodynamic loop
```

---

 Hardware Stack & Tools Used
* **Hardware Description Language (HDL):** Verilog / SystemVerilog / Chisel
* **Instruction Set Architecture:** RISC-V (RV32I Base Integer)
* **Simulation & Verification:** Icarus Verilog / Verilator / GTKWave
* **Synthesis Tarepository* OpenLane / SkyWater 130nm PDK (Targeting ASIC Flow)

---

## How to Run and Simulate
*(Provide instructions here on how to clone your repository, run your compiler, and view the waveforms in GTKWave as your project matures).*

