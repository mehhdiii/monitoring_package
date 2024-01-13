This package is a support package to the [bug-algorithm-using-action-server package](https://github.com/CarmineD8/assignment_2_2023) developed by [Dr. Carmine Recchiuto](https://rubrica.unige.it/personale/UkNDWV1r). This support package is useful to set goal pose and monitoring of the states of bug0-planner package. A brief description of the packages is given below, along with instructions for compiling and running the simulation:

# Package Organization
Just to be clear, there are two seperate packages being discussed here: 
1. bug0-package: bug algorithm using action server [package](https://github.com/CarmineD8/assignment_2_2023)
2. monitoring_package: this repository

# Monitoring Package

## Action Client

This node consumes Action Server implemented in [bug0-package](https://github.com/CarmineD8/assignment_2_2023). It is responsible to set target position of the robot. Additionally, it also subscribes to odometry data and publishes robot's current position and velocity. Details are as follows: 

`planning_client()`: 

function which sends target pose to the action server. It waits for the action server to be ready, and then sends the goal pose.

`initialize_publisher_subscriber()`: 

funciton which initializes publisher (on topic: `current_pose_velocity_publisher`) and subscriber (on topic: `/odom`). 

`publishPoseAndVelocity(msg)`: 

Callback function to the subscriber (topic: `/odom`) which also publishes current pose of the robot on the topic `current_pose_velocity_publisher`.

## ROS Services

## Launch file

# Simulation pseudo code

# Compiling using docker


# Planner Package: Bug Algorithm Using Action Server 
[This](https://github.com/CarmineD8/assignment_2_2023) package implements the bug0 algorithm on ROS noetic. The folder structure is described as follows:

```
├── action
│   ├── Planning.action
├── config
│   ├── ...
├── launch
│   ├── assignment1.launch
│   ├── sim_w1.launch
├── scripts
│   ├── bug_as.py
│   ├── go_to_point_service.py
│   ├── wall_follow_service.py
├── urdf
│   ├── ...
└── world
│   ├── robot2_laser.gazebo
│   ├── robot2_laser.xacro
└── CMakeList.txt
└── package.xml
```
Let us briefly go through each file. 


## Action Server

### `planner.action`: 

`geometry_msgs/PoseStamped target_pose`: the goal of the action server. This is set by the action-client

`geometry_msgs/Pose actual_pose` and `string stat`: the feedback of the action-server. This is returned continously by the action server when the action is is progress. 
#### Description
This is an action message which is used as a data interface when communicating with the action server. It contains the three parts (seperated by three hyphins "---" and in-order):
1. goal (`target_pose`): the goal pose.
2. result (`None`): The result of the action-server. We do not use this in our context. It can although be used to flag success when robot reaches goal.
3. feedback (`actual_pose, stat`): The actual pose of the robot and stats related to it.
   
## ROS Nodes and services

### `scripts/bug_as.py`
This node is responsible for the implementation of the bug0 algorithm. It takes in environment information, and switches between go_to_point_service and wall_follower_service. 

### `scripts/go_to_point_service.py`
Moves the robot towards the target point when no obstacle between robot and target.

### `scripts/wall_follow_service.py`
Makes the robot such that it follows a wall. 

## Launch files:
 
### `launch/assignment1.launch`
launches relevant nodes (bug0, go_to_point, wall_follower, gazebo, rviz) for the simulation

## catkin Package files
- `CMakeList.txt`
- `package.xml`

The above are standard package files for describing build and runtime dependencies. More details can be found on [ros-wiki](https://wiki.ros.org/ROS/Tutorials/CreatingPackage)

