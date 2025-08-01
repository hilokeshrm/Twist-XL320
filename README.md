**Masterplan for Twist: Four-Legged Spider Robotgit Control App**

---

## 1. App Overview & Objectives

**Twist** is a mobile application to control a four-legged "spider" robot (Twist) built on Raspberry Pi 5 and XL320 servos. The app’s objectives are:

* Provide intuitive, real-time control of walking and running gaits
* Display live telemetry: servo angles for each leg and battery status
* Offer responsive, low-latency feedback via WebSockets over Wi‑Fi
* Support future enhancements (video streaming, advanced gaits, payload management)

---

## 2. Target Audience

* **Primary:** Hobbyists and researchers experimenting with legged robotics
* **Secondary:** Educators/demonstrators showcasing robotics concepts to students or at events

---

## 3. Core Features & Functionality

1. **Real-Time Locomotion Control**

   * Virtual joystick for direction (forward/backward/turn)
   * Speed slider or ± buttons to adjust gait speed (walk vs. run)

2. **Telemetry Dashboard**

   * Display of all four legs’ servo angles in degrees
   * Battery level percentage in status bar
   * Mode indicator (Walk vs. Run)

3. **Action Buttons**

   * Start/Stop locomotion
   * Home Position (reset)
   * Calibrate servos
   * Emergency Stop
   * Switch gait patterns

4. **Communication Layer**

   * Bi‑directional WebSockets over robot-hosted Wi‑Fi access point
   * JSON message schema for telemetry and commands
   * 10 Hz telemetry updates (100 ms interval)

5. **Extensibility Hooks**

   * Placeholder for future video streaming view
   * Plugin points for new sensors or payload controls

---

## 4. High-Level Technical Stack

* **Pi Application (Python, asyncio)**

  * **Servo Controller Module:** Async interface to XL320 servos
  * **Battery Monitor Module:** Async ADC reads → percentage
  * **WebSocket Server:** `asyncio`/`websockets` library for push/pull

* **Mobile App (Flutter/Dart)**

  * **UI Framework:** Flutter for a single codebase on iOS/Android
  * **WebSocket Client:** `web_socket_channel` package with streams
  * **JSON Serialization:** `dart:convert` or `json_serializable`

* **Networking**

  * Robot runs as Wi‑Fi hotspot or joins LAN
  * App connects via WebSockets to `ws://<pi-ip>:8765`

---

## 5. Conceptual Data Model

```yaml
Telemetry:
  timestamp: int     # Unix epoch ms
  angles:           # 4 legs × 3 servos
    - [hip, knee, ankle]
    - [hip, knee, ankle]
    - [hip, knee, ankle]
    - [hip, knee, ankle]
  battery: int      # %

Command:
  timestamp: int
  action: string    # set_speed, start, stop, calibrate...
  value: any        # e.g., speed % or gait ID
```

---

## 6. UI Design Principles

* **Clarity & Responsiveness:** Large, thumb‑friendly joystick and sliders; immediate visual feedback
* **Information Hierarchy:** Status bar persistent at top; central controls; detailed telemetry below
* **Safe Interactions:** Emergency Stop and Calibrate buttons distinct and separated
* **Scalable Layout:** Adaptable to various screen sizes, landscape & portrait

---

## 7. Security & Access

* **Network Isolation:** Host robot as Wi‑Fi AP with WPA2 passphrase to restrict access
* **Command Validation:** Pi server verifies action types & ranges before execution
* **Safe Defaults:** Robot idles on unexpected disconnects; requires explicit "Start" to move

---

## 8. Development Phases & Milestones

1. **Prototype (Python & Simple Web UI)**

   * Servo control + battery read via `asyncio`
   * Basic WebSocket server + browser-based joystick demo

2. **Flutter MVP**

   * Implement Flutter UI: joystick, slider, telemetry panel

   * Wire up WebSocketService; test on Pi hotspot

3. **Robustification & Testing**

   * Error handling & reconnection logic
   * Battery calibration & safe shutdown routines

4. **Advanced Features**

   * Add video streaming view (`flutter_webrtc`)
   * Implement multiple gait patterns & autonomous demos

5. **Polish & Deployment**

   * UI/UX refinements; accessibility tweaks
   * Package Flutter build for stores or sideloading

---

## 9. Potential Challenges & Solutions

| Challenge                            | Mitigation                                                     |
| ------------------------------------ | -------------------------------------------------------------- |
| Wi‑Fi dropouts disrupting control    | Auto-reconnect logic; safe stop on disconnect                  |
| Servo library blocking calls         | Use fully async servo driver or run blocking calls in executor |
| Battery voltage noise → incorrect %  | Apply filtering; report only significant changes               |
| Memory leaks in long-lived WebSocket | Periodic health-check & restart server task                    |

---

## 10. Future Expansion Possibilities

* **Autonomous Gait Learning:** Integrate ROS for SLAM and AI-driven gait optimization
* **Payload Handling:** Add lightweight gripper or sensor packages
* **Cloud Dashboard:** Stream telemetry & video to a remote web portal
* **Multi-Robot Coordination:** Control swarms of Twist bots from a single app

---

*Ready for your feedback!* Let me know if there’s anything you’d like to adjust or expand, and I’ll update the masterplan accordingly.
