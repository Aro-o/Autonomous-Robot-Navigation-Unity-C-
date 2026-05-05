# Autonomous Robot Navigation (Unity / C#)

A Unity-based autonomous robot navigation system that uses raycast sensors and physics-based wheel control to navigate dynamically changing obstacle courses without any hard-coded paths. Developed as part of the **MSc Artificial Intelligence & Robotics** programme at the **University of Hertfordshire**.

[![Robot Navigation Demo]
(https://drive.google.com/file/d/1gIYTyXzloWKPyDaWyPPOlZINx66VMDnF/view?usp=drivesdk)

---

## Overview

The robot navigates an unknown track entirely autonomously using a reactive sensor-based control system. It detects road boundaries and obstacles in real time using multiple angled raycasts, adjusts speed and torque dynamically based on terrain slope, and compensates friction properties when cornering or climbing. The controller is generalised — it adapts to any track layout without prior knowledge of the course.

---

## How It Works

The controller runs on Unity's `FixedUpdate()` physics loop (~50 times per second) and consists of four core systems:

### 1. Road Tracking
- Left and right raycast sensors cast wide-angle rays to detect road boundaries
- If the road is lost on one side, the robot steers back towards it
- Prevents the robot from drifting off the track edges

### 2. Obstacle Detection
- Three layers of angled raycasts on each side detect obstacles at varying distances and angles
- Close-range rays (offset 1) react to immediate obstacles
- Mid-range rays (offset 2) give early warning for upcoming hazards
- Steering is applied reactively to avoid detected objects

### 3. Slope & Terrain Compensation
- The `EulerAnglesSensor` reads the robot's pitch and roll angles in real time
- On uphill slopes (pitch > 6°): drive torque is boosted and friction stiffness increases to maintain grip
- On downhill slopes (pitch < -6°): torque is reduced and brakes engage if speed exceeds threshold
- On sharp rolls (> 12°): braking is triggered to prevent rollover

### 4. Dynamic Speed Control
- Drive torque adjusts continuously based on current velocity
- Slow speed triggers a torque boost to get moving
- As speed increases through defined thresholds, torque is progressively reduced
- Prevents the robot from crawling slowly or overshooting corners

---

## Sensor Configuration

| Sensor | Type | Purpose |
|---|---|---|
| `RaycastSensorFront` | Raycast | Forward road detection |
| `RaycastSensorLeft` | Raycast | Left boundary & obstacle detection (3 angle offsets) |
| `RaycastSensorRight` | Raycast | Right boundary & obstacle detection (3 angle offsets) |
| `EulerAnglesSensor` | Euler Angles | Pitch and roll measurement for slope compensation |

---

## Key Parameters

| Parameter | Value | Description |
|---|---|---|
| `maxSteeringAngle` | 30° | Maximum wheel steering angle |
| `driveTorque` | 70 (base) | Motor torque applied to all four wheels |
| `velocityScale` | 1.42 | Speed scaling multiplier |
| `turnResponsiveness` | 1.25 | Steering sensitivity multiplier |
| `firstSensorRange` | 5 units | Close obstacle detection range |
| `secondSensorRange` | 6 units | Mid obstacle detection range |
| `thirdSensorRange` | 6 units | Road boundary detection range |

---

## Technical Implementation

- **4-wheel drive** — motor torque applied to all four `WheelCollider` components
- **Front-wheel steering** — steering angle applied to front left and right colliders
- **Wheel visual sync** — `GetWorldPose()` used to keep visual wheel transforms in sync with physics colliders
- **Layer-based detection** — road and obstacle objects assigned to separate Unity layers (`Road`, `Obs`) for targeted raycasting
- **Friction management** — forward and sideways friction curves stored and dynamically updated per wheel based on terrain conditions
- **Signed angle conversion** — Euler angles normalised to ±180° range for accurate slope reading

---

## Technologies Used

- Unity (C#)
- Unity WheelCollider physics
- Raycast-based perception
- Euler angle sensor for orientation
- Physics-based motor & brake control

---

## Getting Started

### Prerequisites
- Unity 2021.3 LTS or above

### Running the Project

1. Clone this repository:
   ```bash
   git clone https://github.com/Aro-o/Autonomous-Robot-Navigation.git
   ```
2. Open the project in Unity
3. Replace the existing `RobotController.cs` with the one from this repository
4. Press **Play** in the Unity Editor to run the simulation

---

## Author

**Aroosha Rasheed**  
MSc Artificial Intelligence & Robotics, University of Hertfordshire  
[LinkedIn](https://www.linkedin.com/in/aroosha-rasheed-a23950219) • [GitHub](https://github.com/Aro-o)
