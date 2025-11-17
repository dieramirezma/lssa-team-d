# Rail Transportation System - Architectural Design

## Team Information

**Team:** D

**Team Members:**
- Miguel Angel Rubiano
- Martin Erxleben
- Diego Nicolas Ramirez Maldonado
- Jesus Ernesto Quiñones Cely

---

## Software System

**System Name:** Rail Transportation System

**Purpose:**
The Rail Transportation System is responsible for managing and coordinating freight rail operations across Colombia's national railway network. It orchestrates the movement of cargo from ports (Caribbean and Pacific) to inland distribution centers and vice versa. The system integrates real-time fleet tracking, route optimization, cargo inventory management, and IoT-based monitoring of trains, stations, and cargo containers. By providing visibility and control over rail logistics operations, this system ensures efficient, safe, and coordinated cargo transportation as part of the broader national logistics ecosystem.

---

## Iterations

### Iteration 1

#### 1. Model

##### Metamodel

```md
System:
    'sos' ':' systems+=SubSystem*
;
SubSystem:
    'subsystem' ':' name=ID '{'
        elements+=Element*
    '}'
;
Element:
    Component | Connector
;
Component:
    PhysicalComponent | LogicalComponent
;
LogicalComponent:
    'component' type=ComponentType name=ID
;
PhysicalComponent:
    Vehicle | HardwareDevice
;
Connector:
    'connector' type=ConnectorType fromComponent=[Component] '->' toComponent=[Component]
;
ComponentType:
    'frontend' | 'backend' | 'database' | 'channel' | 'loadBalancer' | 'fileStorage' | 'apiGateway'
    | 'sensor' | 'vehicle' | 'container' | 'eventBus' | 'proxy' | 'interface' | 'cache' | 'monitoringService' | 'healthChecker'
;
ConnectorType:
    'RESTful' | 'dbConnector' | 'gRPC' | 'MQTT' | 'SDK' | 'WebSocket' | 'amqp'  | 'CoAP' | 'gsm' | 'sqs' | 'GraphQL' | 'health_probe' | 'logging_stream'
;
HardwareDevice:
    'hardware' type=HardwareType name=ID
;
HardwareType:
    'IoT' | 'radio' | 'weightSensor' | 'velocitySensor' | 'sensor_de_distancia' | 'weather_station' | 'entry_point' | 'sensor_de_caudal' | 'GPS'
;
Vehicle:
    'vehicle' name=ID '{'
        'type' type=VehicleType
        sensors+=HardwareDevice*
    '}'
;
VehicleType:
    'truck' | 'tren' | 'vessel' | 'grua'
;
```

##### Subsystem implementation

```md
sos:
subsystem: rts {
    component apiGateway ros
    component backend idms
    component backend fms
    component backend cms
    component backend rps
    component apiGateway sl
    component frontend station_ui
    component frontend train_ui
    component database cms_db
    component fileStorage rps_db
    component database fms_db
    component channel broker
    component loadBalancer rts_load_balancer
    component cache rts_cache
    component monitoringService rts_observability
    component healthChecker rts_healthcheck

    component vehicle train1 { type tren }
    component container container1 {type container}
    component entryPoint???? station1 {type ???}

    connector dbConnector cms -> cms_db
    connector dbConnector rps -> rps_db
    connector dbConnector fms -> fms_db

    connector amqp ros -> broker
    connector amqp cms -> broker
    connector amqp rps -> broker
    connector amqp fms -> broker
    connector amqp broker -> ros
    connector amqp broker -> cms
    connector amqp broker -> rps
    connector amqp broker -> fms

    connector CoAP sl -> idms
    connector MQTT fms -> idms
    connector MQTT cms -> idms
    connector CoAP idms -> sl
    connector MQTT idms -> fms
    connector MQTT idms -> cms
    connector gsm train1 -> sl
    connector gsm container1 -> sl
    connector CoAP station1 -> sl

    connector RESTful station_ui -> ros
    connector RESTful train_ui -> ros
    connector RESTful rts_load_balancer -> ros
    connector RESTful rts_load_balancer -> cms
    connector RESTful rts_load_balancer -> fms
    connector RESTful rts_load_balancer -> rps
    connector RESTful rts_load_balancer -> idms
    connector health_probe rts_healthcheck -> fms
    connector health_probe rts_healthcheck -> cms
    connector health_probe rts_healthcheck -> rps
    connector logging_stream rts_observability -> broker
    connector RESTful fms -> rts_cache
}
```

##### C&C View

![alt text](image-1.png)

---

#### 2. Objective

##### Case Scenario

Extreme Overload Due to Peak Traffic During the Holiday Season

Simultaneous convergence of multiple critical traffic flows on December 24 at 6:00 a.m.:
- Caribbean Port Management System: Release of 2,500 containers held for urgent customs inspection
- Pacific Port Management System: Simultaneous arrival of 3 cargo ships with 1,800 additional containers
- National Coordination System: Request for mass reallocation of 150 loads from road transport (idle trucks) to rail

Artifact:
- Load Balancer Pattern: Configuration in C&C graph with **round-robin** balancing algorithm with health checks
- Horizontal Scaling Pattern: Multiple instances of critical services according to docker-compose.yml
- Database Connection Pool Pattern: Implemented in app.py and app.py
- Caching Pattern: Caching frequent routes in RPS to reduce redundant calculations
- Asynchronous Processing Pattern: Event Message Broker with persistent queues to decouple processing
- Rate Limiting Pattern: Throttling implemented in the Gateway to protect backend services

---

#### 3. Action

##### Simulation

##### Analysis


---

### Iteration 2

#### 1. Model

##### Metamodel

##### Subsystem implementation

##### C&C View

#### 2. Objective

##### Case Scenario

Extreme Overload Due to Peak Traffic During the Holiday Season

Simultaneous convergence of multiple critical traffic flows on December 24 at 6:00 a.m.:
- Caribbean Port Management System: Release of 2,500 containers held for urgent customs inspection
- Pacific Port Management System: Simultaneous arrival of 3 cargo ships with 1,800 additional containers
- National Coordination System: Request for mass reallocation of 150 loads from road transport (idle trucks) to rail

Artifact:
- Load Balancer Pattern: Configuration in C&C graph with **weighted round-robin** balancing algorithm with health checks
- Horizontal Scaling Pattern: Multiple instances of critical services according to docker-compose.yml
- Database Connection Pool Pattern: Implemented in app.py and app.py
- Caching Pattern: Caching frequent routes in RPS to reduce redundant calculations
- Asynchronous Processing Pattern: Event Message Broker with persistent queues to decouple processing
- Rate Limiting Pattern: Throttling implemented in the Gateway to protect backend services

---

#### 3. Action

##### Simulation

##### Analysis

---

### Iteration 3

#### 1. Model

##### Metamodel

##### Subsystem implementation

##### C&C View

#### 2. Objective

##### Case Scenario

---

#### 3. Action

##### Simulation

##### Analysis

---

### Iteration 4

#### 1. Model

##### Metamodel

##### Subsystem implementation

##### C&C View

#### 2. Objective

##### Case Scenario

---

#### 3. Action

##### Simulation

##### Analysis