```mermaid
%% SmartHome Guardian – Misuse Case Diagram 


%% Actors
actor "Unauthorized User" as UU
actor "Malicious Insider" as MI
actor "Network Attacker" as NA
actor "Fake IoT Device" as FD

%% System Boundary
rectangle "SmartHome Guardian System" {
    %% Misuse Cases
    usecase "MC-01: Unauthorized Lock Control" as MC01
    usecase "MC-02: Fake Device Injection" as MC02
    usecase "MC-03: Alert Suppression" as MC03
    usecase "MC-04: Replay Attack on Commands" as MC04
    usecase "MC-05: Battery Prediction Manipulation" as MC05
    usecase "MC-06: Service Disruption / DoS" as MC06
    usecase "MC-07: Audit Log Tampering" as MC07
}

%% Relationships
UU --> MC01
UU --> MC03
UU --> MC04
UU --> MC06

MI --> MC01
MI --> MC03
MI --> MC07

NA --> MC03
NA --> MC04
NA --> MC06

FD --> MC02
FD --> MC05

%% Notes for clarity
note right of MC01
    Targets: Smart Lock API, Authentication
    Pillars: Security 🔐
end note

note right of MC02
    Targets: Device Identity, Telemetry Validation
    Pillars: Battery 🔋, Security 🔐
end note

note right of MC03
    Targets: Alert Pipeline, Reliability
    Pillars: Reliability 🌐, Security 🔐
end note

note right of MC04
    Targets: Command Replay Prevention
    Pillars: Security 🔐
end note

note right of MC05
    Targets: Battery Prediction Engine
    Pillars: Battery 🔋, Reliability 🌐
end note

note right of MC06
    Targets: Availability, Dashboard
    Pillars: Reliability 🌐
end note

note right of MC07
    Targets: Audit Logs Integrity
    Pillars: Security 🔐, Reliability 🌐
end note

```
---

### ✅ Features of This Diagram

1. **Actors**: Unauthorized User, Malicious Insider, Network Attacker, Fake IoT Device.
2. **Misuse Cases**: MC-01 → MC-07 exactly as defined.
3. **System Boundary**: “SmartHome Guardian System” rectangle.
4. **Connections**: Actors linked to misuse cases they can attempt.
5. **Notes**: Each misuse case annotated with:
   - Targeted subsystem
   - Anchored pillars: Security 🔐, Battery 🔋, Reliability 🌐

---