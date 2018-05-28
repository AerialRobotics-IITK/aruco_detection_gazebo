# Installing gazebo

Downlaod the ubuntu_sim_ros_gazebo.sh and then source it

`source ubuntu_sim_ros_gazebo.sh`

Now goto the src folder of catkin wrkspace and then delete the mavros and mavlink folder because it will throw error if tried to catkin_makke while they are in the folder

now type these codes in the terminal

`sudo apt-get update`

`sudo apt-get upgrade`  

update it so that it does not throws out an error

`sudo apt-get install ros-kinetic-mavros`

`sudo apt-get install ros-kinetic-mavlink`

`sudo apt-get install ros-kinetic-mavros-extras`

`sudo apt-get install ros-kinetic-cv-bridge`

`sudo apt-get install ros-kinetic-image-transport`

`sudo apt-get install ros-kinetic-gazebo-ros`

Now goto `cd ~/src`

`git clone https://github.com/PX4/Firmware.git`

`cd Firmware`

`make posix jmavsim` //not necessary to install its an other type of simulator

`make posix_sitl_default gazebo`

> If error comes then execute the following code

`sudo pip install numpy toml`




