# Constrained Device Application (Connected Devices)

## Lab Module 01

### Description

**What does your implementation do?**

My implementation sets up the core development environment and configuration system for the Constrained Device Application. It establishes a Python virtual environment, configures the necessary file paths, and ensures that configuration files can be loaded correctly. With this foundation, the application can execute basic tests without running into import errors, providing a stable starting point for further development.

**How does your implementation work?**

The solution resolves path-related issues by updating the `DEFAULT_CONFIG_FILE_NAME` in **ConfigConst.py** to use an absolute path (`/home/connected-devices/programmingtheiot/cda-python-components/config/PiotConfig.props`) instead of a relative one. This guarantees consistent access to the configuration file across different execution contexts. The **ConstrainedDeviceApp** class serves as the main entry point, handling configuration loading and initializing the CDA runtime, while the virtual environment isolates dependencies to avoid conflicts with other projects.

### Code Repository and Branch

URL: https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule01

### UML Design Diagram(s)

https://drive.google.com/drive/u/0/folders/1pu0YVipDj8xHfdi8Z8gi6Or5PjkQ7CM3

### Unit Tests Executed

* ConfigUtilDefaultTest
* ConfigUtilCustomTest

### Integration Tests Executed

* ConstrainedDeviceAppTest
