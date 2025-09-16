# Gateway Device Application (Connected Devices)

## Lab Module 02

### Description

**What does your implementation do?**

My Lab Module 02 implementation creates a system monitoring framework for the Gateway Device Application that can track CPU load and JVM memory usage. The implementation extends Lab Module 01 by adding Java-based monitoring classes that collect system performance data from the Gateway Device. The main components include task classes for CPU and memory monitoring, a base class for shared functionality, and a performance manager that coordinates data collection using Java's built-in threading.

**How does your implementation work?**

The implementation works by using Java's ManagementFactory to access system and JVM performance metrics through scheduled thread execution. I created BaseSystemUtilTask as the abstract parent class, then SystemCpuUtilTask uses getSystemLoadAverage() for CPU monitoring while SystemMemUtilTask calculates JVM heap memory utilization percentages. The SystemPerformanceManager uses ScheduledExecutorService to run monitoring tasks at regular intervals and coordinates with the GatewayDeviceApp lifecycle. The manager starts and stops monitoring along with the main application, ensuring system resources are properly managed and performance data is collected throughout the application runtime.

### Code Repository and Branch

URL: https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule02

### UML Design Diagram(s)

https://drive.google.com/file/d/13fnEi6xrrQKONTTsIOYYfgPo9qgpSWmM/view?usp=drive_link

### Unit Tests Executed

* ConfigUtilDefaultTest
* ConfigUtilCustomTest
* SystemCpuUtilTaskTest
* SystemMemUtilTaskTest

### Integration Tests Executed

* GatewayDeviceAppTest
* SystemPerformanceManagerTest
