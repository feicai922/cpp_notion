rqt_graph：显示通信架构
rqt_console：无需配置，自动获取日志
rqt_plot：绘制指定话题数据变化曲线

任务背景
ROS为机器人开发提供各种GUI工具。例如，有一个将每个节点的层次结构显示为图形，且显示当前节点和话题状态的graph；将消息显示为二维图形的plot，等。从ROS Fuerte版本开始，这些GUI开发工具被称为rqt6，它集成了30多种工具，可以作为一个综合的GUI工具来使用。另外，RViz也被集成到rqt的插件中，这使rqt成为ROS的一个不可缺少的GUI工具。

rqt是一个基于Qt的ROS GUI开发框架，以插件的形式实现各种GUI工具。 可以将所有现有的GUI工具作为rqt中的可停靠窗口运行！ 这些工具仍然可以在传统的独立方法中运行，但rqt使得用户更容易在屏幕上管理所有的窗口。 在命令行中运行’rqt’即可开启。用户也可以自己用python或是c++开发自己的插件。

本实验在ROS工程中启动小海龟键盘控制程序移动小海龟，并使用rqt常用工具来显示通信架构、绘制话题上的数据变化曲线、调整ros节点的记录器级别、显示节点的输出信息。

任务需求：
使用rqt工具来显示通信架构、绘制话题上的数据变化曲线、调整ros节点的记录器级别、显示节点的输出信息。

任务分析：
首先创建ROS工作空间与程序包、编写小海龟键盘控制启动程序。启动程序。使用rqt_graph插件显示通信架构，使用rqt_plot插件绘制话题上的数据变化曲线，使用rqt_console插件显示节点的输出信息，使用rqt_logger_level调整ros节点的记录器级别。

任务步骤：
1)基础程序准备

2)rqt_graph插件

3)rqt_plot插件

4)rqt_console插件和rqt_logger_level插件

任务结果：
本实验通过小海龟程序为例，学习如何使用rqt常用插件。

任务实施过程：
1. 使用rqt常用插件
知识点

1) rqt常用插件

实验目的

1)掌握rqt常用插件的使用

实验内容

1)通过小海龟程序为例，学习如何使用rqt常用插件。

首先我们要做的是创建工作空间与ROS工程并安装turtlesim，然后在ROS工程中创建小海龟launch启动文件，最后在分别启动rqt插件观察效果。

2)查看程序运行效果，我们得到结果如下图所示。







实验环境

1) Ubuntu18.04

2) ROS-Melodic

实验步骤

一.基础程序准备

1)打开命令行终端

1.双击桌面终端图标，名称为“终端模拟器”。



2)安装turtlesim包（这里以安装好）

1.打开终端执行下面命令

[Command 001]:

sudo apt-get install ros-melodic-turtlesim

3)创建ROS工作空间

1.创建工作空间目录。

[Command 002]:

mkdir -p ~/catkin_ws/src

cd ~/catkin_ws/src



2.使用catkin_make命令编译空的工作空间。

[Command 003]:

source /opt/ros/melodic/setup.bash

cd ~/catkin_ws/

catkin_make



…



4)创建ROS Catkin程序包

1.创建ROS Catkin程序包。

[Command 004]:

cd ~/catkin_ws/src

catkin_create_pkg rqt_pkg std_msgs rospy roscpp tf turtlesim



5)编译环境

1.编译环境。编译添加工程代码后的工作空间。

[Command 005]:

cd ~/catkin_ws/

catkin_make



…



2.source程序环境，使程序环境生效。

[Command 006]:

source devel/setup.sh

echo $ROS_PACKAGE_PATH



二.rqt_graph插件

1)启动小乌龟程序

1.编写launch文件,首先进入rqt_pkg包内，创建launch文件夹，在创建start_demo.launch文件，并将下面代码粘贴到文件中。launch文件实现多节点的配置和启动，  launch文件中的根元素采用<launch>标签定义。node为节点标签，pkg属性是节点所在功能包的名称，type属性是节点的可执行文件名称，name属性是节点运行时名称。

[Command 007]:

roscd rqt_pkg

mkdir launch

touch launch/start_demo.launch

rosed rqt_pkg start_demo.launch



2.执行rosed命令后按“a”键使文件进入编辑状态，粘贴如下代码。粘贴代码后按Esc键输入“:wq”保存退出文件。

[Code 001]：
```
<launch>

<node pkg=“turtlesim” type=“turtlesim_node” name=“sim”/>

<node pkg=“turtlesim” type=“turtle_teleop_key” name=“teleop” output=“screen”/>

</launch>
```
3.执行start_demo.launch文件启动小乌龟程序，弹出小乌龟。这时以启动一个乌龟节点sim和键盘控制节点teleop，启动成功后，将光标放置到终端上，按上下左右键，可以移动小乌龟。

[Command 008]:

roslaunch rqt_pkg start_demo.launch



2)启动rqt_graph

1.rqt_graph插件是用来显示通信架构的。ROS系统中运行的节点、主题，消息流向是怎样的，这些都可以通过rqt_graph显示出来。如下图椭圆代表节点，箭头上面是话题名称。打开新终端执行下面命令。

[Command 009]:

source ~/catkin_ws/devel/setup.sh

rosrun rqt_graph rqt_graph





三.rqt_plot插件

1)启动rqt_plot插件

1.打开新终端窗口，设置ROS相关环境。

[Command 010]:

cd ~/catkin_ws/

source /opt/ros/melodic/setup.bash

source devel/setup.sh



2.rqt_plot插件,可以画出发布在topic上的数据变化图。如图所示，可以看到在显示龟的x位置(/turtle1/pose/x)、y位置(/turtle1/pose/y)、theta方向(/turtle1/pose/theta)、平移速度(/turtle1/pose/linear_velocity)和旋转速度（/turtle1/pose/angular_velocity）的变化曲线。这里展示了turtlesim功能包，rqt_plot也可以用于表达用户开发节点的数据。特别适合于随着时间的推移显示传感器值，例如速度和加速度。

[Command 011]:

rosrun rqt_plot rqt_plot



四. rqt_console插件和rqt_logger_level插件

1) 启动rqt_console插件

1.打开新终端窗口，设置ROS相关环境。

[Command 012]:

cd ~/catkin_ws/

source /opt/ros/melodic/setup.bash

source devel/setup.sh



2.启动rqt_console插件，rqt_console属于ROS日志框架(logging framework)的一部分，用来显示节点的输出信息。在打开的窗口中，我们可以看到三个子窗口。最上边一个窗口会显示所有ROS系统中的日志，是内容最多的。

[Command 013]:

rosrun rqt_console rqt_console



2) 启动rqt_logger_level插件

1.打开新终端窗口，设置ROS相关环境。

[Command 014]:

cd ~/catkin_ws/

source /opt/ros/melodic/setup.bash

source devel/setup.sh



2.启动rqt_logger_level, rqt_logger_level允许我们修改节点运行时输出信息的日志等级（logger levels）, levels包括 DEBUG、INFO、WARN、ERROR和FATAL。启动之后弹出的窗口如下图，分为三部分节点，日志，级别。将sim节点levels设置为info,之后点击Refresh

[Command 015]:

rosrun rqt_logger_level rqt_logger_level



3.将光标放到乌龟的终端上，移动小乌龟到边缘。这时rqt_console中会出现警告

s