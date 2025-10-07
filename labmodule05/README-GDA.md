# Gateway Device Application (Connected Devices)

## Lab Module 05

### Description

### What does your implementation do?

Lab Module 05 adds the data container layer and JSON transformation features for the Gateway Device Application (GDA).

The implementation introduces three main data classes:
- ActuatorData
- SensorData
- SystemPerformanceData

All three extend BaseIotData and provide structured storage for telemetry and commands. Each class includes getter and setter methods, automatic timestamp updates, and a handleUpdateData() method to copy values between instances.

The SystemPerformanceManager was updated to create SystemPerformanceData objects during telemetry collection. It fills them with CPU and memory usage values, then sends them through the registered listener callbacks.

A new DeviceDataManager was added as the central data handler for the GDA. It implements the IDataMessageListener interface and manages sensor messages, actuator responses, and system performance data from different components.

### How does your implementation work?

For data transformation, the system uses Google's Gson library for JSON serialization. The DataUtil class provides six conversion methods (three for object-to-JSON and three for JSON-to-object) and follows a singleton pattern so it can be accessed anywhere in the application. Gson's toJson() and fromJson() methods handle conversion automatically, mapping Java object properties to JSON fields and back. This keeps the code simple and ensures compatibility with the Python CDA's JSON format.

The DeviceDataManager connects into GatewayDeviceApp through composition. When the app starts, it creates a DeviceDataManager instance and routes all data handling to it. For example, when SystemPerformanceManager generates telemetry, it builds a SystemPerformanceData object, sets the location ID and utilization values, and then calls the handleSystemPerformanceMessage() method on DeviceDataManager. DeviceDataManager logs this data and gets it ready for future transmission to cloud services.

### Code Repository and Branch

URL: https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule05

### UML Design Diagram(s)

Link: https://drive.google.com/file/d/1lxj74MXCHFmEyb3WvRdoR-yTu5MWkt96/view?usp=drive_link

### Unit Tests Executed

- ConfigUtilTest
- SystemCpuUtilTaskTest
- SystemMemUtilTaskTest
- ActuatorDataTest
- SensorDataTest
- SystemPerformanceDataTest
- DataUtilTest
- DataIntegrationTest

### Integration Tests Executed

- SystemPerformanceManagerTest
- DeviceDataManagerNoCommsTest
- GatewayDeviceAppTest
