# baramex

UR5 + Robotiq 85 gripper simulation in Gazebo/MoveIt with pick-and-place and joint/TCP position control (ROS 1).

## Overview

This repository is a snapshot of the `src` folder of a ROS 1 (catkin) workspace, committed in a single commit in November 2022. It gathers the packages needed to simulate a Universal Robots UR5 arm with a Robotiq 85 two-finger gripper in Gazebo, plan motions with MoveIt, and run a simple pick-and-place task. Most of the contents are third-party: vendored upstream packages plus packages from the roboticscasual.com ROS tutorials.

## Repository layout

| Folder | Origin | Contents |
| --- | --- | --- |
| `universal_robot/` | Vendored: ROS-Industrial `universal_robot` (v1.2.7) | UR descriptions, Gazebo and MoveIt configs, driver, kinematics, messages. Also contains `ur_description/urdf/ur5_robotiq85_gripper.urdf.xacro`, the UR5 + gripper model used by the MoveIt config below. |
| `robotiq/` | Vendored: utecrobotics `robotiq` | Robotiq 85 gripper description (`robotiq_description`) and Gazebo plugins (`robotiq_gazebo`). |
| `ur5_gripper_moveit_config/` | roboticscasual.com tutorial | MoveIt configuration for the UR5 + Robotiq 85 gripper, including `demo_gazebo.launch`. |
| `ur5_simple_pick_and_place/` | roboticscasual.com tutorial | Pick-and-place nodes using the MoveIt C++ move group interface (`pick_and_place`, `pick_and_place_collision`, `pick_and_place_collision_depth_camera`) and Gazebo world files. |
| `ur5-joint-position-control/` | roboticscasual.com tutorial | `ros_control` joint position controllers for the UR5 in Gazebo. |
| `ur5-tcp-position-control/` | roboticscasual.com tutorial | `tcp_position_controller` node that moves the UR5 tool center point using KDL inverse kinematics. |

Origins are taken from each package's `package.xml` and README; the four `ur5*` packages each link to the roboticscasual.com tutorial they accompany. The history does not show which files, if any, were changed locally.

## Requirements

ROS 1 with catkin, Gazebo (`gazebo_ros`), MoveIt, `ros_control` and KDL (`kdl_parser`). Per-package dependencies are listed in each `package.xml`.

## Usage

Place the folders under the `src` folder of a catkin workspace, then:

```bash
catkin_make
source devel/setup.bash

# UR5 + gripper in Gazebo with MoveIt and RViz, then the pick-and-place node
roslaunch ur5_gripper_moveit_config demo_gazebo.launch
rosrun ur5_simple_pick_and_place pick_and_place

# UR5 with joint position controllers in Gazebo (starts paused), then TCP control
roslaunch ur5-joint-position-control ur5_gazebo_joint_position_control.launch
rosrun ur5-tcp-position-control tcp_position_controller
```

## Status

2022 project (single commit, November 2022), kept as a record and no longer maintained.

## License

No license file is included. The vendored third-party packages keep the licenses declared in their own `package.xml` files.
