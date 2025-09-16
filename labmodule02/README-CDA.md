# Constrained Device Application (Connected Devices)

## Lab Module 02

### Description

**What does your implementation do?**

My Lab Module 02 implementation creates a system monitoring framework for the Constrained Device Application that can track CPU and memory usage in real-time. The implementation builds on Lab Module 01 by adding monitoring classes that collect system performance data automatically. The main components include task classes for CPU and memory monitoring, a base class for common functionality, and a manager that schedules data collection at regular intervals.

**How does your implementation work?**

The implementation works by using the psutil Python library to get system resource information and APScheduler to run monitoring tasks in the background. I created BaseSystemUtilTask as the parent class that defines common methods, then SystemCpuUtilTask and SystemMemUtilTask extend this base to get specific CPU and memory data. The SystemPerformanceManager coordinates everything by scheduling the monitoring tasks to run in the background using APScheduler's BackgroundScheduler. The manager integrates with ConstrainedDeviceApp so monitoring starts and stops with the application lifecycle. All collected data gets logged for tracking system performance over time.

### Code Repository and Branch

URL: https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule02

### UML Design Diagram(s)

https://drive.google.com/file/d/1jlnvvc0els-L2RlHieayDU1wbGsrQI2j/view?usp=drive_link

### Unit Tests Executed

* ConfigUtilDefaultTest
* ConfigUtilCustomTest
* SystemCpuUtilTaskTest
* SystemMemUtilTaskTest

### Integration Tests Executed

* ConstrainedDeviceAppTest
* SystemPerformanceManagerTest
