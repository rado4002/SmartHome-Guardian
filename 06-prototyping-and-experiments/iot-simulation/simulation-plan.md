# IoT Simulation Plan – Smart Lock Security Scenario

## 1. Scenario: Unauthorized Unlock Attempt

### Goal
Simulate how the system reacts when an unauthorized user tries to unlock a smart lock.

---

## 2. Simulation Setup

- Device: Smart Lock (battery-powered)
- Actor: Unknown / Unauthorized User
- System: SmartHome Guardian

---

## 3. Steps

1. Unauthorized user sends unlock command
2. System receives request
3. System validates authentication token
4. Token is invalid
5. System rejects command
6. System logs event
7. System triggers security alert

---

## 4. Expected Results

- Door remains LOCKED
- Security alert generated
- Event logged in audit system
- Notification sent to Home Owner

---

## 5. Observations (TO FILL AFTER TEST)

- ...