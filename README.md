# Gazebo Simulation
Aruco Visual Markers for Gazebo Simulator

Forked from https://github.com/joselusl/aruco_gazebo

## Installing Gazebo

Goto the Installing gazebo's README to install gazebo and other dependencies

## Installing offboard node

To run offboard example given on [px4 source](https://dev.px4.io/en/ros/mavros_offboard.html)

1. In a new terminal run `roscore` for starting master.

2. In a new terminal open `~/catkin_ws/src` where our ros environment is installed

   Let's create a new package named `offboard` in that src and run it.

   *create a new package with dependencies using `catkin_create_pkg offboard std_msgs rospy roscpp`
   
   *This creates a new CMakeLists.txt, src folder and package.xml
   
   *Edit the CmakeLists.txt add this snippet at last
    `add_executable(offb_node src/offb_node.cpp)`
    `target_link_libraries(offb_node ${catkin_LIBRARIES})`
   
   *Inside the src folder create offb_node.cpp and copy paste the code from [here](https://dev.px4.io/en/ros/mavros_offboard.html)
   
   *type the command in our workspace `catkin_make` to build our catkin package. 
   
   *`rosrun offboard offb_node` to run our offboard package.

3.  In new terminal goto `~/src/Firmware`

    Launch the mavros by command `roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14557"`

4.  In a new terminal goto `~/src/Firmware`
      
     Launch the gazebo simulator by command `make posix_sitl_default gazebo`
    
Now the offboard example is enabled u can see the quad takeoff in the simulator.    


## Installing iris camera

> To install a simple camera facing downwards at bottom of IRIS quadrotor model in Gazebo and publish its frames on ROS(Kinetic).

This tutorial assumes your have cloned Firmware folder in ~/src/Firmware and have successfully all its dependencies.

* To install a camera in IRIS proceed as follows:-

1. Open iris_base.xacro present in ~/src/Firmware/Tools/sitl_gazebo/models/rotors_description/urdf/
1. Search in iris_base.xacro and add the following code just above it and save it.

   Add these lines
   
   <xacro:camera_macro
        namespace="${namespace}"
        parent_link="base_link"
        camera_suffix="red_iris"
        frame_rate="30.0"
        horizontal_fov="1.3962634"
        image_width="800"
        image_height="800"
        image_format="R8G8B8"
        min_distance="0.02"
        max_distance="300"
        noise_mean="0.0"
        noise_stddev="0.007"
        enable_visual="1"
        >
        <box size="0.05 0.05 0.05" />
        <origin xyz="0 0 -0.07" rpy="0 1.57079 0"/>
        </xacro:camera_macro> 
   save it .
    
 * To Run the gazebo simulation follow these
     1. Go to ~src/Firmware    
       run the command `no_sim=1 make posix_sitl_default gazebo`
       as shown in [What's Happening Behind the Scenes](https://dev.px4.io/en/simulation/ros_interface.html) it will                initialize the gazebo
       
    1. To open our iris
       Go to Firmware again in new Terminal 
       run the command bash command `"source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/posix_sitl_default"`
       
       Then run the command `"roslaunch gazebo_ros empty_world.launch world_name:=$(pwd)/Tools/sitl_gazebo/worlds/iris.world"` in the same terminal
  
  Now we can see our iris with camera in gazebo simulator
  
  Open New terminal to see published raw images
  Type "rostopic list" to see the list of topics where we can see '/iris/camera_red_iris/image_raw'
  So our camera is working and publishing our images to ros
  
  You can also visualize it by typing `rosrun rqt_image_view rqt_image_view`
  
  Then see the image of our camera in iris quad in `/iris/camera_red_iris/image_raw`.


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

The calibration data which we got during calibrating the camera in gazebo ; Now create a .yaml file and insert that data into it . Backup the head_camera.yaml file of the usb_camera (@ ~/.ros/camera_info) and replace the newly created file with the name head_camera.yaml.

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

# Hovering over the aruco

clone this offboard node to the src of catkin folder (this is modified one)

Now catkin_make and source it

follow the [aruco detection in gazebo](https://github.com/AerialRobotics-IITK/gazebo_simulation/blob/master/README.md#aruco-detection-in-gazebo) again .

now modify the iris.world and change the size of aruco in the models
