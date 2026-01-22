---
title: "Capella MBSE Mini‑Project — Pressure Control System (PCPS)"
excerpt_separator: "<!--more-->"
categories:
  - Portfolio
  - MBSE
tags:
  - Capella
  - Arcadia
  - Systems Engineering
  - Spacecraft Systems
  - ECLSS
---

## Overview
A MBSE project built in **Capella (Arcadia)** to demonstrate **end‑to‑end traceability** from stakeholder need → operational interactions → functional behavior → logical architecture.

The modeled **Pressurized Crew Pod System (PCPS)** monitors and regulates cabin pressure across **nominal, degraded, and emergency** conditions while interfacing with **avionics, ECLSS, power, structure, ground support**, and the **external atmosphere**.

<!--more-->

---

### 1. Goal & Scope
**Goal:** Model a spacecraft cabin pressure control system with clear traceability from **operational need → system behavior → logical/physical architecture**.

**Scope (PCPS responsibilities):**
- Monitor cabin pressure and detect abnormal conditions
- Regulate pressure via **inflow valves** and **vent path**
- Support **Auto / Manual / Emergency** modes
- Exchange commands/telemetry with avionics and ground systems
- Request make‑up air from ECLSS and report power/health status

---

## 2. Operational Analysis
At the operational level, the environment is captured as interacting actors and external systems:
- **Crew:** selects pressure mode, provides setpoints, can manually override
- **Avionics:** hosts monitoring/state control functions and routes commands/telemetry
- **ECLSS:** provides make‑up air and responds to flow requests
- **Power System:** provides electrical power and receives health/power usage reporting
- **Structure/Pressure Vessel:** contains cabin pressure and supports leak reporting
- **External Atmosphere:** ambient pressure reference + disturbances (e.g., leakage flow)
- **Ground Support Equipment:** preflight/service commands

**Primary operational exchanges:** pressure data and alerts, mode commands, flow requests, electrical power, and telemetry.

### 2a. OAB (Operational Architecture Blank)
![OAB - ]({{ site.baseurl }}/assets/images/%5BOAB%5D%20Operational%20Entities.png)

### 2b. OAIB (Operational Activity Interaction Diagram)
The OAIB captures the operational environment decomposition, showing how the PCPS sits within the broader spacecraft operational context. This view is useful for communicating scope and ownership: what is part of the PCPS operational environment versus what remains external actors/systems.
![OAIB - ]({{ site.baseurl }}/assets/images/%5BOES%5D%20Root%20Operaetion%20activity.png)

### 2c. OCB (Operational Capability Diagram)
![OCB - ]({{ site.baseurl }}/assets/images/%5BOES%5D%20Operational%20Safe%20Capabilities.png)

### 2d. OEBD (Operational Entity Breakdown Diagram)
![OEBD - ]({{ site.baseurl }}/assets/images/%5BOES%5D%20Operational%20Safe%20Entities.png)

### 2e. OES (Operational Entity Scenario)
![OES - ]({{ site.baseurl }}/assets/images/%5BOES%5D%20Maintain%20Safe%20Cabin%20Pressure.png)

---

## 3. System Analysis

### 3a. CSA — Contextual System Actors**
The CSA provides a quick “at-a-glance” view of the PCPS system-of-interest and the external systems it depends on. It helps frame the integration problem by showing the main interacting neighbors—Avionics, Cabin Environment, Structure/Pressure Vessel, ECLSS, Power System, and Crew—before diving into detailed exchanges and functions.
![CSA - ]({{ site.baseurl }}/assets/images/%5BOAB%5D%20Operational%20Entities.png)

### 3b. SAB (System Architecture Diagram)
The **SAB** defines PCPS as the system of interest and makes interfaces explicit.

**Inputs:**
- Cabin/ambient pressure signals (raw + validated)
- Setpoint/mode commands (crew/ground/avionics)
- Structural/leak indications and disturbance inputs

**Outputs:**
- Valve position commands (inflow control)
- Flow requests to ECLSS (make‑up air)
- Alerts, telemetry, and emergency actions (isolate/safe/vent)

![SAB - ]({{ site.baseurl }}/assets/images/%5BSAB%5D%20Structure.png)

### 3c. Functional Dataflow — How pressure is controlled
The functional architecture models a closed‑loop control system:

1. **Acquire Cabin & Ambient Pressure**
2. **Filter & Validate Sensor Data** → calibrated pressure signals
3. **Compute Pressure Error** vs target setpoint
4. **Determine Control Mode** (Auto / Manual / Emergency)
5. **Generate Control Command (Valve Position)**
6. **Regulate Air Inflow** (actuate valves + request make‑up air from ECLSS)
7. **Vent Excess Pressure** on over‑pressure conditions
8. **Generate Telemetry/Status** + log power/sensor health

Emergency behavior is explicit: **detect leak/over‑pressure → emergency protection (isolate/safe/vent/alarms) → emergency event packet** for telemetry.

### 3d. Functional Chains (end‑to‑end coverage)
Key functional chains were highlighted to validate scenario coverage and interface completeness:

- **Nominal Pressure Regulation:** sense → validate → compute error → mode logic → command valves → regulate airflow → stabilize cabin pressure  
- **Over‑pressure Response:** detect over‑pressure → emergency protection → vent path actuation → alert/telemetry  
- **Leak/Disturbance Response:** disturbance/leak input → detection → protective action (isolate/load relief) → reporting  

### SDFB (System Functional Dataflow diagram)
![SDFB - ]({{ site.baseurl }}/assets/images/%5BSDFB%5D%20Root%20System%20Function.png)

---

## 4. Logical Architecture
### 4a. LAB (Logical Architecture Diagram)
Functions were allocated into a logical decomposition that separates sensing, control, and actuation:
- **Pressure Sensor Unit:** cabin/ambient acquisition + filtering/validation  
- **Control Logic Unit:** setpoint tracking, error computation, mode management, command generation  
- **Valve Actuation Unit:** inflow regulation + vent actuation  
- **Power Interface / Conditioning (modeled):** power interface + health/power usage logging  

This structure supports traceability from functions to logical components and provides a clean foundation for later physical allocation (HW/SW, redundancy, etc.).

![LAB - ]({{ site.baseurl }}/assets/images/%5BLAB%5D%20Structure.png)

### 4b. LCBD (Logical Component Breakdown Diagram)
The LCBD decomposes the Logical System into major logical building blocks:
- Pressure Sensor Unit
- Control Logic Unit
- Valve Actuation Unit
- Power Conditioning Unit

![LCBD - ]({{ site.baseurl }}/assets/images/%5BLAB%5D%20Structure.png)

---

## 5. What this project demonstrates
- Correct **Arcadia/Capella layering** from operational context to logical design  
- Explicit **system boundary and interfaces** (crew/avionics/ECLSS/power/structure/ground)  
- Closed‑loop pressure control with **mode logic** and **emergency protection**  
- Use of **functional chains** and **sequence scenarios** to validate end‑to‑end behavior and traceability  
