# Component Diagram

Proposed primary subsystems:

- Device Monitoring Subsystem
- Alert Management Subsystem
- Cloud Processing Subsystem
- User Interface Subsystem
- Identity and Access Subsystem

## Purpose

This file is the architecture-level component view for SmartGuardian. It should show the major subsystems, their responsibilities, and the communication paths between them.

## Logical View Notes

The logical view is centered on subsystem responsibilities and dependencies:

- User Interface Subsystem depends on Identity and Access for protected actions.
- Device Monitoring Subsystem receives and stores simulated device status.
- Alert Management Subsystem evaluates telemetry and security events.
- Cloud Processing Subsystem coordinates persistence, prediction, and notification support.

## Process View Notes

The process view should describe runtime event flow and concurrency considerations:

- Simulated IoT devices submit telemetry.
- The platform validates device identity and payload structure.
- Monitoring logic updates device status.
- Alert logic evaluates battery and security risk.
- Notification and audit flows record alert outcomes.

## Completion Note

The detailed component diagram still needs to be expanded with a Mermaid component-style diagram or equivalent architecture notation.
