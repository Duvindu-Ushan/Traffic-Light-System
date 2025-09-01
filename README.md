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

## 🧩 Suggested I/O Mapping (adjust to your wiring)

> Replace with your actual addresses if different.

**Inputs**
- `X0` — **START** (momentary or maintained)
- `X1` — **STOP** *(prefer NC in field for fail-safe)*

**Outputs — Vehicles**
- `Y0` — **NS Red**
- `Y1` — **NS Yellow**
- `Y2` — **NS Green**
- `Y3` — **WE Red**
- `Y4` — **WE Yellow**
- `Y5` — **WE Green**

**Outputs — Pedestrians**
- `Y6`  — **NS Ped Red**
- `Y7`  — **NS Ped Green**
- `Y10` — **WE Ped Red**
- `Y11` — **WE Ped Green**

---

## 🛠 How to Run

1. **Open the project** in **WinProLadder**  
   *File → Open →* `TrafficLight1.pdw`

2. **Simulate** (recommended first)  
   - Use the built-in simulator  
   - Toggle `X0` (START) to begin, `X1` (STOP) to halt (forces all-red)

3. **Deploy to PLC (optional)**  
   - Connect to your FATEK PLC → set COM parameters → **Download**  
   - Verify I/O mapping and lamp wiring before going live

---

## 🔒 Safety & Interlocks

- NS and WE **greens are never on together** (hard interlocks)  
- Pedestrian greens are **only parallel** to the active vehicle direction  
- **STOP** immediately:
  - Resets timers  
  - Forces **all-red** for vehicles and pedestrians  
  - Returns to a deterministic initial phase on **START**

*(Optionally add a 1–2 s all-red clearance timer between phases if your field hardware requires it.)*

---

## 🧱 Implementation Notes

- Timers (`TMR`) set: **15 s** (vehicle green), **3 s** (vehicle yellow) → **18 s** phase
- Phase coils (e.g., `M_NS_RUN`, `M_WE_RUN`) gate both vehicle and pedestrian outputs
- One-shot on **START** ensures clean initialization
- Simple two-state controller (NS ↔ WE) for clarity and maintainability

---

## 🔧 Customization

- **Change durations:** update preset values of phase timers (15 s / 3 s)
- **Initial phase:** switch startup coil from NS to WE if desired
- **Add clearance:** insert an all-red timer between phases
- **Extend logic:** add call/confirm pushbuttons, audible ped beeper, flashing intervals, countdowns

---

## 📈 Roadmap (Future Development)

- **CCTV integration** for remote monitoring
- **Inductive loop detectors** for demand-based phase extension
- **Axle/weight sensors** for heavy-vehicle priority
- **Lamp fault detection** (burn-out sensing) and watchdogs
- **Communication** (SCADA/Modbus/TCP) and event logging

---
