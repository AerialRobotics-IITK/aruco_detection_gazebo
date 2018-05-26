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
