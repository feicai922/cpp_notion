基于ROS的URDF进行试验用机器人模型建立

实验背景

URDF格式是ROS用来描述机器人的符合XML语言规范的描述文件。在ROS仿真里面应用广泛，用于方便灵活地创建机器人的仿真模型。

本实验为基于URDF创建小车模型。本实验通过URDF描述文件搭建一个机器人小车模型，包括车身和车轮。并通过Rviz将URDF搭建好的模型展示出来。

实验描述：

编写URDF模型描述文件，创建小车模型（包括车身与四个车轮）。并通过Rviz对URDF模型进行展示。

实验环境

1. Ubuntu18.04

2. ROS-Melodic

实验目的

1. 掌握基于ROS的URDF模型建立，掌握基于Rviz的模型展示。实现机器人模型的配置与展示功能

知识点

1. URDF模型文件配置

2. Rviz模型展示

实验分析：



任务实施过程：

一. 创建ROS工作空间与工程

1) 在镜像环境创建ROS工作空间

1.在镜像操作终端创建ros工作空间目录。

[Command 001]:

mkdir -p ~/catkin_ws/src

cd ~/catkin_ws



2.编译工作空间。

[Command 002]:

catkin_make



…



3.查看编译效果。编译成功后生成build、devel文件夹。build包含缓存信息和中间文件。devel包含生成的目标文件，环境变量。

[Command 003]:

ll ~/catkin_ws



2)创建ROS工程包

1.创建一个名为urdf_pkg的ros工程包。

[Command 004]:

cd ~/catkin_ws/src

catkin_create_pkg urdf_pkg urdf rospy



2.查看工程包内容。程序包创建成功，会生成CMakeLists.txt文件、package.xml文件、src目录。CMakeLists.txt中定义了包名、依赖、源文件、目标文件等编译规则。package.xml中包含描述package的包名、版本号、作者、依赖等信息。src 是存放ROS的源代码目录。

[Command 005]:

ll ~/catkin_ws/src/urdf_pkg



二. 编写URDF机器人描述文件

1)编写URDF机器人描述文件

1.在镜像操作终端创建urdf文件目录，launch启动文件存放目录，scripts脚本运行目录。

[Command 006]:

cd ~/catkin_ws/src/urdf_pkg

mkdir urdf launch scripts

ll



2.创建URDF描述文件。

[Command 007]:

touch urdf/small_car.urdf

ll urdf



3.编辑URDF描述文件，创建根节点。使用vi命令编辑文件，按“i”键进入编辑状态。输入代码，按“Esc”键输入“:wq”保存退出文件。

[Command 008]:

vi urdf/small_car.urdf

[Code 001]:

```
<?xml version=“1.0”?>

	<robot name=“smallcar”>

</robot>
```


4.在URDF描述文件中添加小车车身配置，用link节点标记。内容包括形状（方形），尺寸（长宽高单位cm），初始角度（单位π），颜色（颜色数值/255）。

[Code 002]:

```
<link name="base_link">
    <visual>
        <geometry>
            <box size="0.25 0.16 0.05"/>
        </geometry>
        <origin rpy="0 0 0" xyz="0 0 0"/>
        <material name="blue">
            <color rgba="0 0 0.8 1"/>
        </material>
    </visual>
</link>

```

5.在URDF描述文件中添加四个车轮，用link节点标记。包括形状（圆柱），尺寸（高度和半径），颜色。

[Code 003]:

```
<link name="right_front_wheel">
    <visual>
        <geometry>
            <cylinder length="0.02" radius="0.025"/>
        </geometry>
        <material name="black">
            <color rgba="0 0 0 1"/>
        </material>
    </visual>
</link>

<link name="right_back_wheel">
    <visual>
        <geometry>
            <cylinder length="0.02" radius="0.025"/>
        </geometry>
        <material name="black">
            <color rgba="0 0 0 1"/>
        </material>
    </visual>
</link>

<link name="left_front_wheel">
    <visual>
        <geometry>
            <cylinder length="0.02" radius="0.025"/>
        </geometry>
        <material name="black">
            <color rgba="0 0 0 1"/>
        </material>
    </visual>
</link>

<link name="left_back_wheel">
    <visual>
        <geometry>
            <cylinder length="0.02" radius="0.025"/>
        </geometry>
        <material name="black">
            <color rgba="0 0 0 1"/>
        </material>
    </visual>
</link>
```


6.在URDF描述文件中添加车轮与车身的连接设置，用joint节点标记。包括连接性质（continuous连续型连接关节，可以绕一个轴旋转），旋转轴设置（沿z轴旋转），连接物体parent车体、child轮子、方向、位置，以及力、速度、摩擦等属性（基础操作使用默认参数即可）。

[Code 004]:

```
<joint name="right_front_wheel_joint" type="continuous">
    <axis xyz="0 0 1"/>
    <parent link="base_link"/>
    <child link="right_front_wheel"/>
    <origin rpy="1.57075 0 0" xyz="0.1 0.08 -0.03"/>
    <limit effort="100" velocity="100"/>
    <joint_properties damping="0.0" friction="0.0"/>
</joint>

<joint name="right_back_wheel_joint" type="continuous">
    <axis xyz="0 0 1"/>
    <parent link="base_link"/>
    <child link="right_back_wheel"/>
    <origin rpy="1.57075 0 0" xyz="-0.1 0.08 -0.03"/>
    <limit effort="100" velocity="100"/>
    <joint_properties damping="0.0" friction="0.0"/>
</joint>

<joint name="left_front_wheel_joint" type="continuous">
    <axis xyz="0 0 1"/>
    <parent link="base_link"/>
    <child link="left_front_wheel"/>
    <origin rpy="1.57075 0 0" xyz="0.1 -0.08 -0.03"/>
    <limit effort="100" velocity="100"/>
    <joint_properties damping="0.0" friction="0.0"/>
</joint>

<joint name="left_back_wheel_joint" type="continuous">
    <axis xyz="0 0 1"/>
    <parent link="base_link"/>
    <child link="left_back_wheel"/>
    <origin rpy="1.57075 0 0" xyz="-0.1 -0.08 -0.03"/>
    <limit effort="100" velocity="100"/>
    <joint_properties damping="0.0" friction="0.0"/>
</joint>

```

7.保存文件，按“Esc”键输入“:wq”保存并退出文件。

三. URDF机器人模型展示

1)URDF机器人模型展示

1.创建并编写launch启动文件，配置Rviz展示的URDF文件。配置完成后按“Esc”输入“:wq”保存并退出文件。

[Command 009]:

vi launch/car_urdf.launch

[Code 005]:

```
<launch>
    <!-- 定义参数 -->
    <arg name="model" />
    <arg name="gui" default="False" />

    <!-- 设置机器人描述参数 -->
    <param name="robot_description" textfile="$(find urdf_pkg)/urdf/small_car.urdf" />
    <param name="use_gui" value="$(arg gui)" />

    <!-- 启动 joint_state_publisher 节点 -->
    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" />

    <!-- 启动 robot_state_publisher 节点 -->
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="state_publisher" />

    <!-- 启动 rviz 节点 -->
    <node name="rviz" pkg="rviz" type="rviz" args="-d $(find urdf_tutorial)/urdf.vcg" />
</launch>
```

2.执行launch启动文件。

[Command 010]:

source ~/catkin_ws/devel/setup.sh

roslaunch urdf_pkg car_urdf.launch gui:=true



3.在Rviz中配置显示URDF小车模型。Global Options下Fixed Frame选择base_link。



4.在Rviz中配置显示URDF小车模型。点击左下角“Add”在弹出框中选择“RobotModel”，点击“OK”完成模型添加功能。操作完成后在中间展示框内即可看到URDF配置的小车模型。





实验总结

本实验通过URDF描述机器人模拟器搭建仿真机器人小车模型，并在RVIZ中进行展示。通过本实验重点掌握URDF中机器人模型由连接件(link)和连接连接件的关节(joint)等部件组成，以及RVIZ的使用。URDF能够描述机器人的运动学和动力学特征、视觉表示、碰撞模型等，是仿真环境开发常用技术。
