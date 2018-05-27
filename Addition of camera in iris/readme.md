To install a simple camera facing downwards
at bottom of IRIS quadrotor model in Gazebo and publish its frames on ROS(Kinetic).

This tutorial assumes your have cloned Firmware folder in ~/src/Firmware and
have successfully all its dependencies.

I) To install a camera in IRIS proceed as follows:-

1) Open iris_base.xacro present in ~/src/Firmware/Tools/sitl_gazebo/models/rotors_description/urdf/
2) Search in iris_base.xacro and add the
following code just above it and save it.

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
    
  II) To Run the gazebo simulation follow these
     1)Go to ~src/Firmware    
       `run the command "no_sim=1 make posix_sitl_default gazebo"`
       as shown in [What's Happening Behind the Scenes](https://dev.px4.io/en/simulation/ros_interface.html) 
       it will initialize the gazebo
    2)To open our iris
       Go to Firmware again in new Terminal 
       run the command bash command `"source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/posix_sitl_default"`
       
       Then run the command `"roslaunch gazebo_ros empty_world.launch world_name:=$(pwd)/Tools/sitl_gazebo/worlds/iris.world"` in the same terminal
  
  Now we can see our iris with camera in gazebo simulator
  
  Open New terminal to see published raw images
  Type "rostopic list" to see the list of topics where we can see '/iris/camera_red_iris/image_raw'
  So our camera is working and publishing our images to ros
  
  You can also visualize it by typing 'rosrun rqt_image_view rqt_image_view'
  Then see the image of our camera in iris quad in '/iris/camera_red_iris/image_raw'.


