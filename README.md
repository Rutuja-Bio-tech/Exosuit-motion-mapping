# Exosuit-motion-mapping

Gesture-based control system for an exosuit that maps human intent to actuator motion with built-in safety constraints and unified logging, suitable for Gazebo simulation and real-time control.

---

## 📌 Overview

This repository implements a **data-driven motion control pipeline** for an exosuit system.  
Human gestures (from EMG or other sensors) are converted into **semantic intents**, which are then mapped to **safe actuator commands** using a CSV-based configuration.

The design prioritizes:
- Safety
- Reproducibility
- Auditability
- Easy gesture expansion

---

## 🎯 Control Pipeline



Gesture → Intent → Motor Mapping (CSV)
→ Safety Validation
→ Actuator Command
→ Unified Logging


All motion logic is derived from configuration files rather than hard-coded rules.

---

## 📁 Repository Structure



.
├── motor_mapping_extended.csv # Gesture → intent → actuator mapping
├── run_exosuit_alpha.py # One-command demo pipeline
├── logger/
│ ├── init.py
│ └── unified_logger.py # Unified logging module
└── logs/
└── exosuit_alpha_log.csv # Runtime execution logs


---

## 📄 Motor Mapping CSV

### `motor_mapping_extended.csv`

This CSV acts as the **single source of truth** for all movements.

| Column Name | Description |
|------------|-------------|
| `gesture` | Physical gesture detected from sensors |
| `intent` | Classified semantic intent |
| `motorcmd_id` | Actuator or motor group identifier |
| `joint_name` | Target joint (must match URDF) |
| `angle_min_deg` | Reference / start angle |
| `angle_max_deg` | Target angle (safety-limited) |

Updating motion behavior requires **only CSV changes**, not code edits.

---

## 🦴 Safety Enforcement

Spine joints are protected using conservative biomechanical limits:

```python
SPINE_LIMITS = {
    "spine_mid": (-12, 12),
    "spine_lower": (-10, 10)
}


If a command exceeds safe limits:

Motion is blocked

A safe pose is returned

The event is logged as UNSAFE

Haptic feedback can be triggered

📝 Unified Logging

All commands (safe and unsafe) are logged to:

logs/exosuit_alpha_log.csv

Log Fields
Field	Description
timestamp	Execution time
gesture	Input gesture
intent	Classified intent
motorcmd_id	Actuator identifier
angle	Requested angle
safety_flag	SAFE / UNSAFE
haptic_flag	ON / OFF

This enables debugging, replay, dashboards, and safety audits.

▶ Running the Demo

Execute the complete pipeline with a single command:

python run_exosuit_alpha.py


The demo sequence includes:

Hand open / close

Foot stepping

Spine flexion

Micro posture adjustment

Spine locking (safety case)

Console output indicates safe or unsafe actions, and all events are logged.

✅ Task Coverage
Task	Description	Status
Task 1	CSV-based motion mapping	✅
Task 2	Spine safety validation	✅
Task 3	Command generation	✅
Task 4	Unified logging	✅
Task 5	One-command demo	✅
🚀 Future Extensions

ROS2 integration

Real-time EMG / EOG / IMU inputs

Adaptive safety envelopes

Hardware-in-the-loop testing

Clinical rehabilitation workflows

🏁 Summary

Exosuit-motion-mapping demonstrates a modular, safety-first approach to wearable robot control.
The architecture is suitable for research, simulation, and future real-world deployment.
