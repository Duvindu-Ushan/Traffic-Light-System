# 4-Way Traffic Light Controller — FATEK PLC (WinProLadder)

A ladder-logic implementation of a **4-way signalized intersection** built with **WinProLadder** for the **FATEK PLC series**.  
The controller runs **24/7 without human intervention** and splits the junction into two coordinated approaches: **North–South (NS)** and **West–East (WE)**. Vehicles travel **straight only** (no left/right turns). **Pedestrians** cross **in parallel** with the active vehicle direction. An **Emergency Start/Stop** is provided.

> Project file: `TrafficLight1.pdw` (WinProLadder)

---

## ✨ Features

- Two alternating phases: **NS phase** ↔ **WE phase**
- **Vehicle signals:** Red / Yellow / Green
- **Pedestrian signals:** Red / Green
- **Start/Stop** for emergency control and safe recovery
- **Mutual-exclusion interlocks** prevent conflicting greens
- Deterministic startup and safe all-red fail state
- Designed for **continuous 24/7 operation**
- **Future-ready** for sensors and monitoring (CCTV, inductive loops, axle/weight sensors, etc.)

---

## ⏱️ Signal Timings

### Vehicles (per active approach)
- **Green:** 15 s  
- **Yellow:** 3 s  
- **Red:** 15 s *(while the cross approach is active)*

### Pedestrians (parallel to active vehicles)
- **Green:** 18 s  
- **Red:** 18 s *(while the cross approach is active)*

> Each vehicle phase is **18 s** (15 + 3). Pedestrian **Green** aligns with the entire active vehicle phase (18 s). The cross approach remains **Red**.

---

## 🧭 Phase Sequence (One Full Cycle)

1. **NS Green (15 s)** + **NS Ped Green (18 s)**  
2. **NS Yellow (3 s)** + **NS Ped Green continues**  
3. Transition to **WE phase**  
4. **WE Green (15 s)** + **WE Ped Green (18 s)**  
5. **WE Yellow (3 s)** + **WE Ped Green continues**  
6. Transition back to **NS phase** → repeat

At any moment:
- **NS Green/Yellow ⇒ WE Red** (vehicles & pedestrians)  
- **WE Green/Yellow ⇒ NS Red** (vehicles & pedestrians)

---
