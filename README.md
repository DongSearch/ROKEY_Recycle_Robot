
# ♻️ ROKEY Recycle Robot

> AI-powered smart recycling robot system using computer vision and collaborative robotics

---

## 📌 Overview

ROKEY Recycle Robot is an intelligent recycling automation project that combines YOLO-based object detection, RealSense depth sensing, ROS communication, and a Doosan collaborative robot arm for autonomous waste sorting and pick-and-place operations. we distrbiute the one with docker version as well

The system is designed to perform:

- Real-time recyclable object detection
- Single / Cluster object classification
- PCA-based object orientation estimation
- Depth-based 3D coordinate calculation
- Autonomous robotic pick-and-place
- Firebase cloud integration
- ROS-based modular communication

---

# 🏗️ System Architecture

```text
┌──────────────────────┐
│   RealSense Camera   │
└─────────┬────────────┘
          │ RGB + Depth
          ▼
┌──────────────────────┐
│   YOLO Detection     │
│ Segmentation / PCA   │
└─────────┬────────────┘
          │ Object Info
          ▼
┌──────────────────────┐
│  ROS Detection Node  │
└─────────┬────────────┘
          │
          ▼
┌──────────────────────┐
│ Robot Control Node   │
│   (Doosan M0609)     │
└─────────┬────────────┘
          │
          ▼
┌──────────────────────┐
│ Pick & Place System  │
└──────────────────────┘
```

<img width="732" height="428" alt="image" src="https://github.com/user-attachments/assets/d79b573b-2c00-458e-9b9b-bb659abdb0a9" />


---

# 🚀 Main Features

## 1. YOLO-based Object Detection

- Real-time detection using YOLO11m
- Detection of PET bottles, cans, and plastic waste
- Bounding box and confidence visualization

---

## 2. Single / Cluster Classification

The system analyzes object groups using HSV morphology and binary contour mapping.

### Water estimation
it measure how much amount of water they have in the bottle checking refraction
- detect line
- Thick
- Length
- ratio between width and height


binary image -> morphology -> crop -> contour -> thick,length,ratio threshold -> check the number of contour detected -> measure water amount
- <img width="592" height="173" alt="image" src="https://github.com/user-attachments/assets/a5687602-24d5-4531-9094-60a7cc3f86eb" />



https://github.com/user-attachments/assets/0a1b394d-4332-4485-9dda-f3532cfd8c8c




https://github.com/user-attachments/assets/53fe12b7-3b00-4020-9889-10a7853653b7






### Classification Logic

- **Single Object** → Direct pick operation
- **Cluster Object** → MoveC separation motion before picking

---

<img width="1316" height="590" alt="사분면 나누기" src="https://github.com/user-attachments/assets/7cdde458-5fd9-49be-93b7-9615b1224139" />

HSV -> find the largest contour to find the place for process -> yellow mask & binary image -> Morphology -> contour each object -> get Complexity, Solidity, Area -> recognize cluster over specific threshold

## 3. PCA-based Yaw Angle Estimation

Object orientation is estimated using PCA (Principal Component Analysis).

### Purpose

- Gripper alignment
- Front/back bottle orientation detection
- Stable grasping performance

---

## 4. Depth-based 3D Coordinate Estimation

Using Intel RealSense depth information:

- Pixel coordinates → Camera coordinates
- Camera coordinates → Robot base coordinates

are calculated for robotic manipulation.

---

## 5. ROS-based Modular System

Each component operates independently as a ROS node.

### Main Nodes

| Node | Description |
|---|---|
| detection_node | YOLO object detection |
| cluster_node | Cluster classification |
| pose_node | Coordinate & angle estimation |
| robot_move_node | Robot motion control |
| firebase_node | Database & web communication |

---

# 🛠️ Tech Stack

## AI / Vision

- Python
- OpenCV
- Ultralytics YOLO
- NumPy
- PCA
- Intel RealSense SDK

## Robotics

- ROS
- Doosan Robotics M0609
- MoveIt
- Gazebo

## Backend / Cloud

- Firebase Firestore
- WebSocket
- Docker

---

# 📂 Project Structure

```bash
ROKEY_Recycle_Robot/
│
├── vision/
│   ├── yolo_detection.py
│   ├── cluster_analysis.py
│   ├── pca_angle.py
│   └── depth_estimation.py
│
├── robot/
│   ├── robot_move.py
│   ├── gripper_control.py
│   └── coordinate_transform.py
│
├── ros/
│   ├── detection_node.py
│   ├── robot_node.py
│   └── services/
│
├── firebase/
│   ├── firestore_manager.py
│   └── websocket_server.py
│
├── dataset/
├── models/
└── README.md
```

---

# ⚙️ Installation

## 1. Clone Repository

```bash
git clone https://github.com/DongSearch/ROKEY_Recycle_Robot.git
cd ROKEY_Recycle_Robot
```

---

## 2. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 3. Install ROS Package

Install the Doosan Robotics ROS package:

```bash
cd ~/catkin_ws/src
git clone https://github.com/doosan-robotics/doosan-robot
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

---

# ▶️ Run

## YOLO Detection

```bash
python vision/yolo_detection.py
```

---

## ROS Launch

```bash
ros launch recycle_robot system.launch
```

---

## Robot Control

```bash
python robot/robot_move.py
```

---

# 🎯 Detection Pipeline

```text
Camera Input
    ↓
YOLO Detection
    ↓
Binary Mapping
    ↓
Contour Analysis
    ↓
Single / Cluster Classification
    ↓
PCA Angle Estimation
    ↓
3D Coordinate Calculation
    ↓
Robot Pick & Place
```

---

# 🤖 Robot Specifications

| Item | Specification |
|---|---|
| Robot | Doosan M0609 |
| Camera | Intel RealSense D435 |
| Framework | ROS |
| Detection Model | YOLO11m |
| Programming Language | Python |

---

# 📊 Expected Outcomes

- Automated recycling waste sorting
- Reduced manual labor
- Improved sorting accuracy
- Stable grasping through orientation estimation
- Cluster object handling capability
- Real-time monitoring and remote management

---

# 📸 Demo Features

- PET Bottle Detection
- Cluster Analysis
- Yaw Angle Estimation
- Pick & Place Operation

---

# 📚 References

- Ultralytics YOLO
- Intel RealSense SDK
- Doosan Robotics ROS Package
- ROS Official Documentation

---

# 👨‍💻 Contributors
- ROKEY Robotics Team(A-1)

