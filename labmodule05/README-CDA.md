# Constrained Device Application (Connected Devices)

## Lab Module 05

### Description

### What does your implementation do?

Lab Module 05 adds data transformation and JSON serialization features to the CDA.

The SystemPerformanceManager was updated to create SystemPerformanceData objects with CPU and memory usage, and then call the registered listener callbacks to pass this data through the system.

A new DataUtil class was built to handle JSON serialization and deserialization for all three core data types:
- ActuatorData
- SensorData
- SystemPerformanceData

### How does your implementation work?

The implementation uses Python's built-in json library along with a custom JsonDataEncoder, which turns Python objects into dictionaries using their __dict__ attribute. This makes it easy to switch between object instances and JSON strings with the right formatting.

**The data transformation works in both directions:**

- **Object → JSON:** The _generateJsonData() method uses json.dumps() with the custom encoder. It also fixes formatting issues like making boolean values lowercase and normalizing quotes.
- **JSON → Object:** The _formatDataAndLoadDictionary() method first parses JSON strings into dictionaries. Then _updateIotData() uses Python's setattr() to map dictionary key-value pairs to object attributes. This generic method works for all data types by using Python's vars() function for introspection.

The updates to SystemPerformanceManager tie this together by creating structured SystemPerformanceData objects during each telemetry cycle, filling in the location ID and utilization values, and sending them through the registered listener using setDataMessageListener(). This completes the data flow from collection → transformation → callback notification.

### Code Repository and Branch

URL: https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule05

### UML Design Diagram(s)

Link: https://drive.google.com/file/d/1oP90TknJgThCGXOYagoEmrwJ65GO0-XP/view?usp=drive_link

### Unit Tests Executed

- ConfigDefaultUtilTest
- ConfigCustomUtilTest
- SystemCpuUtilTaskTest
- SystemMemUtilTaskTest
- HumiditySensorSimTaskTest
- PressureSensorSimTaskTest
- TemperatureSensorSimTaskTest
- HumidifierActuatorSimTaskTest
- HvacActuatorSimTaskTest
- DataUtilTest
- DataIntegrationTest

### Integration Tests Executed

- SystemPerformanceManagerTest
- SensorAdapterManagerTest
- ActuatorAdapterManagerTest
- DeviceDataManagerNoCommsTest
- ConstrainedDeviceAppTest
