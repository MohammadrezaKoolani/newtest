# Aruco Marker Navigation

## Overview
This repository contains the code and documentation for the Aruco Marker Navigation assignment. The goal of this assignment is to implement a navigation system for a robot to reach specific Aruco markers in both simulation and with a real robot.

## Environment
The robot starts at the coordinates (0, 0), and there are four markers in the environment with IDs 11, 12, 13, and 15. The markers have the following meanings:
- Marker 11: Rotate until you find Marker 12; then reach Marker 12.
- Marker 12: Rotate until you find Marker 13; then reach Marker 13.
- Marker 13: Rotate until you find Marker 15; then reach Marker 15.
- Marker 15: Done! (Task completion marker)

**Note:** "Reach Marker xxx" means that one side of Marker xxx must be at least 200 pixels in the camera frame.

## Implementation Details

### Simulation
In the simulation, the "search" task is done by rotating the camera without rotating the whole robot.

### Real Robot
For the real robot, connect to the ROSbot using SSH, and run the provided launch file:

```python
roslaunch tutorial_pkg all.launch

```
## Running the Code
### Simulation:
In ROS, launch the Gazebo environment and the ROSbot simulation:

```python
roslaunch gazebo_ros empty_world.launch
roslaunch rosbot_bringup rosbot_gazebo.launch
```

###Real Robot
Connect to the ROSbot via SSH and run the following command:
```python
roslaunch tutorial_pkg all.launch
```

## Code Description

#### ROS Robot Controller Logic Code

The robot_controller script serves as the logic for controlling a robot's behavior based on information received from markers. It subscribes to a custom ROS message type (info.msg) that contains marker information.

#### Dependencies:
rospy: ROS Python library
Twist: ROS message type for sending velocity commands
info: Custom ROS message type for exchanging marker information
math: Python math library

#### Initialization:
The ROS node is initialized with the name 'robot_controller'.
Initial values such as the list of markers (marker_list), distance threshold (distance_th), misalignment threshold (misalignment_th), and the initial state (state) are set.
ROS publishers and subscribers are set up, including the /cmd_vel topic for velocity commands and the /info topic for marker information.

#### Callback Function:
The info_clbk function is a callback for the /info topic, updating the script's internal msg variable with the received marker information.

#### Search Function:
The search function is responsible for searching for a specified marker.
It adjusts the robot's angular velocity to rotate until the desired marker is detected.
Upon detection, it transitions the state to 'align'.

#### Align Function:
The align function is called when the robot is misaligned with the marker's center.
It adjusts the robot's angular velocity to align the marker's center with the center of the camera frame.
Upon alignment, it transitions the state to 'drive'.

#### Drive Function:
The drive function moves the robot forward toward the detected marker.
It adjusts the robot's linear velocity based on the distance from the marker.
Upon reaching the desired marker, it updates the marker index and transitions back to the 'search' state.
If all markers are found, the state transitions to 'drive', stopping the robot.

#### Main Loop:
The main_loop function is the main execution loop of the script.
It continuously checks the robot's state and executes the corresponding behavior.

#### Script Execution:
An instance of the RobotController class is created.
The main_loop function is called, and the script continues to run until a ROS interrupt occurs.

