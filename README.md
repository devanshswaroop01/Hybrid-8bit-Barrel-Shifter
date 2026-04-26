# 🔄 8-Bit Hybrid Ring Barrel Shifter

> **A high-performance, low-power 8-bit ring-based barrel shifter designed in 0.12 µm CMOS, evaluated across five logic topologies — culminating in an optimized hybrid architecture with ~11× worst-case delay improvement.**

---

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Logic Styles Implemented](#logic-styles-implemented)
- [Simulation Results](#simulation-results)
- [Key Achievements](#key-achievements)
- [Circuit Diagrams](#circuit-diagrams)
- [Technology Specs](#technology-specs)
- [Future Work](#future-work)
- [References](#references)

---

## 🧠 Overview

This project presents the design and comprehensive comparative analysis of an **8-bit ring-based barrel shifter** implemented using multiple CMOS logic styles. The work evaluates four foundational topologies and proposes an **optimized 3-stage hybrid architecture** that achieves the best speed–power–robustness trade-off among all designs.

**Simulation Environment:** Transient SPICE simulations  
**Technology Node:** 0.12 µm CMOS  
**Supply Voltage:** V<sub>DD</sub> = 1.2 V  
**Metrics Evaluated:** Propagation delay · Glitch magnitude · Voltage integrity · Power consumption  

---

## 🏗️ Architecture

The 8-bit shifter uses a **logarithmic 3-stage structure**, enabling **O(log N)** propagation depth:

Input → Stage 1 (Shift ×1) → Stage 2 (Shift ×2) → Stage 3 (Shift ×4) → Output

| Stage | Operation | Logic |
|------|----------|------|
| Stage 1 | Shift by 1 | PTL + Level Restorer |
| Stage 2 | Shift by 2 | Transmission Gate + Buffer |
| Stage 3 | Shift by 4 | Static CMOS |


The **ring (circular)** configuration routes bits shifted out of MSB back to LSB, enabling both left-rotate and right-rotate operations via select-signal inversion.

---

## ⚙️ Logic Styles Implemented

Five distinct transistor-level designs were implemented and benchmarked:

---

### 1. Static CMOS

- ✅ Full rail-to-rail voltage swing  
- ✅ High noise immunity and robustness  
- ❌ High propagation delay (~1.8 ns worst case)  
- ❌ Severe glitching (>2 ns) due to multi-path race conditions  

---

### 2. Transmission Gate (TG + Inverter)

- ✅ Eliminates V<sub>th</sub> loss via complementary NMOS + PMOS pair  
- ✅ Full voltage swing  
- ⚠️ Moderate delay (~1.3–2 ns)  
- ❌ Sensitive to control signal skew  

---

### 3. PTL with Level Restorer (PTL + LR)

- ✅ Lowest power (~28 µW)  
- ✅ Fastest switching (~300–700 ps)  
- ⚠️ Requires PMOS level restorer for full swing  
- ❌ Reduced drive strength  

---

### 4. Initial Hybrid (PTL → TG → CMOS)

- ✅ Best-case delay: ~234 ps  
- ❌ Worst-case delay: ~6.4 ns (severe inter-stage impedance mismatch)  
- ❌ Unpredictable timing behavior  

---

### 5. ✅ Proposed Optimized Hybrid (Final Design)

| Stage   | Implementation                  | Role                                   |
|--------|--------------------------------|----------------------------------------|
| Stage 1 | PTL + Level Restorer          | Fast initial switching                 |
| Stage 2 | TG + Buffered control signals | Signal stabilization & full swing      |
| Stage 3 | Static CMOS                   | Strong output drive, rail-to-rail      |

**Key optimizations applied:**

- Buffered select signals (S0, S1, S2) to eliminate control-path skew  
- Matched inter-stage impedance to prevent delay spikes  
- Balanced fanout loading across all three stages  
- Reduced parasitic capacitance at internal nodes  

---

## 📊 Simulation Results

### ⚡ Propagation Delay

| Design | Best Case | Worst Case |
|--------|----------|-----------|
| Static CMOS | ~600 ps | ~1.8 ns |
| TG + Inverter | ~500 ps | ~1.3–2 ns |
| PTL + LR | ~300 ps | ~700 ps |
| Initial Hybrid | ~234 ps | ~6.4 ns ❌ |
| **Optimized Hybrid** | **~150 ps** | **~570 ps** ✅ |

✔ ~11× worst-case delay improvement over initial hybrid  

---

### 📉 Glitch Analysis

| Design | Max Glitch Width |
|--------|------------------|
| Static CMOS | >2 ns |
| TG + Inverter | >2 ns |
| PTL + LR | ~400 ps |
| Initial Hybrid | ~800 ps |
| **Optimized Hybrid** | **<600 ps** ✅ |

✔ ~70–80% glitch reduction compared to CMOS/TG designs  

---

### 🔋 Power Consumption

| Design | Power |
|--------|------|
| Static CMOS | ~1.6–1.7 mW |
| TG + Inverter | ~0.7–0.8 mW |
| PTL + LR | ~28 µW |
| Initial Hybrid | ~1.69 mW |
| **Optimized Hybrid** | **~0.71 mW** ✅ |

✔ ~58% power reduction from initial hybrid  

---

### 🔒 Voltage Integrity

| Design | Output Swing |
|--------|-------------|
| Static CMOS | Full |
| TG + Inverter | Full |
| PTL + LR | Degraded (V<sub>th</sub> loss) |
| Initial Hybrid | Restored (partial) |
| **Optimized Hybrid** | **Full (Strong)** ✅ |

---

## 🏆 Key Achievements

## 🏆 Optimized Hybrid Results

| Metric | Result |
|--------|--------|
| ⚡ Worst-case delay | ~570 ps (~11× faster) |
| 🔋 Power consumption | ~0.71 mW (~58% lower) |
| 📉 Glitch width | <600 ps |
| 🔒 Voltage swing | Full rail-to-rail |
| 🏗️ Signal integrity | Strong (no V<sub>th</sub> loss) |
---

## 📂 Circuit Diagrams

📁 circuits/

├── 01_static_cmos/

├── 02_transmission_gate/

├── 03_ptl_level_restorer/

├── 04_initial_hybrid/

└── 05_optimized_hybrid/


---

## ⚙️ Technology Specs

| Parameter | Value |
|----------|------|
| Technology | 0.12 µm CMOS |
| VDD | 1.2 V |
| Architecture | 3-stage logarithmic |
| Data width | 8-bit |
| Shift stages | ×1, ×2, ×4 |
| Simulator | SPICE |
| Measurement | 50% V<sub>DD</sub> crossing |

---

## 🚀 Future Work

- Scale to 16-bit and 32-bit designs  
- Post-layout parasitic extraction (PEX)  
- Implementation in advanced nodes (65 nm / 45 nm)  
- ALU datapath integration  
- Multi-V<sub>t</sub> power optimization  

---

## 📚 References

[1] Weste & Harris – CMOS VLSI Design  
[2] Rabaey – Digital IC Design  
[3] Kang & Leblebici – CMOS Circuits  
[4] Roy et al. – Leakage Reduction  
[5] Chandrakasan – Low Power CMOS  
