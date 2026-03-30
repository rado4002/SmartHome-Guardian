# Global Use Case Diagram

The diagram below refines actor responsibilities and aligns include and extend relationships with UML intent.

```mermaid
usecaseDiagram

actor "Home Owner" as HO
actor "Technician" as Tech
actor "Administrator" as Admin
actor "Simulated IoT Device" as IoT

rectangle "SmartHome Guardian SaaS Platform" {
	usecase "Authenticate User" as Auth
	usecase "Authenticate Device" as AuthDev
	usecase "Register Device" as RegDev
	usecase "View Device Status" as ViewStatus
	usecase "Control Smart Lock" as ControlLock
	usecase "Receive Alert" as ReceiveAlert
	usecase "Receive Security Alert" as SecAlert
	usecase "Receive Predictive Battery Alert" as PredAlert
	usecase "Request Technician Support" as ReqTech
	usecase "Update Device Battery Status" as UpdateBatt
	usecase "Manage Users" as ManageUsers
	usecase "View System Logs" as ViewLogs
	usecase "Send Device Status Data" as SendData
}

%% Direct Associations
HO --> RegDev
HO --> ViewStatus
HO --> ControlLock
HO --> ReceiveAlert
HO --> ReqTech

Tech --> UpdateBatt
Admin --> ManageUsers
Admin --> ViewLogs
IoT --> SendData

%% Mandatory Includes (authentication required)
RegDev ..> Auth : <<include>>
ViewStatus ..> Auth : <<include>>
ControlLock ..> Auth : <<include>>
ReqTech ..> Auth : <<include>>
UpdateBatt ..> Auth : <<include>>
ManageUsers ..> Auth : <<include>>
ViewLogs ..> Auth : <<include>>
SendData ..> AuthDev : <<include>>

%% Conditional Extends (optional behavior)
SecAlert ..> ViewStatus : <<extend>>
PredAlert ..> ViewStatus : <<extend>>
ReqTech ..> PredAlert : <<extend>>
ControlLock ..> ViewStatus : <<extend>>

%% Use Case Generalization
ReceiveAlert <|-- SecAlert
ReceiveAlert <|-- PredAlert
```

## Modeling Notes

- Authentication is modeled as an included use case for all protected user actions.
- Device telemetry submission includes device authentication before data is accepted.
- Receive Alert is modeled as a parent use case, specialized into security and predictive battery alert variants.
- Alerts are modeled as conditional extensions of status monitoring, since they are triggered only under specific conditions.
- Smart lock control extends status monitoring when lock action is initiated from a monitoring context.
- Simulated IoT Device is treated as an external actor sending telemetry to the platform.