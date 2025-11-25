# Semester Project Proposal: Kshavi 2.0 – Smart Wildlife Deterrent System

## Description

This project demonstrates an intelligent wildlife deterrent system based on my previous internship work at Vanadootha. Using four sensor types (thermal/proximity, environmental monitoring, camera/IR vision, and acoustic detection) to identify wildlife intrusion, the system triggers species-specific deterrents including water sprays and ultrasonic/infrasonic emitters while maintaining cloud connectivity for remote monitoring and farmer alerts.

## What - The Problem

Wildlife intrusion causes 30%+ crop losses in rural India. Elephants, wild boars, and deer destroy farmland at night when farmers can't monitor their fields. Current solutions are either too expensive, require constant human presence, or fail in areas with poor network connectivity. Farmers need an affordable, autonomous system that detects wildlife in complete darkness, identifies the animal type, triggers appropriate deterrents immediately at the edge, and provides remote monitoring—all while operating reliably despite infrastructure limitations in rural areas.

## Why - Who Cares?

I built the original Kshavi at Vanadootha and witnessed farmers losing entire harvests to overnight elephant raids. Our system achieved 12% improvement in alert delivery, but lacked species identification and intelligent deterrent selection. This project applies the complete Programming the IoT architecture to enhance Kshavi with multi-sensor fusion, species-specific deterrent logic, and comprehensive cloud integration, solving a real problem I've worked on while demonstrating advanced IoT patterns applicable to security systems, industrial monitoring, and smart agriculture. The goal is to not cause harm to animals through electrocution, killing, or hunting, and to avoid causing trouble for the farmers.

## How - Expected Technical Approach

### CDA (Raspberry Pi + Sensors)

#### Sensors (4 types)

1. **Thermal/Proximity Sensor (PIR Motion + IR Temperature):** Detects movement within 10m range and measures surface temperature to distinguish large warm-blooded animals (elephants ~35°C, boars ~38°C) from humans or small animals

2. **Environmental Sensor (SenseHAT - Temp/Humidity/Pressure):** Monitors ambient conditions; sudden humidity spikes indicate large animal breathing, pressure changes correlate with heavy footsteps

3. **Vision System (Pi Camera + IR LEDs):** Captures images in darkness using infrared illumination, enabling visual confirmation and basic size estimation (large blob = elephant, medium = boar, small = deer)

4. **Acoustic Sensor (USB Microphone):** Detects low-frequency sounds (elephant rumbles 14-24 Hz, boar grunts 200-500 Hz) and ground vibrations from footsteps, providing species identification clues

#### Actuators (2 types)

1. **Water Spray System (Solenoid Valve + Pump - LED emulation):** Directed water bursts effective for most herbivores. GREEN LED pattern simulates spray activation

2. **Acoustic Deterrent Array (Speaker System - LED emulation):**
   - Ultrasonic (20-65 kHz) for deer and smaller animals - BLUE LED
   - Infrasonic (5-20 Hz) for elephants (mimics bee swarms they fear) - RED LED
   - Variable frequency based on detected species - YELLOW LED for mixed mode

#### Edge Intelligence

**Immediate Response Logic:**
- PIR + thermal >35°C + acoustic <20Hz → Elephant detected → Infrasonic (RED) + Water spray (GREEN)
- PIR + thermal >37°C + acoustic 200-500Hz → Boar detected → Ultrasonic (BLUE) + Water spray
- PIR + camera (medium blob) + humidity spike → Deer probable → Ultrasonic (BLUE)
- Multiple sensors agree → High confidence → Immediate deterrent activation

**Image Capture:** Triggered on motion detection, stored locally, sent to GDA for logging

**Communication:** MQTT/TLS to GDA every 5 seconds (sensor data), immediate alerts on detection events

### GDA (Java Application)

#### Multi-Sensor Fusion Analytics

- **Species Classification Algorithm:** Combines thermal signature, acoustic frequency, visual blob size, and behavior patterns to calculate species probability scores

- **Confidence Scoring:**
  - 3+ sensors agree → 90%+ confidence → Full deterrent activation
  - 2 sensors agree → 70-89% confidence → Graduated response
  - 1 sensor only → <70% confidence → Standby/warning mode

- **Pattern Learning:** Tracks deterrent effectiveness per species (did elephant retreat after infrasonic? Required water spray escalation?) and adjusts thresholds

- **False Positive Filtering:** Distinguishes wildlife from cattle, humans, or vehicles using size + thermal + acoustic combinations

#### Local Storage (SQLite)

- Sensor time-series data with species detection flags
- Image archive with timestamps and detection metadata
- Deterrent activation log with effectiveness ratings
- Species activity patterns (time-of-day heatmaps)

#### Actuation Logic

- Graduated deterrent escalation: Start gentle (ultrasonic only), escalate to water spray if animal doesn't retreat within 30 seconds
- Species-specific protocols: Elephants get infrasonic immediately (most effective), deer get ultrasonic, boars get ultrasonic + water
- Override GDA commands only if new sensor data warrants stronger response

### Cloud (AWS IoT / Ubidots)

#### Data Ingestion

- Real-time sensor streams (thermal, environmental, acoustic levels)
- Detection events with species classification and confidence scores
- Deterrent activation history with timestamps
- System health (battery, connectivity, storage)

#### Farmer Dashboard

- Live camera feed (last captured image, 30-second updates)
- Species detection timeline (elephant at 2:15 AM - infrasonic deployed)
- Activity heatmap showing intrusion frequency by hour/species
- Deterrent effectiveness metrics (% successful deterrents per species)
- System health monitoring

#### Remote Control

- Manual deterrent triggers (farmer sees wildlife, activates from phone)
- Species-specific mode selection (force elephant protocol, deer protocol, etc.)
- Sensitivity adjustments based on seasonal behavior
- Image request (trigger camera capture on demand)

### System Architecture Diagram

**Diagram URL:** https://drive.google.com/file/d/1B_jr5lWOc1aw_gS6nZr4vud5DFImu36o/view?usp=sharing

### Species Detection Logic

| Sensors Triggered | Species ID | Confidence | Deterrent Response |
|------------------|------------|------------|-------------------|
| PIR + Thermal(35°C) + Audio(<20Hz) + Camera(large) | Elephant | 95% | Infrasonic (RED) + Water (GREEN) immediately |
| PIR + Thermal(38°C) + Audio(200-500Hz) + Humidity | Boar | 90% | Ultrasonic (BLUE), escalate to Water after 30s |
| PIR + Camera(medium) + Humidity + Thermal(36°C) | Deer | 85% | Ultrasonic (BLUE) only, gentle approach |
| PIR + Audio only | Unknown | 60% | Ultrasonic (BLUE) cautious mode, request image |

**GDA Enhancement:** Uses historical data to refine species classification—if 80% of "elephant" detections occur between 2-4 AM with similar acoustic signatures, future detections at that time with matching audio receive a confidence boost.

## Results - Expected Outcomes

The system will operate continuously for 1+ hour, collecting 720+ readings across four sensor types (thermal, environmental, vision, acoustic) and 120+ system performance samples. It will demonstrate species-specific deterrent selection: elephants triggering immediate infrasonic + water spray (RED + GREEN LEDs), boars triggering ultrasonic with graduated water escalation (BLUE → GREEN), and deer triggering gentle ultrasonic only (BLUE).

The three-tier intelligence will show: CDA immediate response (2+ local triggers based on multi-sensor agreement), GDA analytical classification (2+ pattern-based species identifications with confidence scoring), and cloud remote control (2+ manual overrides or mode adjustments). SQLite will store sensor fusion results, captured images, and deterrent effectiveness data, proving resilience during network interruptions. All communication uses MQTT/TLS encryption.

This demonstrates the complete edge-to-cloud IoT architecture applied to the real Kshavi wildlife problem, adding multi-sensor fusion and species-specific intelligence that significantly improves on my original Vanadootha implementation.
