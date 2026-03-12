# SmartHome Guardian – Vision Document v1.0

**Date:** March 2025  
**Version:** 1.0  
**Project Phase:** Inception (Week 1 – Foundation & Scope Lock)

## 1. Introduction / Purpose

This document presents the high-level vision and strategic direction for the **SmartHome Guardian** project.  

Its purpose is to define:  
- the business motivation  
- the problem context  
- the scope boundaries  

for the system’s **Minimum Viable Product (MVP)**.

The document outlines the core challenges faced by urban homeowners in the **Democratic Republic of Congo (DRC)** and similar emerging markets, and describes how SmartHome Guardian aims to address these challenges through an accessible, web-based monitoring platform.

It also establishes the major capabilities, key assumptions, and constraints that will guide the design and development of the MVP during the 10-week project timeline.

## 2. Problem Statement

Urban and semi-urban regions of the **Democratic Republic of Congo** — including cities such as **Kinshasa**, **Goma**, and **Bukavu** — face several structural challenges that affect household security and energy reliability.

### Major challenges

- **Unstable electricity supply**  
  Power outages and voltage fluctuations are frequent in many areas, forcing households to rely heavily on battery-powered and solar-supported devices to maintain essential functionality. Security devices, smart locks, and sensors often depend on battery systems that can fail without warning when energy levels drop unexpectedly.

- **Significant security concerns**  
  In many urban neighborhoods, homeowners increasingly seek ways to protect their properties and families. Affordable smart home devices such as motion sensors, smart locks, and smart plugs are gradually becoming available in emerging markets.

- **Lack of centralized monitoring**  
  These devices often operate independently and lack a centralized platform that allows homeowners to supervise them easily.

- **Limited technical support and maintenance**  
  Homeowners frequently do not have reliable access to technicians who can diagnose device failures quickly. When a device stops functioning — especially due to battery depletion — the failure may go unnoticed until the system is already compromised.

- **Intermittent internet connectivity**  
  This makes lightweight and resilient systems essential. Existing smart home solutions designed for developed markets are often too complex, expensive, or infrastructure-dependent for widespread adoption in these environments.

- **Critical gap: absence of proactive battery health prediction**  
  Most smart devices provide only simple low-battery warnings, often too late to prevent system interruptions. Unexpected battery failure can disable security devices or interrupt automated home systems, leaving homes vulnerable or causing unnecessary energy inefficiencies.

These combined challenges highlight the need for a **lightweight, centralized monitoring platform** capable of providing proactive alerts and simplified device management, specifically adapted for environments with unstable electricity and limited infrastructure.

## 3. Business / User Opportunity & Objectives

The challenges described above create a strong opportunity to provide urban homeowners with a simple and accessible system for monitoring and managing essential smart home devices.

**SmartHome Guardian** aims to empower middle-class homeowners in urban areas of the DRC by delivering a lightweight web-based platform designed for reliability in infrastructure-constrained environments.

### Main objectives

- Enable homeowners to monitor critical smart home devices remotely through a centralized web dashboard.
- Provide proactive battery health alerts to prevent unexpected device shutdowns in battery-dependent environments.
- Improve residential security through real-time notifications and remote device control.
- Offer a simple and accessible monitoring solution that does not require complex hardware infrastructure.
- Deliver a Software-as-a-Service (SaaS) style web platform accessible from standard browsers without requiring native mobile applications.
- Rely on simulated IoT device behavior in the MVP to enable full functional validation without real hardware dependencies.

By focusing on **simplicity**, **reliability**, and **affordability**, SmartHome Guardian seeks to provide practical, contextually relevant value to households operating in environments where energy stability and security remain ongoing challenges.

## 4. Vision Statement

**SmartHome Guardian** delivers an accessible, secure, web-based smart home monitoring platform tailored for urban homeowners in emerging African markets such as the Democratic Republic of Congo, enabling real-time device oversight, remote security actions, and **explainable predictive battery health alerts** to prevent unexpected failures in unstable power environments.

## 5. Major Features / High-Level Capabilities

The MVP of SmartHome Guardian will focus on a limited set of essential capabilities designed to demonstrate the core value of the system.

### Key features

- User registration and authentication with **role-based access control** (Owner, Admin, Technician)
- Device onboarding and monitoring for **three device types**:
  - Battery-powered **Smart Lock**
  - Battery-powered **Motion Sensor**
  - Mains-powered **Smart Plug**
- Real-time device status monitoring through a centralized dashboard
- Remote control capabilities, including lock and unlock actions for smart locks
- Security and low-battery alerts, including:
  - Instant notifications
  - Predictive alerts with confidence scores and human-readable explanations (e.g., linking discharge trends to usage patterns)
- Basic energy usage summary for monitored smart plug devices
- Simulated IoT device behavior and notifications for demonstration and testing purposes

These capabilities represent the core functional value of the system while maintaining a manageable scope for the MVP.

## 6. Key Assumptions & Constraints

### Assumptions

- Users will access the system primarily through a web browser on a computer or mobile device.
- Simulated device data will adequately represent typical behavior of real IoT devices for the purposes of the MVP.
- Urban homeowners will benefit from proactive alerts and centralized monitoring of critical smart home devices.
- Intermittent connectivity will be mitigated through lightweight design and resilient features (though full offline mode is out-of-scope for MVP).

### Constraints

- The system will operate in **simulation mode only**, without integration with real IoT hardware.
- The battery prediction component will function as a **black-box module**, without implementing a full machine learning pipeline.
- The MVP development timeline is limited to approximately **10 weeks**.
- The platform will be **web-based only**, designed as a responsive web application without native mobile apps.
- The system must remain **lightweight** and resilient to intermittent connectivity conditions.

These constraints ensure that development remains feasible while still demonstrating the core value of the platform.

## 7. Scope Boundaries

To maintain a focused and achievable MVP within the project timeline, the scope of SmartHome Guardian is intentionally limited.

### In Scope

- Monitoring of **three simulated device types**:
  - Smart Lock
  - Motion Sensor
  - Smart Plug
- Real-time device status visualization
- Remote lock/unlock functionality for smart locks
- Instant security and low-battery alerts
- Predictive battery health alerts with explanations
- Basic energy usage dashboard for smart plug devices
- Role-based user management (Owner, Admin, Technician)

### Out of Scope

The following capabilities are intentionally excluded from the MVP:

- Video surveillance cameras
- Voice assistants or voice control integration
- Native mobile applications (Android or iOS)
- Multi-home or large-scale property management
- Direct integration with physical IoT devices
- Advanced machine learning model training and deployment
- Third-party smart home ecosystem integrations

By clearly defining these scope boundaries, the project maintains a realistic and manageable development path while focusing on validating the core concept of proactive smart home monitoring.
