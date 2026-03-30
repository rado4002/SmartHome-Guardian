# Non-Functional Requirements

This section defines quality constraints and operational targets for SmartHome Guardian SaaS Platform.

## NFR Catalog

### NFR-001 Alert Latency

Requirement: Under normal operating conditions, the system shall deliver security and predictive alerts within 2 seconds from event detection.

Verification Criteria:
- p95 end-to-end alert delivery time is less than or equal to 2 seconds in baseline load tests.
- Alert pipeline timestamps show detection-to-dispatch duration within threshold for normal workload.

### NFR-002 Dashboard Response Time

Requirement: The monitoring dashboard shall return device status views within 2 seconds for standard household datasets.

Verification Criteria:
- p95 dashboard response time is less than or equal to 2 seconds for up to 100 linked devices per account.
- Status endpoint tests meet threshold under concurrent user load defined for MVP.

### NFR-003 Command Response Time

Requirement: Smart lock command acknowledgement shall be returned within 2 seconds under normal connectivity.

Verification Criteria:
- p95 lock or unlock command acknowledgement time is less than or equal to 2 seconds.
- Timeout and retry behavior is logged for outlier cases.

### NFR-004 Availability

Requirement: Monitoring, alerting, and core API services shall target 99.5% monthly availability in MVP operation windows.

Verification Criteria:
- Monthly uptime reports for core services are greater than or equal to 99.5%.
- Service health checks are continuously recorded and auditable.

### NFR-005 Reliability and Recovery

Requirement: The system shall recover from transient service or network failures without losing confirmed commands and accepted telemetry.

Verification Criteria:
- Retry and buffering mechanisms preserve accepted telemetry during temporary outages.
- Recovery tests demonstrate no loss of acknowledged control commands.

### NFR-006 Data Integrity

Requirement: Telemetry, alerts, and audit records shall preserve correctness, ordering per device where feasible, and immutable audit history.

Verification Criteria:
- Validation rejects malformed payloads.
- Audit entries are append-only and include actor, action, and timestamp fields.

### NFR-007 Transport Security

Requirement: All client-to-platform and device-to-platform communication shall use encrypted transport.

Verification Criteria:
- External interfaces require TLS-enabled endpoints.
- Security scans show no plaintext transport on production-facing paths.

### NFR-008 Authentication and Authorization

Requirement: The system shall enforce role-based access control for user operations and token-based authentication for device operations.

Verification Criteria:
- Unauthorized requests receive access denied responses.
- Access logs show policy enforcement for protected actions.

### NFR-009 Auditability and Traceability

Requirement: Security-relevant actions and administrative operations shall be fully traceable via audit logs.

Verification Criteria:
- Logs capture user or device identity, action type, timestamp, and result.
- Audit queries allow filtering by actor, action, and time range.

### NFR-010 Privacy and Data Minimization

Requirement: The system shall store only data necessary for monitoring, alerting, diagnostics, and auditing during MVP scope.

Verification Criteria:
- Data schema review confirms only required operational fields are persisted.
- Retention policy definitions exist for telemetry, alerts, and logs.

### NFR-011 Scalability

Requirement: The architecture shall support incremental onboarding of additional device types and increased telemetry throughput without redesign of core platform boundaries.

Verification Criteria:
- At least one new simulated device type can be integrated by adapter or schema extension.
- Load tests show stable ingestion behavior at increased message rates defined for MVP growth.

### NFR-012 Maintainability

Requirement: The system shall maintain clear modular boundaries, documented interfaces, and consistent naming to support future evolution.

Verification Criteria:
- Architecture and requirements artifacts remain traceable and versioned.
- Interface changes are reflected in documentation and change logs.

### NFR-013 Observability

Requirement: The platform shall expose operational metrics, structured logs, and alert-pipeline diagnostics sufficient for troubleshooting.

Verification Criteria:
- Runtime metrics include API latency, alert latency, telemetry ingestion rate, and error rate.
- Diagnostic logs enable root-cause analysis for failed command and alert scenarios.

### NFR-014 Usability

Requirement: Primary users shall be able to complete critical tasks (view status, receive alerts, control lock, request support) with low interaction complexity.

Verification Criteria:
- Scenario walkthroughs with stakeholders confirm completion of critical tasks without training-heavy steps.
- Usability review findings are documented and tracked.

### NFR-015 Compatibility and Access Channels

Requirement: The web interface shall support current major desktop and mobile browsers used in project demonstrations.

Verification Criteria:
- Demonstration checklist passes on at least two modern browser engines.
- Core workflows render correctly on desktop and mobile viewport sizes.

## Traceability Note

These non-functional requirements constrain and support the functional requirements and use cases defined in Phase 2 artifacts.