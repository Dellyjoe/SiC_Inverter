# SiC-Based Modular Inverter System

> This project documents the design and development of a 500 W (1 kW peak) single-phase SiC-based inverter. It is built as a modular, scalable platform for power electronics experimentation, featuring a low-inductance DC-link, isolated gate drivers, an LCL output filter, and a hierarchical schematic design in KiCad.

## Table of Contents
1.  [Project Overview](#1-project-overview)
2.  [System Architecture](#2-system-architecture)
    -   [EMI Input Filter](#21-emi-input-filter)
    -   [Pre-Charge & Inrush Limiting](#22-pre-charge--inrush-limiting)
    -   [DC-Link Capacitor Network](#23-dc-link-capacitor-network)
    -   [Inverter Power Stage](#24-inverter-power-stage)
    -   [Output LCL Filter](#25-output-lcl-filter)
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
*   **Modular functional blocks** (EMI filter, pre‑charge, DC‑link, inverter legs, LCL filter)
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
*   **Topology:** Full-bridge configuration using SiC MOSFET half‑bridge legs
*   **Switching Devices:** 4× Wolfspeed SiC MOSFETs (`C3M0025065K`)
*   **Gate Drivers:** 4× TI Isolated Gate Drivers (`UCC21710QDWRQ1`)
*   **Sensing:** Placeholders for output current and DC bus voltage sensing
*   **Control:** PWM control interface for connection to a microcontroller

### 2.5 Output LCL Filter
*   **Purpose:** Shape inverter PWM into low‑distortion 60 Hz AC and limit grid/load current ripple
*   **Topology:** LCL filter (series–shunt–series)
*   **Components:**
    *   **Series Inductors:**  
        * `L1`: 470 µH, through‑hole power inductor (`Bourns 1140-471K-RC`)  
        * `L2`: 470 µH, through‑hole power inductor (`Bourns 1140-471K-RC`)
    *   **Shunt Capacitor:**  
        * `C_F`: 6.8 µF, 305 Vac polypropylene film (`KEMET C4AF9BW4680T3FK`, 4‑pin MKP)
    *   **Damping Resistor:**  
        * `R_D`: ~10 Ω, ≥3 W TE Connectivity power resistor (`3-2176794-3`) in series with `C_F`
*   **Design Target:**
    *   Resonant frequency ≈ 2 kHz (well above 60 Hz, well below 40 kHz switching)
    *   Damped to avoid excessive peaking around resonance

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
| **Output Filter Topology** | LCL (470 µH – 6.8 µF – 470 µH) | With series damping resistor |

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
*   **Real Parts Only:** Filter and power stages are designed around in‑stock, verifiable components (no “unicorn” magnetics).

---

## 6. Current Status

- [x] EMI filter: **Complete**
- [x] Pre‑charge topology: **Defined**
- [x] DC‑link architecture: **Defined (220 µF + 10 µF film)**
- [x] KiCad hierarchy: **Clean and modular**
- [x] Add pre‑charge components to schematic
- [x] Add DC‑link capacitor bank
- [x] Begin inverter leg schematic
- [x] Define and populate **Output LCL Filter** (L1/L2/C_F/R_D with real parts)
- [ ] Review inverter leg + LCL interaction in simulation
- [ ] Layout planning for high‑current paths and filter placement

---

## 7. Next Steps

1.  Finalize and review the **inverter half‑bridge leg** schematic, including gate drivers.
2.  Simulate the **LCL filter + inverter leg** to verify resonance, damping, and current ripple.
3.  Integrate isolated power supplies for the gate drivers.
4.  Plan PCB layout for:
    *   Low‑inductance DC-link loop
    *   Tight inverter–LCL interconnect
    *   Safe creepage/clearance
5.  Develop a detailed, step-by-step bring‑up procedure (block‑by‑block power‑up and test).

---

## 8. System Block Diagram

![Block Diagram](SiC_Single_Phase_Inverter\Images\SiC_Single_Phase_Inverter_Block_Diagram.png)