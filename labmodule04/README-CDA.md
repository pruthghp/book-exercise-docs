# Constrained Device Application (Connected Devices)

## Lab Module 04

### Description

### What does your implementation do?

Lab Module 04 adds Sense HAT emulator support to the CDA, letting the app read sensor data and control actuators through a virtual hardware interface.

The implementation brings in six emulator task classes:
- **Sensors:** HumiditySensorEmulatorTask, PressureSensorEmulatorTask, TemperatureSensorEmulatorTask – these read data from the Sense HAT emulator.
- **Actuators:** HumidifierEmulatorTask, HvacEmulatorTask, LedDisplayEmulatorTask – these show actuator status on the emulator's 8x8 LED matrix.

The SensorAdapterManager and ActuatorAdapterManager were updated so the system can load either simulator or emulator tasks depending on the enableEmulator setting. This means you can switch between simulation and emulation without touching the code.

### How does your implementation work?

The emulator uses the pisense library, which gives a Python interface to the Sense HAT emulator. When useEmulator is set to True in the config, the adapter managers use Python's import_module to load emulator classes at runtime instead of simulator ones.

- Each sensor emulator task makes a SenseHAT instance in emulation mode and pulls readings directly from the virtual sensors (sh.environ.humidity, sh.environ.pressure, sh.environ.temperature).
- Actuator emulator tasks show messages on the LED display using scroll_text(). For example, when the HVAC turns on, it might show: "HVAC ON: 20.0C".

For this to work, the emulator GUI must be running.

### Code Repository and Branch

URL: https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule04

### UML Design Diagram(s)

Link: https://drive.google.com/file/d/1mOL998PoQ7OkOINPCWh2Z4BydmGbKbm3/view?usp=drive_link

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
- SenseHatEmulatorQuickTest
- HumiditySensorEmulatorTaskTest
- PressureSensorEmulatorTaskTest
- TemperatureSensorEmulatorTaskTest
- HumidifierEmulatorTaskTest
- HvacEmulatorTaskTest
- LedDisplayEmulatorTaskTest
- SensorEmulatorManagerTest
- ActuatorEmulatorManagerTest
