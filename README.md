<div align="center">

# 🤖 K9-X1 — Autonomous Quadruped Robot

![Python](https://img.shields.io/badge/Python-3.11-blue?style=for-the-badge&logo=python&logoColor=white)
![ROS2](https://img.shields.io/badge/ROS2-Humble-22314E?style=for-the-badge&logo=ros&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Raspberry%20Pi%205-C51A4A?style=for-the-badge&logo=raspberry-pi&logoColor=white)
![DOF](https://img.shields.io/badge/DOF-12-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active%20Development-brightgreen?style=for-the-badge)

**A fully custom-built, 3D-printed quadruped robot dog with onboard computer vision, inverse kinematics, and gait planning.**
Every servo, circuit, and line of code — designed and built from scratch.

[Features](#-capabilities) · [Hardware](#-hardware) · [Code](#-core-code) · [Quick Start](#-quick-start) · [Roadmap](#️-roadmap)

---

</div>

## 📊 Specs at a Glance

| | | | |
|:---:|:---:|:---:|:---:|
| **12** DOF (3 per leg) | **2.1 kg** total weight | **~45 cm** body length | **~90 min** battery life |
| MG996R Servos | 3S LiPo 5000mAh | Raspberry Pi 5 | OAK-D Lite Vision |

---

## 🔧 Hardware

| Component | Part | Purpose |
|---|---|---|
| 🧠 **Main Computer** | Raspberry Pi 5 (8GB) | Runs ROS2, vision pipeline, gait controller |
| ⚙️ **Servos** | MG996R × 12 | 3 per leg — hip abduction, hip flexion, knee flex (10 kg·cm) |
| 👁️ **Depth Camera** | OAK-D Lite | Stereo depth + YOLOv8 neural inference for obstacle detection |
| 🎛️ **PWM Driver** | PCA9685 | 16-ch I²C PWM board — drives all 12 servos from a single board |
| 📡 **IMU** | MPU-6050 | 6-axis accelerometer + gyroscope for balance feedback |
| 🔋 **Battery** | 3S LiPo 5000mAh | 11.1V, regulated via buck converters to 5V / 6V rails |
| 🖨️ **Chassis** | Custom 3D Print | PLA+ body panels, TPU foot pads — STEP files included |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────┐
│         Raspberry Pi 5          │
│  ┌──────────┐  ┌─────────────┐  │
│  │ ROS2 Core│  │ Vision Node │  │
│  │          │  │  (OAK-D)    │  │
│  │ Gait     │  │  YOLOv8-n   │  │
│  │ Planner  │  └─────────────┘  │
│  │    +     │  ┌─────────────┐  │
│  │ IK Solver│  │  IMU Node   │  │
│  └────┬─────┘  │  MPU-6050   │  │
│       │        └─────────────┘  │
└───────┼─────────────────────────┘
        │ I²C
   ┌────▼──────┐
   │  PCA9685  │  PWM
   └─┬──┬──┬──┘
     │  │  │
   [FL][FR][BL][BR]  ← 3 servos per leg
```

---

## 💻 Core Code

### Inverse Kinematics Solver

```python
# ik_solver.py — Analytical 3-DOF leg IK
import numpy as np

class LegIKSolver:
    def __init__(self, L1=0.10, L2=0.12, L3=0.08):
        """
        L1 = hip offset (m)
        L2 = thigh length (m)
        L3 = shin length (m)
        """
        self.L1, self.L2, self.L3 = L1, L2, L3

    def solve(self, x, y, z):
        """Return joint angles [θ1, θ2, θ3] in degrees for foot target (x, y, z)."""
        theta1 = np.arctan2(y, x)

        R    = np.sqrt(x**2 + y**2) - self.L1
        D    = np.sqrt(R**2 + z**2)
        cos2 = (D**2 - self.L2**2 - self.L3**2) / (2 * self.L2 * self.L3)
        theta3 = np.arccos(np.clip(cos2, -1, 1))

        beta   = np.arctan2(self.L3 * np.sin(theta3),
                            self.L2 + self.L3 * np.cos(theta3))
        theta2 = np.arctan2(z, R) - beta

        return np.degrees([theta1, theta2, theta3])
```

### Trot Gait Controller

```python
# gait_controller.py — Diagonal pair trot gait
import numpy as np

class TrotGait:
    # Diagonal pairs share phase; opposite pairs offset by 0.5
    PHASE = {'FL': 0.0, 'BR': 0.0, 'FR': 0.5, 'BL': 0.5}

    def trajectory(self, t, leg, stride=0.05, lift=0.03):
        """
        Returns (x, y, z) foot position for leg at normalized time t (0.0–1.0).
        Swing phase: foot lifts and swings forward.
        Stance phase: foot pushes back on ground.
        """
        phase = (t + self.PHASE[leg]) % 1.0

        if phase < 0.5:   # swing
            x = stride * np.sin(np.pi * phase * 2)
            z = lift   * np.sin(np.pi * phase * 2)
        else:             # stance
            x = stride * (1 - 2 * (phase - 0.5))
            z = 0.0

        return x, 0.0, z
```

### Servo Driver

```python
# servo_driver.py — PCA9685 servo control
import board, busio
from adafruit_pca9685 import PCA9685
from adafruit_motor import servo

i2c = busio.I2C(board.SCL, board.SDA)
pca = PCA9685(i2c)
pca.frequency = 50  # Hz

def set_angle(channel, angle):
    """Set servo on given PCA9685 channel to angle (0–180°)."""
    s = servo.Servo(pca.channels[channel], min_pulse=500, max_pulse=2500)
    s.angle = max(0, min(180, angle))

# Leg → channel mapping (hip_ab, hip_flex, knee)
LEG_CHANNELS = {
    'FL': [0, 1, 2],
    'FR': [3, 4, 5],
    'BL': [6, 7, 8],
    'BR': [9, 10, 11],
}
```

---

## 🐾 Gait Modes

| Gait | Speed | Stability | Use Case | Status |
|---|---|---|---|---|
| Crawl (Static Walk) | ~0.15 m/s | ⭐⭐⭐⭐⭐ | Rough terrain, slopes | ✅ Done |
| Trot (Diagonal Pair) | ~0.50 m/s | ⭐⭐⭐⭐ | Default locomotion | ✅ Done |
| Pace (Lateral Pair) | ~0.60 m/s | ⭐⭐⭐ | Narrow corridors | 🔄 In Progress |
| Bound / Gallop | >1.0 m/s | ⭐⭐ | Speed runs | 📋 Planned |

---

## ⚡ Capabilities

- **📐 Inverse Kinematics** — Analytical 3-DOF IK for precise foot placement in 3D space
- **🏔️ Terrain Adaptation** — IMU-driven body leveling on uneven surfaces up to 15°
- **🎯 Object Detection** — YOLOv8-nano running on OAK-D Lite for real-time obstacle avoidance
- **🌐 ROS2 Integration** — Full node graph, URDF robot model, and Rviz2 visualization
- **📱 Remote Control** — Wireless joystick control via WebSocket + custom web dashboard on port 8080
- **🖨️ Fully 3D Printed** — Complete STEP + STL files included for PLA+ chassis and TPU foot pads

---

## 🛠️ Tech Stack

**Robotics & Control**
`Python 3.11` `ROS2 Humble` `NumPy` `smbus2` `adafruit-circuitpython-pca9685`

**Computer Vision**
`OpenCV` `DepthAI SDK` `YOLOv8-nano`

**Web & Comms**
`FastAPI` `WebSockets`

**Visualization & Simulation**
`Rviz2` `URDF`

**Mechanical & Hardware Design**
`Fusion 360` `PrusaSlicer` `KiCad`

---

## 🚀 Quick Start

### Prerequisites
- Raspberry Pi 5 running Raspberry Pi OS (64-bit)
- ROS2 Humble installed → [install guide](https://docs.ros.org/en/humble/Installation.html)
- Python 3.11+

### Install & Run

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/k9-x1.git
cd k9-x1

# 2. Install Python dependencies
pip install -r requirements.txt

# 3. Source ROS2 and build the workspace
source /opt/ros/humble/setup.bash
colcon build --symlink-install
source install/setup.bash

# 4. Launch the full robot stack
ros2 launch k9_bringup full_dog.launch.py

# 5. Open the web dashboard (in a new terminal)
python dashboard/server.py
# → http://localhost:8080
```

### Simulate Only (no hardware needed)

```bash
ros2 launch k9_bringup sim.launch.py
```

---

## 📁 Repository Structure

```
k9-x1/
├── hardware/
│   ├── cad/              # Fusion 360 + STEP + STL files
│   ├── kicad/            # PCB schematics and layouts
│   └── bom.csv           # Full bill of materials
├── src/
│   ├── ik_solver.py      # Inverse kinematics
│   ├── gait_controller.py# Gait patterns (trot, crawl, pace)
│   ├── servo_driver.py   # PCA9685 servo abstraction
│   ├── imu_reader.py     # MPU-6050 balance feedback
│   └── vision_node.py    # OAK-D + YOLOv8 obstacle detection
├── k9_bringup/
│   └── launch/           # ROS2 launch files
├── dashboard/
│   └── server.py         # FastAPI web control dashboard
├── urdf/
│   └── k9x1.urdf         # Robot model for Rviz2
├── requirements.txt
└── README.md
```

---

## 🗺️ Roadmap

- [x] Mechanical build + wiring (all 12 servos, power distribution, chassis)
- [x] Analytical IK solver + stable standing pose
- [x] Trot gait locomotion (forward, backward, turning)
- [ ] Terrain adaptation with IMU closed-loop feedback
- [ ] SLAM + autonomous navigation with Nav2
- [ ] Reinforcement learning gait policy (sim-to-real via MuJoCo)
- [ ] Voice command integration

---

## 📸 Build Gallery

> *Add your photos here!*

| Chassis Assembly | Electronics | Walking Demo |
|:---:|:---:|:---:|
| ![chassis](images/chassis.jpg) | ![electronics](images/electronics.jpg) | ![walking](images/walking.gif) |

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">

Built with ❤️ and too many late nights

⭐ Star this repo if you found it useful!

</div>

