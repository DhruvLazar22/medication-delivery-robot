# Time-Triggered Autonomous Medication Delivery Robot

A low-cost mobile robot that **finds a patient indoors using a wristband** and **delivers the right medicine at the right time**. It needs no LIDAR, beacons or hospital IT infrastructure.

**Course:** Robotics & Automation, VIT Vellore
**Team:** Medha Rao, Smriti Prasad, Dhruv Lazar
**Tools:** ESP32, DS3231 RTC, MPU-6050 IMU, BLE, ROS 2, Kalman filter, A* path planning

---

## How it works

| Layer | What happens |
|---|---|
| 1. Scheduling | The DS3231 real-time clock fires an event at each dose time, even without network. |
| 2. Finding the patient | The patient's ESP32 wristband sends BLE signals plus IMU motion data. A **Kalman filter** fuses the two to estimate where the patient is. |
| 3. Navigation | Differential-drive robot with **A\*** path planning, plus 3 ultrasonic sensors for obstacle avoidance. |
| 4. Safe dispensing | The drawer opens only when the dose is due **and** the wristband is confirmed within 1 m. This prevents wrong-patient errors. |

## Localisation

- **State:** `[x, y, vx, vy]`
- **Prediction step:** uses IMU step displacement.
- **Update step:** converts BLE signal strength to distance with a log-distance path-loss model: `d = d0 · 10^((RSSI0 − RSSI) / (10n))`.
- Fusing BLE with the IMU gave noticeably better accuracy than BLE alone.

## Hardware

| Part | Specification |
|---|---|
| Chassis | 340 × 280 × 420 mm, aluminium + 3D-printed brackets |
| Motors | 2 × 12 V DC, 200 RPM, with encoders, L298N driver |
| Dispenser | Rotary multi-compartment drum (MG996R servo) + servo-controlled door |
| Sensors | 3 × HC-SR04 ultrasonic, IR obstacle sensors |
| Wristband | ESP32 + MPU-6050, 500 mAh LiPo, 3D-printed case |
| Comms | Wi-Fi (MQTT) to a caregiver app |

## Applications

- Hospital wards
- Elderly-care homes
- Home healthcare
- Rural clinics that can't afford LIDAR-based hospital robots

## Future work

- Pilot in a real care facility
- Multiple robots working together
- Integration with electronic health records
