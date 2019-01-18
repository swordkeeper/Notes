# ROS introduction

1. *ROS ==  Robot Operating System*

2. ROS includes 

   1. 传感器程序，电机驱动

   2. 基本机器人算法（地图，识别）

   3. 分布式可视化工具


### Installation

1. 建议版本ubuntu 14.04LTS + ROS indigo<img src='./images/Screen Shot 2019-01-15 at 5.48.06 pm.jpg' >

2. ```bash
   sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'
   wget http://packages.ros.org/ros.key -O -| sudo apt-key add - ##输出名字为 - ， 并添加 - 为apt-key
   sudo apt-get update
   sudo apt-get upgrade dpkg
   sudo apt-get install ros-indigo-desktop-full python-rosinstall ##安装全套系统工具
   sudo rosdep init
   rosdep update 
   echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   sudo apt-get install synaptic ##安装可视化安装工具
   ```

### Roscore

roscore是为ros操作系统中各个独立运行的服务进程提供通讯的内核服务机制。每一个独立运行的进程在启动时都需要到roscore 处发布，注册必要的信息，以便其他几点发现该进程。

Roscore是一个帮助其他节点互相寻找的程序

roscore使用环境变量```ROS_MASTER_URI```对应的uri为```http://hostname:11311/```

Rosparam 命令用于与参数服务器交互

### Catkin

carkin是ros的构架系统：用于生成可执行程序、库、脚本和其他代码可用的接口。

#### 工作区

```bash
# 创建工作区
mkdir -p ~/catkin_ws/src #建立了一个名为catkin_ws的工作区
cd ~/catkin_ws/src
catkin_init_workspace #调用该catkin命令，就在src下连理了CMakeList.txt文件
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

#### 包

工作区中存在包，每个包必须包含```CMakeList.txt```和```package.xml```文件

```bash
#创建包
cd ~/catkin_ws/src
catkin_create_pkg my_awesome_code rospy #创建一个名为my_awesome_code包 并用rospy包解析
```

#### 包程序运行

```bash
rosrun PACKAGE EXECUTABLE [ARGS]  ##启动一个节点
roslaunch PACKAGE LAUNCH_FILE  ##启动包下的launch_file文件，该文件规定了一组节点以及他们的重映射
```

