# Gateway Device Application (Connected Devices)

## Lab Module 01

### Description

**What does your implementation do?**
My implementation establishes the core development environment and configuration framework for the Gateway Device Application. It sets up Java 17 with the Maven build system, configures file paths for the configuration loader, and ensures that the application can compile, load configuration files, and execute basic tests. This creates a stable foundation for the Gateway Device Application to run reliably and serves as the entry point for future lab modules.

**How does your implementation work?**
The solution uses Java's **ConfigUtil** class to load configuration properties from the *PiotConfig.props* file, with the system configured to reference the correct file paths just like the CDA's absolute path handling. Compatibility with Java 17 was ensured to prevent dependency version conflicts, while Maven manages dependencies and provides a consistent build process. The **GatewayDeviceApp** class acts as the main entry point, initializing the configuration system, starting the runtime, and supporting clean shutdown after a specified execution period with proper logging across the lifecycle.

### Code Repository and Branch

URL: https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule01

### UML Design Diagram(s)


### Unit Tests Executed

* ConfigUtilDefaultTest
* ConfigUtilCustomTest

### Integration Tests Executed

* GatewayDeviceAppTest
