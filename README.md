# 🤖 K9-X1 — Robot Dog Project

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-00d4ff?style=for-the-badge&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/ROS2-Humble-ff6b00?style=for-the-badge&logo=ros" alt="ROS2">
  <img src="https://img.shields.io/badge/Hardware-Raspberry%20Pi%205-00ff88?style=for-the-badge&logo=raspberrypi" alt="Raspberry Pi">
  <img src="https://img.shields.io/badge/Status-12--DOF-00d4ff?style=for-the-badge" alt="12-DOF">
  <img src="https://img.shields.io/badge/License-MIT-white?style=for-the-badge" alt="MIT License">
</p>

### **Autonomous Quadruped Robot — Build Log & Source Code**
A fully custom-built, 3D-printed quadruped robot dog with onboard computer vision, inverse kinematics, and gait planning. Every servo, circuit, and line of code — built from scratch.

---

## 📊 Quick Stats

| ⚙️ DOF | ⚖️ Weight | 📏 Body Length | 🔋 Battery Life |
| :--- | :--- | :--- | :--- |
| **12 Servo Joints** | **2.1 kg** | **~45 cm** | **~90 min** |

---

## 🔧 Hardware Breakdown



* **🧠 Raspberry Pi 5 (8GB):** Main compute unit running ROS2 Humble and the primary vision pipeline.
* **⚙️ MG996R Servos × 12:** High-torque actuators (10kg·cm) providing 3-DOF per leg (Hip Abduction, Hip Flexion, Knee Flexion).
* **👁️ OAK-D Lite Depth Cam:** Stereo depth + onboard neural inference for real-time obstacle detection.
* **🎛️ PCA9685 PWM Driver:** 16-channel I²C interface to control all 12 servos from a single bus.
* **📡 MPU-6050 IMU:** 6-axis accelerometer + gyroscope used for closed-loop balance feedback.
* **🔋 3S LiPo 5000mAh:** High-capacity power source regulated via buck converters to 5V/6V rails.

---







