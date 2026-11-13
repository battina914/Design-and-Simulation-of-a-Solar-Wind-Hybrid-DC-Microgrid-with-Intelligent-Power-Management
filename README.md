# Hybrid Renewable Energy Microgrid Simulation

Simulink model of a 6.6 kW hybrid solar-wind DC microgrid featuring P&O MPPT, bidirectional battery management, and an SPWM inverter.

## 📌 Project Overview

This repository demonstrates the system architecture and control strategy for an isolated hybrid AC/DC microgrid. The system integrates solar photovoltaic and wind generation, regulated by a central 300V DC bus, to supply stable 230V, 50Hz AC power to industrial induction motor loads.

### System Architecture
<img width="1742" height="744" alt="Screenshot 2026-07-22 225118" src="https://github.com/user-attachments/assets/9eb2594e-fdff-44d8-8dec-41a42d88e190" />

---
### Core Subsystem Architectures

The project is modularly designed, allowing independent control of each generation and storage unit before integration at the DC link.

**1. Photovoltaic & MPPT Generation**
The solar subsystem implements a Buck Converter controlled by a Perturb & Observe (P&O) MPPT algorithm to extract maximum power under varying irradiance.

<img width="1467" height="742" alt="Screenshot 2026-07-22 231753" src="https://github.com/user-attachments/assets/74efad3b-6521-4ef0-a66d-d7050ef1e9c8" />

---

**2. Wind Power Plant**
The wind subsystem utilizes a turbine block coupled with a rectifier and converter stage to process the variable AC output into a usable DC feed.

<img width="1796" height="720" alt="Screenshot 2026-07-22 231834" src="https://github.com/user-attachments/assets/ed6ffcf3-1385-466d-93f7-a458640fc7fa" />

---

**3. Bidirectional Battery Management (DC Bus Regulation)**
A critical component of the microgrid. This subsystem uses a cascaded PI control strategy (outer voltage loop, inner current loop) to manage the charging and discharging of the battery via a synchronous buck-boost converter, effectively anchoring the DC bus at 300 V.

<img width="1322" height="732" alt="Screenshot 2026-07-22 231907" src="https://github.com/user-attachments/assets/7b51901c-da55-4b04-afd4-9643283d3d61" />

---

**4. SPWM Inverter & LC Filter**
To interface with standard industrial loads, a full-bridge inverter driven by Sinusoidal Pulse Width Modulation (SPWM) synthesizes the AC waveform, which is then smoothed by an LC filter.

<img width="1582" height="742" alt="Screenshot 2026-07-22 231932" src="https://github.com/user-attachments/assets/ad81a7a6-52b4-4b1e-bb90-246af47d1b08" />

---
## ⚙️ Key Features
* **DC Bus Regulation:** Cascaded PI controllers maintain a strict 300 V DC bus under dynamic environmental conditions.
* **Energy Management:** A bidirectional DC-DC converter automatically transitions a 48 V Lead-Acid battery between charging (buck) and discharging (boost) modes.
* **Maximum Power Point Tracking:** Implementation of the Perturb & Observe (P&O) algorithm to maximize solar PV efficiency.
* **Industrial Load Support:** Capable of supplying robust 230 V, 50 Hz AC power to dynamic loads, including handling induction motor inrush currents.

## 📊 Simulation Results

### DC Bus Voltage Regulation
The graph below demonstrates the cascaded PI controller successfully regulating the DC bus voltage. After the initial system startup transient, the bus rapidly stabilizes and maintains a steady 300 V, providing a reliable DC link for the SPWM inverter.

<img width="1869" height="937" alt="Screenshot 2026-07-01 121248" src="https://github.com/user-attachments/assets/ddf52fc0-0c44-49f4-afec-b9964d304766" />

---

### Bidirectional Battery Management
Simulation results indicate that the energy storage system successfully responds to power flow variations. The scope output below demonstrates the battery transitioning from a discharging state to a charging state. This is evidenced by the inversion of the battery current and the subsequent recovery of the State of Charge (SOC) curve.

<img width="1915" height="953" alt="Screenshot 2026-07-22 230805" src="https://github.com/user-attachments/assets/0a3da7e1-dd1a-4f28-9504-46cc9c1ea154" />

### Induction Motor Dynamic Response
The proposed inverter and filter setup was evaluated for its ability to supply industrial loads. The stator and rotor measurements illustrate the initial transient inrush current during the motor startup phase, which safely settles into steady-state AC operation without destabilizing the upstream DC bus.

<img width="1913" height="958" alt="Screenshot 2026-07-22 230833" src="https://github.com/user-attachments/assets/e46f116a-3f92-467b-b6c1-c10314749e32" />

---

## 🔄 Power Flow & Control Strategy

The core objective of the simulated microgrid is to maintain a regulated 300 V DC bus under varying generation and load conditions. The energy management logic is dictated by the cascaded PI controllers in the bidirectional converter. The simulation evaluates three primary operating conditions:

* **Excess Generation (Renewable Power > Load Demand):** 
  When solar and wind generation exceeds the load requirement, the DC bus voltage tends to rise above the 300 V setpoint. The outer voltage PI controller detects this positive error and commands the inner current loop to operate the bidirectional converter in **buck mode**. This absorbs the excess energy, charging the 48 V battery and stabilizing the bus.

* **Deficit Generation (Renewable Power < Load Demand):** 
  When environmental conditions cause a drop in renewable generation, or a heavy load (e.g., motor startup) is applied, the DC bus voltage tends to sag. The controller detects the negative error and operates the converter in **boost mode**. The battery discharges to supply the required deficit power, restoring the 300 V bus.

* **Balanced Generation (Renewable Power = Load Demand):** 
  Under conditions where generation exactly matches the load demand, the battery remains in a standby state. The controller continuously monitors the DC link, ready to dynamically route power upon the next transient event.
  
---
