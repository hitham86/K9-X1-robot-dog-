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

| ⚙️ DOF | ⚖️ Weight | 📏 Length | 🔋 Battery |
| :--- | :--- | :--- | :--- |
| **12 Servo Joints** | **2.1 kg** | **~45 cm** | **90 min** |

---

## 🔧 Hardware


* **🧠 Raspberry Pi 5 (8GB):** Main compute — runs ROS2, vision pipeline, and gait controller.
* **⚙️ MG996R Servos × 12:** 3 per leg — hip abduction, hip flexion, knee flex. 10kg·cm torque.
* **👁️ OAK-D Lite Depth Cam:** Stereo depth + neural inference for obstacle detection.
* **🎛️ PCA9685 PWM Driver:** 16-channel I²C PWM — drives all servos from a single board.
* **📡 MPU-6050 IMU:** 6-axis accelerometer + gyroscope for balance feedback.
* **🔋 3S LiPo 5000mAh:** 11.1V, regulated via buck converter to 5V / 6V rails.

---

## 🏗️ System Architecture

```mermaid
graph TD
    A[Raspberry Pi 5] --> B[ROS2 Humble Core]
    B --> C[Vision Node - OAK-D]
    B --> D[IMU Node - MPU-6050]
    B --> E[Gait Planner]
    E --> F[IK Solver]
    F --> G[PCA9685 Driver]
    G --> H[12x Servos]
    
    style A fill:#0d1428,stroke:#00d4ff,stroke-width:2px,color:#fff
    style H fill:#0d1428,stroke:#ff6b00,stroke-width:2px,color:#fff
