---
title: "Capella MBSE Mini-Project — Pressurized Crew Pod System (PCPS)"
excerpt_separator: "<!--more-->"
categories:
  - Portfolio
  - MBSE
  - Systems Engineering
tags:
  - Capella
  - Arcadia
  - Spacecraft Systems
  - ECLSS
  - Model-Based Systems Engineering
---

## Overview
An MBSE mini-project built in **Capella (Arcadia method)** to demonstrate **end-to-end traceability** from operational need → system behavior → logical architecture.

The modeled **Pressurized Crew Pod System (PCPS)** monitors and regulates cabin pressure across **nominal, degraded, and emergency** conditions while interfacing with **Crew, Avionics, ECLSS, Power, Structure/Pressure Vessel, Ground Support**, and the **External Atmosphere**.

The focus is on **behavioral correctness, interface clarity, and functional allocation**, rather than detailed physical design.

<!--more-->

---

## 1. Goal & Scope

**Goal:**  
Demonstrate structured progression through the Arcadia layers:
**Operational Analysis → System Analysis → Logical Architecture**, maintaining traceability between stakeholder intent, system functions, and logical components.

**Scope (PCPS responsibilities):**
- Monitor cabin and ambient pressure
- Detect abnormal conditions (leak, over-pressure, sensor/power faults)
- Regulate pressure using inflow valves and vent path
- Support **Auto / Manual / Emergency** modes
- Exchange commands and telemetry with avionics and Ground Support
- Request make-up air from ECLSS
- Interface with power and structure for health and integrity reporting

---

## 2. Operational Analysis (OA)

This layer describes the **mission environment and stakeholder behavior**, without assuming any system implementation. The focus is on **who does what**, not how it is built.

### Operational Actors
- **Crew:** monitors pressure, provides setpoints, issues manual override
- **Avionics:** routes telemetry and commands, provides vehicle/mission state
- **ECLSS:** provides make-up air and responds to flow requests
- **Power System:** supplies electrical power and reports power health
- **Structure / Pressure Vessel:** contains cabin atmosphere and reports leak/integrity signals
- **External Atmosphere:** provides ambient pressure reference and leakage disturbances
- **Ground Support Equipment:** provides preflight and servicing commands

---

### 2a. OAB — Operational Architecture Blank
![OAB]({{ site.baseurl }}/assets/images/%5BOAB%5D%20Operational%20Entities.png)

**What it shows:**  
A high-level map of the **operational entities** involved in maintaining safe cabin pressure and how they conceptually relate to the PCPS mission.

**Why it exists:**  
This diagram defines **system scope and ownership**. It answers:  
> What is part of the operational environment, and what is external to the system of interest?

---

### 2b. OAIB — Operational Activity Interaction Diagram
![OAIB]({{ site.baseurl }}/assets/images/%5BOAIB%5D%20Root%20Operational%20Activity.png)

**What it shows:**  
The **operational activities** performed by entities and the **interactions** between those activities.

**Why it exists:**  
This view captures **behavioral collaboration** between stakeholders and systems before any system design is assumed.

---

### 2c. OCB — Operational Capability Diagram
![OCB]({{ site.baseurl }}/assets/images/%5BOCB%5D%20Operational%20Capabilities.png)

**What it shows:**  
The set of **operational capabilities** required to achieve the mission goal, such as:
- Maintain safe cabin pressure
- Respond to leaks and over-pressure
- Support crew control and ground servicing

**Why it exists:**  
This diagram links **stakeholder needs to operational behavior** and provides justification for why system functions must exist later in the model.

---

### 2d. OEBD — Operational Entity Breakdown Diagram
![OEBD]({{ site.baseurl }}/assets/images/%5BOEBD%5D%20Operational%20Entities.png)

**What it shows:**  
A structured **decomposition of operational entities** into their major elements.

**Why it exists:**  
This clarifies **responsibility boundaries** and supports allocation of operational activities to the correct actors.

---

### 2e. OES — Operational Entity Scenario
![OES]({{ site.baseurl }}/assets/images/%5BOES%5D%20Maintain%20Safe%20Cabin%20Pressure.png)

**What it shows:**  
A **time-ordered scenario** for maintaining safe cabin pressure.

**Why it exists:**  
This validates that the defined entities and activities can collectively achieve the mission.

---

## 3. System Analysis (SA)

This layer defines the **system of interest (PCPS)**, its **functions**, and its **interfaces** with external actors.

---

### 3a. CSA — Contextual System Actors
![CSA]({{ site.baseurl }}/assets/images/%5BCSA%5D%20System.png)

**What it shows:**  
The **PCPS system boundary** and the external **actors** that interact with it.

**Why it exists:**  
This is the **integration framing diagram**. It establishes:
- What is inside the system of interest
- What remains external
- Where interfaces must exist

---

### 3b. SAB — System Architecture Blank
![SAB]({{ site.baseurl }}/assets/images/%5BSAB%5D%20Structure.png)

**What it shows:**  
The **allocation of system functions** to either:
- The **PCPS system**, or
- External **actors** (e.g., Crew, Avionics, ECLSS)

Functions are connected by **functional exchanges** representing formal data, command, or physical flows.

**Why it exists:**  
This diagram transforms operational behavior into engineering requirements. Every arrow implies an interface that must be implemented, tested, and verified.

---

### 3c. System Functional Behavior
The PCPS is modeled as a **closed-loop pressure control system**:

1. **Acquire Cabin & Ambient Pressure**  
2. **Filter & Validate Sensor Data**  
3. **Compute Pressure Error** (vs. setpoint)  
4. **Determine Control Mode** (Auto / Manual / Emergency)  
5. **Generate Control Command (Valve Position)**  
6. **Regulate Air Inflow** (valves + ECLSS make-up air request)  
7. **Vent Excess Pressure** (over-pressure path)  
8. **Generate Telemetry / Log Health**

**Emergency path:**  
Detect leak or over-pressure → Execute emergency protection (isolate, safe vent, alarms) → Generate telemetry and alerts.

---

### 3d. SDFB — System Functional Dataflow Diagram
![SDFB]({{ site.baseurl }}/assets/images/%5BSDFB%5D%20Root%20System%20Function.png)

**What it shows:**  
The **data and control flows** between system functions, including sensor signals, control logic, command generation, actuation, and telemetry feedback.

**Why it exists:**  
This ensures no function is disconnected and that every control path has a clear source and sink.

---

### 3e. Functional Chains — End-to-End Scenarios

**Nominal Regulation Chain:**  
Sense → Validate → Compute Error → Mode Logic → Command Valves → Regulate Airflow → Stabilize Cabin Pressure

**Over-Pressure Chain:**  
Detect Over-Pressure → Emergency Protection → Vent Path Actuation → Telemetry / Alerts

**Leak / Disturbance Chain:**  
Disturbance Input → Detection → Isolation / Load Relief → Reporting

**Why these exist:**  
Functional chains prove that the system architecture supports various mission scenarios, not just isolated functions.

---

## 4. Logical Architecture (LA)

This layer introduces **logical components** and allocates the **existing system functions** to them. A **structural realization** of functions is shown.

---

### 4a. LAB — Logical Architecture Blank
![LAB]({{ site.baseurl }}/assets/images/%5BLAB%5D%20Structure.png)

**What it shows:**  
A decomposition of the PCPS into major **logical components** with system functions allocated to each.

**Logical Components:**
- **Pressure Sensor Unit**  
  - Acquire Cabin Pressure  
  - Acquire Ambient Pressure  
  - Filter & Validate Sensor Data  
- **Control Logic Unit**  
  - Compute Pressure Error  
  - Determine Control Mode  
  - Generate Control Command  
  - Detect Leak or Over-Pressure  
  - Execute Emergency Protection  
- **Valve Actuation Unit**  
  - Regulate Air Inflow  
  - Vent Excess Pressure  
- **Power / Interface Unit**  
  - Power Interface  
  - Log Power / Sensor Health  
  - Report Power Health  

**Why it exists:**  
This diagram bridges **functional behavior** and **physical design**. It defines where functions “live” logically before hardware/software decisions are made.

---

### 4b. LCBD — Logical Component Breakdown Diagram
![LCBD]({{ site.baseurl }}/assets/images/%5BLCBD%5D%20Structure.png)

**What it shows:**  
A hierarchical breakdown of logical components forming the PCPS.

**Why it exists:**  
This potentially provides a structure for:
- Future hardware/software partitioning
- Redundancy modeling
- Physical architecture and interface design

---

## 5. What This Project Demonstrates

- System boundary and interface definition
- Closed-loop control modeling with **mode logic and emergency behavior**
- Traceability from **operational need → system function → logical component**
- Use of **functional chains** and **scenarios** to validate architecture completeness

---
