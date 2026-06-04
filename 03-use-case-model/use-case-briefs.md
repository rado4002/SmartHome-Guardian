# Use Case Specifications

## Brief Use Cases (Essential Format)

### UC-01: Authenticate User

Home Owner, Administrator, or Technician provides credentials to gain authorized access.
Actor is registered in the system.
Actor is authenticated and granted role-based access.

### UC-02: Authenticate Device

Simulated IoT Device sends credentials or token to establish trust with the system.
Device is registered.
Device is authorized to communicate.

### UC-03: Register Device

Home Owner adds a new smart device to the system.
Actor is authenticated.
Device is linked to the actor account.

### UC-04: View Device Status

Home Owner requests current state of devices (battery, connectivity, status).
Actor is authenticated.
System provides real-time device data.

### UC-05: Control Smart Lock

Home Owner sends a command to lock or unlock the smart lock.
Actor is authenticated and the device is online.
Lock state is updated and logged.

### UC-06: Receive Security Alert

System notifies Home Owner of suspicious or unauthorized activity.
Security event is detected.
Alert is delivered and logged.

### UC-07: Receive Predictive Battery Alert

System sends Home Owner an alert when battery is predicted to run low.
AI module detects battery risk.
Alert is delivered with prediction explanation.

### UC-08: Request Technician Support

Home Owner requests assistance for device issues.
Actor is authenticated.
Support request is recorded.

### UC-09: Update Device Battery Status

Technician or Simulated IoT Device updates battery data in the system.
Device is authenticated.
Battery level is stored and analyzed.

### UC-10: Manage Users

Administrator manages user accounts and roles.
Administrator is authenticated.
User data is updated.

### UC-11: View System Logs

Administrator reviews audit and security logs.
Administrator is authenticated.
Logs are displayed.

### UC-12: Send Device Status Data

Simulated IoT Device transmits operational data to the system.
Device is authenticated.
Data is stored and processed.

## Fully Dressed Use Cases

Detailed fully dressed descriptions are maintained in a dedicated file for clarity:

- fully-dressed-use-case-specifications.md