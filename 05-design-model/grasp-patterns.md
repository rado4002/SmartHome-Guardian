# GRASP Responsibilities for SmartHome Guardian

This document defines responsibility assignments for SmartHome Guardian using GRASP principles.

GRASP helps justify which software classes should be responsible for creating objects, coordinating use cases, holding domain knowledge, reducing coupling, and maintaining cohesion.

This document aligns with:

* `04-analysis-model/domain-model.md`
* `04-analysis-model/system-sequence-diagrams.md`
* `04-analysis-model/operation-contracts.md`
* `05-design-model/class-diagram.md`
* `05-design-model/communication-diagrams.md`

---

## 1. Scope and Alignment

SmartHome Guardian is modeled using a Boundary-Control-Entity design structure.

### Boundary Classes

Boundary classes handle interaction with external actors and devices.

* `HomeOwnerUI`
* `TechnicianPortal`
* `AdminDashboard`
* `IoTGateway`
* `NotificationBoundary`

### Control Classes

Control classes coordinate use-case behavior.

* `DeviceRegistrationControl`
* `AuthorizationControl`
* `LockCommandControl`
* `SecurityEventControl`
* `TelemetryControl`
* `BatteryPredictionControl`
* `AlertManagementControl`
* `SupportRequestControl`
* `AuditControl`

### Entity Classes

Entity classes hold important domain state.

* `User`
* `HomeOwner`
* `Administrator`
* `Technician`
* `Home`
* `Device`
* `BatteryPoweredDevice`
* `SmartLock`
* `MotionDoorSensor`
* `SmartPlug`
* `TelemetryReading`
* `DeviceHealthRecord`
* `BatteryPrediction`
* `Alert`
* `SecurityAlert`
* `PredictiveBatteryAlert`
* `SecurityEvent`
* `AccessAttempt`
* `LockCommand`
* `SupportRequest`
* `AuditLogEntry`

### Service Classes

Service classes isolate specialized or external technical responsibilities.

* `NotificationService`
* `BatteryPredictionService`

---

## 2. GRASP Pattern Applications

---

## 2.1 Creator

### Problem

Which class should create new objects?

### Application

| Created Object                                         | Creator Class               | Reason                                                                    |
| ------------------------------------------------------ | --------------------------- | ------------------------------------------------------------------------- |
| `Device`, `SmartLock`, `MotionDoorSensor`, `SmartPlug` | `DeviceRegistrationControl` | It coordinates device registration and validates supported device types.  |
| `SecurityEvent`                                        | `SecurityEventControl`      | It receives and processes reported security events.                       |
| `TelemetryReading`                                     | `TelemetryControl`          | It receives telemetry from simulated IoT devices.                         |
| `BatteryPrediction`                                    | `BatteryPredictionControl`  | It coordinates battery risk evaluation.                                   |
| `SecurityAlert`                                        | `AlertManagementControl`    | It manages alert creation and delivery state.                             |
| `PredictiveBatteryAlert`                               | `AlertManagementControl`    | It creates user-facing predictive battery alerts from prediction results. |
| `LockCommand`                                          | `LockCommandControl`        | It coordinates smart lock command requests and results.                   |
| `SupportRequest`                                       | `SupportRequestControl`     | It coordinates support ticket creation.                                   |
| `AuditLogEntry`                                        | `AuditControl`              | It centralizes audit log creation.                                        |

### Rationale

Creation responsibility is assigned to classes that already have the required context or coordinate the related use case.

For example, `LockCommandControl` creates `LockCommand` because it receives the Home Owner request, checks authorization, prepares the command token, and records command success or failure.

---

## 2.2 Information Expert

### Problem

Which class should be responsible for knowing or answering information about a concept?

### Application

| Class                  | Information Responsibility                                                      |
| ---------------------- | ------------------------------------------------------------------------------- |
| `User`                 | Knows user identity, role, and account status.                                  |
| `HomeOwner`            | Owns homes, devices, alerts, lock commands, and support requests.               |
| `Device`               | Knows device identity, type, status, last seen time, and installation location. |
| `BatteryPoweredDevice` | Knows battery level and battery health status.                                  |
| `SmartLock`            | Knows lock state and receives lock commands.                                    |
| `MotionDoorSensor`     | Knows sensor type and sensor state.                                             |
| `SmartPlug`            | Knows plug state and power usage.                                               |
| `TelemetryReading`     | Knows reported device telemetry values.                                         |
| `DeviceHealthRecord`   | Knows the latest health state of a device.                                      |
| `BatteryPrediction`    | Knows remaining hours, confidence, explanation, and prediction window.          |
| `Alert`                | Knows alert severity, status, message, and timestamp.                           |
| `SecurityEvent`        | Knows event type, severity, timestamp, and description.                         |
| `LockCommand`          | Knows command type, result, requested time, and final lock state.               |
| `SupportRequest`       | Knows issue description, status, priority, and creation time.                   |
| `AuditLogEntry`        | Knows actor, action, result, timestamp, and details.                            |

### Rationale

Information Expert assigns responsibility to the class that has the required data.

Examples:

* `SmartLock` is the expert for lock state.
* `SmartPlug` is the expert for power usage, not battery level.
* `BatteryPrediction` is the expert for prediction output, but not for machine learning internals.
* `AuditLogEntry` is the expert for audit record information.

---

## 2.3 Controller

### Problem

Which class should coordinate a system operation from the SSDs?

### Application

| System Operation Area           | Controller                  |
| ------------------------------- | --------------------------- |
| Device registration             | `DeviceRegistrationControl` |
| Authorization and RBAC checks   | `AuthorizationControl`      |
| Remote Smart Lock command       | `LockCommandControl`        |
| Security event processing       | `SecurityEventControl`      |
| Telemetry processing            | `TelemetryControl`          |
| Battery risk evaluation         | `BatteryPredictionControl`  |
| Alert creation and alert status | `AlertManagementControl`    |
| Support request workflow        | `SupportRequestControl`     |
| Audit log coordination          | `AuditControl`              |

### Rationale

Controller responsibilities are assigned to control classes instead of UI or entity classes.

This prevents boundary classes from becoming too complex and prevents entity classes from being overloaded with workflow logic.

Example:

`HomeOwnerUI` receives a request from the Home Owner, but `LockCommandControl` coordinates the lock command use case.

---

## 2.4 Low Coupling

### Problem

How can the design reduce unnecessary dependencies between classes?

### Application

SmartHome Guardian applies Low Coupling by:

* Letting boundary classes call control classes instead of directly manipulating entities.
* Using `AuthorizationControl` for access decisions instead of duplicating authorization logic.
* Using `AlertManagementControl` to create and manage alerts.
* Using `NotificationService` behind `NotificationBoundary` for alert delivery.
* Using `BatteryPredictionService` behind `BatteryPredictionControl`.
* Using `AuditControl` to centralize audit log creation.
* Preventing Home Owner interaction from directly reaching `SmartLock`.

### Rationale

Low Coupling makes the system easier to change.

For example, if the notification mechanism changes later, the core alert flow should not require major changes. Only the notification boundary/service side should change.

---

## 2.5 High Cohesion

### Problem

How can each class stay focused on a clear responsibility?

### Application

| Class Type       | Cohesive Responsibility                   |
| ---------------- | ----------------------------------------- |
| Boundary classes | Actor/device interaction only             |
| Control classes  | Use-case coordination only                |
| Entity classes   | Domain state and closely related behavior |
| Service classes  | Specialized technical behavior            |

### Examples

* `HomeOwnerUI` does not classify events or update telemetry.
* `TelemetryControl` does not deliver notifications directly.
* `BatteryPredictionControl` does not manage lock commands.
* `SmartPlug` only manages plug state and power usage.
* `AuditControl` only coordinates audit log creation.

### Rationale

High Cohesion keeps classes understandable and maintainable.

Each class has one main reason to change.

---

## 2.6 Pure Fabrication

### Problem

Who should handle responsibilities that do not naturally belong to domain entities?

### Application

| Pure Fabrication Class     | Responsibility                                |
| -------------------------- | --------------------------------------------- |
| `NotificationService`      | Sends notifications to users.                 |
| `BatteryPredictionService` | Performs black-box battery prediction.        |
| `AuditControl`             | Coordinates creation of audit records.        |
| `AuthorizationControl`     | Centralizes RBAC and device ownership checks. |

### Rationale

Some responsibilities are necessary but do not belong naturally to domain entities.

For example:

* `SmartLock` should not send notifications.
* `HomeOwner` should not check RBAC rules.
* `BatteryPrediction` should not implement prediction logic.
* `SecurityAlert` should not persist audit records.

Pure Fabrication keeps domain entities clean and focused.

---

## 2.7 Indirection

### Problem

How can the design avoid direct coupling between classes that should not know too much about each other?

### Application

SmartHome Guardian uses Indirection through:

* `NotificationBoundary` between alert logic and notification delivery.
* `BatteryPredictionControl` between telemetry processing and battery prediction.
* `AuthorizationControl` between use-case controllers and RBAC/device ownership rules.
* `AuditControl` between business workflows and audit log creation.
* `IoTGateway` between simulated IoT devices and internal control classes.

### Rationale

Indirection protects the core design from external or unstable details.

For example, `TelemetryControl` does not need to know how battery prediction works internally. It only asks `BatteryPredictionControl` to evaluate risk and receives a black-box result.

---

## 2.8 Protected Variations

### Problem

How can the design protect stable parts of the system from likely future changes?

### Variation Points

| Possible Future Change                    | Protected By                                           |
| ----------------------------------------- | ------------------------------------------------------ |
| Notification channel changes              | `NotificationBoundary`, `NotificationService`          |
| Battery prediction implementation changes | `BatteryPredictionControl`, `BatteryPredictionService` |
| Authorization policy changes              | `AuthorizationControl`                                 |
| Audit storage or format changes           | `AuditControl`, `AuditLogEntry`                        |
| Device telemetry format changes           | `IoTGateway`, `TelemetryControl`                       |
| New support workflow rules                | `SupportRequestControl`                                |

### Rationale

Protected Variations keeps the stable core use-case flow separate from parts likely to change.

Important rule:

The AI module remains black-box. The design may change the prediction implementation later, but SmartHome Guardian should still expose only:

```txt
remainingHours
confidence
explanation
```

---

## 2.9 Polymorphism

### Problem

How should behavior vary by device type without excessive conditional logic?

### Application

The design uses a device hierarchy:

```txt
Device
└── BatteryPoweredDevice
    ├── SmartLock
    └── MotionDoorSensor
└── SmartPlug
```

### Rationale

Device-specific behavior is placed in the appropriate subclass.

Examples:

* `SmartLock` supports lock state and lock commands.
* `MotionDoorSensor` supports sensor state and security detection.
* `SmartPlug` supports plug state and power usage.
* `BatteryPoweredDevice` supports battery-related behavior.
* `SmartPlug` does not inherit from `BatteryPoweredDevice`.

This prevents incorrect behavior such as predicting battery risk for a mains-powered Smart Plug.

---

## 3. Responsibility Summary Matrix

| Class                       | Key Responsibilities                                   | GRASP Patterns                                |
| --------------------------- | ------------------------------------------------------ | --------------------------------------------- |
| `HomeOwnerUI`               | Receive Home Owner actions and display results         | Low Coupling, High Cohesion                   |
| `TechnicianPortal`          | Support request and device health interaction          | Low Coupling, High Cohesion                   |
| `AdminDashboard`            | Admin management interaction                           | Low Coupling, High Cohesion                   |
| `IoTGateway`                | Receive simulated IoT messages                         | Indirection, Low Coupling                     |
| `NotificationBoundary`      | Separate alert management from delivery channel        | Indirection, Protected Variations             |
| `DeviceRegistrationControl` | Register supported devices                             | Controller, Creator                           |
| `AuthorizationControl`      | Verify role and device access                          | Pure Fabrication, Protected Variations        |
| `LockCommandControl`        | Coordinate smart lock commands                         | Controller, Creator, Low Coupling             |
| `SecurityEventControl`      | Process security events                                | Controller, Creator                           |
| `TelemetryControl`          | Process device telemetry                               | Controller, Creator                           |
| `BatteryPredictionControl`  | Coordinate black-box battery risk evaluation           | Controller, Indirection, Protected Variations |
| `AlertManagementControl`    | Create and manage alerts                               | Controller, Creator                           |
| `SupportRequestControl`     | Create and manage support requests                     | Controller, Creator                           |
| `AuditControl`              | Coordinate audit log creation                          | Pure Fabrication, Creator                     |
| `NotificationService`       | Send user notifications                                | Pure Fabrication, Protected Variations        |
| `BatteryPredictionService`  | Produce black-box prediction output                    | Pure Fabrication, Protected Variations        |
| `SmartLock`                 | Maintain lock state and battery-related device state   | Information Expert, Polymorphism              |
| `MotionDoorSensor`          | Maintain sensor state and battery-related device state | Information Expert, Polymorphism              |
| `SmartPlug`                 | Maintain plug state and power usage                    | Information Expert, Polymorphism              |
| `BatteryPrediction`         | Hold prediction output                                 | Information Expert                            |
| `LockCommand`               | Hold command request and result                        | Information Expert                            |
| `SecurityEvent`             | Hold security event details                            | Information Expert                            |
| `SupportRequest`            | Hold support ticket information                        | Information Expert                            |
| `AuditLogEntry`             | Hold audit log details                                 | Information Expert                            |

---

## 4. Consistency Checks

### Class Diagram Consistency

The GRASP responsibility assignments match the current class diagram.

Updated class names used:

* `DeviceRegistrationControl`
* `AuthorizationControl`
* `LockCommandControl`
* `SecurityEventControl`
* `TelemetryControl`
* `BatteryPredictionControl`
* `AlertManagementControl`
* `SupportRequestControl`
* `AuditControl`
* `MotionDoorSensor`
* `NotificationService`
* `BatteryPredictionService`

Deprecated or old names are not used:

* `DeviceController`
* `SecurityAlertController`
* `BatteryMonitoringController`
* `MotionSensor`
* `AIService`
* `AuditLogger`

---

### SSD Consistency

The controller classes support SSD system operations such as:

```txt
reportSecurityEvent(deviceId, eventPayload, timestamp)
requestLockCommand(userId, deviceId, command)
reportTelemetry(deviceId, batteryLevel, deviceStatus, timestamp)
registerDevice(userId, deviceType, deviceName, deviceIdentifier)
createSupportRequest(userId, deviceId, issueDescription)
viewAssignedSupportRequests(technicianId)
```

---

### Communication Diagram Consistency

The GRASP assignments match communication diagram objects such as:

* `:SecurityEventIngestionBoundary`
* `:EventClassificationControl`
* `:AlertManagementControl`
* `:TelemetryIngestionBoundary`
* `:BatteryPredictionControl`
* `:SmartLockControlBoundary`
* `:AuthorizationControl`
* `:LockCommandControl`
* `:SupportRequestControl`
* `:AuditLogEntry`

---

### Scope Consistency

The design remains inside the SmartHome Guardian scope.

Included devices:

* Smart Lock
* Motion/Door Sensor
* Smart Plug

Excluded concepts:

* Cameras
* Video
* Voice assistants
* Mobile apps
* Payments
* Unsupported extra devices

---

## 5. Security Responsibility Notes

SmartHome Guardian must preserve a strong security lens.

Security responsibilities are assigned as follows:

| Security Concern                   | Responsible Class               |
| ---------------------------------- | ------------------------------- |
| RBAC and role verification         | `AuthorizationControl`          |
| Device ownership check             | `AuthorizationControl`          |
| Lock command coordination          | `LockCommandControl`            |
| Conceptual command protection      | `LockCommandControl`            |
| Unauthorized access attempt record | `AccessAttempt`, `AuditControl` |
| Security event processing          | `SecurityEventControl`          |
| Alert creation                     | `AlertManagementControl`        |
| Audit log creation                 | `AuditControl`                  |
| Audit log record data              | `AuditLogEntry`                 |

This avoids placing security responsibilities randomly across UI or device classes.

---

## 6. Black-Box AI Responsibility Notes

Battery prediction is intentionally black-box.

Responsible classes:

| Responsibility                | Class                                          |
| ----------------------------- | ---------------------------------------------- |
| Coordinate prediction request | `BatteryPredictionControl`                     |
| Produce prediction result     | `BatteryPredictionService`                     |
| Store prediction output       | `BatteryPrediction`                            |
| Create user-facing alert      | `AlertManagementControl`                       |
| Deliver alert                 | `NotificationBoundary` / `NotificationService` |

The system must not model or implement machine learning internals.

Allowed prediction output:

```txt
remainingHours
confidence
explanation
```

Example:

```txt
Remaining: 48h
Confidence: 85%
Explanation: Discharge rate increased due to frequent smart lock usage.
```

---

## 7. Supporting Design Patterns

The following non-GRASP design patterns may support the design.

| Pattern              | Intended Use                                      | Rationale                                                                                                               |
| -------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Observer             | Alert propagation                                 | Security and battery events can trigger alert behavior without tightly coupling event sources to notification delivery. |
| Layered Architecture | Boundary, control, entity, and service separation | Keeps UI, workflow, domain state, and technical services separated.                                                     |
| Strategy             | Battery prediction policy                         | Different black-box prediction strategies could be swapped without changing telemetry flow.                             |
| Adapter              | IoT device message handling                       | Simulated IoT device messages can be adapted into internal telemetry or security event formats.                         |
| Facade               | Simplified subsystem access                       | Boundary classes can call use-case controls without knowing internal collaboration details.                             |

---

## 8. Build → Observe → Refine Note

The first GRASP draft correctly identified important patterns such as Creator, Information Expert, Controller, Low Coupling, High Cohesion, Pure Fabrication, Indirection, and Protected Variations.

The refined version improves the draft by:

* Aligning responsibility names with the latest class diagram.
* Replacing old class names with current design classes.
* Removing battery responsibility from Smart Plug.
* Adding `AuthorizationControl`, `TelemetryControl`, `AlertManagementControl`, `SupportRequestControl`, and `AuditControl`.
* Preserving the black-box AI rule.
* Strengthening security responsibility assignment.
* Connecting GRASP decisions to SSDs, operation contracts, communication diagrams, and class diagram artifacts.

---

## 9. Micro-Experiment: Validate GRASP Responsibilities

### Goal

Check whether each responsibility is assigned to the most appropriate class.

### Action

Simulate these events:

```txt
1. Home Owner registers a Smart Lock.
2. Motion/Door Sensor reports suspicious motion.
3. Smart Lock reports low battery telemetry.
4. Home Owner sends unlock command.
5. Unauthorized user attempts a lock command.
6. Smart Plug reports power usage.
7. Home Owner creates a support request.
```

### Expected Responsibility Flow

```txt
1. DeviceRegistrationControl creates SmartLock.
2. SecurityEventControl creates SecurityEvent and AlertManagementControl creates SecurityAlert.
3. TelemetryControl creates TelemetryReading and BatteryPredictionControl evaluates risk.
4. LockCommandControl creates LockCommand and coordinates transmission.
5. AuthorizationControl denies access and AuditControl records the denied attempt.
6. TelemetryControl processes Smart Plug telemetry without battery prediction.
7. SupportRequestControl creates SupportRequest and AuditControl records the action.
```

### Observation

The responsibility assignments support the main system behaviors while keeping each class focused. The design avoids placing too much logic in UI, device, or service classes. It also preserves SmartHome Guardian’s security requirements, black-box AI constraint, and approved device scope.

---

## 10. Conclusion

The SmartHome Guardian design applies GRASP consistently:

* Boundary classes handle external interaction.
* Control classes coordinate use cases.
* Entity classes hold domain state.
* Service classes isolate technical variation.
* Security responsibilities are centralized and traceable.
* Battery prediction remains black-box.
* Smart Plug remains mains-powered and outside battery prediction.

This structure supports maintainability, traceability, and clear academic justification of responsibility assignment.
