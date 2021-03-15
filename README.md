# turtlebot3_robot_localization_ws

This repository's goal is to make a ready to use simulation that use robot_localization package for sensor fusing.

This simulation works under ROS2 Foxy.\
The simulation will use a turtlebot3 waffle in the turtlebot world.\
Turtlebot's urdf and sdf files were modified to add a gps sensor.\
Also the urdf will use the wheel encoder odometry

There are two launch file for robot_localization package :
- First one is sensor fusion with odometry and IMU sensor.
- Second one is  sensor fusion with odometry, IMU and GPS sensor.


Download this repository and enter it:
~~~
git clone https://github.com/cocodmdr/turtlebot3_robot_localization_ws.git
cd turtlebot3_robot_localization_ws
~~~

Install required dependencies and build package:
~~~
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install
~~~

Start gazebo with turtlebot3, it has IMU and GPS sensors.\
Make sure you sourced ROS2 Foxy in each terminal.
~~~
source ~/turtlebot3_robot_localization_ws/install/setup.bash
export TURTLEBOT3_MODEL=waffle
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:~/turtlebot3_robot_localization_ws/src/turtlebot3_simulations/turtlebot3_gazebo/models/
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
~~~

If gazebo is not working properly, kill and relaunch.
~~~
killall gazebo 
killall gzserver
killall gzclient
~~~

Start either the first example or the second one **but not both**:

1. Start robot_localization local ekf example:
~~~
source ~/turtlebot3_robot_localization_ws/install/setup.bash
ros2 launch robot_localization ekf.launch.py
~~~

2. Start robot_localization ekf with gps example:
~~~
source ~/turtlebot3_robot_localization_ws/install/setup.bash
ros2 launch robot_localization dual_ekf_navsat_example_simulation.launch.py
~~~

set datum (see:http://wiki.ros.org/ROS/YAMLCommandLine)
~~~
source ~/turtlebot3_robot_localization_ws/install/setup.bash
ros2 service call /datum robot_localization/srv/SetDatum '{geo_pose: {position: {latitude: 47.84063717753856, longitude: 10.619951493275392, altitude: 740.0870444467291}, orientation: {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}}'
ros2 service call /datum robot_localization/srv/SetDatum '{geo_pose: {position: {latitude: 47.84063717753856, longitude: 10.619951493275392, altitude: 740.0870444467291}, orientation: {x: 0.0, y: 0.0, z: 0.383, w: 0.924}}}'
ros2 service call /datum robot_localization/srv/SetDatum '{geo_pose: {position: {latitude: 47.818646333333334, longitude: 10.623770833333333, altitude: 740.0870444467291}, orientation: {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}}'
ros2 service call /datum robot_localization/srv/SetDatum '{geo_pose: {position: {latitude: 47.840636, longitude: 10.619921, altitude: 740.0870444467291}, orientation: {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}}'
~~~

You can control the virtual TurtleBot3 by using keyboard teleoperation:
~~~
source ~/turtlebot3_robot_localization_ws/install/setup.bash
export TURTLEBOT3_MODEL=waffle
ros2 run turtlebot3_teleop teleop_keyboard
~~~

Visualize in Rviz:
~~~
source  ~/turtlebot3_robot_localization_ws/install/setup.bash
rviz2 -d ~/turtlebot3_robot_localization_ws/src/turtlebot3_simulations/turtlebot3_gazebo/rviz/tb3_gazebo_robot_localization.rviz
~~~

In Rviz you can use the **reset** button when you relaunch the gazebo simulation because there are sometimes problems due to time jumps.

## Turtlebot GPS navigation 

Start gazebo with turtlebot3, it has IMU and GPS sensors.\
Make sure you sourced ROS2 Foxy in each terminal.
~~~
source ~/turtlebot3_robot_localization_ws/install/setup.bash
export TURTLEBOT3_MODEL=waffle
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:~/turtlebot3_robot_localization_ws/src/turtlebot3_simulations/turtlebot3_gazebo/models/
ros2 launch turtlebot3_gazebo empty_world.launch.py
~~~