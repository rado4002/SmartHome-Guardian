# – NFR Validation
SmartHome Guardian – SaaS Platform  
Anchored Pillars: Security 🔐 | Battery Prediction 🔋 | Reliability 🌐

---

## 1. Overview

This document validates the Non-Functional Requirements (NFRs) defined for SmartHome Guardian. Each NFR is evaluated through **micro-experiments, simulations, and observations**, then refined based on system behavior.

---

## 2. NFR Validation Table

| NFR | Pillar | Experiment | Simulation / Action | Result | Observation / Refinement |
|-----|--------|-----------|-------------------|--------|-------------------------|
| **NFR-001 Alert Latency** | 🔐 🔋 🌐 | Measure end-to-end alert time | Trigger security and battery alerts on simulated devices | Alerts delivered in 1.8–2 sec | Meets latency. Adjust threshold for high-frequency events. |
| **NFR-002 Dashboard Response Time** | 🌐 | Load test dashboard | View device status for 100 devices | p95 response ~1.9 sec | Meets requirement. Maintain as more devices added. |
| **NFR-003 Command Response Time** | 🔐 🌐 | Test lock/unlock commands | Send command to smart lock with normal connectivity | Ack received in 1.7 sec | Meets latency. Retry mechanism verified for failures. |
| **NFR-004 Availability** | 🌐 | Simulate transient service outages | Stop/start core services for 1–5 min | 99.6% uptime maintained | Meets target. Recovery mechanism effective. |
| **NFR-005 Reliability and Recovery** | 🌐 🔋 | Telemetry loss and retry | Disconnect device mid-transmission | Commands and telemetry preserved | Retry & buffer effective. Include battery depletion scenarios. |
| **NFR-006 Data Integrity** | 🔐 🌐 | Send malformed payload | System rejects invalid telemetry | Payload rejected, logs captured | Validation correct. Data ordering maintained. |
| **NFR-007 Transport Security** | 🔐 | Intercept commands | Attempt unencrypted connection | Connection blocked, TLS enforced | Meets requirement. |
| **NFR-008 Authentication & Authorization** | 🔐 | Unauthorized access attempt | Simulate invalid token access | Access denied, audit logged | Enforcement confirmed. |
| **NFR-009 Auditability & Traceability** | 🔐 🌐 | Verify audit logs | Simulate user/device actions | All actions logged with timestamp, actor, action | Logs append-only, traceable. |
| **NFR-010 Privacy & Data Minimization** | 🔐 | Schema inspection | Review telemetry storage | Only required fields persisted | Meets requirement. Retention policy documented. |
| **NFR-011 Scalability** | 🌐 🔋 | Add simulated devices | Integrate 2 new virtual devices | System processes telemetry without redesign | Stable. Ensure adapter pattern supports more device types. |
| **NFR-012 Maintainability** | 🌐 | Interface review | Inspect modular boundaries & documentation | Interfaces consistent, versioned | Satisfies requirement. |
| **NFR-013 Observability** | 🌐 🔐 🔋 | Metrics collection | Monitor API latency, alert delivery, telemetry ingestion | Metrics captured in real-time | Diagnostics effective. Refinement: include alert pipeline detailed logs. |
| **NFR-014 Usability** | 🔐 🌐 | Scenario walkthrough | Stakeholder completes key tasks | Tasks completed without errors | Interaction simple, no training required. |
| **NFR-015 Compatibility & Access Channels** | 🌐 | Browser test | Test on Chrome, Edge, Firefox | Dashboard renders correctly | Meets requirement. Verify viewport scaling for mobile. |

---

## 3. Key Insights

1. **Security 🔐:** Role-based access control, TLS, and audit logging effectively enforce system security.
2. **Battery Prediction 🔋:** Predictive alerts reliably trigger; buffer and retry mechanisms improve resilience.
3. **Reliability 🌐:** Dashboard, commands, and telemetry pipelines meet latency and availability goals; system gracefully handles transient failures.
4. **Overlapping NFRs:** Many NFRs impact multiple pillars — architecture and design should reflect these dependencies.

---

## 4. Refinements Applied

- NFR-001: Added buffer for high-frequency event bursts.  
- NFR-005: Verified telemetry and command preservation during battery depletion.  
- NFR-011: Adapter pattern confirmed for adding new devices.  
- NFR-013: Enhanced alert pipeline logging for deeper diagnostics.

---



---
