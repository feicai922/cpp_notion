## gmapping
```
roslaunch mbot_gazebo mbot_laser_nav_gazebo.launch
roslaunch mbot_navigation gmapping_demo.launch
roslaunch mbot_teleop mbot_teleop.launch
rosrun map_server map_saver -f cloister_gmapping
```
## hector
复制tool文件夹中hector功能包文件到/opt/ros中的指定文件夹
chmod +x给hector_mapping可执行文件授权
```
roslaunch mbot_gazebo mbot_laser_nav_gazebo.launch
roslaunch mbot_navigation hector_demo.launch
roslaunch mbot_teleop mbot_teleop.launch
rosrun map_server map_saver -f cloister_gmapping
```