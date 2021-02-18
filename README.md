# turtlebot3_robot_localization_ws

This repository's goal is to make a ready to use simulation that use robot_localization package for sensor fusing\
This simulation works under ROS2 Foxy\
The simulation will use a turtlebot3 waffle in the turtlebot world\
The turtlebot urdf and sdf files were modified to add a gps sensor\
Also the urdf will use an encoder wheel odometry\
There are two launch file for robot_localization package :\
-first one is sensor fusion with odometry and IMU sensor\
-second one is  sensor fusion with odometry, IMU and GPS sensor

download robot_localisation in your src folder
~~~
cd turtlebot3_robot_localization_ws/src/
git clone --single-branch --branch foxy-devel https://github.com/cra-ros-pkg/robot_localization.git
~~~

install required dependencies and build package
~~~
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install --packages-select robot_localization
~~~

start gazebo with turtlebot3, it has IMU and GPS sensors
~~~
source turtlebot3_robot_localization_ws/install/setup.bash
export TURTLEBOT3_MODEL=waffle
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/home/student/turtlebot3_robot_localization_ws/src/turtlebot3_simulations/turtlebot3_gazebo/models
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
~~~

if gazebo is not working properly, kill and relaunch
~~~
killall gazebo 
killall gzserver
killall gzclient
~~~

start robot_localization local ekf example
~~~
source turtlebot3_robot_localization_ws/install/setup.bash
ros2 launch robot_localization ekf.launch.py
~~~

start robot_localization ekf with gps example
~~~
source turtlebot3_robot_localization_ws/install/setup.bash
ros2 launch robot_localization dual_ekf_navsat_example.launch.py
~~~

You can control the virtual TurtleBot3 by using Teleoperation
~~~
source turtlebot3_robot_localization_ws/install/setup.bash
export TURTLEBOT3_MODEL=waffle
ros2 run turtlebot3_teleop teleop_keyboard
~~~

start joystick and play around
~~~
source xaver_ros2/xaver_ros2_ws/install/setup.bash
ros2 launch joystick joystick.launch.py
~~~

visualize in rviz
~~~
source turtlebot3_robot_localization_ws/install/setup.bash
rviz2 -d /home/student/turtlebot3_robot_localization_ws/src/turtlebot3_simulations/turtlebot3_gazebo/rviz/tb3_gazebo_robot_localization.rviz
~~~

