---
title: "Capella MBSE Mini-Project: Pressurized Crew Pod System (PCPS)"
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

The modeled Pressurized Crew Pod System (PCPS) monitors and regulates cabin pressure across **nominal, degraded, and emergency** conditions while interfacing with **Crew, Avionics, ECLSS, Power, Structure/Pressure Vessel, Ground Support**, and the **External Atmosphere**.

The focus is on behavioral correctness, interface clarity, and functional allocation, rather than detailed physical design.

<!--more-->

---

## 1. Goal & Scope

Goal: 
Demonstrate structured progression through the Arcadia layers:
**Operational Analysis → System Analysis → Logical Architecture**, maintaining traceability between stakeholder intent, system functions, and logical components.

Scope (PCPS responsibilities):
- Monitor cabin and ambient pressure
- Detect abnormal conditions
- Regulate pressure using inflow valves and vent path
- Support Auto & Manual modes
- Exchange commands and telemetry with avionics and Ground Support
- Request make-up air from ECLSS
- Interface with power and structure for reporting

---

## 2. Operational Analysis (OA)

This layer describes the mission environment and stakeholder behavior, **without assuming any system** implementation. The focus is on **who does what**, not how it is built.

### Operational Actors
- **Pressure control system:** regulates air supply and monitors cabin pressure
- **Crew:** issues manual override and provides setpoints (saved for later)
- **Avionics:** routes telemetry, provides vehicle state (saved for later)
- **ECLSS:** provides make-up air and responds to flow requests (saved for later)
- **Power System:** supplies electrical power and reports power health (saved for later)
- **Structure / Pressure Vessel:** contains cabin atmosphere and reports leakage (saved for later)
- External Atmosphere (saved for later): exchanges air and provides ambient pressure reference
- Ground Support Equipment (saved for later): provides preflight and servicing commands

---

### 2a. OAB (Operational Architecture Blank)
![OAB]({{ site.baseurl }}/assets/images/%5BOAB%5D%20Operational%20Entities.png)

This is a high-level map of the **operational entities** involved in maintaining safe cabin pressure and how they conceptually relate to the mission.
This diagram defines **system scope and ownership**. It answers:  
> What is part of the operational environment, and what is external to the system of interest?

---

### 2b. OAIB (Operational Activity Interaction Diagram)
![OAIB]({{ site.baseurl }}/assets/images/%5BOAIB%5D%20Root%20Operational%20Activity.png)

The **operational activities** performed by entities and the **interactions** between those activities.
This view captures behavioral collaboration.

---

### 2c. OES (Operational Entity Scenario)
![OES]({{ site.baseurl }}/assets/images/%5BOES%5D%20Maintain%20Safe%20Cabin%20Pressure.png)

This shows time-ordered scenario for maintaining safe cabin pressure and that the defined entities and activities can collectively achieve the mission.

---

## 3. System Analysis (SA)

This layer defines the **system of interest**, its functions, and its interfaces with external actors.

---

### 3a. CSA (Contextual System Actors)
![CSA]({{ site.baseurl }}/assets/images/%5BCSA%5D%20System.png)

This shows the **PCPS system boundary** and the external **actors** that interact with it. It establishes:
- What is inside the system of interest
- What remains external
- Where interfaces exist

---

### 3b. SAB (System Architecture Blank)
![SAB]({{ site.baseurl }}/assets/images/%5BSAB%5D%20Structure.png)

This shows the **allocation of system functions** to the PCPS system or the external actors (e.g., Crew, Avionics, etc). Functions are connected by **functional exchanges** representing data, command, or physical flows.
This diagram transforms operational behavior into requirements. Every arrow implies an interface that must be implemented, tested, and verified.

---

### 3c. System Functional Behavior
The PCPS is modeled as a **closed-loop pressure control system**:

1. **Acquire Cabin & Ambient Pressure**  
2. **Filter & Validate Sensor Data**  
3. **Compute Pressure Error** (vs. setpoint)  
4. **Determine Control Mode** (Auto / Manual in case of emergency)  
5. **Generate Control Command (Valve Position)**  
6. **Regulate Air Inflow** (valves + ECLSS make-up air request)  
7. **Vent Excess Pressure** 
8. **Generate Telemetry / Log Health**

---

### 3d. SDFB (System Functional Dataflow Diagram)
![SDFB]({{ site.baseurl }}/assets/images/%5BSDFB%5D%20Root%20System%20Function.png)

**What it shows:**  
The **data and control flows** between system functions, including sensor signals, control logic, command generation, actuation, and telemetry feedback.

**Why it exists:**  
This ensures no function is disconnected and that every control path has a clear source and sink.

Functional Chains: 

**Nominal Regulation Chain:**  
Periodically Monitor → Compute Pressure Error → Command Valves → Regulate Airflow → Logging

**Leak Response Chain:**  
Detect Leak → Compute Pressure Error → Defaults to Auto Mode but Crew Can Override → Command Valves → Regulate Airflow → Logging

Functional chains prove that the system architecture supports various mission scenarios, not just isolated functions.

---

## 4. Logical Architecture (LA)

This layer introduces **logical components** and allocates the existing system functions to them. A structural realization of functions is shown.

---

### 4a. LAB — Logical Architecture Blank
![LAB]({{ site.baseurl }}/assets/images/%5BLAB%5D%20Structure.png)

This is a decomposition of the PCPS into major **logical components** with system functions allocated to each.

Logical Components:
- Pressure Sensor Unit  
- Control Logic Unit  
- Valve Actuation Unit  
- Power / Interface Unit  

This diagram defines where functions “live” logically before hardware/software decisions are made.

---

### 4b. LCBD — Logical Component Breakdown Diagram
![LCBD]({{ site.baseurl }}/assets/images/%5BLCBD%5D%20Structure.png)

A hierarchical breakdown of logical components forming the PCPS. It’s simple here but it potentially provides a structure for future hardware/software partitioning or physical architecture.

---

## 5. What This Project Demonstrates

- System boundary and interface definition
- Closed-loop control modeling with **mode logic and emergency behavior**
- Traceability from **operational need → system function → logical component**
- Use of **functional chains** and **scenarios** to validate architecture completeness

---
