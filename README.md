# 4-Way Traffic Light Controller — FATEK PLC (WinProLadder)

A ladder-logic implementation of a **4-way signalized intersection** built with **WinProLadder** for **FATEK PLCs**.  
The intersection is split into two approaches: **North–South (NS)** and **West–East (WE)**. Vehicles go **straight only** (no turns). **Pedestrians** cross **in the same (parallel) direction** as vehicle flow. Start/Stop switches are provided.

> Project file: `TrafficLight1.pdw` (WinProLadder)

---

## ✨ Features

- Two coordinated phases: **NS phase** ↔ **WE phase**
- **Vehicle signals:** Red (15 s), Yellow (3 s), Green (15 s)
- **Pedestrian signals:** Red (18 s), Green (18 s) — aligned with the active vehicle phase
- **Mutual exclusion** between NS and WE greens (interlock)
- **Start/Stop** control for safe startup and immediate halt
- Structured timers and state coils for clarity & extensibility

---

## 🧭 Control Logic Overview

### Phasing & Right-of-Way
- **Phase A (NS runs):**
  - NS: **Green 15 s → Yellow 3 s → Red**
  - WE: **Red 18 s**
  - Pedestrians (NS crosswalks parallel to NS traffic): **Green 18 s**  
    (begins with NS Green and ends with NS Yellow)
- **Phase B (WE runs):**
  - WE: **Green 15 s → Yellow 3 s → Red**
  - NS: **Red 18 s**
  - Pedestrians (WE crosswalks parallel to WE traffic): **Green 18 s**

> Each complete vehicle phase is **18 s** (15 + 3). Pedestrian timing (**18 s**) matches the parallel vehicle phase.

### Sequence (one full cycle)
1. **NS Green (15 s)** + **NS Ped Green (15 s → continues through Yellow)**
2. **NS Yellow (3 s)** + **NS Ped Green (remaining 3 s)**  
   (WE stays Red the whole 18 s)
3. **WE Green (15 s)** + **WE Ped Green (15 s → continues through Yellow)**
4. **WE Yellow (3 s)** + **WE Ped Green (remaining 3 s)**  
   (NS stays Red the whole 18 s)

> At any time: **NS Green/Yellow** implies **WE Red**, and **WE Green/Yellow** implies **NS Red**.

---

## ⏱️ Timing Summary

| Signal Group          | State  | Duration |
|-----------------------|--------|----------|
| Vehicles (NS or WE)   | Green  | 15 s     |
| Vehicles (NS or WE)   | Yellow | 3 s      |
| Vehicles (NS or WE)   | Red    | 15 s     |
| Pedestrians (parallel)| Green  | 18 s     |
| Pedestrians (cross)   | Red    | 18 s     |

> Pedestrian green is synchronous with the active vehicle approach (parallel crossing). Cross-traffic pedestrians remain Red.



