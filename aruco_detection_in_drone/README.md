# Aruco detection in gazebo

Clone this repo to downloads

[source to clone](https://github.com/joselusl/aruco_gazebo.git)

Now copy the aruco_7(not the aruco7_pad folder) folder to `~src/Firmware/Tools/sitl_gazebo/Models`

Now replace the iris.world given in this repository file to `~src/Firmware/Tools/sitl_gazebo/Worlds`

Now run `roscore`

goto `~src/Firmware`

In other terminal execute this code

`no_sim=1 make posix_sitl_default gazebo`

In other one `source Tools/setup_gazebo.bash $(pwd) $(pwd)/build/posix_sitl_default`

`roslaunch gazebo_ros empty_world.launch world_name:=$(pwd)/Tools/sitl_gazebo/worlds/iris.world`

`roslaunch mavros px4.launch fcu_url:="udp://:14540@192.168.1.36:14557"`
