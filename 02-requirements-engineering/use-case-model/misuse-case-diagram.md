# Misuse Case Diagram

This misuse-case view documents threat actors and abuse scenarios relevant to SmartHome Guardian.

```mermaid
usecaseDiagram

actor "Unauthorized User" as UU
actor "Malicious Insider" as MI
actor "Network Attacker" as NA
actor "Fake IoT Device" as FD

rectangle "SmartHome Guardian System" {
    usecase "MC-01: Unauthorized Lock Control" as MC01
    usecase "MC-02: Fake Device Injection" as MC02
    usecase "MC-03: Alert Suppression" as MC03
    usecase "MC-04: Replay Attack on Commands" as MC04
    usecase "MC-05: Battery Prediction Manipulation" as MC05
    usecase "MC-06: Service Disruption / DoS" as MC06
    usecase "MC-07: Audit Log Tampering" as MC07
}

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
```

## Modeling Notes

| Misuse Case | Targeted Subsystems | Security Pillars Impacted |
| --- | --- | --- |
| MC-01 | Smart lock API, authentication | Security |
| MC-02 | Device identity, telemetry validation | Security, Reliability |
| MC-03 | Alert pipeline, notification reliability | Security, Reliability |
| MC-04 | Command channel integrity and anti-replay controls | Security |
| MC-05 | Battery prediction engine and telemetry trust | Reliability |
| MC-06 | Availability of API and dashboard services | Reliability |
| MC-07 | Audit trail integrity and traceability | Security, Reliability |