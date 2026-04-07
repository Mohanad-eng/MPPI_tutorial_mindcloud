# MPPI_tutorial_mindcloud
* First we will use turtlebot3 named waffle
``export TURTLEBOT3_MODEL=waffle``

* every terminal you will run export it
  
why ?

TURTLEBOT3_MODEL 

This is a specific variable used by TurtleBot3 packages.

It tells ROS:

“Which robot model should I load?”

Then it decides:

1. which URDF (robot model) to load
2. which sensors to enable
3. which topics/nodes to start

**This only works in the current terminal session.**

**If you open a new terminal → you must run it again.**

👉 To make it permanent:

``echo "export TURTLEBOT3_MODEL=waffle" >> ~/.bashrc`` 

``source ~/.bashrc``

## second to use mppi we must have  amap we will use slam_toolbox pkg from nav2 :

``sudo apt install -y ros-jazzy-slam-toolbox``

``ros2 launch slam_toolbox online_async_launch.py \use_sim_time:=true``
  
## then to control turtlebot3 we use the tekleop to make a map 
``export TURTLEBOT3_MODEL=waffle`` 

``ros2 run turtlebot3_teleop teleop_keyboard``

* Drive around the entire world until the map looks complete in RViz2. 

here is an image to how setup rviz2 

* then save map
  
``mkdir -p ~/mppi_ws/map``
``ros2 run nav2_map_server map_saver_cli \
  -f ~/mppi_ws/map/map \
  --ros-args -p use_sim_time:=true``

  map shape
  
* Verify there is a map:
``bashls ~/mppi_ws/map/``

# should show: map.pgm  map.yaml

## The Work Flow is As follows : 

Terminal 1 → Gazebo

Terminal 2 → SLAM (build map once)

Terminal 3 → Teleop (drive to build map)

Terminal 4 → Save map

Terminal 5 → Nav2 with MPPI

Terminal 6 → RViz2

## You have a custom turtlebot3 workspace. 
 Source It and start the project 
# commands of mppi 
## Terminal 1 :

``export TURTLEBOT3_MODEL=waffle``

``source ~/turtlebot3_ws/install/setup.bash``

``ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py``

## Terminal 2 :

``export TURTLEBOT3_MODEL=waffle``

``source ~/turtlebot3_ws/install/setup.bash``

`` ros2 launch turtlebot3_navigation2 navigation2.launch.py   use_sim_time:=true   map:=/home/mohanad/mppi_ws/map/map.yaml   params_file:=/home/mohanad/mppi_ws/params/nav2_mppi_params.yaml``

## In Rviz2 :

Click "2D Pose Estimate" → click where the robot is on the map → drag to set direction
Wait a few seconds for AMCL to localize

Click "Nav2 Goal" → click anywhere on the map → drag to set direction
Robot should start moving using MPPI

> Photos from Rviz Window 

![Photo From Rviz2 Simulation](<Screenshot from 2026-04-07 07-44-41.png>)

![Photo From Rviz2 Simulation](<Photo1>)

> And here is a Video for the simulation

![video](<Screencast from 2026-04-07 07-41-59.webm>)
