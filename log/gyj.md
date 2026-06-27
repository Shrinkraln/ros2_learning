# 1.系统架构
## 1. 命令行
1. 节点查看
```bash
ros2 node list
ros2 node info /ndoe_name
```
```bash
imain@xxl:~/workspace/training_summer$ ros2 node info /turtlesim 
/turtlesim
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
    #话题订阅:话题类型(这个类型是在ros包的路径下面指定了包括数据类型的定义)
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/color_sensor: turtlesim/msg/Color
    /turtle1/pose: turtlesim/msg/Pose
  Service Servers:
    /clear: std_srvs/srv/Empty
    /kill: turtlesim/srv/Kill
    /reset: std_srvs/srv/Empty
    /spawn: turtlesim/srv/Spawn
    /turtle1/set_pen: turtlesim/srv/SetPen
    /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
    /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    /turtlesim/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /turtlesim/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /turtlesim/get_parameters: rcl_interfaces/srv/GetParameters
    /turtlesim/list_parameters: rcl_interfaces/srv/ListParameters
    /turtlesim/set_parameters: rcl_interfaces/srv/SetParameters
    /turtlesim/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
    #服务提供:服务类型(服务立即生效，不被阻塞，如清理数据)
  Service Clients:
    #没有调用其他node提供的服务
  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
    #动作可以被阻塞，如指定移动到目标点，只返回结果
  Action Clients:


```
2. 话题查看
```bash
ros2 topic list
ros2 topic echo /topic_name
ros2 topic info /topic_name
ros2 topic hz /topic_name
ros2 topic bw /topic_name
```
```bash
imain@xxl:~/workspace/training_summer$ ros2 topic info /turtle1/pose 
Type: turtlesim/msg/Pose
Publisher count: 1
Subscription count: 0

imain@xxl:~/workspace/training_summer$ ros2 topic hz /turtle1/pose 
average rate: 62.470
	min: 0.015s max: 0.017s std dev: 0.00050s window: 64
average rate: 62.485
	min: 0.015s max: 0.017s std dev: 0.00052s window: 127
average rate: 62.487
	min: 0.015s max: 0.017s std dev: 0.00051s window: 190

imain@xxl:~/workspace/training_summer$ ros2 topic bw /turtle1/pose 
Subscribed to [/turtle1/pose]
1.52 KB/s from 63 messages
	Message size mean: 0.02 KB min: 0.02 KB max: 0.02 KB
1.50 KB/s from 100 messages
	Message size mean: 0.02 KB min: 0.02 KB max: 0.02 KB
1.51 KB/s from 100 messages
	Message size mean: 0.02 KB min: 0.02 KB max: 0.02 KB

imain@xxl:~/workspace/training_summer$ ros2 interface show geometry_msgs/msg/Twist  # 再查结构
# This expresses velocity in free space broken into its linear and angular parts.
Vector3  linear
	float64 x
	float64 y
	float64 z
Vector3  angular
	float64 x
	float64 y
	float64 z
```
3. 话题发送
-r 循环频率 -t 次数
```bash
#--rate 1发送频率
imain@xxl:~/workspace/training_summer$ ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist  "{linear:{x: 2.0, y: 0.0 ,z: 0.0},angular: {x: 0.0,y: 0.0 ,z: 1.8}}"
publisher: beginning loop
publishing #1: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=2.0, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=1.8))
publishing #2: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=2.0, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=1.8))
```
4. 服务调用
```bash
imain@xxl:~/workspace/training_summer$ ros2 service list
/clear
/kill
/reset
/spawn
/teleop_turtle/describe_parameters
/teleop_turtle/get_parameter_types
/teleop_turtle/get_parameters
/teleop_turtle/list_parameters
/teleop_turtle/set_parameters
/teleop_turtle/set_parameters_atomically
/turtle1/set_pen
/turtle1/teleport_absolute
/turtle1/teleport_relative
/turtlesim/describe_parameters
/turtlesim/get_parameter_types
/turtlesim/get_parameters
/turtlesim/list_parameters
/turtlesim/set_parameters
/turtlesim/set_parameters_atomically

imain@xxl:~/workspace/training_summer$ ros2 service type /spawn 
turtlesim/srv/Spawn

imain@xxl:~/workspace/training_summer$ ros2 interface show turtlesim/srv/Spawn 
float32 x
float32 y
float32 theta
string name # Optional.  A unique name will be created and returned if this is empty
---
string name

imain@xxl:~/workspace/training_summer$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2,y: 2, theta: 0.2 , name: ''}"
requester: making request: turtlesim.srv.Spawn_Request(x=2.0, y=2.0, theta=0.2, name='')
response:
turtlesim.srv.Spawn_Response(name='turtle2')
```
4. 动作调用
```bash
imain@xxl:~/workspace/training_summer$ ros2 action list
/turtle1/rotate_absolute
/turtle2/rotate_absolute

imain@xxl:~/workspace/training_summer$ ros2 action
usage: ros2 action [-h]
                   Call `ros2 action <command> -h` for more detailed
                   usage. ...

Various action related sub-commands
options:
  -h, --help            show this help message and exit
Commands:
  info       Print information about an action
  list       Output a list of action names
  send_goal  Send an action goal

  Call `ros2 action <command> -h` for more detailed usage.


imain@xxl:~/workspace/training_summer$ ros2 action info /turtle1/rotate_absolute 
Action: /turtle1/rotate_absolute
Action clients: 1
    /teleop_turtle
Action servers: 1
    /turtlesim

imain@xxl:~/workspace/training_summer$ ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 3.0}"
Waiting for an action server to become available...
Sending goal:
     theta: 3.0

Goal accepted with ID: 47ccf62905c44589b6c2805017f6bd5f

Result:
    delta: -0.04800000041723251

Goal finished with status: SUCCEEDED
```
5. 对于以上1+3的总结
```bash
#对于接口，node info就可以查看节点所有话题，服务，动作，也包含其接口类型
ros2 node info /turtlesim
#对于接口具体的数据类型，interface shoe 查看yaml
#输出可能是含有-----分隔开的，其中第一部分为输入，第二部分为结果返回，第三部分为过程反馈
ros2 interface show turtlesim/srv/SetPen
#对于发布使用各自api
ros2 topic pub topic_name topic_type "yaml"
ros2 service call service_name servive_type "yaml"
ros2 action send_goal action_name action_type "yaml"
```
6. 记录数据
```bash
imain@xxl:~/workspace/training_summer$ ros2 bag record /turtle1/cmd_val

imain@xxl:~/workspace/training_summer$ ros2 bag play rosbag2_2026_06_27-16_37_33/
[INFO] [1782549574.616968581] [rosbag2_storage]: Opened database 'rosbag2_2026_06_27-16_37_33/rosbag2_2026_06_27-16_37_33_0.db3' for READ_ONLY.
[WARN] [1782549574.616991782] [rosbag2_cpp]: No topics were listed in metadata.
[INFO] [1782549574.617004714] [rosbag2_player]: Set rate to 1
[INFO] [1782549574.618225993] [rosbag2_player]: Adding keyboard callbacks.
[INFO] [1782549574.618240532] [rosbag2_player]: Press SPACE for Pause/Resume
[INFO] [1782549574.618246960] [rosbag2_player]: Press CURSOR_RIGHT for Play Next Message
[INFO] [1782549574.618251492] [rosbag2_player]: Press CURSOR_UP for Increase Rate 10%
[INFO] [1782549574.618255507] [rosbag2_player]: Press CURSOR_DOWN for Decrease Rate 10%
[INFO] [1782549574.618462569] [rosbag2_storage]: Opened database 'rosbag2_2026_06_27-16_37_33/rosbag2_2026_06_27-16_37_33_0.db3' for READ_ONLY.
```
# 2. 核心概念
## 1. 工作空间
```bash
#初始化rosdep包管理
sudo apt install -y python3-pip
sudo pip3 install rosdepc
sudo rosdepc init
rosdepc update
#创建工作区
mkdir -p ~/workspace/dev_ws/src
cd ~/workspace/dev_ws/src
git clone  https://gitee.com/guyuehome/ros2_21_tutorials.git
cd ~/workspace/dev_ws
rosdepc install -i --from-path src --rosdistro humble -y
sudo apt install python3-colcon-ros
colcon build
#总共是四个文件夹，src 放源码，build 放编译目标文件，install 方链接可执行文件
#添加到环境
source ~/workspace/dev_ws/intstall/local_setup.sh
```