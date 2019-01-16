# ROS introduction

1. *ROS ==  Robot Operating System*

2. ROS includes 

   1. 传感器程序，电机驱动

   2. 基本机器人算法（地图，识别）

   3. 分布式可视化工具


###Installation

1. 建议版本ubuntu 14.04LTS + ROS indigo<img src='./images/Screen Shot 2019-01-15 at 5.48.06 pm.jpg' >

2. ```bash
   sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'
   wget http://packages.ros.org/ros.key -O -| sudo apt-key add - ##输出名字为 - ， 并添加 - 为apt-key
   sudo apt-get update
   sudo apt-get dpkg upgrade
   sudo apt-get install ros-indigo-desktop-full python-rosinstall ##安装全套系统工具
   sudo rosdep init
   rosdep update 
   echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   sudo apt-get install synaptic ##安装可视化安装工具
   ```

### Roscore

roscore是为ros操作系统中各个独立运行的服务进程提供通讯的内核服务机制。每一个独立运行的进程在启动时都需要到roscore 处发布，注册必要的信息，以便其他几点发现该进程。