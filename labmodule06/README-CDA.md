# Constrained Device Application (Connected Devices)

## Lab Module 06

Be sure to implement all the PIOT-CDA-* issues (requirements) listed at PIOT-INF-06-001 - Lab Module 06.

## Description

### What does your implementation do?

This implementation enables **MQTT-based publish/subscribe communication** for the **Constrained Device Application (CDA)**. The **MqttClientConnector** class acts as the main communication bridge between the CDA and an **MQTT broker (Mosquitto)**, facilitating two-way messaging for telemetry data, actuator commands, and system management information.

It implements the **IPubSubClient** interface and leverages the **Eclipse Paho MQTT Python client** to handle connections, manage topic subscriptions, publish messages, and respond to MQTT protocol events through callback functions.

By integrating with the **DeviceDataManager**, the connector automatically manages connection setup and teardown during application startup and shutdown. It also allows configurable topic subscriptions to receive actuator commands from remote services.

The implementation supports all three **MQTT Quality of Service (QoS) levels (0, 1, and 2)** and covers all **14 MQTT 3.1.1 control packets** through detailed test cases. These include:
- **Connection management:** CONNECT / CONNACK
- **Keep-alive operations:** PINGREQ / PINGRESP
- **Message publishing and acknowledgment:** PUBLISH / PUBACK (QoS 1), PUBLISH / PUBREC / PUBREL / PUBCOMP (QoS 2)
- **Subscription management:** SUBSCRIBE / SUBACK, UNSUBSCRIBE / UNSUBACK
- **Graceful disconnection:** DISCONNECT

Overall, this setup ensures reliable and standards-compliant MQTT communication between the CDA and the broker.

### How does your implementation work?

This implementation follows a **layered architecture**, where the **MqttClientConnector** functions as the communication layer between the CDA and the MQTT broker.

During initialization, the connector loads configuration parameters from **PiotConfig.props**, such as the broker host, port number, keep-alive interval, default QoS level, and client ID. The **connectClient()** method then creates a **Paho MQTT client** using the specified client ID and clean session flag, registers key callback handlers (**onConnect, onDisconnect, onMessage, onPublish, onSubscribe**), establishes a TCP connection to the broker, and starts the network loop thread for asynchronous message handling.

Each callback method serves a distinct purpose:
- **onConnect** – Logs a successful connection to the broker.
- **onDisconnect** – Manages disconnection events and cleanup.
- **onMessage** – Decodes incoming UTF-8 payloads and triggers the registered **IDataMessageListener** for processing.
- **onPublish** – Confirms successful message publication.
- **onSubscribe** – Verifies that topic subscriptions are active.

The **publishMessage()** method ensures the validity of the resource topic and QoS level before publishing a message to the broker. It uses **wait_for_publish()** to confirm reliable delivery. Similarly, **subscribeToTopic()** and **unsubscribeFromTopic()** manage topic subscriptions while validating QoS levels.

Integration with the **DeviceDataManager** occurs through the **startManager()** and **stopManager()** lifecycle methods. These methods handle connection setup, subscribe to the **CDA_ACTUATOR_CMD_RESOURCE** topic for receiving actuator commands, and ensure a clean disconnection during shutdown. The **setDataMessageListener()** method enables callback delegation for incoming MQTT messages, allowing the **DeviceDataManager** to route actuator commands to the **ActuatorAdapterManager**.

Finally, configuration flags such as **enableMqttClient** in **PiotConfig.props** provide runtime control over MQTT functionality, while detailed logging throughout the implementation supports easy debugging and real-time monitoring of MQTT operations.

## Code Repository and Branch

URL: https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule06

## UML Design Diagram(s)

URL: https://drive.google.com/file/d/1qwTDVsxSa8LT7nwTwfijMDUZepIgaRu2/view?usp=sharing

## Unit Tests Executed

- **Old:** All Part 01 and Part 02 unit tests
- **New:** None

## Integration Tests Executed

- **Old:** All Part 01 and Part 02 integration tests
- **New:**
  - **test_MqttClientConnector**
  - **test_MqttClientControlPacket**

**EOF**
