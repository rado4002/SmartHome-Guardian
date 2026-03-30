# Fully Dressed Use Case Specifications

## FD-UC-01: Control Smart Lock

- Use case name: Control Smart Lock
- Scope: SmartHome Guardian SaaS Platform
- Level: User goal
- Primary Actor: Home Owner
- Stakeholders and interests:
  - Home Owner: Wants secure and immediate lock or unlock response.
  - Administrator: Wants traceable actions and policy compliance.
  - Security team: Wants reduced unauthorized access risk.
- Preconditions:
  - Home Owner is authenticated.
  - Smart lock is registered to the account.
  - Smart lock is online and reachable.
- Success Guarantee:
  - Requested lock state is applied.
  - Action is recorded in audit logs with timestamp and actor ID.
  - User receives confirmation of final lock state.
- Main Success Scenario:
  1. Home Owner opens lock control interface.
  2. System shows current lock state.
  3. Home Owner selects lock or unlock command.
  4. System validates authorization and command policy.
  5. System sends command to smart lock.
  6. Smart lock executes command and returns status.
  7. System updates dashboard state.
  8. System stores audit log entry.
  9. System confirms success to Home Owner.
- Extensions:
  - 4a. Authorization fails: System denies action and logs security event.
  - 5a. Device timeout: System retries once, then reports failure.
  - 6a. Device rejects command: System preserves previous state and notifies user.
  - 8a. Logging service unavailable: System queues log event for deferred write.
- Special Requirements:
  - Command acknowledgement latency should be under 2 seconds in normal conditions.
  - All command channels must use encrypted transport.
  - Every control action must be non-repudiable in audit logs.
- Technology and Data variation list:
  - Command channel: WebSocket or HTTPS API.
  - Device protocol adapter: MQTT bridge or REST bridge.
  - Input variation: Manual tap from dashboard or quick action widget.
- Frequency of occurrence:
  - Moderate to high (multiple times per day per active household).
- Miscellaneous:
  - Open issue: Define behavior when two users send conflicting commands simultaneously.

## FD-UC-02: Send Device Status Data

- Use case name: Send Device Status Data
- Scope: SmartHome Guardian SaaS Platform
- Level: Sub function
- Primary Actor: Simulated IoT Device
- Stakeholders and interests:
  - Home Owner: Wants accurate, up-to-date device health information.
  - Technician: Wants reliable diagnostics data for maintenance.
  - Administrator: Wants stable system load and trustworthy telemetry.
- Preconditions:
  - Device is registered and authenticated.
  - Device has active connectivity to gateway or cloud endpoint.
  - Data schema version is supported by the platform.
- Success Guarantee:
  - Telemetry packet is accepted and stored.
  - Device status views are refreshed with latest values.
  - Downstream analytics can consume the new data.
- Main Success Scenario:
  1. Device collects telemetry values (battery, status, connectivity, energy).
  2. Device creates payload with timestamp and device identifier.
  3. Device sends payload to platform ingestion endpoint.
  4. Platform validates authentication token and payload schema.
  5. Platform stores telemetry in status repository.
  6. Platform updates latest-state cache for dashboard queries.
  7. Platform emits internal event for alert and analytics pipelines.
  8. Platform returns acknowledgement to device.
- Extensions:
  - 3a. Transmission failure: Device retries using backoff policy.
  - 4a. Token invalid: Platform rejects payload and requests re-authentication.
  - 4b. Schema invalid: Platform rejects payload with validation error code.
  - 5a. Storage unavailable: Platform writes to buffer queue and processes later.
- Special Requirements:
  - Ingestion endpoint availability target should support continuous monitoring service.
  - Telemetry data in transit must be encrypted.
  - Processing must preserve event ordering per device where feasible.
- Technology and Data variation list:
  - Transport protocol: MQTT publish or HTTPS POST.
  - Payload format: JSON compact or JSON verbose profile.
  - Sampling variation: periodic interval or event-triggered update.
- Frequency of occurrence:
  - Very high (continuous stream from each active device).
- Miscellaneous:
  - Open issue: Final retention period for raw telemetry versus aggregated metrics is pending.
