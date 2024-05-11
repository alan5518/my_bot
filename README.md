## Robot Package Template

Commands used


Hestia ssh

AJC ----- ssh hestia@192.168.94.1
               ssh hestia@Hestia.local


Robot dimensions

Wheel separation 26
Dia 9


Firewall disable

sudo ufw disable



simulation launch files 

ros2 launch my_bot launch_sim.launch.py

ros2 launch my_bot launch_sim.launch.py world:=src/my_bot/worlds/obstacles.world

ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped

rviz2 -d src/my_bot/config/sim.rviz




robot launch files

ros2 launch my_bot launch_robot.launch.py

ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped

rviz2 -d src/my_bot/config/sim.rviz




SLAM (put sim_time:=true for simulation and false for robot)


* in the Rpi terminal

ros2 launch my_bot launch_robot.launch.py

ros2 launch rplidar_ros rplidar.launch.py

* in PC

ros2 launch slam_toolbox online_async_launch.py slam_params_file:=./src/my_bot/config/mapper_params_online_async.yaml use_sim_time:=false  

rviz2 -d src/my_bot/config/sim.rviz  
 ------> add map and set topic to map

ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped


AMCL  ------>use instead of PC steps in SLAM

ros2 run nav2_map_server map_server --ros-args -p yaml_filename:=my_map_save.yaml -p use_sim_time:=true

ros2 run nav2_util lifecycle_bringup map_server

ros2 run nav2_amcl amcl --ros-args -p use_sim_time:=true

ros2 run nav2_util lifecycle_bringup amcl




Nav2 launch files (put sim_time:=true for simulation and false for robot)

* in the Rpi terminal

ros2 launch my_bot launch_robot.launch.py

ros2 launch rplidar_ros rplidar.launch.py

ros2 run twist_mux twist_mux --ros-args --params-file ./src/my_bot/config/twist_mux.yaml -r cmd_vel_out:=diff_cont/cmd_vel_unstamped

* in PC

ros2 launch slam_toolbox online_async_launch.py slam_params_file:=./src/my_bot/config/mapper_params_online_async.yaml use_sim_time:=false  

rviz2 -d src/my_bot/config/sim.rviz   
------> add 2 maps and set topics to map and global costmap

ros2 launch nav2_bringup navigation_launch.py use_sim_time:=false

ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/cmd_vel_tel



Lidar launch/run files

// ros2 launch sllidar_ros2 view_sllidar_a1_launch.py
ros2 launch rplidar_ros rplidar.launch.py   ---->main
ros2 launch rplidar_ros view_rplidar.launch.py  ---->to view
ros2 run rplidar_ros rplidar_composition --ros-args -p serial_port:=/dev/ttyUSB0 -p frame_id:=laser -p angle_compensate:=true -p scan_mode:=Standard    ---->lidar node




Encoder cmds  ---->for testing encoders and motors

pyserial-miniterm -e /dev/ttyUSB0 57600

ros2 run serial_motor_demo driver --ros-args -p serial_port:=/dev/tty/ACM0 -p baud_rate:=57600 loop_rate:=30 encoder_cpr:=3450

ros2 run serial_motor_demo driver \
  --ros-args -p serial_port:=/dev/tty/ACM0 \
  --ros-args -p baud_rate:=57600 \


ros2 run serial_motor_demo gui
