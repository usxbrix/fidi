# fidi robot

a robot based on Lego 42065 Tracked Racer and the Beaglbone Blue

Using various nodes:


## Nodes overview

* differential driver node using the onboard motor ports
* publisher for IMU messages from MPU9250
* range sensor vl53l0x_driver

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

* ~left_motor = 1
* ~right_motor = 2
* ~timeout = 5
* ~maxspeed = 0.4
* ~minspeed = 0.1
* ~wheelbase = 0.2
* ~turnspeed = 1
* ~duty_factor = 1

### Run

`rosrun ros-blue diff_motor_driver _left_motor:=3 _right_motor:=4 _minspeed:=0.137 _maxspeed:=0.364 _duty_factor:=2.2 _wheelbase:=0.13`

Publish to cmd_vel manually

`rostopic pub -1 /cmd_vel geometry_msgs/Twist -- '[1.0, 0.0, 0.0]' '[0.0, 0.0, 0]'`


# Visualizing rviz

The Beaglebone Blue has no display port. So for visualization an aditional system is required.

`export ROS_MASTER_URI=http://beaglebone:11311`

Starting rviz after exporting the MASTER

rviz has issues with other localizations. (no model shown)
export LC_NUMERIC="en_US.UTF-8"

`rviz`

red - x green - y blue -z
