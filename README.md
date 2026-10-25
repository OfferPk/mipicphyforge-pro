# MIPICPHYForge Pro (v1.0.0)

### MIPI C-PHY v2.1 3-Phase Symbol Codec Studio & 10-Point Normative Auditor

[![Standard](https://img.shields.io/badge/Standard-MIPI%20C--PHY%20v2.1-blue.svg)](https://www.mipi.org) [![Trio Rate](https://img.shields.io/badge/Trio%20Rate-Up%20to%203.5%20Gsps%20%2F%208.0%20Gbps-orange.svg)](#) [![Efficiency](https://img.shields.io/badge/Encoding-7%3A16%20Symbol%20Mapping-brightgreen.svg)](#) [![Normative Auditor](https://img.shields.io/badge/Auditor-10--Point%20Normative%20PASS-green.svg)](#) [![License](https://img.shields.io/badge/License-Commercial%20B2B-purple.svg)](#)

---

## 1\. Executive Summary

**MIPICPHYForge Pro** is an engineering-grade physical layer studio, 3-phase symbol codec, and normative verification auditor for the **MIPI Alliance C-PHY v2.1** specification.

High-resolution smartphone camera sensors (8K video, 108MP+ multi-camera zoom setups) and automotive Autonomous Driving Assistance Systems (ADAS 8MP surround vision) require massive transmission bandwidth (\$15\\dots 40,\\text{Gbit/s}\$) from image sensors to application processors. Traditional differential interfaces like MIPI D-PHY require 8 to 10 physical conductors, creating severe flex PCB routing congestion and electromagnetic emissions.

**MIPI C-PHY v2.1 revolutionizes mobile and automotive vision physical layers:**

1. **3-Wire Trio Architecture**: Encodes data across three conductors (\$A, B, C\$) simultaneously rather than traditional differential pairs.  
2. **7:16 Symbol Mapping**: Decomposes 16-bit binary words into 7 base-5 symbol transitions (\$5^7 \= 78,125 \> 65,536\$), delivering **2.286 bits per symbol transition** (nearly triple the efficiency of D-PHY per pin).  
3. **Embedded Clock Recovery**: Every symbol boundary guarantees a physical voltage state change on at least two conductors, completely **eliminating dedicated clock trace pairs**.  
4. **Massive Throughput**: Delivers up to **3.5 Gsps (8.0 Gbit/s per trio)**, achieving **24.0 Gbit/s across just 9 wires (3 trios)** with near-zero common-mode radiation.

MIPICPHYForge Pro provides an interactive single-page web workbench and a zero-dependency Python 3 CLI runner that enables image sensor digital designers, camera module validation leads, and SerDes test engineers to synthesize, dissect, and audit C-PHY physical transmissions.

---

## 2\. Key Capabilities & Features

- **3-Phase Symbol Codec**:  
  - Full implementation of the 6 wire states (\$+x, \-x, \+y, \-y, \+z, \-z\$) across lines \$A, B, C\$ (\$+3/4, 0, \-3/4\$ levels).  
  - 5 legal symbol transitions per Unit Interval (UI): Flip, Rotate CW Same/Invert, Rotate CCW Same/Invert.  
  - Guaranteed transition non-zero property on every UI for embedded clock recovery.  
- **7:16 Bijective Mapper & Demapper**:  
  - Base-5 decomposition mapping 16-bit binary words to 7 symbols.  
  - Full reverse demapper reconstructing words and bytes from symbol sequences.  
- **High-Speed Serialization Engine**:  
  - Symbol rates from 0.8 to 3.5 Gsps (\$0.285,\\text{ns}\$ to \$1.25,\\text{ns}\$ UI).  
  - Effective throughput up to 8.0 Gbit/s per trio.  
  - CSI-2/DSI-2 packet framing with CCITT CRC-16 (\$x^{16} \+ x^{12} \+ x^5 \+ 1\$).  
- **10-Point Normative Zero-Trust Compliance Auditor**:  
  - `RULE-CPHY-01`: Symbol Rate Boundary Conformance (\$0.8\\dots 3.5,\\text{Gsps}\$).  
  - `RULE-CPHY-02`: 3-Phase Wire State Exclusivity (\$+3/4, 0, \-3/4\$ conductor levels).  
  - `RULE-CPHY-03`: Transition Non-Zero Property (State must transition every UI).  
  - `RULE-CPHY-04`: 16-Bit to 7-Symbol Mapping Cardinality (\$5^7 \= 78,125 \> 65,536\$).  
  - `RULE-CPHY-05`: Unit Interval (UI) Timing Jitter Margin (\$\\ge 0.285,\\text{ns}\$).  
  - `RULE-CPHY-06`: CCITT-16 Packet Data Integrity.  
  - `RULE-CPHY-07`: Embedded Clock Recovery Eye Margin.  
  - `RULE-CPHY-08`: Differential Trio Voltage Amplitude Margins (\$V\_{\\text{OD}} \\approx 200,\\text{mV}\$).  
  - `RULE-CPHY-09`: Multi-Trio Deskew Synchronization Window (\$\\le 0.2,\\text{UI}\$).  
  - `RULE-CPHY-10`: Sensor Frame Serialization Latency Ceiling (\$\< 1000,\\text{ns}\$).

---

## 3\. Directory Layout

2026-10-12 — MIPICPHYForge Pro/

├── README.md                                \# Root product specification

├── build/

│   ├── cphy-cli.py                         \# Standalone Python CLI utility

│   └── cphy-pro-v1.0.0.zip                 \# Release distribution archive

├── documentation/

│   ├── ARCHITECTURE.md                     \# MIPI C-PHY v2.1 protocol specification

│   ├── USER\_GUIDE.md                       \# Web studio & CLI user manual

│   └── MONETIZATION\_AND\_MARKETING\_PLAYBOOK.md \# Commercialization & GTM strategy

├── screenshots/

│   └── cphy-studio-preview.svg             \# UI vector screenshot

└── source-code/

    ├── index.html                          \# Engineering web studio application

    ├── cphy.css                            \# Dark automotive theme stylesheet

    ├── cphy-engine.js                      \# Core 3-phase symbol codec & CRC engine

    ├── cphy-auditor.js                     \# 10-point normative compliance ruleset

    └── app.js                              \# UI interactive application controller

---

## 4\. Quickstart Guide

### Web Application

Open `source-code/index.html` in any modern web browser. No server or Internet connection required.

### CLI Usage (Python 3.8+)

\# Synthesize a C-PHY 3-phase transmission at 2.5 Gsps

python3 build/cphy-cli.py synth \--rate 2.5 \--data "0xC0,0xDE,0xC1,0xA0"

\# Demap 7 symbols to a 16-bit word

python3 build/cphy-cli.py demap \--symbols "0,1,2,3,4,1,2"

\# Run 10-point normative compliance audit

python3 build/cphy-cli.py audit \--rate 2.5 \--data "0xC0,0xDE,0xC1,0xA0"

### Running Automated Test Suite

python3 test\_cphy.py

Expected output:

\============================================================

MIPICPHYForge Pro \- MIPI C-PHY v2.1 Verification Suite

\============================================================

\[PASS\] TEST 1: 7:16 Bijective Symbol Mapping (0xC1A0 \-\> \[3, 3, 2, 1, 4, 0, 3\] \-\> 0xC1A0)

\[PASS\] TEST 2: CCITT CRC-16 Calculation (Result: 0xC350)

\[PASS\] TEST 3: 3-Phase Transmission Synthesis (21 Symbols, UI: 0.40 ns)

\[PASS\] TEST 4: 3.5 Gsps Peak Rate (8.00 Gbps Effective Throughput per Trio)

\[PASS\] TEST 5: State Transition Non-Zero Property (Guaranteed Phase Edge Verified)

\[PASS\] TEST 6: Normative Auditor \- Compliant Transmission (10/10 PASS)

\[PASS\] TEST 7: Fault Injection \- Corrupted CRC-16 Correctly Flagged

\[PASS\] TEST 8: Out-of-Bounds Symbol Rate (0.5 Gsps) Correctly Flagged

\[PASS\] TEST 9: Sensor Frame Serialization Latency (8.4 ns \< 1000 ns)

\[PASS\] TEST 10: CLI Execution & JSON Serialization

\============================================================

RESULTS: 10 Passed, 0 Failed (100.0% Pass Rate)

\============================================================

---

## 5\. Commercial Licensing & Target Market

- **Target Customers**: Image Sensor Semiconductor Companies (Sony, onsemi, Omnivision, Samsung System LSI), Mobile Application Processor Vendors (Qualcomm, MediaTek, Apple), Automotive ADAS Vision Tier-1s (Mobileye, Continental, Bosch, Magna), and Display Driver IC (DDIC) Designers.  
- **Pricing & Packaging**:  
  - **Single Engineer Desktop License**: \$1,490 / seat / year  
  - **Tier-1 Team Site License (Floating 10 seats)**: \$8,490 / year  
  - **OEM Enterprise Core (HIL Automation & Escrow)**: \$22,500 one-time perpetual license

---

*MIPICPHYForge Pro is developed and certified by the Autonomous Digital Product Factory.*