# Activity Diagrams

This document presents activity diagrams for key SmartHome Guardian workflows.

Activity diagrams model the dynamic behavior of the system by showing the flow of control from one action or activity to another. They are used here to describe workflow-level behavior without introducing implementation code.

These diagrams follow UML activity diagram concepts:

* Initial node
* Final node
* Action/activity states
* Transitions
* Decision branches with guard conditions
* Swimlanes for responsibility grouping
* Fork/join synchronization where parallel work is meaningful

GitHub Mermaid does not support every UML activity diagram symbol exactly, so these diagrams use GitHub-compatible Mermaid notation while preserving the intended UML meaning.

## Mermaid Notation Mapping

| UML Activity Diagram Concept  | GitHub Mermaid Representation                                                 |               |   |
| ----------------------------- | ----------------------------------------------------------------------------- | ------------- | - |
| Initial node                  | `Start((Start))`                                                              |               |   |
| Final node                    | `End((End))`                                                                  |               |   |
| Action state / Activity state | `Action([Action name])`                                                       |               |   |
| Transition                    | `A --> B`                                                                     |               |   |
| Branch / Decision             | `Decision{Question?}`                                                         |               |   |
| Guard condition               | `-->                                                                          | "[condition]" | ` |
| Swimlane                      | `subgraph LaneName["Swimlane: Name"]`                                         |               |   |
| Fork / Join synchronization   | `Fork([Fork / synchronization bar])` and `Join([Join / synchronization bar])` |               |   |

---

## AD-01: Device Telemetry Ingestion

### Purpose

Show how SmartHome Guardian receives telemetry from simulated IoT devices, validates it, stores it, updates device health, and decides whether battery prediction is needed.

```mermaid
flowchart TD

Start((Start))

subgraph DeviceLane["Swimlane: Simulated IoT Device"]
    SendTelemetry([Send telemetry])
end

subgraph SystemLane["Swimlane: SmartHome Guardian System"]
    ReceiveTelemetry([Receive telemetry])
    ValidateDevice([Validate device identity])
    ValidateSchema([Validate telemetry schema])
    RejectTelemetry([Reject telemetry])
    StoreTelemetry([Store telemetry reading])
    UpdateHealth([Update device health record])
    CheckDeviceType{Is device battery-powered?}
    RunPrediction([Forward telemetry history for battery risk evaluation])
    SkipPrediction([Skip battery prediction])
    RecordProcessedAudit([Record telemetry processed audit log])
    RecordRejectedAudit([Record rejected telemetry audit log])
end

End((End))

Start --> SendTelemetry
SendTelemetry --> ReceiveTelemetry
ReceiveTelemetry --> ValidateDevice

ValidateDevice -->|"[invalid device]"| RejectTelemetry
ValidateDevice -->|"[valid device]"| ValidateSchema

ValidateSchema -->|"[invalid schema]"| RejectTelemetry
ValidateSchema -->|"[valid schema]"| StoreTelemetry

RejectTelemetry --> RecordRejectedAudit
RecordRejectedAudit --> End

StoreTelemetry --> UpdateHealth
UpdateHealth --> CheckDeviceType

CheckDeviceType -->|"[Smart Lock or Motion/Door Sensor]"| RunPrediction
CheckDeviceType -->|"[Smart Plug]"| SkipPrediction

RunPrediction --> RecordProcessedAudit
SkipPrediction --> RecordProcessedAudit
RecordProcessedAudit --> End
```

### Notes

* Telemetry may come from Smart Lock, Motion/Door Sensor, or Smart Plug.
* Smart Lock and Motion/Door Sensor are battery-powered.
* Smart Plug is mains-powered and does not trigger battery prediction.
* Invalid telemetry is rejected and audit logged.
* Battery prediction remains black-box.
* The swimlanes separate device responsibility from system responsibility.

---

## AD-02: Security Alert Handling

### Purpose

Show how SmartHome Guardian handles a security event from a Motion/Door Sensor or Smart Lock and decides whether to create and deliver a security alert.

```mermaid
flowchart TD

Start((Start))

subgraph DeviceLane["Swimlane: Simulated IoT Device"]
    ReportEvent([Report security event])
end

subgraph SystemLane["Swimlane: SmartHome Guardian System"]
    ReceiveEvent([Receive security event])
    ValidateDevice([Validate device identity])
    ValidatePayload([Validate event payload])
    RejectEvent([Reject security event])
    ClassifyEvent([Classify security event])
    RiskDecision{Is event an intrusion risk?}
    StoreLowRisk([Store low-risk security event])
    CreateSecurityAlert([Create SecurityAlert])
    SendAlert([Send alert to Home Owner])
    DeliveryDecision{Was alert delivered?}
    MarkDelivered([Mark alert as delivered])
    MarkPendingRetry([Mark alert as pending retry])
    RecordRejectAudit([Record rejected event audit log])
    RecordSuccessAudit([Record security alert success audit log])
    RecordFailureAudit([Record alert delivery failure audit log])
end

subgraph OwnerLane["Swimlane: Home Owner"]
    ReceiveAlert([Receive security alert])
end

End((End))

Start --> ReportEvent
ReportEvent --> ReceiveEvent
ReceiveEvent --> ValidateDevice

ValidateDevice -->|"[invalid device]"| RejectEvent
ValidateDevice -->|"[valid device]"| ValidatePayload

ValidatePayload -->|"[invalid payload]"| RejectEvent
ValidatePayload -->|"[valid payload]"| ClassifyEvent

RejectEvent --> RecordRejectAudit
RecordRejectAudit --> End

ClassifyEvent --> RiskDecision

RiskDecision -->|"[no intrusion risk]"| StoreLowRisk
StoreLowRisk --> RecordSuccessAudit
RecordSuccessAudit --> End

RiskDecision -->|"[intrusion risk]"| CreateSecurityAlert
CreateSecurityAlert --> SendAlert
SendAlert --> DeliveryDecision

DeliveryDecision -->|"[delivered]"| ReceiveAlert
ReceiveAlert --> MarkDelivered
MarkDelivered --> RecordSuccessAudit
RecordSuccessAudit --> End

DeliveryDecision -->|"[delivery failed]"| MarkPendingRetry
MarkPendingRetry --> RecordFailureAudit
RecordFailureAudit --> End
```

### Notes

* The event may be caused by suspicious motion, door activity, or lock-related access behavior.
* The system does not include cameras or video analysis.
* Low-risk events may be stored without notifying the Home Owner.
* Failed notification delivery does not delete the alert.
* Audit logging is required for rejected, successful, and failed alert flows.

---

## AD-03: Predictive Battery Alert Handling

### Purpose

Show how SmartHome Guardian evaluates battery risk and creates an explainable predictive battery alert when a supported battery-powered device may reach a critical battery level.

```mermaid
flowchart TD

Start((Start))

subgraph DeviceLane["Swimlane: Battery-Powered Simulated IoT Device"]
    SendBatteryTelemetry([Send battery telemetry])
end

subgraph SystemLane["Swimlane: SmartHome Guardian System"]
    ReceiveBatteryTelemetry([Receive battery telemetry])
    ValidateTelemetry([Validate telemetry])
    StoreTelemetry([Store telemetry reading])
    LoadHistory([Load telemetry history])
    Fork([Fork / synchronization bar])
    UpdateHealth([Update device health record])
    EvaluateRisk([Evaluate battery risk using black-box prediction])
    Join([Join / synchronization bar])
    PredictionOutput([Produce remainingHours, confidence, explanation])
    RiskDecision{Risk threshold reached?}
    ConfidenceDecision{Confidence acceptable?}
    StorePrediction([Store BatteryPrediction])
    CreateAlert([Create PredictiveBatteryAlert])
    SendAlert([Send predictive alert])
    DeliveryDecision{Was alert delivered?}
    MarkDelivered([Mark alert as delivered])
    MarkPendingRetry([Mark alert as pending retry])
    RecordNoAlertAudit([Record no-alert audit log])
    RecordAlertAudit([Record predictive alert audit log])
    RecordFailureAudit([Record delivery failure audit log])
end

subgraph OwnerLane["Swimlane: Home Owner"]
    ReceivePredictiveAlert([Receive predictive battery alert])
end

End((End))

Start --> SendBatteryTelemetry
SendBatteryTelemetry --> ReceiveBatteryTelemetry
ReceiveBatteryTelemetry --> ValidateTelemetry

ValidateTelemetry -->|"[invalid telemetry]"| RecordNoAlertAudit
RecordNoAlertAudit --> End

ValidateTelemetry -->|"[valid telemetry]"| StoreTelemetry
StoreTelemetry --> LoadHistory
LoadHistory --> Fork

Fork --> UpdateHealth
Fork --> EvaluateRisk

UpdateHealth --> Join
EvaluateRisk --> Join

Join --> PredictionOutput
PredictionOutput --> RiskDecision

RiskDecision -->|"[no risk]"| RecordNoAlertAudit
RiskDecision -->|"[risk detected]"| ConfidenceDecision

ConfidenceDecision -->|"[low confidence]"| RecordNoAlertAudit
ConfidenceDecision -->|"[acceptable confidence]"| StorePrediction

StorePrediction --> CreateAlert
CreateAlert --> SendAlert
SendAlert --> DeliveryDecision

DeliveryDecision -->|"[delivered]"| ReceivePredictiveAlert
ReceivePredictiveAlert --> MarkDelivered
MarkDelivered --> RecordAlertAudit
RecordAlertAudit --> End

DeliveryDecision -->|"[delivery failed]"| MarkPendingRetry
MarkPendingRetry --> RecordFailureAudit
RecordFailureAudit --> End
```

### Notes

* This workflow applies only to battery-powered devices:

  * Smart Lock
  * Motion/Door Sensor
* Smart Plug is excluded from predictive battery alerts.
* The AI module is treated as a black box.
* The prediction output must include:

  * remaining hours
  * confidence
  * explanation
* The fork/join shows that device health updating and battery risk evaluation may be treated as parallel workflow responsibilities after telemetry is stored.
* The system does not expose or implement machine learning internals.

Example predictive alert:

```txt
Remaining: 48h
Confidence: 85%
Explanation: Discharge rate increased due to frequent smart lock usage.
```

---

## AD-04: Smart Lock Command Request

### Purpose

Show how a Home Owner requests a lock/unlock command and how SmartHome Guardian handles authentication, authorization, command transmission, device response, failure, and audit logging.

```mermaid
flowchart TD

Start((Start))

subgraph OwnerLane["Swimlane: Home Owner"]
    RequestCommand([Request lock or unlock command])
    ReceiveConfirmation([Receive final lock state])
    ReceiveDenial([Receive access denied response])
    ReceiveFailure([Receive command failure response])
end

subgraph SystemLane["Swimlane: SmartHome Guardian System"]
    AuthenticateUser([Authenticate user session])
    AuthDecision{Is user authenticated?}
    AuthorizeDevice([Authorize user for target Smart Lock])
    PermissionDecision{Is user authorized?}
    ValidateDevice([Validate target device type])
    DeviceDecision{Is device a Smart Lock?}
    PrepareCommand([Prepare protected command token])
    SendCommand([Transmit command to Smart Lock])
    ResponseDecision{Did Smart Lock respond?}
    UpdateLockState([Update Smart Lock final state])
    DenyCommand([Deny command request])
    RejectCommand([Reject invalid device command])
    MarkFailed([Mark command as failed])
    RecordSuccessAudit([Record successful command audit log])
    RecordDeniedAudit([Record denied command audit log])
    RecordFailureAudit([Record failed command audit log])
end

subgraph DeviceLane["Swimlane: Simulated IoT Device - Smart Lock"]
    ExecuteCommand([Execute protected lock command])
    ReturnFinalState([Return final lock state])
    NoResponse([No response or timeout])
end

End((End))

Start --> RequestCommand
RequestCommand --> AuthenticateUser
AuthenticateUser --> AuthDecision

AuthDecision -->|"[not authenticated]"| DenyCommand
AuthDecision -->|"[authenticated]"| AuthorizeDevice

AuthorizeDevice --> PermissionDecision
PermissionDecision -->|"[not authorized]"| DenyCommand
PermissionDecision -->|"[authorized]"| ValidateDevice

ValidateDevice --> DeviceDecision
DeviceDecision -->|"[not Smart Lock]"| RejectCommand
DeviceDecision -->|"[Smart Lock]"| PrepareCommand

PrepareCommand --> SendCommand
SendCommand --> ExecuteCommand
ExecuteCommand --> ResponseDecision

ResponseDecision -->|"[response received]"| ReturnFinalState
ReturnFinalState --> UpdateLockState
UpdateLockState --> ReceiveConfirmation
ReceiveConfirmation --> RecordSuccessAudit
RecordSuccessAudit --> End

ResponseDecision -->|"[timeout or offline]"| NoResponse
NoResponse --> MarkFailed
MarkFailed --> ReceiveFailure
ReceiveFailure --> RecordFailureAudit
RecordFailureAudit --> End

DenyCommand --> ReceiveDenial
ReceiveDenial --> RecordDeniedAudit
RecordDeniedAudit --> End

RejectCommand --> ReceiveDenial
```

### Notes

* The Home Owner never communicates directly with the Smart Lock.
* SmartHome Guardian acts as the control boundary.
* RBAC and device ownership checks happen before command transmission.
* The command is conceptually protected/encrypted before being sent.
* Timeout and offline behavior are explicitly modeled.
* Successful, denied, rejected, and failed commands are audit logged.

---

## AD-05: Technician Support Request Workflow

### Purpose

Show how a Home Owner creates a support request and how a Technician views and updates support work.

```mermaid
flowchart TD

Start((Start))

subgraph OwnerLane["Swimlane: Home Owner"]
    CreateRequest([Create support request])
    ReceiveTicket([Receive ticket id and status])
    ReceiveRequestRejected([Receive rejection reason])
end

subgraph SystemLane["Swimlane: SmartHome Guardian System"]
    ValidateOwner([Validate Home Owner session])
    ValidateDevice([Validate device ownership])
    ValidateIssue([Validate issue description])
    ValidationDecision{Is request valid?}
    RejectRequest([Reject support request])
    CreateTicket([Create SupportRequest ticket])
    SetStatusOpen([Set status to open])
    RecordCreateAudit([Record support request audit log])
    ValidateTechnician([Validate Technician role])
    RoleDecision{Is user a Technician?}
    DenyAccess([Deny access])
    LoadRequests([Load visible support requests])
    UpdateStatus([Update support request status])
    RecordUpdateAudit([Record support update audit log])
end

subgraph TechnicianLane["Swimlane: Technician"]
    OpenPortal([Open support portal])
    ViewRequests([View assigned support requests])
    SelectRequest([Select support request])
    SubmitStatusUpdate([Submit status update])
    ReceiveAccessDenied([Receive access denied response])
end

End((End))

Start --> CreateRequest
CreateRequest --> ValidateOwner
ValidateOwner --> ValidateDevice
ValidateDevice --> ValidateIssue
ValidateIssue --> ValidationDecision

ValidationDecision -->|"[invalid request]"| RejectRequest
RejectRequest --> ReceiveRequestRejected
ReceiveRequestRejected --> RecordCreateAudit
RecordCreateAudit --> End

ValidationDecision -->|"[valid request]"| CreateTicket
CreateTicket --> SetStatusOpen
SetStatusOpen --> RecordCreateAudit
RecordCreateAudit --> ReceiveTicket
ReceiveTicket --> OpenPortal

OpenPortal --> ValidateTechnician
ValidateTechnician --> RoleDecision

RoleDecision -->|"[not Technician]"| DenyAccess
DenyAccess --> ReceiveAccessDenied
ReceiveAccessDenied --> End

RoleDecision -->|"[Technician]"| LoadRequests
LoadRequests --> ViewRequests
ViewRequests --> SelectRequest
SelectRequest --> SubmitStatusUpdate
SubmitStatusUpdate --> UpdateStatus
UpdateStatus --> RecordUpdateAudit
RecordUpdateAudit --> End
```

### Notes

* The Home Owner creates the support request.
* The support request must concern a registered supported device.
* The Technician can view only support requests allowed by role and assignment rules.
* Support request creation and status updates are audit logged.
* This workflow does not require implementation code.

---

## GitHub Mermaid Compatibility Notes

These diagrams avoid Mermaid parsing errors by using:

* Quoted guard labels: `-->|"[condition]"|`
* Simple node IDs: `ValidateDevice`, `RecordAudit`, `End`
* Rounded action nodes: `Action([Action name])`
* Decision nodes: `Decision{Question?}`
* Simple start and end nodes: `Start((Start))`, `End((End))`
* No raw bracket labels like `-->|[condition]|`
* No special Unicode symbols inside Mermaid nodes
* No `<br/>` inside node labels

---

## Activity Diagram Modeling Decisions

1. Activity diagrams show workflow-level behavior, not object structure.
2. Each diagram starts with an initial node and ends with a final node.
3. Action states are represented as rounded action nodes.
4. Decision nodes are represented with diamonds.
5. Guard conditions are written on outgoing transitions using quoted bracket-style labels.
6. Swimlanes group activities by responsible actor or system area.
7. Fork/join notation is approximated for GitHub Mermaid compatibility.
8. Smart Plug is included only for telemetry and power usage monitoring.
9. Smart Plug is excluded from predictive battery alerting.
10. Security-related workflows include audit logging.
11. Smart Lock command handling includes authentication, authorization, protected command transmission, timeout handling, and audit logging.
12. Predictive battery logic remains black-box and explainable.
13. The diagrams avoid implementation details such as databases, frameworks, APIs, or code.

---

## Traceability Targets

These activity diagrams support and refine behavior from:

```txt
03-use-case-model/fully-dressed-use-cases.md
04-analysis-model/system-sequence-diagrams.md
04-analysis-model/operation-contracts.md
05-design-model/class-diagram.md
05-design-model/communication-diagrams.md
05-design-model/grasp-patterns.md
07-validation/system-test-scenarios.md
02-requirements/requirements-traceability-matrix.md
```

---

## Build → Observe → Refine Note

These activity diagrams were created from the project’s SSDs, operation contracts, communication diagrams, class diagram, and GRASP responsibility assignments.

They refine SmartHome Guardian behavior by showing workflow decisions such as:

* valid vs invalid telemetry
* battery-powered vs mains-powered devices
* intrusion risk vs low-risk event
* prediction risk vs no risk
* authorized vs unauthorized lock command
* device response vs timeout
* valid vs invalid support request
* Technician access allowed vs denied

The diagrams help validate whether the modeled system behavior is complete before moving into architecture and final validation.

---

## Micro-Experiment: Validate Activity Workflows

### Goal

Check whether the activity diagrams describe real SmartHome Guardian behavior across common and failure scenarios.

### Action

Simulate these events:

```txt
1. Smart Lock sends valid battery telemetry.
2. Smart Plug sends power usage telemetry.
3. Motion/Door Sensor reports suspicious motion.
4. Unknown device sends telemetry.
5. Home Owner sends unlock command to Smart Lock.
6. Unauthorized user attempts lock command.
7. Smart Lock times out.
8. Home Owner creates a support request.
9. Technician views assigned support requests.
```

### Expected Result

```txt
1. Telemetry is accepted and battery prediction is evaluated.
2. Telemetry is accepted but battery prediction is skipped.
3. Security event is classified and a security alert is created if intrusion risk is detected.
4. Telemetry is rejected and audit logged.
5. Command is authorized, protected, sent, confirmed, and audit logged.
6. Command is denied and audit logged.
7. Command is marked failed and audit logged.
8. Support request is created and audit logged.
9. Technician sees only allowed support requests.
```

### Observation

The activity diagrams cover the main operational paths and failure paths of SmartHome Guardian. They preserve security, auditability, device scope, and black-box predictive alert behavior while avoiding implementation-level detail.

---

