# Gateway Device Application (Connected Devices)
## Lab Module 11

### Description

#### What does your implementation do?

This implementation establishes cloud integration with Ubidots IoT platform, enabling the Gateway Device Application (GDA) to communicate with cloud services via MQTT over TLS. The system collects sensor data from the Constrained Device Application (CDA) and system performance metrics from the GDA, transmits this data to Ubidots for storage and analysis, and receives actuation commands from the cloud service to control edge devices.

**Key capabilities include:**
- Secure MQTT connectivity to Ubidots cloud service with TLS encryption and token-based authentication
- Bidirectional data flow between edge devices and cloud platform
- Automatic topic provisioning and subscription management with asynchronous connection handling
- LED actuation event processing from cloud-triggered commands
- System performance data decomposition into individual metrics (CPU and memory utilization)

#### How does your implementation work?

The implementation uses `CloudClientConnector` as the primary interface to Ubidots, delegating MQTT operations to `MqttClientConnector` which handles the underlying protocol communication. When the cloud connection completes, the `IConnectionListener` callback triggers automatic subscription to LED actuation topics. Data flows from the CDA through `DeviceDataManager` to `CloudClientConnector`, where it's converted to Ubidots-compatible `TimeAndValuePayloadData` JSON format and published to cloud topics following the `/v1.6/devices/` structure.

**Core workflow:**
- `MqttClientConnector` manages dual connections: local MQTT broker for CDA communication and Ubidots cloud broker for cloud integration
- `CloudClientConnector` implements `IConnectionListener` to receive connection completion notifications and provision cloud topics
- `SystemPerformanceData` is split into separate CPU and memory sensor readings before cloud transmission
- LED actuation events from Ubidots are received by `LedEnablementMessageListener`, converted to `ActuatorData`, and forwarded through `DeviceDataManager` to the CDA
- Asynchronous connection handling ensures LED topic subscription occurs only after cloud connection fully establishes


### Code Repository and Branch:

**URL:** [https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule11](https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule11)

### UML Design Diagram(s):

**URL:** [https://drive.google.com/file/d/1w44KFUMbII6dWYZ6IJBRS4HqJdCsrX2Z/view?usp=sharing](https://drive.google.com/file/d/1w44KFUMbII6dWYZ6IJBRS4HqJdCsrX2Z/view?usp=sharing)


### Unit Tests Executed:

- TimeAndValuePayloadDataTest

### Integration Tests Executed:

- MqttClientConnectorTest
- CloudClientConnectorTest
