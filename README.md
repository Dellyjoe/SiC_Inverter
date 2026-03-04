# SiC-Based Modular Inverter System

> This project documents the design and development of a 500W (1kW peak) single-phase SiC-based inverter. It is built as a modular, scalable platform for power electronics experimentation, featuring a low-inductance DC-link, isolated gate drivers, and a hierarchical schematic design in KiCad.

## Table of Contents
1.  [Project Overview](#1-project-overview)
2.  [System Architecture](#2-system-architecture)
    -   [EMI Input Filter](#21-emi-input-filter)
    -   [Pre-Charge & Inrush Limiting](#22-pre-charge--inrush-limiting)
    -   [DC-Link Capacitor Network](#23-dc-link-capacitor-network)
    -   [Inverter Power Stage](#24-inverter-power-stage)
3.  [Electrical Specifications](#3-electrical-specifications)
4.  [Development Tools & Equipment](#4-development-tools--equipment)
5.  [Design Philosophy](#5-design-philosophy)
6.  [Current Status](#6-current-status)
7.  [Next Steps](#7-next-steps)
8.  [System Block Diagram](#8-system-block-diagram-drawio-export)

---

## 1. Project Overview

This project is a **modular, SiC‑based inverter system** designed with professional engineering practices:

*   **Hierarchical schematic design** (KiCad 9)
*   **Modular functional blocks** (EMI filter, pre‑charge, DC‑link, inverter legs)
*   **High‑performance SiC MOSFETs**
*   **Low‑inductance DC‑link architecture**
*   **Lab‑bench‑friendly bring‑up strategy**

The goal is to create a **flexible, scalable inverter platform** suitable for experimentation, learning, and future expansion into higher power levels.

---

## 2. System Architecture

The inverter is organized into clear functional blocks.

### 2.1 EMI Input Filter
*   Differential‑mode LC filter (**`L_DM:`** `2300HT-470-H-RC`, **`C_IN:`** `R71PI410050H6K`)
*   Common‑mode choke (`7448258022`)
*   Provides clean, low‑noise DC to the rest of the system
*   **Outputs:** `BUS+_Filtered`, `BUS-_Filtered`

### 2.2 Pre‑Charge & Inrush Limiting
*   Limits inrush current to a target of **~5 A**
*   Uses an **80 Ω** pre‑charge resistor
*   Relay/MOSFET bypass path for full‑power operation
*   Interfaces between the EMI filter and DC-link
*   **Outputs:** `DC+`, `DC−`

### 2.3 DC‑Link Capacitor Network
*   **Bulk Storage:** **220 µF** total electrolytic capacitance (10 × `ESH226M500AL4AA`)
*   **High-Frequency Decoupling:** **10 µF** total film capacitance (5 × `MKP1848C52050JK2`)
*   **Provides:**
    *   Stable energy storage for the DC bus
    *   Low-impedance path for high-frequency switching currents
    *   Minimized voltage ripple and overshoot

### 2.4 Inverter Power Stage
*   **Topology:** Full-bridge configuration using SiC MOSFET half‑bridge legs.
*   **Switching Devices:** 4x Wolfspeed SiC MOSFETs (`C3M0025065K`).
*   **Gate Drivers:** 4x TI Isolated Gate Drivers (`UCC21710QDWRQ1`).
*   **Sensing:** Placeholders for output current and DC bus voltage sensing.
*   **Control:** PWM control interface for connection to a microcontroller.

---

## 3. Electrical Specifications

| Parameter | Value | Notes |
| :--- | :--- | :--- |
| **Input Voltage Range** | 335 - 425 VDC | |
| **Nominal DC Bus Voltage** | 400 VDC | |
| **Output Voltage** | 120 Vrms | Single-phase AC |
| **Output Frequency** | 60 Hz | |
| **Continuous Power** | 500 W | |
| **Peak Power** | 1 kW | |
| **Continuous Current (RMS)** | 4.1 Arms | Output |
| **Continuous Current (DC)** | 1.25 A | Input at nominal voltage |
| **Switching Frequency** | 40 kHz | Initial target |
| **Target Inrush Current** | < 5 A | Limited by pre-charge circuit |
| **Total DC‑Link Capacitance** | 220 µF (electrolytic) + 10 µF (film) | |
| **Capacitor Voltage Rating** | 500 V | |

---

## 4. Development Tools & Equipment
*   **CAD:** KiCad 9 (Hierarchical Schematics)
*   **Simulation:** TINA-TI / LTspice
*   **Version Control:** Git
*   **Documentation:** draw.io for diagrams, Markdown for READMEs

---
## 5. Design Philosophy
*   **Start Simple:** Build and validate one block at a time (e.g., EMI filter first).
*   **Safety First:** Incorporate pre-charge, fusing, and ESD protection from the start.
*   **Modularity:** Each functional block should be a self-contained schematic sheet.
*   **Low Inductance:** Keep the DC-link path short and wide to minimize switching-induced voltage overshoot.

---

## 6. Current Status

- [x] EMI filter: **Complete**
- [x] Pre‑charge topology: **Defined**
- [x] DC‑link architecture: **Defined (220 µF + 10 µF film)**
- [x] KiCad hierarchy: **Clean and modular**
- [x] Add pre‑charge components to schematic
- [x] Add DC‑link capacitor bank
- [ ] **Next steps:**    
    - Begin inverter leg schematic

---

## 7. Next Steps

1.  Finalize and build the **Pre‑Charge_Inrush_Limiting** schematic sheet.
2.  Add the **DC‑link capacitor network** to its schematic sheet.
3.  Start the **inverter half‑bridge leg** schematic, including gate drivers.
4.  Integrate isolated power supplies for the gate drivers.
5.  Develop a detailed, step-by-step bring‑up procedure.

---

## 8. System Block Diagram (draw.io export)

> [!TIP]
> **Placeholder:** Insert exported PNG/SVG from your draw.io high‑level block diagram here.
> `![Block Diagram](path/to/image.png)`

