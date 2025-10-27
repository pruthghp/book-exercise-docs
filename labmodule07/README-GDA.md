# Gateway Device Application (Connected Devices)

## Lab Module 07

## Description

### What does your implementation do?

This implementation enables **MQTT-based publish/subscribe communication** for the **Gateway Device Application (GDA)** using Java. The **MqttClientConnector** class acts as the primary communication interface between the GDA and an **MQTT broker (Mosquitto)**, supporting two-way messaging for telemetry data collection from constrained devices, actuator command distribution, and system management operations.

The connector implements both the **IPubSubClient** interface (for publish/subscribe operations) and the **MqttCallbackExtended** interface (for handling MQTT protocol events through callback mechanisms). Built using the **Eclipse Paho Java MQTT client library**, the implementation supports synchronous message delivery via **MqttClient** for reliable request–response communication and can also be configured to use **MqttAsyncClient** for asynchronous, concurrent message handling.

Integration with the **DeviceDataManager** automates lifecycle management, establishing broker connections during application startup, subscribing to multiple resource topics (such as GDA management status, CDA actuator responses, sensor messages, and system performance data), and performing graceful disconnection during shutdown.

The implementation fully supports all three **MQTT Quality of Service (QoS) levels (0, 1, and 2)** and handles all **14 MQTT 3.1.1 control packets** through detailed test cases, covering:

- **Connection establishment:** CONNECT / CONNACK
- **Keep-alive operations:** PINGREQ / PINGRESP
- **Message publishing and acknowledgments:** PUBLISH / PUBACK (QoS 1), PUBLISH / PUBREC / PUBREL / PUBCOMP (QoS 2)
- **Subscription management:** SUBSCRIBE / SUBACK, UNSUBSCRIBE / UNSUBACK
- **Graceful disconnection:** DISCONNECT

Overall, this setup ensures reliable, flexible, and standard-compliant MQTT communication between the GDA and the broker, adaptable for both synchronous and asynchronous use cases.

### How does your implementation work?

This implementation follows a **layered architecture**, where the **MqttClientConnector** manages all MQTT communication, while the **DeviceDataManager** oversees application-level message handling and routing.

During initialization, the connector retrieves configuration parameters from **PiotConfig.props**, including the broker address, port, communication protocol, keep-alive interval, default QoS level, and client operation mode (synchronous or asynchronous). The constructor sets up a **MemoryPersistence** mechanism for message storage, creates **MqttConnectOptions** with key parameters (keep-alive interval, persistent sessions by setting clean session to false, and automatic reconnection enabled), and constructs the broker URL.

The **connectClient()** method instantiates the **Paho MqttClient** using the broker address, generated client ID, and persistence configuration. It registers the connector as the callback handler for MQTT events, establishes a TCP connection to the broker, and starts the message processing loop.

Each callback method handles a specific MQTT protocol event:

- **connectComplete()** – Logs successful connections and reconnection status.
- **connectionLost()** – Handles unexpected disconnections and logs errors.
- **deliveryComplete()** – Confirms successful message publication.
- **messageArrived()** – Processes incoming messages on subscribed topics.

The **publishMessage()** method validates topics and QoS levels, converts message strings to byte arrays, creates **MqttMessage** objects with appropriate QoS settings, and publishes them to the broker. The **subscribeToTopic()** and **unsubscribeFromTopic()** methods manage topic subscriptions while validating QoS levels for correctness.

Integration with the **DeviceDataManager** occurs through three main lifecycle methods:
- **initManager()** – Instantiates the **MqttClientConnector** when MQTT functionality is enabled.
- **startManager()** – Connects to the broker and subscribes to four key resource topics:
  - GDA management status messages
  - CDA actuator responses
  - CDA sensor messages
  - CDA system performance messages
- **stopManager()** – Unsubscribes from all topics and gracefully disconnects from the broker.

Finally, a comprehensive test suite validates all core functionalities, including connection lifecycle management, keep-alive mechanisms, and publish/subscribe operations across all QoS levels. This ensures full compliance with the **MQTT 3.1.1 protocol** and reliable communication between devices and the GDA.

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

---

**EOF**
