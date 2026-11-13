# Engineering Project Details: Hybrid DC Microgrid

##  Problem Statement
Integrating intermittent renewable energy sources (solar and wind) into a stable power supply is challenging due to environmental variability. Furthermore, supplying industrial loads, such as induction motors, requires a robust system capable of handling transient inrush currents without destabilizing the primary power bus.

##  Motivation
The motivation behind this project was to design and simulate a proof-of-concept isolated microgrid that could reliably manage energy flow between generation, storage, and consumption. The goal was to practically apply power electronics and control system theory to regulate a shared DC link.

##  Proposed Solution
The simulation implements a 6.6 kW hybrid microgrid architecture featuring:
* A central 300 V DC bus regulated by a cascaded PI controller.
* A bidirectional DC-DC converter managing a 48 V battery as an energy buffer.
* Perturb & Observe (P&O) MPPT algorithms to maximize solar energy extraction.
* An SPWM-driven full-bridge inverter to supply 230 V, 50 Hz AC power to dynamic motor loads.

## Key Learnings
During the development of this simulation, several critical engineering concepts were practically reinforced:
* **Control Loop Tuning:** Balancing the bandwidth of the outer voltage loop and inner current loop in the cascaded PI controller to achieve stability.
* **Power Electronics Routing:** Designing the synchronous buck-boost topology to handle bidirectional energy flow.
* **Transient Management:** Evaluating the impact of motor startup inrush currents on the upstream DC bus voltage and sizing the system to absorb these transients.

##  Project Limitations & Future Scope
As a simulation-based proof-of-concept, the current model has specific engineering limitations:
1. **Isolated Operation:** The microgrid operates exclusively in islanded mode. It currently lacks a grid-tie inverter and phase-locked loop (PLL) to synchronize with a utility grid for backup support or energy export.
2. **Local Control Only:** The system relies on local PI controllers for millisecond-level stability but lacks a supervisory Energy Management System (EMS) for long-term load prioritization or predictive scheduling.
3. **Single Point of Failure:** The battery bidirectional converter is exclusively responsible for regulating the DC bus. If the battery reaches its minimum state of charge or trips, an alternative DC bus regulation strategy would be required.
