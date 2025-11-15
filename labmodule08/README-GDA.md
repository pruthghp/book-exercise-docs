## Gateway Device Application (Connected Devices)
### Lab Module 08
---
## Description:

### **What does your implementation do?**

My Lab Module 08 implementation creates a CoAP server for the Gateway Device Application (GDA) using the Eclipse Californium framework, enabling RESTful communication with Constrained Device Applications through a hierarchical resource-oriented architecture.

**Key features:**
- Three specialized resource handlers organized in PIOT/ConstrainedDevice/ResourceName hierarchy
- GetActuatorCommandResourceHandler provides actuator commands with observable support for real-time push notifications
- UpdateTelemetryResourceHandler receives sensor data via PUT requests
- UpdateSystemPerformanceResourceHandler receives system performance metrics via PUT requests
- Resource discovery through standard .well-known/core endpoint
- Support for GET, PUT, POST, and DELETE operations with appropriate response codes
- Integration with DeviceDataManager through IDataMessageListener and IActuatorDataListener interfaces

---
### **How does your implementation work?**

CoapServerGateway initializes during DeviceDataManager construction, creating a Californium CoapServer and registering resource handlers through initDefaultResources(). The createAndAddResourceChain() method parses resource paths into segments, builds the tree hierarchy with intermediate CoapResource nodes, and attaches handlers at leaf positions.

**Implementation flow:**
- Update handlers receive PUT requests with JSON payloads, convert to SensorData or SystemPerformanceData using DataUtil, and invoke DeviceDataManager callbacks
- GetActuatorCommandResourceHandler implements IActuatorDataListener to receive updates from DeviceDataManager, maintains local ActuatorData state, and calls super.changed() to notify observers
- Server lifecycle integrates with DeviceDataManager's startManager()/stopManager() methods
- MessageTracer interceptors attached to all endpoints for debugging
- Observable pattern enabled via setObservable(true) for real-time CDA notifications

---
## Code Repository and Branch:

**URL:** [https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule08](https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule08)

---
## UML Design Diagram(s):

**Link:** [UML Class Diagram](https://drive.google.com/file/d/1aopzO0FYRp3JCByrdvljhm1cxh_f2OHM/view?usp=sharing)

---
## Unit Tests Executed:

**None**

---
## Integration Tests Executed:

- CoapClientToServerConnectorTest
- CoapServerGatewayTest

---
**EOF.**
