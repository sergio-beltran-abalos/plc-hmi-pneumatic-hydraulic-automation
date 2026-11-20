# Industrial Electropneumatic & Electrohydraulic Automation — Siemens S7-1200 PLC & KTP700 HMI

[![Siemens PLC](https://img.shields.io/badge/PLC-Siemens%20S7--1200%20(TIA%20Portal)-009999?style=flat-square)](#1-siemens-s7-1200-plc-ladder-programming)
[![HMI](https://img.shields.io/badge/HMI-SIMATIC%20KTP700%20(WinCC%20%7C%20PROFINET)-006487?style=flat-square)](#2-wincc-simatic-ktp700-hmi-interface)
[![FluidSIM](https://img.shields.io/badge/Simulation-FluidSIM%20Pneumatics%20%26%20Hydraulics-1E3A8A?style=flat-square)](#3-fluidsim-circuit-design--displacement-step-analysis)
[![Festo](https://img.shields.io/badge/Hardware-Festo%20Industrial%20Benches-0091DC?style=flat-square)](#4-physical-festo-bench-commissioning)

> End-to-end industrial automation portfolio covering **FluidSIM circuit simulation**, **electropneumatic & electrohydraulic control**, **Siemens S7-1200 PLC Ladder (`LAD`) programming** (`SET/RESET` step sequencers, `TON` IEC timers, `CTU` counters, 8-group cascades, and `0–10 V` analog scaling via `NORM_X`/`SCALE_X`), **2-screen WinCC SIMATIC KTP700 HMI supervision**, and **physical commissioning on Festo training benches**.

<p align="center">
  <img src="assets/hmi_main_screen.png" alt="SIMATIC KTP700 Main Screen" width="47%" />
  <img src="assets/hmi_process_screen.png" alt="SIMATIC KTP700 Process Control Screen" width="47%" />
</p>

---

## Core Industrial Automation Competencies

- **PLC Programming (`Siemens S7-1200` / `TIA Portal V15`):** Structured `LAD` programming covering single/continuous cycles, `SET`/`RESET` state-machine step sequencers, `IEC_Timer` (`TON`) delays, `CTU` cycle counters, and multi-group cascade architectures.
- **Analog Signal Conditioning (`0–10 V`):** Raw 16-bit peripheral normalization (`0 – 27648` via `NORM_X`) and linear engineering scaling (`SCALE_X`) for real-time hydraulic pressure (**bar**) and motor rotational speed (**RPM**).
- **HMI / SCADA Design (`WinCC` / `KTP700 Basic`, PROFINET):** 2-screen operator interface with global function-key templates (`F1` Home, `F8` Stop Runtime), live decimal telemetry, and bidirectional hydraulic motor control (`CW` / `CCW`).
- **Fluid Power Engineering (`FluidSIM` & `Festo`):** Pneumatic intuitive/cascade/step-sequencer design, electrohydraulic directional control (3-position solenoid valves), hydraulic accumulators, and industrial 3-wire proximity & piezoresistive pressure sensors.

---

## 1. Siemens S7-1200 PLC Ladder Programming

### 1.1 `0–10 V` Analog Scaling (`NORM_X` → `SCALE_X`)
Industrial pressure and tachometer sensors deliver `0–10 V` signals mapped to `0–27648` raw counts on the S7-1200 analog input module. Each signal is normalized (`Int → Real`, `0.0–1.0`) via `NORM_X` and scaled via `SCALE_X` into **bar** and **RPM** for live HMI supervision:

<p align="center">
  <img src="assets/analog_scaling_ladder.png" alt="TIA Portal NORM_X and SCALE_X Ladder Logic" width="84%" />
</p>

### 1.2 Step-Sequencer State Machine with `SET`/`RESET` & `IEC_Timer` (`TON`)
Implemented electrohydraulic and electropneumatic step transitions using interlocking memory bits (`%M0.5`–`%M1.3`), latched solenoid outputs (`%Q0.0`, `%Q0.1`), and timed dwells (`TON` 2 s / 3 s):

<p align="center">
  <img src="assets/ladder_step_sequencer_ton.png" alt="TIA Portal Step Sequencer with TON Timer 1" width="46%" />
  <img src="assets/ladder_step_sequencer_timer2.png" alt="TIA Portal Step Sequencer with TON Timer 2" width="46%" />
</p>

### 1.3 Timed Cascade & Cycle Counting (`(A+ A- 1s B+ 2s B-) × 3`)
Combined cascade memory groups with `TON` delay timers and a `CTU` up-counter (`PV = 3`) for repeatable multi-cycle execution:

<p align="center">
  <img src="assets/pneumatic_cascade_ladder.png" alt="TIA Portal Timed Cascade Ladder" width="47%" />
  <img src="assets/ladder_ctu_counter_cascade.png" alt="TIA Portal CTU Counter and Timed Cascade" width="47%" />
</p>

### 1.4 8-Group Complex Cascade (`A+ B+ A- B- D- A+ D+ A-`)
Multi-actuator pneumatic sequence decomposed into 8 mutually exclusive cascade groups (`K1`–`K8`) to eliminate conflicting pilot signals across repeated actuator strokes:

<p align="center">
  <img src="assets/ladder_8group_cascade_logic.png" alt="8-Group Cascade State Transitions in Ladder" width="45%" />
  <img src="assets/ladder_8group_outputs.png" alt="8-Group Cascade Output Coil Mapping" width="45%" />
</p>

---

## 2. WinCC SIMATIC KTP700 HMI Interface

Designed in **TIA Portal WinCC** for the **SIMATIC KTP700 Basic (`6AV2 123-2GB03-0AX0`)** over **PROFINET**:
- **Screen 1 — Main (`Pantalla Principal`):** System header, live RTC date/time, screen navigation button, and global template mapping (`F1`: Home screen via `ActivateScreen`, `F8`: Stop Runtime via `StopRuntime`).
- **Screen 2 — Process (`Pantalla de Proceso`):** Live numeric indicators (2 decimal precision) for **Hydraulic Pressure (`bar`)** and **Motor Speed (`RPM`)**, plus operator pushbuttons toggling **Clockwise (`Horario`)** and **Counter-Clockwise (`Antihorario`)** hydraulic motor rotation via `InvertBit` PLC memory tags.

---

## 3. FluidSIM Circuit Design & Displacement-Step Analysis

### 3.1 Electrohydraulic Bidirectional Motor & Hydraulic Accumulator
Simulated 3-position solenoid directional control (`V2`) for CW/CCW hydraulic motor actuation, hydraulic accumulator charge/discharge isolation (`V1`), and displacement/speed verification:

<p align="center">
  <img src="assets/fluidsim_electrohydraulic_motor.png" alt="FluidSIM Electrohydraulic Bidirectional Motor Circuit" width="48%" />
  <img src="assets/fluidsim_hydraulic_accumulator.png" alt="FluidSIM Hydraulic Accumulator Circuit and Waveform" width="48%" />
</p>
<p align="center">
  <img src="assets/hydraulic_circuit_diagram.png" alt="Accumulator and Bidirectional Hydraulic Motor Architecture" width="46%" />
</p>

### 3.2 Electropneumatic Relay Cascade & Festo Step Sequencer
Verified power circuits, 24 V relay Ladder schematics, Festo step-sequencer modules, and multi-cycle displacement-step (`Fase-Paso`) diagrams prior to PLC translation:

<p align="center">
  <img src="assets/fluidsim_electropneumatic_power.png" alt="FluidSIM Electropneumatic Power Stage" width="47%" />
  <img src="assets/fluidsim_electropneumatic_relay_cascade.png" alt="FluidSIM 24V Relay Cascade Control Circuit" width="49%" />
</p>
<p align="center">
  <img src="assets/displacement_step_diagram_cascade.png" alt="Displacement-Step Verification Diagram" width="49%" />
  <img src="assets/fluidsim_festo_step_sequencer.png" alt="FluidSIM Festo Step-Sequencer Pneumatic Circuit" width="43%" />
</p>

---

## 4. Physical Festo Bench Commissioning

All simulated circuits and TIA Portal PLC/HMI programs were wired and commissioned on physical **Festo** industrial electropneumatic and electrohydraulic benches over **PROFINET / Ethernet**:

<p align="center">
  <img src="assets/festo_pneumatic_bench.jpg" alt="Festo Electropneumatic Bench with Siemens S7-1200 PLC" width="32%" />
  <img src="assets/festo_electrohydraulic_motor_bench.jpg" alt="Festo Electrohydraulic Bidirectional Motor Bench" width="32%" />
  <img src="assets/festo_hydraulic_bench.jpg" alt="Festo Electrohydraulic Accumulator and S7-1200 Bench" width="32%" />
</p>

---

## Technical Progression Summary

| Module | Scope | Tools & Industrial Hardware |
| :---: | :--- | :--- |
| **1** | Intuitive & Cascade Pneumatic Design | FluidSIM Pneumatics, Festo benches, timers, counters, reed sensors |
| **2** | Pneumatic Step Sequencer & Electropneumatics | Festo step module, 24 V relays, 3-wire proximity sensors |
| **3** | **Electropneumatic Cascade on Siemens S7-1200** | **TIA Portal V15, S7-1200 Ladder (`TON`, `CTU`, 8-group cascade), Festo solenoid valves** |
| **4** | Hydraulic Circuits & Bidirectional Hydraulic Motor | FluidSIM Hydraulics, 3-position solenoid valves, pressure/flow/RPM characterization |
| **5** | **PLC Analog Scaling (`0–10 V`), Accumulator & KTP700 HMI** | **S7-1200 (`NORM_X`/`SCALE_X`), WinCC KTP700 (PROFINET), Festo hydraulic bench** |

---

## Context & Attribution

- **Course:** Hydraulic and Pneumatic Systems (`MT4002`) — University of Engineering and Technology (UTEC), 2025
- **Contributors (PLC/HMI Implementations):** Sergio Edmundo Beltran Abalos, Arianna Mariajose Castro Carpio

> *Note: Original university lab guide documents are intentionally omitted to respect academic copyright; this repository showcases only personally implemented PLC Ladder logic, WinCC HMI screens, FluidSIM circuits, and physical Festo bench commissioning results.*
