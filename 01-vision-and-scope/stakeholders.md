# SmartHome Guardian – Stakeholder Analysis

## 1. Overview

Stakeholders are individuals or entities that interact with or are affected by the SmartHome Guardian system.

Understanding stakeholders helps define:
- System requirements
- Responsibilities
- Security constraints (RBAC)
- System priorities

---

## 2. Primary Stakeholders (Direct Interaction)

### 2.1 Home Owner

#### Description
The main user of the system who owns and manages smart home devices.

#### Goals
- Monitor all devices in one dashboard
- Receive alerts (battery, security)
- Control smart lock remotely
- Avoid unexpected device failures

#### Pain Points
- Battery dies unexpectedly
- No visibility of device status
- Security threats not detected early

#### System Responsibilities
- Provide real-time device status
- Send predictive battery alerts
- Notify about unauthorized access
- Allow remote lock/unlock

---

### 2.2 Administrator

#### Description
Responsible for managing the system platform and users.

#### Goals
- Ensure system availability
- Manage users and roles
- Monitor system health

#### Pain Points
- System downtime
- Poor user management
- Lack of visibility into system-wide issues

#### System Responsibilities
- Provide user management tools
- Maintain system logs
- Ensure system reliability and security

---

### 2.3 Technician

#### Description
Handles device maintenance and troubleshooting.

#### Goals
- Diagnose device issues
- Replace or repair faulty devices
- Access device logs

#### Pain Points
- Lack of diagnostic data
- Delayed issue detection

#### System Responsibilities
- Provide device logs and status history
- Allow diagnostic access (restricted)
- Highlight devices at risk (battery, failure)

---

### 2.4 Simulated IoT Device

#### Description
Represents smart devices (smart lock, sensor, smart plug) interacting with the system.

#### Goals
- Send telemetry data (battery, status)
- Receive commands (e.g., lock/unlock)

#### Pain Points
- Network interruptions
- Battery limitations

#### System Responsibilities
- Receive and process device data
- Send commands reliably
- Handle offline/online transitions

---

## 3. Secondary Stakeholders (Indirect Interaction)

### 3.1 Security Environment (Contextual Stakeholder)

#### Description
Represents external risks such as unauthorized access attempts.

#### Impact
- Drives need for security alerts
- Influences system design (logging, detection)

---

### 3.2 Infrastructure Constraints

#### Description
Environmental conditions affecting system performance:
- Unstable electricity
- Intermittent internet

#### Impact
- Requires offline tolerance
- Requires lightweight system design
- Affects reliability strategies

---

## 4. Stakeholder Influence vs Interest

| Stakeholder        | Influence | Interest | Notes |
|------------------|----------|---------|------|
| Home Owner       | High     | High    | Primary user, drives core features |
| Administrator    | High     | Medium  | Controls system operation |
| Technician       | Medium   | Medium  | Supports maintenance |
| IoT Device       | Medium   | Low     | Data provider |
| Environment      | High     | Low     | Impacts constraints |

---

## 5. Security Considerations (MANDATORY)

- Role-Based Access Control (RBAC):
  - Home Owner → control own devices only
  - Technician → diagnostic access only
  - Admin → full system access

- Audit Logs:
  - All actions must be recorded (lock/unlock, alerts)

- Threat Awareness:
  - Unauthorized access attempts must trigger alerts

---

## 6. Key Insights

- The Home Owner is the most critical stakeholder → system design revolves around their needs
- The system must balance:
  - Security (alerts, access control)
  - Reliability (battery, offline mode)
- Devices are not “users” but active participants in the system

---

## 7. Stakeholder → System Mapping

| Stakeholder       | System Interaction |
|------------------|------------------|
| Home Owner       | UI dashboard, alerts, commands |
| Administrator    | System management interface |
| Technician       | Diagnostic interface |
| IoT Device       | API / telemetry communication |
