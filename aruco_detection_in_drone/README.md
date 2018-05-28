# Aruco detection in gazebo

### First launch the simulator in here

Clone this repo to downloads

[source to clone the repository](https://github.com/joselusl/aruco_gazebo.git)

Now copy the aruco_7(not the aruco7_pad folder) folder to `~src/Firmware/Tools/sitl_gazebo/Models`

Now replace the iris.world given in this repository file to `~src/Firmware/Tools/sitl_gazebo/Worlds`

Now run `roscore`

Now  `rosrun offboard offb_node`

goto `~src/Firmware`

`roslaunch mavros px4.launch fcu_url:="udp://:14540@192.168.1.36:14557"`

In other terminal execute this code

In other one `source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/posix_sitl_default`

`roslaunch gazebo_ros empty_world.launch world_name:=$(pwd)/Tools/sitl_gazebo/worlds/iris.world`


`no_sim=1 make posix_sitl_default gazebo`

### Time to detect Aruco now

The calibration data which we got during calibration the camera calibration in gazebo ; Now create a .yaml file and insert that data into it . Backup the head_camera.yaml file of the usb_camera (@ ~/.ros/camera_info) and replace the newly created file with the name head_camera.yaml.

goto `catkin_ws/src/aruco_ros/kinetic/devel/aruco_ros/launch/single.launch` and change line 13
in it with this code. 

<script>
  remap from="/image" to="/image_rect_color"
</script>
and save it.

`catkin_make` and `source devel/setup.sh`

> to increase the size of aruco
> goto `home/src/Firmware/Tools/sitl_gazebo/Models/aruco_visual_marker_7` and edit the model.sdf file make the size 0.5

 `roslaunch usb_cam usb_cam-test.launch`
 
 `rosrun image_proc image_proc image_raw:=/iris/camera_red_iris/image_raw camera_info:=/usb_cam/camera_info`
 

`roslaunch aruco_ros single.launch markerId:=7 markerSize:=0.5`

`rosrun rqt_image_view rqt_image_view` and select stream .. /aruco.single/result

Now you can get the pose data from the topic aruco.single/pose
