# GRASP Responsibilities for SmartHome Guardian

This document defines responsibility assignments using GRASP principles and aligns with the current detailed design artifacts.

## 1. Scope and Alignment

This GRASP analysis is consistent with:

- `05-detailed-design/class-diagrams/core-class-diagram.md`
- `05-detailed-design/interaction-diagrams/alert-flow-interaction.md`
- `03-analysis-model/system-sequence-diagrams/core-use-cases-ssd.md`

Covered classes:

- Boundary: `HomeOwnerUI`, `TechnicianPortal`, `AdminDashboard`, `IoTGateway`
- Control: `DeviceController`, `SecurityAlertController`, `BatteryMonitoringController`
- Entity: `SmartLock`, `MotionSensor`, `SmartPlug`, `User`, `DeviceRegistry`, `BatteryPrediction`
- Service: `NotificationService`, `AuditLogger`, `AIService`

## 2. GRASP Pattern Applications

### 2.1 Creator

Problem: Which class should create device instances?

Application:

- `DeviceRegistry` creates `SmartLock`, `MotionSensor`, and `SmartPlug`.

Rationale: `DeviceRegistry` holds device registration context and lifecycle responsibilities.

### 2.2 Information Expert

Problem: Which class should own domain-specific behavior?

Application:

- `SmartLock` owns lock state and battery reporting.
- `MotionSensor` owns motion detection and battery reporting.
- `SmartPlug` owns state and power usage reporting.
- `BatteryPrediction` owns prediction output data (`remainingHours`, `confidence`, `explanation`).

Rationale: These entities contain the data needed to fulfill these responsibilities.

### 2.3 Controller

Problem: Which objects coordinate system operations?

Application:

- `DeviceController` coordinates remote command operations.
- `SecurityAlertController` coordinates security event handling and alert dispatch.
- `BatteryMonitoringController` coordinates telemetry-based battery monitoring and predictive alerts.

Rationale: Use-case orchestration is centralized in control classes, keeping boundary and entity classes focused.

### 2.4 Low Coupling

Problem: How to minimize dependency impact across components?

Application:

- UI boundary classes call controller classes instead of directly manipulating entities.
- Alert dispatch is delegated to `NotificationService`.
- Audit recording is delegated to `AuditLogger`.
- Prediction logic is delegated to `AIService`.

Rationale: Cross-cutting and integration concerns are isolated, reducing ripple effects from change.

### 2.5 High Cohesion

Problem: How to keep each class focused?

Application:

- Entity classes focus on domain state and core behaviors.
- Controller classes focus on workflow coordination.
- Service classes focus on infrastructure-style responsibilities (notification, logging, prediction).

Rationale: Each class has a narrow, clear purpose.

### 2.6 Pure Fabrication

Problem: Who should handle responsibilities that do not naturally belong to domain entities?

Application:

- `NotificationService` handles alert delivery.
- `AuditLogger` handles audit persistence.
- `AIService` handles predictive analysis.

Rationale: These services improve maintainability and prevent domain model pollution.

### 2.7 Protected Variations and Indirection

Problem: How to shield core logic from likely changes?

Application:

- Controllers interact with services (`NotificationService`, `AIService`, `AuditLogger`) through clear operation boundaries.
- External concerns (delivery channels, prediction implementation, logging backend) are isolated behind service classes.

Rationale: Core use-case flows remain stable even when service implementations evolve.

## 3. Responsibility Summary Matrix

| Class | Key Responsibilities | GRASP Patterns |
| --- | --- | --- |
| `DeviceRegistry` | Register and create device entities | Creator, High Cohesion |
| `SmartLock` | Maintain lock state and battery report | Information Expert, High Cohesion |
| `MotionSensor` | Detect motion and report battery | Information Expert, High Cohesion |
| `SmartPlug` | Maintain plug state and report power usage | Information Expert, High Cohesion |
| `DeviceController` | Coordinate lock/device commands | Controller, Low Coupling |
| `SecurityAlertController` | Process security events and dispatch alerts | Controller, Low Coupling |
| `BatteryMonitoringController` | Monitor battery and trigger predictive alerts | Controller, Low Coupling |
| `AIService` | Generate battery risk predictions | Pure Fabrication, Indirection |
| `NotificationService` | Deliver user notifications | Pure Fabrication, Low Coupling |
| `AuditLogger` | Persist audit events | Pure Fabrication, Low Coupling |
| `BatteryPrediction` | Hold prediction result details | Information Expert |
| `User` | Hold user identity/role information | Information Expert |

## 4. Consistency Checks

- Responsibility owners match class names in the core class diagram.
- Controller behavior matches SSD and interaction diagrams.
- Alert flow responsibilities align with state transitions in the alert state machine.
- No deprecated classes are used (`DeviceManager`, `DoorSensor`, `AIAnalyzer`, `AlertService`, `BatteryMonitor`).

## 5. Conclusion

The current design applies GRASP in a consistent way:

- Entities handle domain knowledge.
- Controllers handle use-case coordination.
- Services handle cross-cutting concerns.

This structure supports traceability, maintainability, and clear academic justification of responsibility assignment.