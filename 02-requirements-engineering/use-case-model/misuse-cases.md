# – Misuse Cases (Attack Modeling)
SmartHome Guardian

Anchored Pillars: Security 🔐 | Battery Prediction 🔋 | Reliability 🌐

---

## 1. Overview

This document defines the **misuse cases** for the SmartHome Guardian system. Misuse cases are used to model potential attacks, threats, and system failures in order to **strengthen security, reliability, and battery prediction integrity**.

---

## 2. Threat Model

### Assets
- Smart Lock control commands 🔐
- Device telemetry (battery, status) 🔋
- User authentication sessions 🔐
- Alert pipeline (security + battery alerts) 🚨
- Audit logs 🌐

### Attack Surfaces
- Web dashboard
- API endpoints
- Device simulation channel
- Authentication tokens
- Alert system pipeline

### Threat Actors
- Unauthorized user
- Malicious insider
- Network attacker (MITM)
- Fake IoT device (spoofed telemetry)

---

## 3. Misuse Case Catalog

### 🔐 MC-01: Unauthorized Smart Lock Control
- **Description:** Attacker tries to unlock/lock smart home devices without permission.
- **Attack Steps:**
  1. Obtain or guess invalid token
  2. Send `unlock()` request to API
  3. Attempt repeated requests to bypass rate limits
- **Targets:** Authentication (NFR-008), Command API (NFR-003)
- **Expected System Response:** Reject request, log attempt, trigger security alert
- **Defense Requirements:** Strong token validation, rate limiting, audit logging (NFR-009)

---

### 🔐 MC-02: Fake IoT Device Injection
- **Description:** Attacker pretends to be a legitimate Smart Lock or Sensor.
- **Attack Steps:**
  1. Register fake device ID
  2. Send fake battery + motion data
  3. Manipulate predictive alert system
- **Targets:** Battery prediction system 🔋, Data integrity (NFR-006)
- **Expected System Response:** Reject unknown device, validate device identity
- **Defense Requirements:** Device authentication layer, signed telemetry

---

### 🔐 MC-03: Alert Suppression Attack
- **Description:** Attacker prevents security alerts from being delivered.
- **Attack Steps:**
  1. Flood system with noise requests
  2. Delay/block alert pipeline
  3. Mask real intrusion event
- **Targets:** Alert system (NFR-001), Reliability pipeline (NFR-005)
- **Expected System Response:** Alerts still delivered within threshold, prioritize critical events
- **Defense Requirements:** Priority queue for alerts, retry mechanism

---

### 🔐 MC-04: Replay Attack on Smart Lock Commands
- **Description:** Attacker resends previously captured valid unlock command.
- **Attack Steps:**
  1. Capture valid unlock request
  2. Replay same request multiple times
- **Targets:** Command system (NFR-003), Security layer (NFR-007)
- **Expected System Response:** Reject duplicate requests, detect replay pattern
- **Defense Requirements:** Nonce or timestamp validation, session-bound commands

---

### 🔋 MC-05: Battery Prediction Manipulation
- **Description:** Attacker manipulates battery telemetry to hide low battery state.
- **Attack Steps:**
  1. Send fake high battery values
  2. Delay real depletion reporting
  3. Avoid predictive alerts
- **Targets:** Battery prediction engine 🔋, Observability (NFR-013)
- **Expected System Response:** Detect inconsistent usage patterns, flag anomalies
- **Defense Requirements:** Cross-validation of usage vs battery drain, historical anomaly detection

---

### 🌐 MC-06: Service Disruption (Availability Attack)
- **Description:** Attacker overloads dashboard or API to make system unavailable.
- **Attack Steps:**
  1. Send high-frequency requests
  2. Exhaust system resources
  3. Slow down alert delivery
- **Targets:** Availability (NFR-004), Dashboard performance (NFR-002)
- **Expected System Response:** System degrades gracefully, core alerts still function
- **Defense Requirements:** Rate limiting, load shedding, alert pipeline prioritization

---

### 🔐 MC-07: Audit Log Tampering Attempt
- **Description:** Attacker tries to modify or delete security logs.
- **Attack Steps:**
  1. Gain admin-level access attempt
  2. Attempt to alter audit records
- **Targets:** Auditability (NFR-009), Data integrity (NFR-006)
- **Expected System Response:** Logs immutable (append-only), tampering logged
- **Defense Requirements:** Write-once log storage, separation of log writer/viewer roles

---

## 4. Key Observations

### From Gomaa (Architecture)
- Security requirements directly define authentication, device identity, and audit subsystems.
- Reliability attacks highlight the need for queueing and retry mechanisms.
- Battery attacks require data validation and anomaly detection.

### From Larman (Iterative Refinement)
- Each misuse case uncovers weak points.
- Refines requirements and system architecture.
- Example: MC-05 → anomaly detection added to battery prediction rules.

---

## 5. Refinement Actions

- 🔐 Security: Device verification, replay protection, immutable audit logs.
- 🔋 Battery: Cross-validation of telemetry, anomaly detection.
- 🌐 Reliability: Priority-based alert delivery, API rate limiting.

---
