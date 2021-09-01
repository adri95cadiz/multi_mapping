# multi_mapping

ROS package for multiple heterogeneous platforms mapping and navigating in real time. Uses Hector Quadrotor drones and Turtlebot3 Burger robots collaborating with Hector-SLAM and SLAM Gmapping to construct a map together and navigate the environment.

## Installation 

**Depends on the multi_robot and multi_drone packages from my repositories you can find here:**
https://github.com/adri95cadiz/multi_robot
https://github.com/adri95cadiz/multi_drone

**Install them and all of their dependencies and execute:**
```
$ catkin_make
```

## Usage

**Launch multiple mappers**
```
$ roslaunch multi_mapping main.launch
```

**Launch slam gmapping for the robot**
```
$ ROS_NAMESPACE=mapper1 roslaunch turtlebot3_slam turtlebot3_gmapping.launch set_base_frame:=mapper1/base_footprint set_odom_frame:=mapper1/odom set_map_frame:=mapper1/map
```

**Launch hector gmapping for the drone**
```
$ ROS_NAMESPACE=mapper2 roslaunch multi_drone gmapping.launch drone_name:=mapper2
```

**Merge map data**
```
$ roslaunch multi_mapping multi_map_merge.launch
```

**Launch rviz**
```
$ rosrun rviz rviz -d `rospack find multi_drone`/rviz/multi_mapping_slam.rviz
```

**Start drone**
```
$ rosservice call mapper2/enable_motors true
```

**Send Takeoff message to drone**
```
$ rostopic pub -r 0.5 mapper2/cmd_vel geometry_msgs/Twist  '{linear:  {x: 0, y: 0.0, z: 0.1}, angular: {x: 0.0,y: 0.0,z: 0.0}}'
```

**Launch navigation system**
```
$ roslaunch multi_mapping navigation.launch
```

**Run rviz navigation**
```
$ rosrun rviz rviz -d `rospack find multi_mapping`/rviz/multi_mapping_navigation.rviz
```

**Launch multi_map_merge again to update map**
```
$ roslaunch multi_mapping multi_map_merge.launch
```

**Save map**
```
$ rosrun map_server map_saver -f ~/map
```
