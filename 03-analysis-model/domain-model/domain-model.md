# Domain Model

classDiagram
```mermaid
%% =======================
%% USERS
%% =======================
class User {
  userId
  name
  role
}

class HomeOwner
class Administrator
class Technician

User <|-- HomeOwner
User <|-- Administrator
User <|-- Technician

%% =======================
%% DEVICES
%% =======================
class Device {
  deviceId
  status
  lastSeen
}

class SmartLock {
  lockState
}

class Sensor {
  sensorType
}

class SmartPlug {
  powerUsage
}

Device <|-- SmartLock
Device <|-- Sensor
Device <|-- SmartPlug

%% =======================
%% BATTERY
%% =======================
class Battery {
  level
  healthStatus
}

Device "1" *-- "1" Battery : has

%% =======================
%% ALERTS
%% =======================
class Alert {
  alertId
  timestamp
  severity
}

class PredictiveAlert {
  remainingTime
  confidence
  explanation
}

class SecurityAlert

Alert <|-- PredictiveAlert
Alert <|-- SecurityAlert

Device "1" --> "*" Alert : generates

%% =======================
%% SECURITY EVENTS
%% =======================
class SecurityEvent {
  eventId
  timestamp
}

class AccessAttempt {
  success
}

SecurityEvent <|-- AccessAttempt

Device "1" --> "*" SecurityEvent : detects

%% =======================
%% RELATIONSHIPS
%% =======================
HomeOwner "1" --> "*" Device : owns
Technician "1" --> "*" Device : maintains
Administrator "1" --> "*" Device : manages

HomeOwner "1" --> "*" Alert : receives

```