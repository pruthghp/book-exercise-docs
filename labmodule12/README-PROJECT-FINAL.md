# Lab Module 12 - Semester Project - Final Write-up
## Smart Wildlife Deterrent System (Kshavi 2.0)


## Description

KSHAVI 2.0 is a smart wildlife deterrent system that protects crops from animal raids. The system uses multiple sensors to detect and identify specific animals like elephants, deer, and boar, then automatically triggers deterrents and sends alerts to farmers. Everything connects to a cloud dashboard for real-time monitoring from anywhere.

## What - The Problem

Wildlife damage to crops is a major problem for farmers near forests. Elephants, deer, and wild boar raid farms at night and destroy entire crops in hours. This causes huge financial losses and creates dangerous conflicts between humans and animals. Traditional methods like fences or people staying up all night are expensive, unreliable, or dangerous.

Current systems fail because they use single sensors that can't tell the difference between real threats and false alarms like cattle or vehicles. Farmers need a smarter system that can identify which animal is present and respond automatically without constant human monitoring.


## Why - Who Cares?

I care about this problem because wildlife-human conflict is getting worse as natural habitats shrink. Farmers lose their livelihoods, and animals get hurt in retaliation. There needs to be a better way for both to coexist safely.

This project shows how IoT technology can solve real problems in rural areas. By combining multiple sensors with smart detection logic and cloud connectivity, we can create an affordable system that protects crops while keeping humans and animals safe. The multi-sensor approach prevents false alarms, and cloud integration lets farmers monitor fields from their phone.


## How - Expected Technical Approach and Outcomes

The system uses three layers: a Constrained Device Application (CDA) with sensors and actuators, a Gateway Device Application (GDA) that processes data, and Ubidots cloud service for visualization and alerts. The CDA collects thermal, acoustic, and humidity data and controls an LED display showing detection status.

The key feature is multi-sensor fusion. The system only triggers alerts when BOTH thermal detects body heat (32-40°C) AND acoustic identifies species-specific frequencies. This prevents false alarms. The GDA processes data, stores it in CSV files, and forwards to cloud. When an animal is detected, Ubidots sends voice alerts and can trigger deterrents. The system achieved 100% of requirements and successfully demonstrated automatic detection, cloud integration, and event responses.


## System Diagram

**URL:** https://drive.google.com/file/d/1rnw3jWMK4aunmixiRb6wFpp6JwkXILx0/view?usp=drive_link

The system follows a distributed IoT design with three layers. The CDA runs simulated sensors using SenseHAT emulator and handles sensor readings and actuator control. It collects thermal, acoustic, and humidity data every few minutes and publishes via MQTT to the GDA.

The GDA receives sensor data, runs multi-sensor fusion analysis to detect valid animals, and manages external communications. It stores data in CSV files and forwards to cloud via MQTT. The GDA also has SMTP for emails and sends actuator commands back to CDA. The cloud layer uses Ubidots for an 8-widget dashboard and automated event triggers that send voice alerts when animals are detected.


## Sensors and Actuators

### CDA Sensor 1: Thermal Proximity Sensor
Simulated using SenseHAT temperature readings to demonstrate infrared body heat detection logic. Valid detection range is 32-40°C for warm-blooded animals.

### CDA Sensor 2: Acoustic Sensor
Simulated using SenseHAT pressure sensor with mathematical mapping to frequency ranges. Species patterns: Deer (150-199 Hz), Elephant (200-249 Hz), Boar (250+ Hz).

### CDA Sensor 3: Humidity Sensor
Simulated using SenseHAT humidity readings for environmental monitoring.

### CDA Actuator 1: LED Display + Simulated Deterrents
SenseHAT emulated 8x8 LED matrix displays color-coded alerts: Green (safe), Blue (deer), Red (elephant), Yellow (boar). Water spray and acoustic deterrents log activation commands for hardware integration readiness.


## Protocols Implementation

### CDA to GDA Protocol: MQTT
CDA publishes sensor data and system performance to GDA using MQTT QoS 1. Topics follow PIOT/ConstrainedDevice/SensorMsg pattern. CDA subscribes to actuator commands.

### GDA to CDA Protocol: MQTT
GDA sends actuator commands to CDA via MQTT for LED control and deterrent activation with bidirectional acknowledgment.

### GDA to Cloud Protocol: MQTT
CloudClientConnector forwards all sensor and performance data to Ubidots in real-time via MQTT.

### Cloud to GDA Protocol: MQTT + SMTP
Ubidots uses MQTT for actuator commands. GDA implements SMTP client for email alerts as backup notification channel.


## Cloud Services

### Cloud Service 1 (Data Ingress): Ubidots Data Collection
All sensor data (thermal, acoustic, humidity) and system metrics (CPU, memory) stream to Ubidots. Platform creates time-series variables automatically and provides storage infrastructure.

### Cloud Service 2 (Data Egress): Ubidots Events Service
Automated triggers monitor sensor data: Deer (150-199 Hz + 32-40°C), Elephant (200-249 Hz + 32-40°C), Boar (250+ Hz + 32-40°C). When conditions met, sends voice alerts and publishes actuator commands. Animal Detection Switch event uses "Back to Normal" logic for automatic on/off control.


## Screen Shots - Cloud Services Configuration

### Screenshot 1: Ubidots Events Configuration
Ubidots Events page showing four events (Deer, Elephant, Boar, Animal Switch) with multi-sensor trigger conditions and configured actions.

**URL:** https://drive.google.com/file/d/10c5rDJIKVZVayVwTgmF2cnzt3zX5MqYl/view?usp=drive_link

**URL:** https://drive.google.com/file/d/1-lt6BHo8Sv8rSwqla1PlHDYbN39hhz6E/view?usp=drive_link

### Screenshot 2: Ubidots Device Variables
Ubidots device view with all variables: sensor data (acoustic, thermal, humidity), system performance (CPU, memory), and control variable (animal-detection-switch).

**URL:** https://drive.google.com/file/d/18I3NG_ZYhzuXELWjknO2g3NVfrDN1Ykt/view?usp=drive_link

### Screenshot 3: Event Trigger and Actuation
Live detection event captured. Animal Detection Switch shows ON state, acoustic reads 220 Hz (Elephant range), thermal shows 36°C (valid temperature). Event log confirms "Elephant Detection" triggered with voice alert sent and LED command published to CDA.

**URL:** https://drive.google.com/file/d/1V4rgUev9CzG9XXzgog7uyy2RWxI-nF5j/view?usp=sharing

**URL:** https://drive.google.com/file/d/13duSrdqdSOLPT6MV6s0uqx5uU3FVkjCW/view?usp=drive_link

**URL:** https://drive.google.com/file/d/1Bem8trV0F-fM64Diun8wx6PnvEdHM0Vg/view?usp=drive_link


## Project Repositories

### CDA Repository (Python - Constrained Device Application)
**URL:** https://github.com/pruthghp/cda-lab-modules-pruthghp/tree/labmodule12

### GDA Repository (Java - Gateway Device Application)
**URL:** https://github.com/pruthghp/gda-lab-modules-pruthghp/tree/labmodule12

