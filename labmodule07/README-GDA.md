# Gateway Device Application (Connected Devices)

## Lab Module 07

## Description

## What does your implementation do?

<div align="justify">
This implementation enables <b>MQTT-based publish/subscribe communication</b> for the <b>Gateway Device Application (GDA)</b> using Java. The <b>MqttClientConnector</b> class acts as the primary communication interface between the GDA and an <b>MQTT broker (Mosquitto)</b>, supporting two-way messaging for telemetry data collection from constrained devices, actuator command distribution, and system management operations.
</div>

<div align="justify">
The connector implements both the <b>IPubSubClient</b> interface (for publish/subscribe operations) and the <b>MqttCallbackExtended</b> interface (for handling MQTT protocol events through callback mechanisms). Built using the <b>Eclipse Paho Java MQTT client library</b>, the implementation supports synchronous message delivery via <b>MqttClient</b> for reliable request–response communication and can also be configured to use <b>MqttAsyncClient</b> for asynchronous, concurrent message handling.
</div>

<div align="justify">
Integration with the <b>DeviceDataManager</b> automates lifecycle management, establishing broker connections during application startup, subscribing to multiple resource topics (such as GDA management status, CDA actuator responses, sensor messages, and system performance data), and performing graceful disconnection during shutdown.
</div>

<div align="justify">
The implementation fully supports all three <b>MQTT Quality of Service (QoS) levels (0, 1, and 2)</b> and handles all <b>14 MQTT 3.1.1 control packets</b> through detailed test cases, covering:
</div>

- **Connection establishment:** CONNECT / CONNACK
- **Keep-alive operations:** PINGREQ / PINGRESP
- **Message publishing and acknowledgments:** PUBLISH / PUBACK (QoS 1), PUBLISH / PUBREC / PUBREL / PUBCOMP (QoS 2)
- **Subscription management:** SUBSCRIBE / SUBACK, UNSUBSCRIBE / UNSUBACK
- **Graceful disconnection:** DISCONNECT

<div align="justify">
Overall, this setup ensures reliable, flexible, and standard-compliant MQTT communication between the GDA and the broker, adaptable for both synchronous and asynchronous use cases.
</div>


## How does your implementation work?

<div align="justify">
This implementation follows a <b>layered architecture</b>, where the <b>MqttClientConnector</b> manages all MQTT communication, while the <b>DeviceDataManager</b> oversees application-level message handling and routing.
</div>

<div align="justify">
During initialization, the connector retrieves configuration parameters from <b>PiotConfig.props</b>, including the broker address, port, communication protocol, keep-alive interval, default QoS level, and client operation mode (synchronous or asynchronous). The constructor sets up a <b>MemoryPersistence</b> mechanism for message storage, creates <b>MqttConnectOptions</b> with key parameters (keep-alive interval, persistent sessions by setting clean session to false, and automatic reconnection enabled), and constructs the broker URL.
</div>

<div align="justify">
The <b>connectClient()</b> method instantiates the <b>Paho MqttClient</b> using the broker address, generated client ID, and persistence configuration. It registers the connector as the callback handler for MQTT events, establishes a TCP connection to the broker, and starts the message processing loop.
</div>

Each callback method handles a specific MQTT protocol event:
- **connectComplete()** – Logs successful connections and reconnection status.
- **connectionLost()** – Handles unexpected disconnections and logs errors.
- **deliveryComplete()** – Confirms successful message publication.
- **messageArrived()** – Processes incoming messages on subscribed topics.

<div align="justify">
The <b>publishMessage()</b> method validates topics and QoS levels, converts message strings to byte arrays, creates <b>MqttMessage</b> objects with appropriate QoS settings, and publishes them to the broker. The <b>subscribeToTopic()</b> and <b>unsubscribeFromTopic()</b> methods manage topic subscriptions while validating QoS levels for correctness.
</div>

<div align="justify">
Integration with the <b>DeviceDataManager</b> occurs through three main lifecycle methods:
</div>

- **initManager()** – Instantiates the **MqttClientConnector** when MQTT functionality is enabled.
- **startManager()** – Connects to the broker and subscribes to four key resource topics:
  - GDA management status messages
  - CDA actuator responses
  - CDA sensor messages
  - CDA system performance messages
- **stopManager()** – Unsubscribes from all topics and gracefully disconnects from the broker.

<div align="justify">
Finally, a comprehensive test suite validates all core functionalities, including connection lifecycle management, keep-alive mechanisms, and publish/subscribe operations across all QoS levels. This ensures full compliance with the <b>MQTT 3.1.1 protocol</b> and reliable communication between devices and the GDA.
</div>

## Code Repository and Branch

URL: https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule07

## UML Design Diagram(s)

URL: https://drive.google.com/file/d/1AVPSEQJfmzTvcD9CqFfqFADDxyCOYXw3/view?usp=sharing

## Unit Tests Executed

- **Old:** All Part 01 and Part 02 unit tests
- **New:** None

## Integration Tests Executed

- **Old:** All Part 01 and Part 02 integration tests
- **New:**
  - **MqttClientConnectorTest**
  - **MqttClientControlPacketTest**

**EOF**
