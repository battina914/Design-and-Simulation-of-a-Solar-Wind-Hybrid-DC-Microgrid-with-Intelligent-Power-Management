# Hybrid Renewable Energy Microgrid Simulation

Simulink simulation of an autonomous 6.6 kW solar-wind hybrid DC microgrid featuring Perturb & Observe (P&O) MPPT, bidirectional battery storage regulation, and an SPWM inverter stage.

## 📌 Project Overview

This repository presents the modeling, topology design, and control methodology for an isolated hybrid AC/DC microgrid. The system combines photovoltaic and wind generation into a common 300 V DC distribution bus, which feeds a single-phase 230 V, 50 Hz AC network designed to drive dynamic industrial induction motor loads.

### System Architecture
<img width="1742" height="744" alt="Screenshot 2026-07-22 225118" src="https://github.com/user-attachments/assets/9eb2594e-fdff-44d8-8dec-41a42d88e190" />

---

### Core Subsystem Architectures

The microgrid employs a modular structure, enabling decoupled control of each energy resource and storage asset before coupling onto the shared DC bus.

#### 1. Photovoltaic Generation & MPPT
The solar PV generation stage utilizes a DC-DC Buck Converter governed by a Perturb & Observe (P&O) Maximum Power Point Tracking (MPPT) algorithm, ensuring maximum power extraction across dynamic irradiance and temperature profiles.

<img width="1467" height="742" alt="Screenshot 2026-07-22 231753" src="https://github.com/user-attachments/assets/74efad3b-6521-4ef0-a66d-d7050ef1e9c8" />

---

#### 2. Wind Power Conversion
The wind energy conversion subsystem models a wind turbine coupled to an AC-DC rectification and conversion stage, transforming variable AC generation into a conditioned DC feed for the common bus.

<img width="1796" height="720" alt="Screenshot 2026-07-22 231834" src="https://github.com/user-attachments/assets/ed6ffcf3-1385-466d-93f7-a458640fc7fa" />

---

#### 3. Bidirectional Battery Management (DC Bus Regulation)
Serving as the primary voltage-forming element of the microgrid, this subsystem manages a 48 V Lead-Acid battery bank via a synchronous buck-boost bidirectional DC-DC converter. A cascaded dual-loop PI control architecture (an outer voltage regulation loop paired with a high-bandwidth inner current loop) actively stabilizes the DC link at 300 V through controlled charge and discharge cycles.

<img width="1322" height="732" alt="Screenshot 2026-07-22 231907" src="https://github.com/user-attachments/assets/7b51901c-da55-4b04-afd4-9643283d3d61" />

---

#### 4. SPWM Inverter & LC Filter
To supply standard industrial AC loads, a full-bridge DC-AC inverter driven by Sinusoidal Pulse Width Modulation (SPWM) synthesizes the fundamental AC waveform. An LC low-pass filter suppresses high-frequency switching harmonics, delivering a smooth sinusoidal voltage.

<img width="1582" height="742" alt="Screenshot 2026-07-22 231932" src="https://github.com/user-attachments/assets/ad81a7a6-52b4-4b1e-bb90-246af47d1b08" />

---

## ⚙️ Key Features
* **Rigorous DC Bus Stabilization:** Cascaded PI closed-loop control holds the central DC bus firmly at 300 V despite fluctuating generation and dynamic loading.
* **Automated Energy Buffering:** The bidirectional DC-DC converter seamlessly toggles a 48 V Lead-Acid battery between charging (buck mode) and discharging (boost mode).
* **Optimized Solar Harvesting:** Perturb & Observe (P&O) MPPT implementation tracks peak power from the photovoltaic array under varying weather conditions.
* **Resilient Industrial Load Delivery:** Synthesizes low-distortion 230 V, 50 Hz AC power, sustaining heavy induction motor startup transients without bus collapse.

---

## 📊 Simulation Results

### DC Bus Voltage Regulation
The simulation demonstrates rapid setpoint acquisition and tight voltage regulation under the cascaded PI controller. Following the initial startup transient, the bus quickly settles to a stable 300 V, establishing a stiff DC link for the downstream SPWM inverter.

<img width="1869" height="937" alt="Screenshot 2026-07-01 121248" src="https://github.com/user-attachments/assets/ddf52fc0-0c44-49f4-afec-b9964d304766" />

---

### Bidirectional Battery Management
The energy storage subsystem responds dynamically to power imbalances on the DC bus. The scope capture below highlights the transition of the battery from discharging to charging mode, confirmed by current reversal and the subsequent positive slope of the State of Charge (SOC) trajectory.

<img width="1915" height="953" alt="Screenshot 2026-07-22 230805" src="https://github.com/user-attachments/assets/0a3da7e1-dd1a-4f28-9504-46cc9c1ea154" />

---

### Induction Motor Dynamic Response
The inverter and output filter were subjected to dynamic motor starting conditions. Stator and rotor current profiles capture the severe startup inrush current, which smoothly damps down to nominal steady-state AC operation while the upstream DC link maintains steady regulation.

<img width="1913" height="958" alt="Screenshot 2026-07-22 230833" src="https://github.com/user-attachments/assets/e46f116a-3f92-467b-b6c1-c10314749e32" />

---

## 🔄 Power Flow & Control Strategy

The primary control objective is maintaining a constant 300 V DC bus across varying generation levels and load profiles. Real-time power balance is governed by the cascaded PI controllers of the bidirectional converter across three operating regimes:

* **Surplus Generation Mode (Renewable Power > Load Demand):** 
  When combined solar and wind generation exceeds total load demand, the DC bus voltage begins to rise above 300 V. The outer voltage controller senses this positive voltage error and directs the inner current loop to operate the converter in **buck mode**, directing surplus power into the 48 V battery bank to absorb excess energy and clamp the bus voltage.

* **Deficit Generation Mode (Renewable Power < Load Demand):** 
  When renewable generation drops or a high-draw load event occurs (such as motor startup), the DC bus voltage tends to sag below 300 V. The controller detects this negative voltage error and transitions the converter to **boost mode**, drawing power from the 48 V battery bank to compensate for the generation deficit and restore the 300 V bus.

* **Equilibrium Mode (Renewable Power = Load Demand):** 
  When total generation matches power consumption, net current into the battery reaches zero and the unit remains in standby. The control loops continuously supervise the bus, ready to transition immediately when transient disturbances occur.

---
