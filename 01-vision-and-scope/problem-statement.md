# SmartHome Guardian – Problem Statement

## 1. Problem Overview

In emerging urban environments such as the Democratic Republic of Congo, smart home devices (e.g., smart locks, motion sensors, smart plugs) are increasingly adopted by homeowners. However, there is a lack of centralized, reliable, and intelligent systems to monitor these devices effectively.

Homeowners face critical challenges in maintaining device reliability, ensuring security, and managing limited power resources due to unstable electricity and reliance on battery-powered devices.

---

## 2. Core Problems Identified

### 2.1 Lack of Centralized Monitoring
Users cannot easily view the status of all their smart devices in one place. Device information is fragmented or unavailable in real time.

### 2.2 Battery Failure Risk
Battery-powered devices (especially smart locks and sensors) may unexpectedly run out of power, leading to:
- Loss of access control (e.g., locked out)
- Security vulnerabilities (e.g., inactive sensors)

There is currently no predictive system that warns users before failure occurs.

### 2.3 Limited Security Awareness
Users are not proactively notified of:
- Unauthorized access attempts
- Suspicious motion events

This reduces the effectiveness of smart home security systems.

### 2.4 Unreliable Infrastructure
Environmental constraints include:
- Unstable electricity supply
- Intermittent internet connectivity

Existing solutions are not adapted to these conditions.

### 2.5 Lack of Explainable Intelligence
Even when alerts exist, they are often:
- Reactive (too late)
- Not understandable (no explanation)

Users need clear, explainable predictions about device behavior.

---

## 3. Target Users

- Home Owner: Needs visibility, control, and alerts
- Administrator: Manages system and users
- Technician: Maintains and diagnoses devices
- Simulated IoT Device: Sends status and receives commands

---

## 4. Desired Solution

A web-based SaaS platform that:

- Provides centralized monitoring of smart home devices
- Tracks real-time device status (battery, activity)
- Enables remote control (e.g., lock/unlock)
- Detects and reports security events
- Predicts battery depletion in advance (24–72 hours)
- Explains predictions clearly (confidence + reasoning)
- Works under unreliable power and network conditions

---

## 5. Key Value Proposition

SmartHome Guardian transforms smart home systems from:

❌ Reactive → (after failure)  
✅ Predictive → (before failure)

❌ Opaque alerts → (no explanation)  
✅ Explainable insights → (clear reasoning)

❌ Fragmented devices → (isolated)  
✅ Unified system → (centralized monitoring)

---

## 6. System Boundary (High-Level)

The system will:

INCLUDE:
- Monitoring smart locks, sensors, smart plugs
- Receiving device telemetry (battery, events)
- Sending commands (lock/unlock)
- Generating alerts (security + predictive)

EXCLUDE:
- Direct hardware control implementation
- Complex AI/ML modeling (black-box only)
- Mobile applications
- Camera/video systems

---

## 7. Success Criteria

The system is considered successful if:

- Users receive early warnings before battery failure
- Unauthorized access attempts are detected and reported
- Users can monitor and control devices remotely
- The system remains usable under intermittent connectivity
- Alerts are understandable and actionable