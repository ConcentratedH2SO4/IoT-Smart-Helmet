# 🏍️ Smart Safety Helmet (IoT + ESP-NOW)

**A life-saving IoT system that enforces helmet usage, prevents drunk driving, and automatically alerts emergency contacts in case of an accident.**

---

## 📖 Overview
This project is a dual-unit system designed to enhance motorcycle safety. It consists of a **Helmet Unit** (Transmitter) and a **Bike Unit** (Receiver). The bike's ignition is controlled wirelessly based on the safety status of the rider.

If the rider is not wearing the helmet, is under the influence of alcohol, or if a crash is detected, the bike will not start (or will trigger an emergency alert).

### 🌟 Key Features
* **⛑️ Ignition Interlock:** The bike will not start unless the helmet is worn (detected via Reed Switch).
* **🍺 Alcohol Detection:** Prevents ignition if alcohol levels exceed the safety threshold (MQ-3 Sensor).
* **💥 Accident Detection:** Monitors G-force (Accelerometer) and vibration to detect crashes.
* **📡 Wireless Control:** Uses **ESP-NOW** (protocol) for instant, offline communication between Helmet and Bike.
* **🆘 Cloud Alerts:** Sends "CRASH DETECTED" email/notification with **Live Google Maps Link** to emergency contacts via **Blynk**.

---

## ⚙️ System Architecture

The system is divided into two modules:

1.  **Helmet Unit (Sender):** Reads sensors (GPS, Accel, Alcohol, Reed) and makes safety decisions. It sends an `ON` or `OFF` command to the bike.
2.  **Bike Unit (Receiver):** Listens for commands and physically toggles the ignition relay. It also has a safety "Watchdog" that cuts the engine if the signal from the helmet is lost for >10 seconds.

---

## 🔌 Circuitry & Wiring

### 1. Helmet Unit (Transmitter)
* **Microcontroller:** ESP32 Dev Module
* **Power:** 3.7V Li-Po Battery (connected to 5V/3.3V via boost converter)

| Component | ESP32 Pin | Connection Details |
| :--- | :--- | :--- |
| **Reed Switch** | GPIO 21 | One leg to Pin 21, other to GND (Detects head inside helmet) |
| **Vibration Sensor** (SW-420) | GPIO 23 | Digital Output (DO) pin |
| **Alcohol Sensor** (MQ-3) | GPIO 33 | Analog Output (AO) pin |
| **Buzzer** | GPIO 19 | Positive leg to Pin 19 |
| **Accelerometer** (ADXL335) | GPIO 34 | X-Axis |
| | GPIO 35 | Y-Axis |
| | GPIO 32 | Z-Axis |
| **GPS Module** (Neo-6M) | GPIO 16 (RX2) | Connects to GPS **TX** |
| | GPIO 17 (TX2) | Connects to GPS **RX** |

### 2. Bike Unit (Receiver)
* **Microcontroller:** ESP32 Dev Module
* **Power:** Bike Battery (12V) -> Buck Converter -> 5V ESP32

| Component | ESP32 Pin | Connection Details |
| :--- | :--- | :--- |
| **Relay Module** | GPIO 27 | IN Pin (Active LOW relay recommended) |
| **Ignition Wires** | Relay COM/NO | Cut the bike's kill switch wire; connect ends to Common (COM) and Normally Open (NO) |

> **Note:** The Bike Unit requires a stable power source. Ensure the ground (GND) is common if using multiple power sources.

---

## 🛠️ Software & Libraries
This project is built using the **Arduino IDE**.

### Required Libraries
You need to install the following libraries via the Arduino Library Manager:
1.  `Blynk` (by Volodymyr Shymanskyy)
2.  `TinyGPSPlus` (by Mikal Hart)
3.  `WiFi` & `esp_now` (Standard ESP32 built-ins)

---

## 🚀 How to Run

### Step 1: Configuration
Before uploading, open the code files and update the `Configuration` sections:

* **WiFi:** Enter your `SSID` and `Password`.
* **Blynk:** Add your `Template ID`, `Device Name`, and `Auth Token`.
* **MAC Address:**
    1.  Upload the **Bike Unit** code first.
    2.  Open Serial Monitor (`115200` baud).
    3.  Copy the MAC Address printed in the logs.
    4.  Paste this address into the `BASE_MAC_ADDR` variable in the **Helmet Unit** code.

### Step 2: Upload
1.  Connect the Helmet ESP32 and upload `SmartHelmet_Sender_HelmetUnit.ino`.
2.  Connect the Bike ESP32 and upload `SmartHelmet_Receiver_BikeUnit.ino`.

### Step 3: Test
1.  Power on both units.
2.  Bring a magnet close to the Helmet (Reed Switch) to simulate wearing it.
3.  The Relay on the Bike Unit should click **ON**.
4.  Simulate a crash (shake the helmet vigorously) -> The Relay should click **OFF** and you should receive a Blynk Notification.

---

## 🔮 Future Improvements
* Add a backup battery monitor for the helmet.
* Implement "Fall Detection" using Machine Learning (Edge Impulse) on the accelerometer data.
* Add an OLED display on the bike dashboard for status (Connected/Disconnected).

---

## 📜 License
This project is open-source. Feel free to modify and distribute.