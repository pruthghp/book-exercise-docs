# Constrained Device Application (Connected Devices)

## Lab Module 03

### Description

### What does your implementation do?

Lab Module 03 builds the basic sensor and actuator simulation setup for the Constrained Device Application (CDA). The main piece is the DeviceDataManager, which acts as the hub for handling all data inside the CDA.

DeviceDataManager works with three managers:
- SystemPerformanceManager – keeps track of CPU and memory use.
- SensorAdapterManager – creates simulated environmental sensor data (humidity, pressure, temperature).
- ActuatorAdapterManager – runs simulated actuators (HVAC, humidifier).

The system also has automatic HVAC control based on temperature. DeviceDataManager checks the sensor data and, if the temperature goes outside the set range (18.0°C to 20.0°C), it sends a command to the HVAC actuator to bring it back in range.

### How does your implementation work?

The setup uses a callback-based design with the IDataMessageListener interface. Each manager points to DeviceDataManager as its listener, so whenever new telemetry data is created, the manager calls back into DeviceDataManager. DeviceDataManager then processes the data and takes the right action. For example, if a temperature reading is too high or too low, it creates an ActuatorData command and sends it to ActuatorAdapterManager to adjust the HVAC.

All sensor and actuator tasks are built on common base classes (BaseSensorSimTask and BaseActuatorSimTask). These provide shared features, and the subclasses add the specific simulation details using pre-generated datasets.

Finally, the whole system runs inside ConstrainedDeviceApp, which starts and stops the DeviceDataManager to control the CDA lifecycle.

### Code Repository and Branch

URL: https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule03

### UML Design Diagram(s)

Link: https://drive.google.com/file/d/1S98i-VQJML_6x7o9tZXthOdQu7VGs8mA/view?usp=drive_link

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

### Integration Tests Executed

- SystemPerformanceManagerTest
- SensorAdapterManagerTest
- ActuatorAdapterManagerTest
- DeviceDataManagerNoCommsTest
- ConstrainedDeviceAppTest
