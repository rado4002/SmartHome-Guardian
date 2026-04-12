# System Sequence Diagrams (SSD)

This document provides system-level sequence diagrams for the core use cases.

## SSD-01: Intrusion Detection and Alert Dispatch

```mermaid
sequenceDiagram
autonumber
actor Device as Simulated IoT Device
participant System as SmartHome Guardian System
participant Classifier as Event Classification Service
participant Notifier as Notification Service
actor HomeOwner as Home Owner
participant Audit as Audit Logger

Device->>System: sendSecurityEvent(deviceId, eventPayload)
System->>Classifier: classify(eventPayload)
Classifier-->>System: eventType=intrusionRisk, severity
System->>Notifier: dispatchSecurityAlert(userId, alertPayload)
Notifier-->>HomeOwner: deliverSecurityAlert(alertId, message)
System->>Audit: logEvent(actor=deviceId, action=securityAlertGenerated, result=success)
Audit-->>System: logRecorded
System-->>Device: acknowledge(eventAccepted)
```

## SSD-02: Remote Smart Lock Control

```mermaid
sequenceDiagram
autonumber
actor HomeOwner as Home Owner
participant System as SmartHome Guardian System
participant Lock as Smart Lock
participant Audit as Audit Logger

HomeOwner->>System: requestLockCommand(deviceId, command)
System->>System: authorize(userId, deviceId)
alt authorized and device online
	System->>Lock: execute(command)
	Lock-->>System: commandResult(success)
	System->>Audit: logEvent(actor=userId, action=lockCommand, result=success)
	Audit-->>System: logRecorded
	System-->>HomeOwner: confirmFinalState(state=locked/unlocked)
else authorization failure
	System->>Audit: logEvent(actor=userId, action=lockCommand, result=denied)
	System-->>HomeOwner: accessDenied
else device timeout/offline
	System->>Lock: retryOnce(command)
	Lock-->>System: noResponse
	System->>Audit: logEvent(actor=userId, action=lockCommand, result=failed-timeout)
	System-->>HomeOwner: commandFailedDeviceOffline
end
```

## SSD-03: Device Health Check and Predictive Battery Notification

```mermaid
sequenceDiagram
autonumber
actor Device as Simulated IoT Device
participant System as SmartHome Guardian System
participant Predictor as Battery Prediction Service
participant Notifier as Notification Service
actor HomeOwner as Home Owner
participant Audit as Audit Logger

Device->>System: sendTelemetry(deviceId, batteryLevel, status, timestamp)
System->>System: validateDeviceAuthAndSchema()
System->>Predictor: evaluateBatteryRisk(deviceId, telemetrySeries)
Predictor-->>System: riskResult(remainingHours, confidence, explanation)
alt risk threshold reached
	System->>Notifier: dispatchPredictiveAlert(userId, riskResult)
	Notifier-->>HomeOwner: deliverPredictiveAlert(alertId)
	System->>Audit: logEvent(actor=deviceId, action=predictiveAlertGenerated, result=success)
else no risk
	System->>Audit: logEvent(actor=deviceId, action=telemetryProcessed, result=noAlert)
end
System-->>Device: acknowledge(telemetryAccepted)
```