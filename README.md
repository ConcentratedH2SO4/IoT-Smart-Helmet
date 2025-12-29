IoT Smart Helmet: Advanced Safety & Accident Detection System
An intelligent, dual-unit IoT ecosystem designed to enforce rider safety, prevent drunk driving, and provide real-time emergency response through automated crash detection and GPS tracking.
📖 Project Overview
Motorcycle accidents often result in fatalities due to two main factors: lack of protective gear and delayed emergency response. This project addresses both by creating a synchronized system between the Helmet Unit (Transmitter) and the Bike Unit (Receiver).

The system utilizes a wireless "Handshake" protocol—the bike simply will not start unless the rider is safely equipped and sober.

How it Works:
Safety Verification: The Helmet Unit checks for a secure fit (Reed Switch) and sobriety (MQ-3 Sensor).
Wireless Command: If safety conditions are met, the Helmet sends an "Authorize" signal to the Bike via ESP-NOW.
Active Monitoring: During the ride, the system monitors for high-G impacts.
Emergency Alert: Upon detecting a crash, the system fetches live GPS coordinates and pushes a "CRASH DETECTED" alert to emergency contacts via the Blynk IoT Cloud.

The system is divided into two modules:
Helmet Unit (Sender): Reads sensors (GPS, Accel, Alcohol, Reed) and makes safety decisions. It sends an ON or OFF command to the bike.
Bike Unit (Receiver): Listens for commands and physically toggles the ignition relay. It also has a safety "Watchdog" that cuts the engine if the signal from the helmet is lost for >10 seconds.
🔌 Circuitry & Wiring
1. Helmet Unit (Transmitter)
Microcontroller: ESP32 Dev Module
Power: 3.7V Li-Po Battery (connected to 5V/3.3V via boost converter)
Component	ESP32 Pin	Connection Details
Reed Switch	GPIO 21	One leg to Pin 21, other to GND (Detects head inside helmet)
Vibration Sensor (SW-420)	GPIO 23	Digital Output (DO) pin
Alcohol Sensor (MQ-3)	GPIO 33	Analog Output (AO) pin
Buzzer	GPIO 19	Positive leg to Pin 19
Accelerometer (ADXL335)	GPIO 34	X-Axis
GPIO 35	Y-Axis
GPIO 32	Z-Axis
GPS Module (Neo-6M)	GPIO 16 (RX2)	Connects to GPS TX
GPIO 17 (TX2)	Connects to GPS RX
2. Bike Unit (Receiver)
Microcontroller: ESP32 Dev Module
Power: Bike Battery (12V) -> Buck Converter -> 5V ESP32
Component	ESP32 Pin	Connection Details
Relay Module	GPIO 27	IN Pin (Active LOW relay recommended)
Ignition Wires	Relay COM/NO	Cut the bike's kill switch wire; connect ends to Common (COM) and Normally Open (NO)
Note: The Bike Unit requires a stable power source. Ensure the ground (GND) is common if using multiple power sources.

🛠️ Software & Libraries
This project is built using the Arduino IDE.

Required Libraries
You need to install the following libraries via the Arduino Library Manager:

Blynk (by Volodymyr Shymanskyy)
TinyGPSPlus (by Mikal Hart)
WiFi & esp_now (Standard ESP32 built-ins)
🚀 How to Run
Step 1: Configuration
Before uploading, open the code files and update the Configuration sections:

WiFi: Enter your SSID and Password.
Blynk: Add your Template ID, Device Name, and Auth Token.
MAC Address:
Upload the Bike Unit code first.
Open Serial Monitor (115200 baud).
Copy the MAC Address printed in the logs.
Paste this address into the BASE_MAC_ADDR variable in the Helmet Unit code.
Step 2: Upload
Connect the Helmet ESP32 and upload SmartHelmet_Sender_HelmetUnit.ino.
Connect the Bike ESP32 and upload SmartHelmet_Receiver_BikeUnit.ino.
Step 3: Test
Power on both units.
Bring a magnet close to the Helmet (Reed Switch) to simulate wearing it.
The Relay on the Bike Unit should click ON.
Simulate a crash (shake the helmet vigorously) -> The Relay should click OFF and you should receive a Blynk Notification.
🔮 Future Improvements
Add a backup battery monitor for the helmet.
Implement "Fall Detection" using Machine Learning (Edge Impulse) on the accelerometer data.
Add an OLED display on the bike dashboard for status (Connected/Disconnected).
📜 License
This project is open-source. Feel free to modify and distribute.
