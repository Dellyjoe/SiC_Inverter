markdown
# SiC-Based Modular Inverter System

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [System Architecture](#2-system-architecture)
    - [EMI Input Filter](#21-emi-input-filter)
    - [Pre-Charge & Inrush Limiting](#22-pre-charge--inrush-limiting)
    - [DC-Link Capacitor Network](#23-dc-link-capacitor-network)
    - [Inverter Power Stage](#24-inverter-power-stage-future-section)
3. [Electrical Specifications](#3-electrical-specifications)
4. [Development Tools & Equipment](#4-development-tools--equipment)
5. [Design Philosophy](#5-design-philosophy)
6. [Current Status](#6-current-status)
7. [Next Steps](#7-next-steps)
8. [System Block Diagram](#8-system-block-diagram-drawio-export)

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
*   Differential‑mode LC filter  
*   Common‑mode choke  
*   Provides clean, low‑noise DC to the rest of the system  
*   **Outputs:** `BUS+_Filtered`, `BUS-_Filtered`

### 2.2 Pre‑Charge & Inrush Limiting
*   Limits inrush current to ~5 A  
*   Uses an 80 Ω pre‑charge resistor  
*   Relay/MOSFET bypass path for full‑power operation  
*   Interfaces between EMI filter and DC‑link  
*   **Outputs:** `DC+`, `DC−`

### 2.3 DC‑Link Capacitor Network
*   **220 µF total electrolytic capacitance** (10 × 22 µF / 500 V)
*   **10 µF film capacitance** (5 × 2 µF)
*   **Provides:**
    *   Energy storage  
    *   Ripple reduction  
    *   High‑frequency current support  
    *   Low ESR/ESL through parallelization  

### 2.4 Inverter Power Stage (future section)
*   SiC MOSFET half‑bridge legs  
*   Gate drivers  
*   Current/voltage sensing  
*   PWM control interface  

---

## 3. Electrical Specifications


| Parameter | Value |
| :--- | :--- |
| **DC Bus Voltage (Nominal)** | 400 VDC |
| **Capacitor Voltage Rating** | 500 V |
| **Total DC‑Link Capacitance** | 220 µF electrolytic + 10 µF film |
| **Target Inrush Current** | 5 A |
| **Pre‑Charge Resistor** | 80 Ω |
| **Inverter Power Target** | ~1 kW (designed for up to ~2.5 A continuous) |
| **Switching Devices** | SiC MOSFETs (C3M0025065K family) |
| **Gate Driver** | UCC21710 (isolated) |
| **Switching Frequency** | TBD (likely 20–40 kHz) |

---

## 4. Development Tools & Equipment

### 4.1 Test & Measurement
*   **Siglent SDS814X HD‑OB**  
    *   100 MHz, 4‑channel oscilloscope  
    *   High‑resolution mode for power electronics  
*   **Siglent SDG1022X‑Plus**  
    *   25 MHz dual‑channel function generator  
    *   Used for gate‑drive testing and signal injection  
*   **Differential probes** (planned)  
    *   For safe high‑side switching waveform measurement  

### 4.2 Design Tools
*   **KiCad 9** for schematic capture and PCB layout  
*   **draw.io** for high‑level block diagrams  
*   **GitHub** for version control and documentation  

---

## 5. Design Philosophy

This inverter is built around:

*   **Modularity:** Each subsystem lives on its own schematic sheet with clear I/O pins.
*   **Scalability:** The architecture supports future upgrades (higher power, more legs, different control strategies).
*   **Low Inductance:** DC‑link and power stage layout emphasize short loops and parallel capacitance.
*   **Safety & Bring‑Up Discipline:** Pre‑charge, measurement equipment, and staged testing are built into the workflow.

---

## 6. Current Status

- [x] EMI filter: **Complete**  
- [x] Pre‑charge topology: **Defined**  
- [x] DC‑link architecture: **Defined (220 µF + 10 µF film)**  
- [x] KiCad hierarchy: **Clean and modular**  
- [ ] **Next steps:**  
    - Add pre‑charge components to schematic  
    - Add DC‑link capacitor bank  
    - Begin inverter leg schematic  

---

## 7. Next Steps

1. Build the **Pre‑Charge_Inrush_Limiting** sheet  
2. Add the **DC‑link capacitor network**  
3. Start the **inverter half‑bridge leg** schematic  
4. Integrate gate drivers and isolated supplies  
5. Plan the bring‑up procedure  

---

## 8. System Block Diagram (draw.io export)

> [!TIP]
> **Placeholder:** Insert exported PNG/SVG from your draw.io high‑level block diagram here.  
> `![Block Diagram](path/to/image.png)`

**Recommended export settings:**  
- Resolution: 2×  
- Transparent background  
- Include grid OFF  
- Include page border OFF  

`EMI Filter` → `Pre-Charge` → `DC-Link` → `Inverter Legs`