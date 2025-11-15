## Constrained Device Application (Connected Devices)
### Lab Module 09
---
## Description:

### **What does your implementation do?**

My Lab Module 09 implementation creates a fully asynchronous CoAP client for the Constrained Device Application (CDA) using the aiocoap library, enabling comprehensive communication with the Gateway Device Application's CoAP server across all standard CoAP operations.

**Key features:**
- AsyncCoapClientConnector implements six core CoAP operations: Discovery, GET, PUT, POST, DELETE, and OBSERVE
- Resource discovery enumerates available resources on the GDA server through .well-known/core
- GET requests retrieve actuator command data from GDA
- PUT and POST requests transmit sensor telemetry and system performance data upstream to GDA
- DELETE requests remove resources from the server
- OBSERVE support enables real-time push notifications from observable resources, eliminating polling
- Support for both confirmable (CON) and non-confirmable (NON) message delivery modes
- Integration with DeviceDataManager through IDataMessageListener for processing incoming actuator commands

---
### **How does your implementation work?**

AsyncCoapClientConnector employs an event-driven asynchronous architecture using Python's asyncio framework with threading integration. During initialization, _initEventLoop() creates a dedicated asyncio event loop running in a separate daemon thread executing loop.run_forever(), and _initClientContext() establishes the aiocoap Context for CoAP communication.

**Implementation flow:**
- Public methods (sendGetRequest, sendPutRequest, etc.) construct resource paths, schedule async handlers on the event loop using run_coroutine_threadsafe(), and wait for results with configurable timeouts
- Async handlers create aiocoap Message objects with appropriate codes (GET, PUT, POST, DELETE), message types (CON/NON), and UTF-8 encoded payloads, then send via clientContext.request()
- Response handlers decode JSON payloads; _onGetResponse() specifically converts ActuatorData JSON and invokes DeviceDataManager.handleActuatorCommandMessage() callback
- OBSERVE implementation in _handleStartObserveRequest() sends GET with observe=0, stores request in observeRequests dictionary, processes initial response, then enters async for loop over req.observation for continuous notifications
- stopObserver() cancels the observation task, calls _handleStopObserveRequest() for cleanup, and removes entries from observeTasks and observeRequests dictionaries
- DeviceDataManager creates AsyncCoapClientConnector during initialization when enableCoapClient is true, passing itself as the data message listener

---
## Code Repository and Branch:

**URL:** [https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule09](https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule09)

---
## UML Design Diagram(s):

**Link:** [UML Class Diagram](https://drive.google.com/file/d/1Q1hMQSZ5YqQCl-0-fr5NTiQuniWa1Lu8/view?usp=sharing)

---
## Unit Tests Executed:

**None**

---
## Integration Tests Executed:

- CoapAsyncClientConnectorTest (Discovery, GET, PUT, POST, DELETE, OBSERVE)

---
**EOF.**
