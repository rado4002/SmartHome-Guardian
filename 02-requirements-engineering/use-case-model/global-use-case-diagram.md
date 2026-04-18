# Global Use Case Diagram

The diagram below refines actor responsibilities and aligns include and extend relationships with UML intent.

```mermaid
flowchart TB

%% Actors
HO["Home Owner"]
Tech["Technician"]
Admin["Administrator"]
IoT["Simulated IoT Device"]

%% System Boundary
subgraph System["SmartHome Guardian SaaS Platform"]

Auth["Authenticate User"]
AuthDev["Authenticate Device"]
RegDev["Register Device"]
ViewStatus["View Device Status"]
ControlLock["Control Smart Lock"]
ReceiveAlert["Receive Alert"]
SecAlert["Receive Security Alert"]
PredAlert["Receive Predictive Battery Alert"]
ReqTech["Request Technician Support"]
UpdateBatt["Update Device Battery Status"]
ManageUsers["Manage Users"]
ViewLogs["View System Logs"]
SendData["Send Device Status Data"]

end

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

%% Includes (modeled as dependencies)
RegDev -. include .-> Auth
ViewStatus -. include .-> Auth
ControlLock -. include .-> Auth
ReqTech -. include .-> Auth
UpdateBatt -. include .-> Auth
ManageUsers -. include .-> Auth
ViewLogs -. include .-> Auth
SendData -. include .-> AuthDev

%% Extends (optional behavior)
SecAlert -. extend .-> ViewStatus
PredAlert -. extend .-> ViewStatus
ReqTech -. extend .-> PredAlert
ControlLock -. extend .-> ViewStatus

%% Generalization
SecAlert --> ReceiveAlert
PredAlert --> ReceiveAlert

```

## Modeling Notes

- Authentication is modeled as an included use case for all protected user actions.
- Device telemetry submission includes device authentication before data is accepted.
- Receive Alert is modeled as a parent use case, specialized into security and predictive battery alert variants.
- Alerts are modeled as conditional extensions of status monitoring, since they are triggered only under specific conditions.
- Smart lock control extends status monitoring when lock action is initiated from a monitoring context.
- Simulated IoT Device is treated as an external actor sending telemetry to the platform.