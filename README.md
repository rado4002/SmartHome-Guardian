# SmartHome Guardian

**A Web-Based Smart Home Monitoring Platform with Explainable Predictive Battery Health Alerts**  
Tailored for urban homeowners in emerging markets (e.g., Democratic Republic of Congo)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Project Stage: Inception Complete](https://img.shields.io/badge/Stage-Inception%20Complete-blue)](https://github.com/YOUR_USERNAME/SmartHome-Guardian)

## Overview

SmartHome Guardian is a lightweight, responsive web-based SaaS platform designed to help urban homeowners in infrastructure-constrained environments (unstable electricity, intermittent connectivity, high security risks) monitor and manage essential smart home devices.

**Core Focus (MVP –  academic project)**  
- Monitor 3 simulated device types only:  
  - Battery-powered **Smart Lock**  
  - Battery-powered **Motion/Door Sensor**  
  - Mains-powered **Smart Plug** (energy reference)  
- Real-time status, remote lock/unlock, security alerts  
- Proactive, **explainable** predictive low-battery alerts (black-box module)  
- Role-based access: Home Owner, Administrator, Technician  
- Simulation-only (no real IoT hardware)

Built following **Craig Larman**'s iterative OOA/D approach (*Applying UML and Patterns*, 3rd ed.) and **Hassan Gomaa**'s COMET method (*Software Modeling and Design*).

## Project Goals & Scope

- Demonstrate requirements engineering, UML modeling, security design, and explainable AI boundaries  
- Address real-world DRC urban constraints (unstable power, security risks, limited maintenance)  
- Strict  MVP: 3 devices, black-box prediction, web-only, no cameras/voice/mobile/hardware

**In Scope** → See [Vision Document](docs/Vision-Document-v1.0.md)  
**Out of Scope** → Multi-home, solar integration, real IoT, advanced ML, native apps

## Repository Structure

```
SmartHome-Guardian/
│
├── docs/                 # Project documentation
│   ├── SRS/              # Software Requirements Specification
│   ├── Vision           # Vision and scope documents
│   ├── requirements/     # Requirements lists
│   └── traceability/     # Requirements traceability matrices
│
├── diagrams/             # System design diagrams
│   ├── use-case/         # Use Case diagrams
│   ├── class/            # Class diagrams
│   ├── sequence/         # Sequence diagrams
│   ├── activity/         # Activity diagrams
│   ├── state/            # State diagrams
│   └── deployment/       # Deployment architecture diagrams
│
├── assets/               # Supporting visual materials
│   ├── maps/
│   ├── icons/
│   └── screenshots/
│
└── prototype/            # Future simulation or prototype implementation (optional)
```
