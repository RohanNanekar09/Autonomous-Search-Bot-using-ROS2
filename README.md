# Autonomous-Search-Bot-using-ROS2
project:
  name: Autonomous Search Bot
  description: >
    A ROS2-based autonomous mobile robot capable of mapping unknown environments
    using SLAM and navigating to user-defined goals using Nav2.

key_features:
  - Real-time mapping using SLAM Toolbox
  - Autonomous navigation using Nav2 stack
  - Goal-based navigation (RViz / voice commands)
  - Obstacle avoidance with costmaps
  - Modular ROS2 node architecture
  - Simulation support using Gazebo

tech_stack:
  middleware: ROS2 (Jazzy)
  language: C++ (cpp)
  slam: slam_toolbox
  navigation: nav2
  visualization: rviz2
  simulator: gazebo
 

system_workflow:
  - step: 1
    name: SLAM Mapping
    description: Robot explores unknown environment and builds a map in real-time

  - step: 2
    name: Map Saving
    description: Generated map is saved for later use

  - step: 3
    name: Localization
    description: Robot localizes itself on the saved map using AMCL

  - step: 4
    name: Navigation
    description: Nav2 computes path and navigates to goal positions

deep_process:
  slam:
    input: LiDAR / sensor data
    output: Occupancy grid map
    node: slam_toolbox

  localization:
    method: AMCL
    input: Saved map + sensor data
    output: Robot pose (x, y, theta)

  planning:
    global_planner: Computes optimal path
    local_planner: Avoids obstacles in real-time

  control:
    output: Velocity commands (/cmd_vel)

ros2_commands:

  mapping:
    description: Start SLAM mapping
    command: |
      ros2 launch slam_toolbox online_async_launch.py

  teleop:
    description: Control robot manually during mapping
    command: |
      ros2 run teleop_twist_keyboard teleop_twist_keyboard

  save_map:
    description: Save generated map
    command: |
      ros2 run nav2_map_server map_saver_cli -f ~/map

  localization:
    description: Start localization using saved map
    command: |
      ros2 launch nav2_bringup localization_launch.py map:=~/map.yaml

  navigation:
    description: Start Nav2 navigation stack
    command: |
      ros2 launch nav2_bringup navigation_launch.py use_sim_time:=true

  rviz:
    description: Open RViz for visualization
    command: |
      rviz2

  set_goal:
    description: Set navigation goal from RViz
    method: "Use '2D Goal Pose' tool in RViz"

file_structure:
  maps:
    - map.yaml
    - map.pgm

  launch:
    - slam_launch.py
    - navigation_launch.py
    - localization_launch.py

  config:
    - nav2_params.yaml
    - amcl_params.yaml

future_improvements:
  - Multi-goal navigation
  - Dynamic obstacle handling
  - Improved localization accuracy
  - Real robot deployment

author:
  name: Rohan Nanekar
  field: Automation and Robotics Engineering
