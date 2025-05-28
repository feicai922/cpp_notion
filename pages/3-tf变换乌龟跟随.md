任务背景
tf是一个让用户随时间跟踪多个参考系的功能包，它使用一种树型数据结构，根据时间缓冲并维护多个参考系之间的坐标变换关系，可以帮助用户在任意时间，将点、向量等数据的坐标，在两个参考系中完成坐标变换。

通俗地讲，tf用于描述机器人相对于地图原点的位置、描述机器人上各个零件之间的相对位置关系。比如激光雷达产生的数据是基于激光雷达自身中心的，但是这组数据相对于机器人原点的位置关系可以通过tf计算得出。

本实验乌龟跟随，通过编写广播器发布乌龟的参考系，编写监听器计算乌龟参考系之间的差异，使第二只乌龟跟随第一只乌龟移动。之后启动tf辅助工具，理解一个点在坐标系之间的坐标变换。

任务需求：
一. 编写turtle_tf_broadcaster广播器发布乌龟的参考系和turtle_tf_listener监听器计算乌龟参考系之间的差异程序，实现乌龟跟随。使用tf分析工具view_frames，rviz，tf_echo等

任务分析：
通过键盘控制发布turtle1的速度消息，使turtle1运动，广播器接收位置消息并广播turtle1参考系与world参考系之间的tf数据。监听器获取turtle1与turtle2坐标系之间的tf数据，并根据tf数据计算出turtle2的速度消息使其运动。实现turtle2跟随turtle1。



任务步骤：
1)创建ROS工作空间与工程

2)编写turtle_tf_broadcaster广播器（发布乌龟的参考系程序）

3)编写turtle_tf_listener监听器（计算乌龟参考系之间的差异程序）

4) tf辅助工具

任务结果：


任务实施过程：
1. 使用tf驱动turtlesim多机器人
知识点

1) tf工具包，tf辅助工具

实验目的

1) 掌握tf工具包和tf辅助工具的使用

实验内容

1)通过使用tf坐标转换工具，实现乌龟跟随。

首先我们要做的是创建工作空间与ROS工程，然后在ROS工程中创建并编写turtle_tf_broadcaster.py，turtle_tf_listener.py，start_demo.launch程序。启动start_demo.launch成功后第二只乌龟跟随第一只乌龟移动，之后启动tf辅助工具观察每个坐标系之间的关系等。

2)执行tf辅助工具view_frames，rviz，tf_echo等。

实验环境

1) Ubuntu18.04

2) ROS-Melodic

实验步骤

一.创建ROS工程

1)打开命令行终端

1.双击桌面终端图标，名称为“终端模拟器”。



2)创建ROS工作空间

1.创建工作空间目录。

[Command 001]:

mkdir -p ~/catkin_ws/src

cd ~/catkin_ws/src



2.使用catkin_make命令编译空的工作空间。

[Command 002]:

source /opt/ros/melodic/setup.bash

cd ~/catkin_ws/

catkin_make



…



3)创建ROS Catkin程序包

1.创建ROS Catkin程序包。

[Command 003]:

cd ~/catkin_ws/src

catkin_create_pkg zxy_tf roscpp rospy tf turtlesim



2.拷贝geometry2源码包，因为Python3中使用tf需要安装，tf需要通过这两个源码包进行安装。

[Command 004]:

cp -r ~/resource/package/zxy_tf/geometry2 ./



4)编译环境

1.编译环境。编译添加工程代码后的工作空间。

[Command 005]:

cd ~/catkin_ws/

catkin_make --cmake-args -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/local/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.6m -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.6m.so 

…

 …



2.source程序环境，使程序环境生效。

[Command 006]:

source devel/setup.sh

echo $ROS_PACKAGE_PATH



二. 编写turtle_tf_broadcaster广播器

1)在ROS工程中创建脚本目录

1. 使用roscd命令切换到ROS工程目录，并创建nodes目录。

[Command 007]:

roscd zxy_tf

mkdir nodes



2)创建并编辑turtle_tf_broadcaster程序发布乌龟的参考系

1.使用rosed命令创建turtle_tf_broadcaster程序发布乌龟的参考系。为文件赋执行权限。使用rosed命令可以不用关注文件具体存储目录。

[Command 008]:

touch nodes/turtle_tf_broadcaster.py

chmod +x nodes/turtle_tf_broadcaster.py

rosed zxy_tf turtle_tf_broadcaster.py



2.执行rosed命令后按“a”键使文件进入编辑状态，粘贴如下代码。粘贴代码后按Esc键输入“:wq”保存退出文件。

[Code 001]：
```
#!/usr/bin/env python3

import sys
import roslib
import rospy
import tf
import turtlesim.msg

def handle_turtle_pose(msg, turtlename):
    # 创建tf的广播器
    br = tf.TransformBroadcaster()
    # 广播world与海龟坐标系之间的tf数据
    br.sendTransform((msg.x, msg.y, 0),
                     tf.transformations.quaternion_from_euler(0, 0, msg.theta),
                     rospy.Time.now(),
                     turtlename,
                     "world")

if __name__ == '__main__':
    # 初始化ROS节点
    rospy.init_node('turtle_tf_broadcaster')
    # 获取海龟的名字
    turtlename = rospy.get_param('~turtle')
    # 订阅海龟的位姿话题
    rospy.Subscriber('/%s/pose' % turtlename,
                     turtlesim.msg.Pose,
                     handle_turtle_pose,
                     turtlename)
    # 循环等待回调函数
    rospy.spin()
```
三.编写turtle_tf_listener监听器

1)创建turtle_tf_listener程序

1.使用rosed命令创建turtle_tf_listener程序。为文件赋执行权限。

[Command 009]:

roscd zxy_tf

touch nodes/turtle_tf_listener.py

chmod +x nodes/turtle_tf_listener.py

rosed zxy_tf turtle_tf_listener.py



2.执行rosed命令后按“a”键使文件进入编辑状态，粘贴如下代码。粘贴代码后按Esc键输入“:wq”保存退出文件。

[Code 002]：
```python
#!/usr/bin/env python3

import sys
import rospy
import math
import tf
import geometry_msgs.msg
import turtlesim.srv

if __name__ == '__main__':
    # 初始化ROS节点
    rospy.init_node('tf_turtle')

    # 创建tf的监听器
    listener = tf.TransformListener()

    # 请求产生turtle2
    rospy.wait_for_service('spawn')
    spawner = rospy.ServiceProxy('spawn', turtlesim.srv.Spawn)
    spawner(4, 2, 0, 'turtle2')

    # 创建发布turtle2速度控制指令的发布者
    turtle_vel = rospy.Publisher('turtle2/cmd_vel', geometry_msgs.msg.Twist, queue_size=1)

    rate = rospy.Rate(10.0)  # 10 Hz

    while not rospy.is_shutdown():
        # 获取turtle1与turtle2坐标系之间的tf数据
        try:
            (trans, rot) = listener.lookupTransform('/turtle2', '/turtle1', rospy.Time(0))
        except (tf.LookupException, tf.ConnectivityException, tf.ExtrapolationException):
            continue

        # 根据turtle1与turtle2坐标系之间的位置关系，发布turtle2的速度控制指令
        angular = 4 * math.atan2(trans[1], trans[0])
        linear = 0.5 * math.sqrt(trans[0] ** 2 + trans[1] ** 2)

        cmd = geometry_msgs.msg.Twist()
        cmd.linear.x = linear
        cmd.angular.z = angular

        turtle_vel.publish(cmd)

        rate.sleep()
```

2)编写start_demo.launch启动文件

1.在功能包下创建launch文件夹，创建 start_demo.launch。为文件赋执行权限。

[Command 010]:

roscd zxy_tf

mkdir launch

touch launch/start_demo.launch

rosed zxy_tf start_demo.launch



2.执行rosed命令后按“a”键使文件进入编辑状态，粘贴如下代码。粘贴代码后按Esc键输入“:wq”保存退出文件。

[Code 003]：
```
<launch>
    <!-- 海龟仿真器 -->
    <node pkg="turtlesim" type="turtlesim_node" name="sim" />

    <!-- 键盘控制 -->
    <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen" />

    <!-- 两只海龟的tf广播 -->
    <node name="turtle1_tf_broadcaster" pkg="zxy_tf" type="turtle_tf_broadcaster.py" respawn="false" output="screen">
        <param name="turtle" type="string" value="turtle1" />
    </node>

    <node name="turtle2_tf_broadcaster" pkg="zxy_tf" type="turtle_tf_broadcaster.py" respawn="false" output="screen">
        <param name="turtle" type="string" value="turtle2" />
    </node>

    <!-- 监听tf广播，并且控制turtle2移动 -->
    <node pkg="zxy_tf" type="turtle_tf_listener.py" name="listener" />
</launch>
```
3) 执行start_demo.launch文件，启动程序。

1. 执行start_demo.launch文件。执行成功后会出现两只小海龟，第二只跟随第一只移动

[Command 011]:

roslaunch zxy_tf start_demo.launch



四.使用tf辅助工具

1) view_frames

1. 是一个可视化完整的坐标树工具。首先启动小海龟程序，然后打开一个新终端执行下面的命令，会在/catkin_ws目录下生成一个ros系统中坐标系树的pdf文件，如下图：

[Command 012]:

source /opt/ros/melodic/setup.bash

rosrun tf view_frames



2.打开frames.pdf文件，该文件描述了参考系之间的联系。三个节点分别是三个参考系，而/world是其他两个乌龟参考系的父参考系。还包含一些调试需要的发送频率、最近时间等信息。



2) tf_echo

1.tf_echo命令用来查看两个广播参考系之间的关系。我们可以看一下第二只得乌龟坐标是怎么根据第一只乌龟得出来的。

[Command 013]:

rosrun tf tf_echo turtle1 turtle2



3) rviz和tf

1.通过rviz软件来可视化显示。移动小乌龟，观察坐标系的变化。

[Command 014]:

rosrun rviz rviz -d `rospack find turtle_tf`/rviz/turtle_rviz.rviz



….



2.通过左下方添加按钮，添加一个tf。网格中间的点就是world，左上角就是turtle1和turtle2

