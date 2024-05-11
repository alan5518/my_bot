# Hestia


## Commands used


### Hestia ssh

      ssh hestia@192.168.94.1

      ssh hestia@Hestia.local



### Robot dimensions

- Wheel separation 26
- Dia 9



### Firewall disable

      sudo ufw disable



### Simulation launch files ---->Seperate terminals

      ros2 launch my_bot launch_sim.launch.py

      ros2 launch my_bot launch_sim.launch.py world:=src/my_bot/worlds/obstacles.world

      ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped

      rviz2 -d src/my_bot/config/sim.rviz




### Robot launch files ---->Seperate terminals

      ros2 launch my_bot launch_robot.launch.py

      ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped

      rviz2 -d src/my_bot/config/sim.rviz




## for SLAM and Navigation in simulation use :
   
      ros2 launch my_bot launch_sim.launch.py world:=src/my_bot/worlds/obstacles.world

### instead of :

      ros2 launch my_bot launch_robot.launch.py
      
      ros2 launch rplidar_ros rplidar.launch.py


## SLAM (put sim_time:=true for simulation and false for robot)



### in the Rpi terminal ---->Seperate terminals

      ros2 launch my_bot launch_robot.launch.py


      ros2 launch rplidar_ros rplidar.launch.py


### in PC ---->Seperate terminals

      ros2 launch slam_toolbox online_async_launch.py slam_params_file:=./src/my_bot/config/mapper_params_online_async.yaml use_sim_time:=false

      rviz2 -d src/my_bot/config/sim.rviz

      ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped

- in rviz add map and set topic to map


### AMCL  ------>use instead of PC steps in SLAM

      ros2 run nav2_map_server map_server --ros-args -p yaml_filename:=my_map_save.yaml -p use_sim_time:=true

      ros2 run nav2_util lifecycle_bringup map_server

      ros2 run nav2_amcl amcl --ros-args -p use_sim_time:=true

      ros2 run nav2_util lifecycle_bringup amcl

      rviz2 -d src/my_bot/config/sim.rviz

      ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/diff_cont/cmd_vel_unstamped
      
 - in Rviz add map and set topic to map, use 2d pose estimate




## Nav2 launch files (put sim_time:=true for simulation and false for robot)


### in the Rpi terminal ---->Seperate terminals

      ros2 launch my_bot launch_robot.launch.py

      ros2 launch rplidar_ros rplidar.launch.py

      ros2 run twist_mux twist_mux --ros-args --params-file ./src/my_bot/config/twist_mux.yaml -r cmd_vel_out:=diff_cont/cmd_vel_unstamped


### in PC ---->Seperate terminals

      ros2 launch slam_toolbox online_async_launch.py slam_params_file:=./src/my_bot/config/mapper_params_online_async.yaml use_sim_time:=false

      rviz2 -d src/my_bot/config/sim.rviz

      ros2 launch nav2_bringup navigation_launch.py use_sim_time:=false

      ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/cmd_vel_tel

- in rviz add 2 maps and set topics to map and global costmap



### Lidar launch/run files ----> install rplidar package ----> I used 1.12

      ros2 launch rplidar_ros rplidar.launch.py
      
- ros2 launch rplidar_ros view_rplidar.launch.py  ---->to view
- ros2 run rplidar_ros rplidar_composition --ros-args -p serial_port:=/dev/ttyUSB0 -p frame_id:=laser -p angle_compensate:=true -p scan_mode:=Standard    ---->lidar node




### Encoder cmds  ---->for testing encoders and motors  -----> uplode Ros_arduino_bridge to the arduino https://github.com/joshnewans/ros_arduino_bridge/tree/main

      pyserial-miniterm -e /dev/ttyUSB0 57600

- install pyserial

- commands ---> e for encoder val, r to reset, o for open loop control, m for close loop control

- ros2 run serial_motor_demo driver --ros-args -p serial_port:=/dev/tty/ACM0 -p baud_rate:=57600 loop_rate:=30 encoder_cpr:=3450

- ros2 run serial_motor_demo driver \
  --ros-args -p serial_port:=/dev/tty/ACM0 \
  --ros-args -p baud_rate:=57600 \


- ros2 run serial_motor_demo gui
