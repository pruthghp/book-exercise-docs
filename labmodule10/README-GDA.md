# Gateway Device Application (Connected Devices)

## Lab Module 10

### Description

#### What does your implementation do?

The implementation enables comprehensive bidirectional MQTT communication between the CDA and GDA with the following capabilities:

- **Secure Communication**: Supports TLS/SSL encrypted connections with X.509 certificate validation and optional username/password authentication loaded from separate credential files
- **Automated Subscriptions**: Automatically subscribes to three CDA topics (ActuatorResponse, SensorMsg, SystemPerfMsg) upon connection using dedicated inner class message listeners
- **Intelligent Humidity Control**: Monitors humidity sensor data from the CDA and triggers humidifier actuation commands when humidity remains below 30% or above 50% for 300 seconds, with automatic OFF commands when nominal levels (40%) are restored
- **Message Routing**: Receives and processes sensor data, system performance metrics, and actuator responses through type-specific listeners that convert JSON payloads to typed data objects
- **Bidirectional Control**: Publishes actuator commands to the CDA via MQTT while receiving and logging actuator response confirmations, completing the feedback loop

#### How does your implementation work?

**MQTT Communication:**

The MqttClientConnector uses MqttAsyncClient with three inner class listeners implementing IMqttMessageListener. Upon connection, connectComplete() automatically subscribes to CDA topics:

- **ActuatorResponseMessageListener**: Handles actuator responses from CDA
- **SensorDataMessageListener**: Processes sensor data and routes to DeviceDataManager
- **SystemPerformanceDataMessageListener**: Handles system performance metrics

Each listener deserializes JSON payloads using DataUtil and invokes the corresponding DeviceDataManager callback. The connection initialization loads TLS certificates and credentials from configuration files, falling back to insecure connections if encryption fails.

**Humidity Threshold Control:**

DeviceDataManager tracks humidity readings using latestHumiditySensorData and latestHumiditySensorTimeStamp. When handleSensorMessage() receives humidity data, it calls handleHumiditySensorAnalysis() which compares readings against floor (30%) and ceiling (50%) thresholds. If humidity remains exceptional for 300 seconds (calculated using ChronoUnit.SECONDS.between()), the method creates an ActuatorData command with ON/OFF state and publishes it to the CDA via sendActuatorCommandToCda(). This enables distributed control where CDA handles temperature locally while GDA manages humidity remotely.

### Code Repository and Branch:

**URL**: https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule10

### UML Design Diagram(s):

**Link**: https://drive.google.com/file/d/1LBusMDC9h644PGvXFNZrr4uiDKCFd5EU/view?usp=sharing

### Unit Tests Executed:

- None

### Integration Tests Executed:

- MqttClientConnectorTest
- DeviceDataManagerSimpleCdaActuationTest
- GatewayDeviceApp

