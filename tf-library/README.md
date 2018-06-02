# using tf-library for quaternion rotation

first download the offboard node from here and paste it your src folder

`launch mavros px4.launch fcu_url:=/dev/ttyACM0:115200` //baudrate is 11500 which tells the rate at which data is bing published

`dmesg`

`rosrun offboard offb_node`

Now start qGroundControl and do the calibration.
