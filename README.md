# fidi robot

a robot based on Lego 42065 Tracked Racer and the Beaglbone Blue

Using various nodes:


## Nodes overview

* differential driver node using the onboard motor ports
* publisher for IMU messages from MPU9250
* range sensor vl53l0x_driver
* for basic navigation [move_basic](https://github.com/UbiquityRobotics/move_basic) package by [UbiquityRobotics](https://github.com/UbiquityRobotics)

# launch

`roslaunch fidi_robot fidi.launch`

* loads model urdf/model.urdf
* creates static transform base_link -> imu_link
* launches vl53l0x_driver
* launches IMU node


## Differential Motor Driver

Max speed(linear velocity): 0,364 m/s @ duty cycle 0,8 (to not overload the motor controller with the Lego PF L motors)

Max angular velocity based on max speed: 5,6 rad/s @ 0,364 m/s and wheelbase 0,13

### Parameter

* ~left_motor = 3
* ~right_motor = 4
* ~timeout = 5
* ~maxspeed = 0.364
* ~minspeed = 0.137
* ~wheelbase = 0.13
* ~turnspeed = 1
* ~duty_factor = 1

### Run

`rosrun bbblue_drivers diff_motor_driver _left_motor:=3 _right_motor:=4 _minspeed:=0.137 _maxspeed:=0.364 _duty_factor:=2.2 _wheelbase:=0.17`

Publish to cmd_vel manually

`rostopic pub -1 /cmd_vel geometry_msgs/Twist -- '[1.0, 0.0, 0.0]' '[0.0, 0.0, 0]'`

## Basic Navigation (move_basic)

### Run

`rosrun move_basic move_basic _min_angular_velocity:=2.8 _max_angular_velocity:=10`

Publish simple navigation target

180 degree turn
```
rostopic pub -1 /move_base_simple/goal geometry_msgs/PoseStamped "header:
  seq: 1
  stamp:
    secs: 0
    nsecs: 0
  frame_id: 'base_link'
pose:
  position:
    x: 0.0
    y: 0
    z: 0.0
  orientation:
    x: 0.0
    y: 0.0
    z: 1
    w: 0.0 "


```

# Joystic control


https://kofler.info/bluetooth-konfiguration-im-terminal-mit-bluetoothctl/
bluetoothctl
scan on
pair 
trust
connect


`sudo apt install ros-melodic-joy ros-melodic-teleop-twist-joy`

http://wiki.ros.org/joy/Tutorials/ConfiguringALinuxJoystick
`rosparam set joy_node/dev "/dev/input/js0"`

`rosrun joy joy_node`

http://wiki.ros.org/teleop_twist_joy
nintendo joy-con (L)
`rosrun teleop_twist_joy teleop_node _axis_linear:=4 _axis_angular:=5 _scale_angular:=3 _enable_button:=15 `

# Visualizing rviz

The Beaglebone Blue has no display port. So for visualization an aditional system is required.

`export ROS_MASTER_URI=http://beaglebone:11311`

Starting rviz after exporting the MASTER

rviz has issues with other localizations. (no model shown)
export LC_NUMERIC="en_US.UTF-8"

`rviz`

red - x green - y blue -z
