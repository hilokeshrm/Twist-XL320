# Stage 1 Build Plan: Spider Robot Controller (Python + Flutter + Wi-Fi)

## 🔰 Overview

This stage focuses on:
- Basic leg control using XL320 servos
- Real-time control via Flutter mobile app over WebSocket
- Telemetry streaming (leg angles, battery level)
- Operating the Pi as a Wi-Fi hotspot for the app

---

## 🗂️ Project Structure

### Raspberry Pi (Python)
```

robot/
├── core/
│   ├── movement.py         # Walking/running motion logic
│   ├── servo\_driver.py     # XL320 servo communication
│   └── telemetry.py        # Battery & joint angle reporting
├── network/
│   ├── websocket\_server.py # WebSocket server (asyncio)
│   └── protocol.py         # JSON message format handler
├── utils/
│   └── logger.py
└── main.py                 # Async entry point

```

### Flutter App
```

flutter\_app/
├── lib/
│   ├── ui/                 # Battery, angles, control buttons
│   ├── services/           # WebSocket communication
│   └── main.dart

```

---

## 🛠️ Development Milestones

### ✅ Phase 1: Environment Setup
- [ ] Set up Raspberry Pi 5 with Raspberry Pi OS
- [ ] Enable UART and install Python 3.11+
- [ ] Connect 1 leg (3 XL320 servos) to Pi (via TTL interface)
- [ ] Install WebSocket and serial libraries (`websockets`, `pyserial`)

---

### 🚶 Phase 2: Servo Movement Control
- [ ] Implement `servo_driver.py` for position setting and angle reading
- [ ] Create walking and running gaits in `movement.py`
- [ ] Test gaits using Python CLI before UI

---

### 📡 Phase 3: WebSocket Server
- [ ] Create WebSocket server using `asyncio` + `websockets`
- [ ] Define JSON protocol:
  - `{"command": "walk"}`  
  - `{"telemetry": {"battery": 92, "angles": [...]}}`
- [ ] Handle incoming control messages
- [ ] Send telemetry every 100ms (battery, angles)

---

### 🔋 Phase 4: Battery Monitoring (Optional but Recommended)
- [ ] Connect ADC (e.g., MCP3008) via SPI
- [ ] Feed battery voltage via voltage divider
- [ ] Calibrate voltage-to-percentage logic

---

### 📱 Phase 5: Flutter App MVP
- [ ] Set up project with `web_socket_channel`
- [ ] Design UI:
  - Buttons for walk/run
  - Battery bar
  - Joint angle indicators (simple graph/values)
- [ ] Connect to Pi’s hotspot
- [ ] Send commands & receive telemetry

---

### ✅ Phase 6: Integration & Testing
- [ ] Connect app to Pi's WebSocket server
- [ ] Validate real-time control latency
- [ ] Stress test walking with 4 legs
- [ ] Test in both indoor and outdoor conditions

---

## 🧩 Optional Add-Ons (if time permits)
- [ ] Emergency stop command
- [ ] Manual joint override mode in app
- [ ] Basic error logging UI for diagnostics

---

## 🔐 Security Consideration
- For this stage, no authentication
- Restrict Pi’s Wi-Fi to local use only
- Add handshake/PIN exchange in Stage 2 if needed

---

## 🚀 Stage 1 Completion Definition

You’ve completed Stage 1 when:
- The robot walks/runs using app commands
- The app shows real-time leg angles and battery level
- The whole system runs over local Wi-Fi hosted by the Pi

---

## 🧭 What’s Next?
→ Move to Stage 2 with ROS2 integration for:
- Modular control
- SLAM/IMU integration
- Vision and autonomy


---