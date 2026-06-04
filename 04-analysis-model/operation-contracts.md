# Operation Contracts

This document defines operation contracts for the system operations identified in the SmartHome Guardian System Sequence Diagrams.

Operation contracts describe what each system operation changes in the system state. They do not describe how the system internally performs the operation.

Each contract follows this structure:

* Operation name
* Cross references
* Preconditions
* Postconditions
* Exceptions

---

## OC-01: reportSecurityEvent

### Operation Name

```txt
reportSecurityEvent(deviceId, eventPayload, timestamp)
```

### Cross References

* SSD-01: Report Security Event and Notify Home Owner
* Related use case: Intrusion Detection and Alert Dispatch
* Related requirements:

  * Security event ingestion
  * Security alert generation
  * Device authentication
  * Audit logging

### Preconditions

* The device identifier is provided.
* The event payload is provided.
* The timestamp is provided.
* The sending device is registered in SmartHome Guardian.
* The sending device is an allowed device type, such as Motion/Door Sensor or Smart Lock.

### Postconditions

* The system validates the device identity.
* The system validates the event payload format.
* A security event record is created.
* The event is associated with the correct device.
* The event is associated with the correct Home Owner account.
* The event severity is classified.
* If the event indicates intrusion risk, a security alert is created.
* If an alert is created, the Home Owner is notified.
* An audit log entry is recorded for the received event.
* The device receives either an acceptance or rejection response.

### Exceptions

* If the device is unknown, the event is rejected.
* If the device is not authorized, the event is rejected.
* If the payload is invalid, the event is rejected.
* If the event is low-risk, the event is stored but no Home Owner alert is sent.
* If notification delivery fails, the event and alert remain recorded, and the notification failure is logged.

---

## OC-02: eventAccepted

### Operation Name

```txt
eventAccepted(eventId)
```

### Cross References

* SSD-01: Report Security Event and Notify Home Owner
* Related use case: Intrusion Detection and Alert Dispatch

### Preconditions

* A valid security event has been received.
* The event has passed device authentication and schema validation.
* A security event record has been created.

### Postconditions

* The Simulated IoT Device receives confirmation that the event was accepted.
* The accepted event is traceable using its event identifier.
* The event remains available for alerting, audit review, and later validation.

### Exceptions

* If the event record cannot be created, acceptance is not returned.
* If the system cannot persist the event, the device receives an error or rejection response.

---

## OC-03: eventRejected

### Operation Name

```txt
eventRejected(reason)
```

### Cross References

* SSD-01: Report Security Event and Notify Home Owner
* Related use case: Intrusion Detection and Alert Dispatch

### Preconditions

* A security event submission was attempted.
* The submitted event failed validation, authorization, or schema checks.

### Postconditions

* The event is not accepted as a valid security event.
* A rejection reason is returned to the Simulated IoT Device.
* A failed event ingestion attempt is recorded in the audit log.
* No security alert is sent to the Home Owner.

### Exceptions

* If audit logging fails, the rejection still occurs, but the logging failure is recorded as a system reliability issue.
* If the rejection reason cannot be determined, a generic rejection reason is returned.

---

## OC-04: securityAlertIssued

### Operation Name

```txt
securityAlertIssued(alertId, deviceId, severity, message)
```

### Cross References

* SSD-01: Report Security Event and Notify Home Owner
* Related use case: Intrusion Detection and Alert Dispatch
* Related requirements:

  * Security alert dispatch
  * User notification
  * Audit logging

### Preconditions

* A valid security event exists.
* The event has been classified as requiring Home Owner notification.
* The affected device is associated with a Home Owner account.

### Postconditions

* A security alert is created.
* The alert is associated with the security event.
* The alert is associated with the affected device.
* The alert is associated with the Home Owner.
* The alert contains severity and a readable message.
* The Home Owner receives or is queued to receive the alert.
* An audit log entry records that a security alert was generated.

### Exceptions

* If the Home Owner cannot be reached, the alert remains stored and notification failure is logged.
* If the device-to-owner relationship cannot be resolved, alert dispatch is blocked and logged.
* If the alert message cannot be generated, the system records the event but flags the alert creation failure.

---

## OC-05: requestLockCommand

### Operation Name

```txt
requestLockCommand(userId, deviceId, command)
```

### Cross References

* SSD-02: Remotely Control Smart Lock
* Related use case: Remote Smart Lock Control
* Related requirements:

  * Remote lock/unlock control
  * Authentication
  * Authorization
  * RBAC
  * Encrypted command transmission
  * Audit logging

### Preconditions

* The Home Owner is authenticated.
* The target device identifier is provided.
* The command is provided.
* The command is either `lock` or `unlock`.
* The target device is registered.
* The target device is a Smart Lock.
* The Home Owner has permission to control the target Smart Lock.

### Postconditions

* The system validates the user session.
* The system checks whether the user is authorized to control the device.
* The system checks whether the target device is a supported Smart Lock.
* If authorized, a secure command token is prepared.
* If the device is available, the command is transmitted to the Smart Lock.
* The lock command attempt is recorded in the audit log.
* The Home Owner receives a confirmation, denial, or failure response.

### Exceptions

* If the user is not authenticated, the request is rejected.
* If the user is not authorized, the command is denied.
* If the device is not a Smart Lock, the command is rejected.
* If the device is offline, the command fails with a device unavailable response.
* If the command is invalid, the request is rejected.
* If command transmission fails, the failure is recorded in the audit log.

---

## OC-06: transmitLockCommand

### Operation Name

```txt
transmitLockCommand(deviceId, commandToken)
```

### Cross References

* SSD-02: Remotely Control Smart Lock
* Related use case: Remote Smart Lock Control

### Preconditions

* The Home Owner command request has been authorized.
* The target device is a registered Smart Lock.
* A command token has been prepared.
* The command token represents a valid lock or unlock command.

### Postconditions

* The command is sent to the Simulated IoT Device representing the Smart Lock.
* The command transmission attempt is recorded.
* The system waits for command completion, timeout, or failure.
* The command is conceptually protected against unauthorized tampering.

### Exceptions

* If the device is offline, no successful command completion is recorded.
* If the command token is invalid, the command is not transmitted.
* If the device does not respond within the expected time, the command is marked as failed.
* If the command is interrupted, the failure is logged.

---

## OC-07: lockCommandCompleted

### Operation Name

```txt
lockCommandCompleted(deviceId, finalState)
```

### Cross References

* SSD-02: Remotely Control Smart Lock
* Related use case: Remote Smart Lock Control

### Preconditions

* A lock command was previously transmitted.
* The Smart Lock device responded to the command.
* The response includes the final lock state.

### Postconditions

* The system records the command result.
* The Smart Lock state is updated.
* The command result is associated with the correct device.
* The command result is associated with the requesting Home Owner.
* A successful command audit log entry is created.
* The Home Owner can be informed of the final lock state.

### Exceptions

* If the final state is missing or invalid, the command result is marked uncertain.
* If the response comes from an unexpected device, the response is rejected.
* If the state update cannot be persisted, the system records a persistence failure.

---

## OC-08: lockCommandConfirmed

### Operation Name

```txt
lockCommandConfirmed(deviceId, finalState)
```

### Cross References

* SSD-02: Remotely Control Smart Lock
* Related use case: Remote Smart Lock Control

### Preconditions

* A lock command was completed successfully.
* The final lock state is known.
* The requesting Home Owner is still associated with the command request.

### Postconditions

* The Home Owner receives confirmation of the command result.
* The confirmation includes the device identifier and final state.
* The system has recorded the successful command attempt.
* The displayed lock state matches the latest accepted device response.

### Exceptions

* If the Home Owner session expires before confirmation, the result remains recorded.
* If confirmation delivery fails, the command result remains stored and the notification failure is logged.

---

## OC-09: lockCommandDenied

### Operation Name

```txt
lockCommandDenied(reason)
```

### Cross References

* SSD-02: Remotely Control Smart Lock
* Related use case: Remote Smart Lock Control

### Preconditions

* A Home Owner attempted to control a Smart Lock.
* The authorization check failed, or the request violated system permissions.

### Postconditions

* The lock command is not transmitted to the Smart Lock.
* The Home Owner receives a denial response.
* The denial reason is recorded.
* A failed or denied command attempt is recorded in the audit log.
* The Smart Lock state remains unchanged.

### Exceptions

* If the denial reason cannot be disclosed safely, a generic access denied message is returned.
* If audit logging fails, the denial still occurs and the logging failure is flagged.

---

## OC-10: lockCommandFailed

### Operation Name

```txt
lockCommandFailed(deviceId, reason)
```

### Cross References

* SSD-02: Remotely Control Smart Lock
* Related use case: Remote Smart Lock Control

### Preconditions

* A lock command was authorized.
* The command could not be completed because of timeout, offline status, or communication failure.

### Postconditions

* The command is marked as failed.
* The Smart Lock state is not updated unless a trusted final state is received.
* The Home Owner receives a failure response.
* The failure reason is recorded.
* An audit log entry is created for the failed command attempt.

### Exceptions

* If the device later reconnects and reports a state, the system may reconcile the latest device state.
* If the failure reason is unknown, the system returns a generic device unavailable response.
* If repeated failures occur, the device may be flagged for technical review.

---

## OC-11: reportTelemetry

### Operation Name

```txt
reportTelemetry(deviceId, batteryLevel, deviceStatus, timestamp)
```

### Cross References

* SSD-03: Report Telemetry and Issue Predictive Battery Alert
* Related use case: Device Health Check and Predictive Battery Notification
* Related requirements:

  * Telemetry ingestion
  * Device health monitoring
  * Predictive battery alerting
  * Explainable AI output
  * Audit logging

### Preconditions

* The device identifier is provided.
* The battery level is provided for battery-powered devices.
* The device status is provided.
* The timestamp is provided.
* The sending device is registered.
* The sending device is one of the supported device types.

### Postconditions

* The system validates the device identity.
* The system validates the telemetry schema.
* A telemetry reading is created.
* The telemetry reading is associated with the correct device.
* The device health status is updated.
* For Smart Lock and Motion/Door Sensor devices, battery risk is evaluated.
* If battery risk threshold is reached, a predictive battery alert is created.
* If an alert is created, it includes remaining hours, confidence, and explanation.
* The telemetry processing result is recorded in the audit log.
* The device receives an acceptance or rejection response.

### Exceptions

* If the device is unknown, telemetry is rejected.
* If the telemetry schema is invalid, telemetry is rejected.
* If the battery level is outside a valid range, telemetry is rejected or flagged.
* If the device is a Smart Plug, battery prediction is not performed.
* If prediction cannot be completed, telemetry is stored and prediction failure is logged.
* If alert delivery fails, the alert remains stored and notification failure is logged.

---

## OC-12: telemetryAccepted

### Operation Name

```txt
telemetryAccepted(readingId)
```

### Cross References

* SSD-03: Report Telemetry and Issue Predictive Battery Alert
* Related use case: Device Health Check and Predictive Battery Notification

### Preconditions

* A telemetry submission has passed validation.
* A telemetry reading has been created.

### Postconditions

* The Simulated IoT Device receives confirmation that telemetry was accepted.
* The accepted telemetry can be traced using its reading identifier.
* The device health record reflects the accepted telemetry.
* The telemetry is available for later battery trend analysis.

### Exceptions

* If the telemetry cannot be persisted, no acceptance is returned.
* If the device record cannot be updated, the telemetry is flagged for review.

---

## OC-13: telemetryRejected

### Operation Name

```txt
telemetryRejected(reason)
```

### Cross References

* SSD-03: Report Telemetry and Issue Predictive Battery Alert
* Related use case: Device Health Check and Predictive Battery Notification

### Preconditions

* A telemetry submission was attempted.
* The submission failed validation, authorization, or schema checks.

### Postconditions

* The telemetry is not accepted as a valid reading.
* A rejection reason is returned to the Simulated IoT Device.
* A failed telemetry ingestion attempt is recorded.
* No predictive battery alert is generated from the rejected telemetry.

### Exceptions

* If the rejection reason cannot be safely disclosed, a generic rejection reason is returned.
* If audit logging fails, the rejection still occurs and the logging failure is flagged.

---

## OC-14: predictiveBatteryAlertIssued

### Operation Name

```txt
predictiveBatteryAlertIssued(deviceId, remainingHours, confidence, explanation)
```

### Cross References

* SSD-03: Report Telemetry and Issue Predictive Battery Alert
* Related use case: Device Health Check and Predictive Battery Notification
* Related requirements:

  * Predictive low-battery alert
  * Explainable alert output
  * Battery health monitoring

### Preconditions

* A valid telemetry reading exists.
* The device is battery-powered.
* The battery prediction logic has produced a risk result.
* The risk result indicates that the device may reach a critical battery level within the configured prediction window.

### Postconditions

* A predictive battery alert is created.
* The alert is associated with the affected device.
* The alert is associated with the Home Owner.
* The alert includes estimated remaining battery time.
* The alert includes confidence.
* The alert includes a human-readable explanation.
* The Home Owner receives or is queued to receive the predictive alert.
* The alert generation is recorded in the audit log.

### Exceptions

* If the device is not battery-powered, no predictive battery alert is generated.
* If confidence is below the acceptable threshold, the system may store the risk result without sending an alert.
* If explanation generation fails, the alert is not considered complete.
* If notification delivery fails, the alert remains stored and the failure is logged.

---

## OC-15: registerDevice

### Operation Name

```txt
registerDevice(userId, deviceType, deviceName, deviceIdentifier)
```

### Cross References

* SSD-04: Register Device
* Related use case: Register Smart Home Device
* Related requirements:

  * Device registration
  * Device ownership
  * Supported device scope
  * RBAC

### Preconditions

* The Home Owner is authenticated.
* The device type is provided.
* The device name is provided.
* The device identifier is provided.
* The device type is one of the supported device types:

  * Smart Lock
  * Motion/Door Sensor
  * Smart Plug

### Postconditions

* The device type is validated.
* A device record is created.
* The device is associated with the Home Owner account.
* The initial device status is recorded.
* The device becomes available for monitoring.
* Device ownership is established.
* A device registration audit log entry is created.
* The Home Owner receives a registration success or rejection response.

### Exceptions

* If the device type is unsupported, registration is rejected.
* If the device identifier already exists, registration is rejected.
* If the user is not authenticated, registration is rejected.
* If required device information is missing, registration is rejected.
* If the device record cannot be created, registration fails.

---

## OC-16: deviceRegistered

### Operation Name

```txt
deviceRegistered(deviceId, registrationStatus)
```

### Cross References

* SSD-04: Register Device
* Related use case: Register Smart Home Device

### Preconditions

* Device registration has completed successfully.
* A valid device record exists.
* The device is associated with the Home Owner account.

### Postconditions

* The Home Owner receives confirmation of device registration.
* The response includes the device identifier.
* The response includes the registration status.
* The device can now appear in device monitoring views.

### Exceptions

* If confirmation delivery fails, the device remains registered.
* If the device cannot be displayed immediately, the system records the registration and refreshes later.

---

## OC-17: deviceRegistrationRejected

### Operation Name

```txt
deviceRegistrationRejected(reason)
```

### Cross References

* SSD-04: Register Device
* Related use case: Register Smart Home Device

### Preconditions

* A device registration request was submitted.
* The request failed validation, scope, duplication, or authorization checks.

### Postconditions

* No new device record is created.
* The Home Owner receives a rejection reason.
* The failed registration attempt is recorded.
* Unsupported devices remain outside the system scope.

### Exceptions

* If the rejection reason cannot be safely disclosed, a generic rejection reason is returned.
* If audit logging fails, registration is still rejected and the logging failure is flagged.

---

## OC-18: createSupportRequest

### Operation Name

```txt
createSupportRequest(userId, deviceId, issueDescription)
```

### Cross References

* SSD-05: Create Support Request
* Related use case: Request Technical Support
* Related requirements:

  * Support request creation
  * Technician workflow
  * Device issue tracking

### Preconditions

* The Home Owner is authenticated.
* The device identifier is provided.
* The issue description is provided.
* The device is registered.
* The device belongs to the Home Owner or is accessible under the Home Owner account.

### Postconditions

* A support ticket is created.
* The ticket is associated with the Home Owner.
* The ticket is associated with the affected device.
* The ticket stores the issue description.
* The ticket receives an initial status, such as `open`.
* The ticket becomes available for Technician review or assignment.
* A support request creation audit log entry is recorded.
* The Home Owner receives the support ticket identifier and status.

### Exceptions

* If the user is not authenticated, the request is rejected.
* If the device is unknown, the request is rejected.
* If the user does not own or have access to the device, the request is denied.
* If the issue description is empty, the request is rejected.
* If the ticket cannot be created, the operation fails and the failure is logged.

---

## OC-19: supportRequestCreated

### Operation Name

```txt
supportRequestCreated(ticketId, status)
```

### Cross References

* SSD-05: Create Support Request
* Related use case: Request Technical Support

### Preconditions

* A support ticket has been created successfully.
* The ticket has a valid identifier.
* The ticket has an initial status.

### Postconditions

* The Home Owner receives the ticket identifier.
* The Home Owner receives the ticket status.
* The ticket remains available for Technician review.
* The support request can be tracked later.

### Exceptions

* If confirmation delivery fails, the support ticket remains stored.
* If the ticket status cannot be displayed, a generic created response is returned.

---

## OC-20: viewAssignedSupportRequests

### Operation Name

```txt
viewAssignedSupportRequests(technicianId)
```

### Cross References

* SSD-05: Create Support Request
* Related use case: Request Technical Support
* Related requirements:

  * Technician support workflow
  * Role-based access control

### Preconditions

* The Technician is authenticated.
* The Technician has the correct role.
* One or more support requests exist or may exist in the system.

### Postconditions

* The system verifies the Technician role.
* The system retrieves support requests visible to the Technician.
* The support request list is prepared.
* The Technician receives a list of support ticket summaries.
* Access to unrelated or unauthorized support records is restricted.

### Exceptions

* If the user is not authenticated, access is rejected.
* If the user is not a Technician, access is denied.
* If no support requests are assigned or available, an empty list is returned.
* If support records cannot be retrieved, the system returns an error and logs the failure.

---

## OC-21: supportRequestList

### Operation Name

```txt
supportRequestList(ticketSummary)
```

### Cross References

* SSD-05: Create Support Request
* Related use case: Request Technical Support

### Preconditions

* The Technician is authorized to view support requests.
* The support request query has completed.

### Postconditions

* The Technician receives support ticket summaries.
* Each visible ticket summary may include ticket identifier, device reference, issue summary, status, and priority.
* Restricted Home Owner or device information is not exposed beyond what the Technician role requires.

### Exceptions

* If no support tickets are available, an empty list is returned.
* If data retrieval fails, no list is returned and the failure is logged.

---

## Contract Summary

The operation contracts define how system events from the SSDs change the conceptual system state.

The most important state changes are:

* Security events are accepted, rejected, stored, and possibly converted into alerts.
* Smart Lock commands are authorized, transmitted, completed, denied, or failed.
* Device telemetry is accepted, rejected, stored, and possibly converted into predictive battery alerts.
* Devices are registered only if they match the supported project scope.
* Support requests are created and made available for Technician handling.
* Security-sensitive actions are audit logged.
* Predictive battery alerts remain black-box and explainable.

These contracts provide a bridge between:

```txt
03-use-case-model/
04-analysis-model/system-sequence-diagrams.md
05-design-model/class-diagram.md
05-design-model/communication-diagrams.md
07-validation/system-test-scenarios.md
02-requirements/requirements-traceability-matrix.md
```

---

## Build → Observe → Refine Note

The SSDs identified the system operations. These operation contracts refine those operations by describing their required conditions, resulting state changes, and failure cases.

The next refinement step is to use these contracts to improve:

* The domain model
* The design class diagram
* Communication diagrams
* Validation scenarios
* Requirements traceability matrix

Each operation should eventually trace back to at least one use case and at least one requirement.
