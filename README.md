# ros-slam-with-turtlebot3-YOLO-LiDAR
Autonomous warehouse exploration and mapping system using a TurtleBot3 and ROS2. Features AprilTag-triggered object detection (YOLOv8), SLAM-based localization, and a finite state machine for mission coordination.
# This project is part of my university course projetct ARAMS – Autonomous Robot Exploration and Mapping System (ROS 2)

> **Autonomous exploration, AprilTag-based identification, and object mapping using TurtleBot3 in a simulated warehouse environment**

📹 **Demo Video** Click to see it on action: [FULL VIDEO](https://drive.google.com/drive/u/3/folders/1Fd8xL_St2xONka2jJihXiYY5CI0iJ5w3)
#Short Demonstration 
https://github.com/user-attachments/assets/6a3be1e8-1bb1-430f-8730-63843ce6faee


📄👥 **Authors**
[Panithi Thunkao](https://www.linkedin.com/in/panithi-thungkao-a2234b224/) 
Duy Linh Nguyen

🔒 **Source Code:** Private to FH Aachen (architecture and results documented below)

---

## 📌 Project Overview

ARAMS (Autonomous Robot Exploration and Mapping System) is a ROS 2–based autonomous robotics project developed for a TurtleBot3 platform.  
The system enables a mobile robot to:

- Navigate autonomously in a simulated warehouse
- Detect **AprilTags** and associated objects
- Perform **object recognition using YOLOv8**
- Map detected objects to AprilTag IDs
- Record results automatically
- Return safely to its original starting position

The project demonstrates **end-to-end autonomy**, combining perception, localization, navigation, and mission coordination using modular ROS 2 nodes.

---

## 🎯 Objectives

- Autonomous navigation using **Nav2**
- Reliable **AprilTag detection and gating**
- Real-time object detection using **YOLOv8**
- Robust localization with **AMCL + SLAM Toolbox**
- Mission control via a **finite state machine (FSM)**
- Persistent logging of detected objects

---

## 🧠 System Architecture

The system is composed of multiple ROS 2 packages, each responsible for a specific subsystem.

### 🔹 Core Nodes

| Package | Node | Description |
|------|------|------------|
| `coordinator` | `/coordinator` | Mission logic and state machine |
| `object_detection` | `/yolo_listen` | YOLOv8-based object detection |
| `tag_detection` | `/apriltag_node` | AprilTag detection |
| `tag_detection` | `/tag_gate_node` | Enables/disables tag forwarding |
| `robot_slam` | `/map_server`, `/amcl`, `/slam_toolbox` | Mapping & localization |
| `tag_to_image_saver` | `/tag_triggered_image_saver` | Auto-save images on tag detection |

---

## 🚀 Launch Structure

| Launch File | Purpose |
|------------|--------|
| `bringup.launch.py` | Full autonomous system startup |
| `nav.launch.py` | Navigation stack (Nav2) |
| `slamlocalization.launch.yaml` | Localization using pre-built map |
| `slamtoolbox.launch.yaml` | Mapping support |
| `track.launch.yaml` | AprilTag detection |

The **bringup launch file** acts as the main entry point and ensures all subsystems start in the correct order.

---

## 🧭 Navigation & Localization

- **Nav2 Stack** for global and local planning
- **AMCL** for probabilistic localization
- **Pre-built warehouse map** (`.pgm` + `.yaml`)
- Configured via `nav2_params.yaml`
- Obstacle avoidance using local & global costmaps
- Recovery behaviors for robust navigation

---

## 👁️ Perception Pipeline

### AprilTag Detection
- Detects unique AprilTag IDs in the environment
- Gated to reduce noise and false positives
- Used as reference points for object association

### Object Detection (YOLOv8)
- Subscribes to camera images
- Runs inference only when enabled
- Publishes bounding boxes and classifications
- Annotated images optionally generated

---

## 🧩 Mission Coordinator (FSM)

The **coordinator node** manages the entire mission using a finite state machine:

1. Start from known pose
2. Navigate warehouse waypoints
3. Detect AprilTags
4. Trigger object detection
5. Match objects to tags using:
   - Pixel distance
   - Confidence thresholds
   - Debounce logic
6. Record results
7. Return to start position

📁 Results are saved to:
``` txt 
mapped_objects.txt 
```
Each entry includes:

AprilTag ID

Associated object type
## 📸 Data Collection

Whenever an AprilTag is detected:
Camera images are automatically saved and Images are annotated with:
Tag ID
Timestamp
Supports dataset creation for future ML training

## 🛠 Technologies Used
ROS 2
Nav2
SLAM Toolbox
AMCL
YOLOv8
AprilTag
TurtleBot3
Python
**Gitlab for version control**
