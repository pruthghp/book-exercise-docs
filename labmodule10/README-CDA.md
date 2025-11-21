# Constrained Device Application (Connected Devices)

## Lab Module 10

---

## Description:

### What does your implementation do?

This implementation extends the Constrained Device Application (CDA) to support secure, bidirectional MQTT communication with the Gateway Device Application (GDA). The CDA now receives actuator commands from the GDA via MQTT subscriptions, processes them through a callback-driven architecture, and autonomously responds to environmental conditions by triggering HVAC actuation when temperature thresholds are violated. All sensor data, system performance metrics, and actuator responses are automatically published to the GDA via MQTT, enabling comprehensive remote monitoring and control.

**Key capabilities added:**
- TLS encryption support for secure MQTT broker connections
- Automatic subscription to actuator command topics upon broker connection
- Topic-specific callback handling for incoming actuator commands from GDA
- Temperature-based automation that triggers HVAC adjustments when readings exceed configured ceiling (20°C) or drop below floor (18°C)
- Upstream transmission of all telemetry data (sensors, system performance, actuator responses) to GDA via MQTT
- Duplicate command filtering to prevent redundant actuations
- Non-blocking asynchronous MQTT operations to avoid deadlock

### How does your implementation work?

The implementation uses a layered callback architecture where MqttClientConnector handles MQTT protocol operations and DeviceDataManager orchestrates data flow. When connectClient() establishes a broker connection, the onConnect() callback automatically subscribes to the CDA_ACTUATOR_CMD_RESOURCE topic and registers onActuatorCommandMessage() as the topic-specific handler. Incoming actuator commands are deserialized from JSON to ActuatorData objects and passed to DeviceDataManager.handleActuatorCommandMessage(), which routes them through ActuatorAdapterManager to the appropriate emulator task (HvacEmulatorTask, HumidifierActuatorSimTask, etc.).

**Data flow mechanisms:**
- **Inbound (GDA → CDA):** MQTT subscription → onActuatorCommandMessage() → JSON deserialization → DeviceDataManager.handleActuatorCommandMessage() → ActuatorAdapterManager → Actuator task execution
- **Outbound (CDA → GDA):** Sensor/SysPerfManager → handleSensorMessage()/handleSystemPerformanceMessage() → JSON encoding → _handleUpstreamTransmission() → MQTT publish to GDA
- **Autonomous Control:** SensorAdapterManager polls every 5 seconds → _handleSensorDataAnalysis() checks temp thresholds → triggers handleActuatorCommandMessage() if violated → HVAC actuation → response sent to GDA
- **TLS Security:** Conditionally enabled via enableEncryption flag → loads PEM certificate → applies ssl.PROTOCOL_TLS_CLIENT → overrides port to 8883

The constructor parameter disableAllComms allows DeviceDataManager to bypass MQTT/CoAP initialization for isolated callback testing. Asynchronous operation is ensured by removing the blocking wait_for_publish() call, allowing the Paho MQTT client's background thread to handle publish confirmations without blocking subscription callbacks.


## Code Repository and Branch:

**URL:** https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule10


## UML Design Diagram(s):

**Link:** https://drive.google.com/file/d/1dH_bf2AlImDt7V7T0tT8BtsQxVeXSiSh/view?usp=sharing


## Unit Tests Executed:

- None

## Integration Tests Executed:

- MqttClientConnectorTest
- MqttClientPerformanceTest
- DeviceDataManagerCallbackTest
- DeviceDataManagerIntegrationTest
- ConstrainedDeviceApp

