xacro文件：
## mbot_base.xacro
```
<?xml version="1.0"?>
<robot name="mbot" xmlns:xacro="http://www.ros.org/wiki/xacro">

    <!-- PROPERTY LIST -->
    <xacro:property name="M_PI" value="3.1415926"/>
    <xacro:property name="base_radius" value="0.20"/>
    <xacro:property name="base_length" value="0.16"/>

    <xacro:property name="wheel_radius" value="0.06"/>
    <xacro:property name="wheel_length" value="0.025"/>
    <xacro:property name="wheel_joint_y" value="0.19"/>
    <xacro:property name="wheel_joint_z" value="0.05"/>

    <xacro:property name="caster_radius" value="0.015"/> <!-- wheel_radius - ( base_length/2 - wheel_joint_z) -->
    <xacro:property name="caster_joint_x" value="0.18"/>

    <!-- Defining the colors used in this robot -->
    <material name="yellow">
        <color rgba="1 0.4 0 1"/>
    </material>
    <material name="black">
        <color rgba="0 0 0 0.95"/>
    </material>
    <material name="gray">
        <color rgba="0.75 0.75 0.75 1"/>
    </material>
    
    <!-- Macro for robot wheel -->
    <xacro:macro name="wheel" params="prefix reflect">
        <joint name="${prefix}_wheel_joint" type="continuous">
            <origin xyz="0 ${reflect*wheel_joint_y} ${-wheel_joint_z}" rpy="0 0 0"/>
            <parent link="base_link"/>
            <child link="${prefix}_wheel_link"/>
            <axis xyz="0 1 0"/>
        </joint>

        <link name="${prefix}_wheel_link">
            <visual>
                <origin xyz="0 0 0" rpy="${M_PI/2} 0 0" />
                <geometry>
                    <cylinder radius="${wheel_radius}" length = "${wheel_length}"/>
                </geometry>
                <material name="gray" />
            </visual>
        </link>
    </xacro:macro>

    <!-- Macro for robot caster -->
    <xacro:macro name="caster" params="prefix reflect">
        <joint name="${prefix}_caster_joint" type="continuous">
            <origin xyz="${reflect*caster_joint_x} 0 ${-(base_length/2 + caster_radius)}" rpy="0 0 0"/>
            <parent link="base_link"/>
            <child link="${prefix}_caster_link"/>
            <axis xyz="0 1 0"/>
        </joint>

        <link name="${prefix}_caster_link">
            <visual>
                <origin xyz="0 0 0" rpy="0 0 0"/>
                <geometry>
                    <sphere radius="${caster_radius}" />
                </geometry>
                <material name="black" />
            </visual>
        </link>
    </xacro:macro>

    <xacro:macro name="mbot_base">
        <link name="base_footprint">
            <visual>
                <origin xyz="0 0 0" rpy="0 0 0" />
                <geometry>
                    <box size="0.001 0.001 0.001" />
                </geometry>
            </visual>
        </link>

        <joint name="base_footprint_joint" type="fixed">
            <origin xyz="0 0 ${base_length/2 + caster_radius*2}" rpy="0 0 0" />        
            <parent link="base_footprint"/>
            <child link="base_link" />
        </joint>

        <link name="base_link">
            <visual>
                <origin xyz=" 0 0 0" rpy="0 0 0" />
                <geometry>
                    <cylinder length="${base_length}" radius="${base_radius}"/>
                </geometry>
                <material name="yellow" />
            </visual>
        </link>

        <wheel prefix="left" reflect="-1"/>
        <wheel prefix="right" reflect="1"/>

        <caster prefix="front" reflect="-1"/>
        <caster prefix="back" reflect="1"/>
    </xacro:macro>
</robot>
```

## mbot.xacro
```
<?xml version="1.0"?>
<robot name="arm" xmlns:xacro="http://www.ros.org/wiki/xacro">

    <xacro:include filename="$(find mbot_description)/urdf/xacro/mbot_base.xacro" />

    <mbot_base/>

</robot>
```

# 模型显示
## 方法一：将xacro文件转换成URDF文件后显示
rosrun xacro xacro.py mbot.xacro > mbot.urdf

## 方法二：直接调用xacro文件解析器
```
<argname="model" default="$(find xacro)/xacro --inorder '$(find mbot description)/urdf/xacro/mbot.xacro"" />
<paramname="robot_description" command="$(arg model)" />
```
## 方法二的launch文件：display_mbot_base_xacro.launch
```
<launch>
	<arg name="model" default="$(find xacro)/xacro --inorder '$(find mbot_description)/urdf/xacro/mbot.xacro'" />
	<arg name="gui" default="true" />

	<param name="robot_description" command="$(arg model)" />

    <!-- 设置GUI参数，显示关节控制插件 -->
	<param name="use_gui" value="$(arg gui)"/>

    <!-- 运行joint_state_publisher节点，发布机器人的关节状态  -->
	<node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" />

	<!-- 运行robot_state_publisher节点，发布tf  -->
	<node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" />

    <!-- 运行rviz可视化界面 -->
	<node name="rviz" pkg="rviz" type="rviz" args="-d $(find mbot_description)/config/mbot.rviz" required="true" />

</launch>
```