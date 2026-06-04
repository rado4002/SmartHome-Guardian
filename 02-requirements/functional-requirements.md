# Functional Requirements

This section defines the functional behavior that SmartHome Guardian SaaS Platform must provide.

## Functional Requirements List

### FR-001 User Authentication

Requirement: The system shall authenticate Home Owner, Technician, and Administrator accounts before allowing protected actions.

Acceptance Criteria:
- Given valid credentials, when a user signs in, then the system grants access according to role.
- Given invalid credentials, when a user signs in, then the system denies access and returns an authentication error.

### FR-002 Device Authentication

Requirement: The system shall authenticate each simulated IoT device before accepting telemetry or battery updates.

Acceptance Criteria:
- Given a valid device token, when a device sends data, then the system accepts the request.
- Given an invalid or expired token, when a device sends data, then the system rejects the request.

### FR-003 Device Registration

Requirement: The system shall allow an authenticated Home Owner to register a new device and bind it to their account.

Acceptance Criteria:
- Given an authenticated Home Owner, when valid device details are submitted, then the device is linked to that account.
- Given duplicate device identity, when registration is attempted, then the system prevents duplicate registration.

### FR-004 View Device Status

Requirement: The system shall provide authenticated Home Owners with current device status including connectivity, battery level, and operational state.

Acceptance Criteria:
- Given an authenticated Home Owner, when device dashboard is opened, then latest known status for each linked device is shown.
- Given no recent telemetry, when status is displayed, then the device is marked as stale or offline.

### FR-005 Control Smart Lock

Requirement: The system shall allow an authenticated Home Owner to send lock and unlock commands to registered smart locks.

Acceptance Criteria:
- Given an online lock and authorized user, when lock or unlock is requested, then the command is executed and confirmation is returned.
- Given device offline, when lock or unlock is requested, then the system reports command failure and keeps current state unchanged.

### FR-006 Security Alert Notification

Requirement: The system shall generate and deliver security alerts to Home Owners when suspicious events are detected.

Acceptance Criteria:
- Given a detected security event, when alert processing runs, then a security alert is created and sent.
- Given successful delivery or failure, when processing ends, then alert status is recorded.

### FR-007 Predictive Battery Alert Notification

Requirement: The system shall generate predictive battery alerts when battery-risk conditions are identified.

Acceptance Criteria:
- Given battery-risk threshold reached, when analysis executes, then a predictive battery alert is issued.
- Given a predictive alert, when user views details, then an explanation summary is available.

### FR-008 Technician Support Request

Requirement: The system shall allow authenticated Home Owners to create technician support requests for device issues.

Acceptance Criteria:
- Given an authenticated Home Owner, when support request form is submitted, then a support request record is created.
- Given request creation, when technician queue is viewed, then the request appears with status and timestamp.

### FR-009 Battery Status Update

Requirement: The system shall accept battery status updates from authenticated Technician actions and authenticated IoT device telemetry.

Acceptance Criteria:
- Given authenticated update source, when battery data is submitted, then the system stores the new battery value.
- Given updated battery value, when risk analysis runs, then alert conditions are re-evaluated.

### FR-010 User and Role Management

Requirement: The system shall allow authenticated Administrators to create, update, deactivate, and role-assign user accounts.

Acceptance Criteria:
- Given an authenticated Administrator, when user changes are submitted, then account and role updates are persisted.
- Given non-administrator role, when user management is attempted, then access is denied.

### FR-011 Audit and System Log Access

Requirement: The system shall allow authenticated Administrators to view audit and system logs for monitoring and traceability.

Acceptance Criteria:
- Given authenticated Administrator, when logs page is opened, then audit records are listed with actor, action, and timestamp.
- Given filter criteria, when log query is submitted, then matching log entries are returned.

### FR-012 Device Telemetry Ingestion

Requirement: The system shall ingest simulated IoT telemetry data and store it for status dashboards and alert processing.

Acceptance Criteria:
- Given authenticated device payload, when telemetry is received, then data is validated and stored.
- Given malformed payload, when telemetry is received, then request is rejected with validation error.

### FR-013 Event Classification

Requirement: The system shall classify incoming security-relevant events to support appropriate alert generation.

Acceptance Criteria:
- Given incoming event data, when classification logic runs, then event category is assigned.
- Given unknown pattern, when classification fails, then event is marked for fallback handling and logged.

### FR-014 Alert History and Tracking

Requirement: The system shall retain alert records and expose their status history for authorized users.

Acceptance Criteria:
- Given generated alerts, when alert history is queried, then records are returned with type, time, and delivery status.
- Given alert delivery retry, when retry occurs, then status transition is recorded.

## Traceability Note

The requirements above align with the Phase 2 use cases in:
- ../use-case-model/global-use-case-diagram.md
- ../use-case-model/use-case-specifications.md