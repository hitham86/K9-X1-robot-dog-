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

## 🏗️ System Architecture

```mermaid
graph TD
    A[Raspberry Pi 5] --> B[ROS2 Humble Node Graph]
    B --> C[Vision Node - OAK-D]
    B --> D[IMU Node - MPU-6050]
    B --> E[Gait Planner - Trot/Crawl]
    E --> F[IK Solver - Analytical]
    F --> G[PCA9685 Driver Board]
    G --> H[12x MG996R Servos]
    
    style A fill:#0d1428,stroke:#00d4ff,stroke-width:2px,color:#fff
    style H fill:#0d1428,stroke:#ff6b00,stroke-width:2px,color:#fff
    style B fill:#0a0f1e,stroke:#00ff88,color:#fff









import numpy as np

class LegIKSolver:
    def __init__(self, L1=0.10, L2=0.12, L3=0.08):
        # Link lengths: hip → thigh → shin (meters)
        self.L1, self.L2, self.L3 = L1, L2, L3

    def solve(self, x, y, z):
        """Solve for joint angles given foot target (x, y, z)."""
        theta1 = np.arctan2(y, x)

        R = np.sqrt(x**2 + y**2) - self.L1
        D = np.sqrt(R**2 + z**2)
        cos2 = (D**2 - self.L2**2 - self.L3**2) / (2 * self.L2 * self.L3)
        theta3 = np.arccos(np.clip(cos2, -1, 1))

        alpha = np.arctan2(z, R)
        beta = np.arctan2(self.L3 * np.sin(theta3),
                          self.L2 + self.L3 * np.cos(theta3))
        theta2 = alpha - beta

        return np.degrees([theta1, theta2, theta3])









🐾 Gait ModesGait ModeSpeedStabilityPrimary Use CaseStatusCrawl~0.15 m/s⭐⭐⭐⭐⭐Rough terrain & slopes✓ DoneTrot~0.5 m/s⭐⭐⭐⭐Default locomotion✓ DonePace~0.6 m/s⭐⭐⭐Narrow corridor navigation⟳ WIPGallop>1.0 m/s⭐⭐High-speed sprints






