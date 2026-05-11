# Line Tracking with Obstacle Detection and Avoidance

ROS 2 Humble project for a TurtleBot3-style robot that follows a dark line and avoids obstacles in Gazebo Classic using camera and LiDAR data.

## Overview

The project combines three ROS 2 nodes:

- `line_detector`: processes `/camera/image_raw` and publishes lateral line error on `/line_error`
- `obstacle_detector`: processes `/scan` and publishes obstacle state and distance topics
- `controller`: uses PID line following plus a finite-state obstacle avoidance routine to publish `/cmd_vel`

## Repository Structure

```text
.
├── README.md
└── mars_anti2/
    ├── run.txt
    └── src/
        └── line_tracker/
            ├── line_tracker/
            │   ├── controller.py
            │   ├── line_detector.py
            │   └── obstacle_detector.py
            ├── launch/
            │   └── simulation.launch.py
            ├── worlds/
            │   └── line_world.world
            ├── urdf/
            │   └── turtlebot3_burger.urdf
            ├── config/
            │   └── rviz_config.rviz
            ├── package.xml
            ├── setup.py
            └── README.md
```

## Prerequisites

- Ubuntu 22.04
- ROS 2 Humble
- Gazebo Classic
- `colcon`

## Build

```bash
cd mars_anti2
source /opt/ros/humble/setup.bash
colcon build --packages-select line_tracker
source install/setup.bash
```

## Run the Simulation

```bash
cd mars_anti2
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch line_tracker simulation.launch.py
```

## Useful Topics

```bash
ros2 topic echo /line_error
ros2 topic echo /obstacle/detected
ros2 topic echo /obstacle/min_distance
ros2 topic echo /cmd_vel
```

## Key Parameters

The main controller and perception parameters are defined in:

- `mars_anti2/src/line_tracker/launch/simulation.launch.py`

Important parameters include:

- `Kp`, `Ki`, `Kd`
- `linear_speed`
- `max_angular`
- `obstacle_distance_threshold`
- `phase1_turn_duration`
- `phase2_forward_duration`
- `phase3_turn_duration`
- `phase4_forward_duration`

## Notes

- The package-level documentation is also available at `mars_anti2/src/line_tracker/README.md`.
- `mars_anti2/run.txt` contains a one-line command sequence for building and launching the project.
