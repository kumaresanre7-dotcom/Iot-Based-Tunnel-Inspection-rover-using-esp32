# IoT-Based Tunnel Inspection Rover using ESP32

An ESP32-powered robotic rover for remote inspection of hazardous tunnel
environments with real-time video streaming, gas detection, and live
monitoring via the Blynk IoT dashboard.

## Features
- Live video streaming via ESP32-CAM
- Real-time gas, temperature & ultrasonic sensor monitoring
- PWM-based motor control using an L298N driver
- Remote control and alerts via the Blynk IoT dashboard
- Automated hazard alerts (gas threshold, obstacle detection)

## Hardware Used
| Component | Purpose |
|---|---|
| ESP32 (main controller) | Central processing & WiFi |
| ESP32-CAM | Live video stream |
| MQ-2 / MQ-135 Gas Sensors | Gas / smoke detection |
| DHT11 Temperature & Humidity Sensor | Ambient environment monitoring |
| HC-SR04 Ultrasonic Sensor | Obstacle detection |
| L298N Motor Driver | PWM motor control |
| DC Gear Motors (x4) | Rover locomotion |
| 18650 Li-ion Batteries (x3) | Portable power |

## Communication Protocols Used
UART (ESP32 ↔ ESP32-CAM serial link), analog I/O (sensor readings), PWM (motor control), WiFi/TCP-IP (Blynk IoT cloud)

## Software & Tools
- Arduino IDE
- Embedded C / C++
- Blynk IoT Platform
- Libraries: `Blynk`, `BlynkSimpleEsp32`, `DHT sensor library`, `esp_camera`

## System Architecture
The rover system is divided into two main modules:

1. **Rover Controller (ESP32)** — handles motor control, sensor data acquisition, Blynk communication, obstacle detection, and gas monitoring.
2. **Camera Controller (ESP32-CAM)** — handles camera initialization, the video streaming server, and sending the camera stream link to the main ESP32 over serial.

## Project Photos

<table>
  <tr>
    <td><img src="images/rover_top.jpg" width="350" alt="Top view — component layout"/><br/><em>Top view — chassis, motor driver, battery pack, ESP32</em></td>
    <td><img src="images/rover_front.jpg" width="350" alt="Front view — sensor array"/><br/><em>Front view — ultrasonic, gas, DHT11, and camera modules</em></td>
  </tr>
  <tr>
    <td><img src="images/rover_side1.jpg" width="350" alt="Side view — sensor bank"/><br/><em>Angled view — full sensor bank</em></td>
    <td><img src="images/rover_side2.jpg" width="350" alt="Side view — full chassis"/><br/><em>Rear-angle view — wheels and chassis</em></td>
  </tr>
</table>

## Project Folder Structure

```
.
├── README.md
├── LICENSE
├── esp32_camera.ino    ← ESP32-CAM firmware (video streaming server)
├── project_main.ino    ← Main ESP32 rover firmware (motors, sensors, Blynk)
└── images/
    ├── rover_top.jpg
    ├── rover_front.jpg
    ├── rover_side1.jpg
    └── rover_side2.jpg
```

## Working Principle

1. The **ESP32 connects to WiFi** and communicates with the **Blynk IoT platform**.
2. Sensor data (gas levels, temperature, humidity, and distance) is continuously monitored.
3. If dangerous conditions are detected, alerts are sent to the **Blynk mobile application**.
4. The **ESP32-CAM streams live video** and sends its stream link to the main ESP32 over a serial (UART) link.
5. The rover can be driven remotely using **Blynk virtual buttons/joystick**.

## How to Run

1. Clone this repo.
2. Open `project_main.ino` in Arduino IDE — this is the main rover firmware (motors + sensors + Blynk).
3. Near the top of `project_main.ino`, replace the placeholders with your own values:
   ```cpp
   #define BLYNK_TEMPLATE_ID   "your template id"
   #define BLYNK_TEMPLATE_NAME "your template name"
   #define BLYNK_AUTH_TOKEN    "your template auth_token key"
   ...
   char ssid[] = "your wifi name";
   char pass[] = "your wifi password";
   ```
   *(Get your template ID/name and auth token from your own Blynk console — never commit real credentials to a public repo.)*
4. Separately, open `esp32_camera.ino` for the ESP32-CAM module and set its WiFi `ssid`/`password` near the top. This sketch is based on the standard Arduino-ESP32 `CameraWebServer` example, so it also needs a `board_config.h` matching your camera board (e.g. AI-Thinker) — copy that file in from the example matching your board before compiling.
5. Install the required libraries via Arduino IDE's Library Manager (Blynk, DHT sensor library).
6. Flash `project_main.ino` to the main ESP32, and `esp32_camera.ino` to the ESP32-CAM module.
7. Open the Blynk app, connect to your device, and drive the rover.

## Applications
- Mining tunnel safety inspection
- Sewer and drainage pipeline inspection
- Post-disaster structural damage assessment
- Industrial confined-space hazard monitoring

## Author
**R E Kumaresan**

[LinkedIn](https://www.linkedin.com/in/r-e-kumaresan-9a4a59264) · [kumaresanre7@gmail.com](mailto:kumaresanre7@gmail.com)

## License
This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
