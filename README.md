


# Remote Gardening System

**Smart Arduino-Based Gardening Automation System**

**Authors:** Mohammad Ninad Mahmud Nobo, Masnoon Muztahid, Rafiqul Islam Rayan

---


<p align="center">
  <a href="https://www.youtube.com/watch?v=m3LLqLAPCik" target="_blank">
    <img src="https://img.youtube.com/vi/m3LLqLAPCik/0.jpg" alt="Remote Gardening System Demo" width="480"/>
  </a>
</p>

<p align="center">
  <b>[🔗 Watch Full Demo Video](https://www.youtube.com/watch?v=m3LLqLAPCik)</b>
</p>

---

## Table of Contents

- [Introduction](#introduction)
- [My Contribution](#my-contribution)
- [Impact Highlights](#impact-highlights)
- [Demo Video](#project-demo)
- [Features](#features)
- [Hardware Requirements](#hardware-requirements)
- [Library Dependencies](#library-dependencies)
- [Pin Configuration](#pin-configuration)
- [Bluetooth Command Map](#bluetooth-command-map)
- [Build and Upload](#build-and-upload)
- [Typical Runtime Flow](#typical-runtime-flow)
- [Safety and Reliability Notes](#safety-and-reliability-notes)
- [Improvement Opportunities](#known-improvement-opportunities)
- [Why This Project Stands Out](#why-this-is-a-strong-featured-project)
- [Folder Structure](#folder-structure)
- [License](#license)

---

## Introduction

In an era of smart homes and sustainable living, automating plant care is both a technical challenge and a real-world necessity. Gardening Brain is an Arduino-powered embedded system that brings together environmental sensing, actuator automation, and wireless control to create a robust, semi-autonomous gardening assistant. This project was developed as a featured coursework submission for microcontroller and embedded systems engineering.

---

## My Contribution

- **Arduino Coding:** Designed and implemented the full control logic in C++ for the Arduino platform, including sensor reading, actuator control, Bluetooth command parsing, and LCD feedback.
- **System Planning:** Defined the automation logic, hardware-software interaction, and user command structure for robust, real-world operation.
- **Hardware Structure:** Planned the wiring, pin mapping, and modular structure for easy assembly and troubleshooting.

---

## Impact Highlights

- Delivered a production-ready Arduino codebase for real-time plant monitoring and automation
- Enabled seamless integration of sensors, actuators, and wireless control for hands-off gardening
- Improved reliability and user experience through robust error handling and feedback mechanisms

---


## 📺 Project Demo

<p align="center">
  <a href="https://www.youtube.com/watch?v=m3LLqLAPCik" target="_blank">
    <img src="https://img.youtube.com/vi/m3LLqLAPCik/0.jpg" alt="Remote Gardening System Demo" width="480"/>
  </a>
</p>

<p align="center">
  <b>[🔗 Watch Full Demo Video](https://www.youtube.com/watch?v=m3LLqLAPCik)</b>
</p>

---

## Features

### 🤖 Live Sensor Monitoring
Continuously reads:
- DHT11 temperature and humidity sensor
- Soil moisture sensor (analog)
- Rain sensor (analog)
If DHT reading fails, previous valid values are reused and displayed.

### 💧 Automatic Watering
Soil moisture is compared against a threshold (`moistureThreshold = 512`).
- Dry soil → water motor ON
- Moist soil recovered → water motor OFF

### 🧪 Timed Fertilizer and Pesticide Cycles
Both pumps run automatically by periodic schedule:
- Pesticide: ON every 120s, runs for 3s
- Fertilizer: ON every 60s, runs for 3s

### 🚿 Spray Motor Manual Control
Spray motor can be toggled from Bluetooth commands.

### 🌞 Smart Shade Control
Shade is controlled by a stepper motor:
- Temperature above 30°C → shade opens/deploys
- Temperature below 30°C → shade closes
- User can manually toggle shade via Bluetooth

### 🌧️ Rain Awareness and Alerts
Rain levels are classified into:
- No rain
- Moderate rain
- Heavy rain
The controller sends weather updates and heavy-rain warnings through Bluetooth.

### 📟 LCD + Bluetooth Feedback
Every major event is shown on LCD and also sent to Bluetooth terminal, including motor status and weather updates.

---


## Project Snapshot

- **Platform:** Arduino-compatible board (UNO/Nano style pin mapping)
- **Main Code:** `Gardening Brain/Gardening brain.ino`
- **Communication:** SoftwareSerial Bluetooth at 9600 baud
- **Display:** 16x2 I2C LCD (address `0x27`)



---



## Hardware Requirements

- Arduino UNO/Nano (or equivalent)
- DHT11 sensor
- Soil moisture sensor (analog output)
- Rain sensor (analog output)
- HC-05 / HC-06 Bluetooth module
- 16x2 I2C LCD module
- Stepper motor + suitable driver/power stage
- 4 relay channels / transistor driver channels for:
  - Spray motor
  - Pesticide pump
  - Fertilizer pump
  - Soil water motor
- External power supply for motors/pumps (recommended)



## Library Dependencies

Install these Arduino libraries before building:

- `SoftwareSerial` (bundled with Arduino core)
- `DHT sensor library` by Adafruit
- `LiquidCrystal_I2C`
- `Stepper` (bundled with Arduino core)



## Pin Configuration

### Digital Pins

- D2 -> Spray motor control
- D3 -> Pesticide pump control
- D4 -> Fertilizer pump control
- D5 -> Soil water motor control
- D6 -> DHT11 data pin
- D7, D8, D9, D12 -> Stepper motor control
- D10 -> Bluetooth RX (SoftwareSerial)
- D11 -> Bluetooth TX (SoftwareSerial)

### Analog Pins

- A0 -> Soil moisture input
- A1 -> Rain sensor input
- A4/A5 -> I2C LCD (SDA/SCL)



## Bluetooth Command Map

The sketch reads command bytes and compares decimal ASCII values as strings.

- `I` (ASCII 73): send temperature, humidity, and soil moisture
- `U` (ASCII 85): send all motor status
- `A` (ASCII 65): spray motor ON
- `B` (ASCII 66): spray motor OFF
- `S` (ASCII 83): manual shade toggle ON/OFF
- `R` (ASCII 82): send current rain condition



## Build and Upload

1. Open Arduino IDE.
2. Open `Gardening Brain/Gardening brain.ino`.
3. Install required libraries from Library Manager.
4. Select correct board and COM port.
5. Upload the sketch.
6. Open Serial Monitor (optional) at 9600 baud for debug prints.
7. Pair Bluetooth module and test commands from a phone Bluetooth terminal app.



## Typical Runtime Flow

1. Controller boots and initializes sensors, LCD, serial links.
2. Reads temperature/humidity, soil moisture, and rain values.
3. Displays measurements on LCD.
4. Runs automation for watering/fertilizer/pesticide.
5. Monitors temperature/rain and manages shade and alerts.
6. Accepts Bluetooth commands for information and manual overrides.



## Safety and Reliability Notes

- Use separate power for pumps/motors to avoid brownouts.
- Add flyback protection and proper drivers for inductive loads.
- Never drive motors/pumps directly from Arduino I/O pins.
- Calibrate soil and rain thresholds for your specific sensors.
- Verify stepper mechanical limits before long travel movement.



## Known Improvement Opportunities

These are good next enhancements for a production-ready version:

- Add non-blocking timing (replace delays where possible)
- Add hysteresis/debounce around thresholds
- Store calibration and schedules in EEPROM
- Add real-time clock (RTC) for true time-of-day schedules
- Replace single-byte command parsing with robust command protocol
- Add overcurrent/fault detection for pump channels



## Why This Is a Strong Featured Project

- Integrates sensing, control, communication, and UI in one embedded system
- Demonstrates both autonomous logic and remote user control
- Solves a practical problem with clear real-world impact
- Shows end-to-end microcontroller engineering workflow



## Folder Structure

```text
CSE-316-Microcontroller-and-Microprocessor-Project/
|-- Gardening Brain/
|   |-- Gardening brain.ino
|-- README.md
```



## License

No explicit license file is included yet. Add a `LICENSE` file if you plan to open-source this project publicly.
