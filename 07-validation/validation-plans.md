# Validation Plans

This document collects non-code validation plans for SmartGuardian. These plans support requirements and modeling evaluation without introducing implementation source code.

## 1. IoT Simulation Plan: Unauthorized Unlock Attempt

### Goal

Simulate how the system reacts when an unauthorized user tries to unlock a smart lock.

### Simulation Setup

- Device: Smart Lock (battery-powered)
- Actor: Unknown or unauthorized user
- System: SmartHome Guardian

### Steps

1. Unauthorized user sends unlock command.
2. System receives request.
3. System validates authentication token.
4. Token is invalid.
5. System rejects command.
6. System logs event.
7. System triggers security alert.

### Expected Results

- Door remains locked.
- Security alert is generated.
- Event is logged in the audit system.
- Notification is sent to the Home Owner.

### Observations

To be completed during validation review.

## 2. API Validation Plan

Document API-style interactions at the modeling level:

- Endpoint or operation under review
- Actor or device source
- Input data
- Expected response
- Expected status or validation result
- Related functional requirement

This is a requirements/modeling artifact, not an implementation test suite.

## 3. Alert Algorithm Evaluation

Describe the logic, assumptions, and evaluation criteria used for alert behavior and battery-risk predictions.

Recommended evaluation criteria:

- Correct security alert classification
- Correct predictive battery alert trigger
- Explainability of alert reasoning
- False positive and false negative risk
- Alignment with alert latency and auditability NFRs

## 4. Performance Evaluation

Use this section to summarize expected or reviewed outcomes for:

- Alert latency
- Message delivery reliability
- Throughput under simulated load

Include setup, measurements, and interpretation.
