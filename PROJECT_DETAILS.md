# Engineering Project Details: Hybrid DC Microgrid

## Problem Statement
Harvesting energy from intermittent renewable sources such as solar photovoltaics and wind turbines presents significant grid-stability challenges due to unpredictable environmental fluctuations. Supplying dynamic industrial loads—notably induction motors—compounds this difficulty by introducing severe startup inrush currents that can destabilize the central power bus without adequate buffering and rapid control.

## Motivation
This project was conceived to model and simulate an autonomous, proof-of-concept isolated DC microgrid capable of orchestrating reliable energy distribution among generation assets, energy storage, and industrial loads. The objective was to implement and validate practical power electronics topologies and closed-loop control strategies designed to maintain tight regulation over a shared DC link.

## Proposed Solution
The simulation models a 6.6 kW hybrid DC microgrid infrastructure incorporating:
* **Central DC Bus Regulation:** A shared 300 V DC distribution bus maintained by a dual-loop cascaded PI controller.
* **Energy Storage Subsystem:** A bidirectional DC-DC converter interfacing a 48 V battery bank that acts as an active energy buffer.
* **Maximum Power Extraction:** A Perturb and Observe (P&O) MPPT tracking algorithm optimizing solar PV energy harvest under fluctuating conditions.
* **AC Inversion Stage:** A Sinusoidal Pulse-Width Modulation (SPWM) full-bridge inverter paired with an LC filter delivering stable 230 V, 50 Hz AC power to dynamic induction motor loads.

## Key Engineering Insights
Developing and analyzing this simulation highlighted several foundational power engineering principles:
* **Cascaded Control Loop Tuning:** Decoupling and tuning the bandwidths of the outer voltage regulation loop and the inner current loop to achieve fast transient response without sacrificing steady-state stability.
* **Bidirectional Power Conversion:** Implementing and controlling a synchronous buck-boost topology to govern bidirectional power transfer between the low-voltage battery (48 V) and the high-voltage bus (300 V).
* **Transient Load Management:** Assessing the destabilizing impact of high starting inrush currents from induction motors on upstream DC bus dynamics, and sizing storage and capacitive filtering to damp these disturbances.

## Limitations & Future Scope
As a simulation-based proof-of-concept, the current implementation has several recognized engineering constraints:
1. **Stand-Alone / Islanded Operation:** The microgrid operates exclusively off-grid. It lacks a grid-tied inverter and a Phase-Locked Loop (PLL) mechanism required for utility synchronization, import/export operations, or external grid backup.
2. **Decentralized Local Control:** Subsystems depend purely on millisecond-level local PI controllers. The model does not include a high-level Energy Management System (EMS) for state-of-charge optimization, predictive dispatch, or automated load prioritization.
3. **Single Point of Dependency for Bus Regulation:** The bidirectional battery converter alone bears the responsibility for stabilizing the DC bus voltage. If the battery bank trips or reaches its minimum depth of discharge, an alternative bus-forming mechanism would be necessary to keep the system operational.
