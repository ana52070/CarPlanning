学习链接：[文档预览 - Gitee.com](https://gitee.com/gwmunan/ros2/wikis/pages?sort_id=9601241&doc_id=4855084)[http://fishros.org/doc/nav2](http://fishros.org/doc/nav2)

[http://fishros.org/doc/ros2/humble/](http://fishros.org/doc/ros2/humble/)
## 速通ROS2

### 初学者：CLI工具

#### 1.配置环境


##### 1.导入设置文件
1.您需要在每次打开新的Shell时运行此命令以便访问ROS 2命令，如下所示：
```
source /opt/ros/humble/setup.bash
```

如果您不想每次打开新的shell时都要进行源命令设置（跳过任务1），您可以将该命令添加到您的shell启动脚本中：
```
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```
要撤销此操作，请找到您系统的shell启动脚本，并删除附加的源命令。


##### 2.检查环境变量
使用ROS 2设置文件会设置几个操作ROS 2所必需的环境变量。如果您在查找或使用ROS 2软件包时遇到问题，请确保您的环境已经正确设置，可以使用以下命令：
```
printenv | grep -i ROS
```

检查变量，如 `ROS_DISTRO` 和 `ROS_VERSION` 是否设置正确。
```bash
chuiyu@chuiyu:~$ printenv | grep -i 
ROS ROS_VERSION=2 
ROS_PYTHON_VERSION=3 
ROS_DOMAIN_ID=1 
AMENT_PREFIX_PATH=/opt/ros/humble 
PYTHONPATH=/opt/ros/humble/lib/python3.10/site-packages:/opt/ros/humble/local/lib/python3.10/dist-packages LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/gazebo-11/plugins:/opt/ros/humble/opt/rviz_ogre_vendor/lib:/opt/ros/humble/lib/x86_64-linux-gnu:/opt/ros/humble/lib ROS_LOCALHOST_ONLY=0 PATH=/opt/ros/humble/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin ROS_DISTRO=humble
```
如果环境变量没有正确设置，请返回到您所遵循的安装指南的 ROS 2 包安装部分。如果您需要更具体的帮助（因为环境设置文件可能来自不同的地方），您可以从社区 [获取答案](https://robotics.stackexchange.com/)。


**2.1 ROS_DOMAIN_ID 变量**
有关 ROS 域 ID 的详细信息，请参阅 [域 ID](http://fishros.org/doc/ros2/humble/Concepts/Intermediate/About-Domain-ID.html) 文章。

确定了ROS 2节点组的唯一整数后，可以使用以下命令设置环境变量：
```bash
export ROS_DOMAIN_ID=<your_domain_id>


# 为了在不同的shell会话之间保持这个设置，可以将该命令添加到shell启动脚本中：
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
```

**2.2 ROS_LOCALHOST_ONLY 变量**

默认情况下，ROS 2通信不限于本地主机。`ROS_LOCALHOST_ONLY`环境变量允许您将ROS 2通信限制为仅限本地主机。这意味着您的ROS 2系统及其主题、服务和操作将对本地网络上的其他计算机不可见。在某些情况下，使用``ROS_LOCALHOST_ONLY``非常有帮助，比如在教室中，多个机器人可能会向同一主题发布，从而导致奇怪的行为。您可以使用以下命令设置环境变量：
```bash
export ROS_LOCALHOST_ONLY=1

# 为了在不同的shell会话之间保持这个设置，可以将该命令添加到shell启动脚本中：
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bashrc
```
##### 总结

在使用之前，需要正确配置ROS 2开发环境。可以通过两种方式实现：要么在每次打开新的shell时使用源文件，要么将源命令添加到启动脚本中。

如果您在使用ROS 2时遇到任何定位或使用软件包的问题，首先应该检查您的环境变量，并确保其设置为您预期的版本和发行版。



#### 2.使用turtlesim、ros2和rqt

##### 背景
Turtlesim是一个轻量级的用于学习ROS 2的模拟器。它以最基本的方式展示了ROS 2的功能，让您对以后在真实机器人或机器人模拟中要做什么有个基本的了解。

ros2工具是用户管理、检视和与ROS系统交互的方式。它支持多个命令，针对系统及其操作的不同方面。用户可以使用它来启动一个节点、设置参数、监听话题等等。ros2工具是ROS 2核心安装的一部分。

rqt 是一个用于ROS 2的图形用户界面（GUI）工具。rqt 中的所有操作都可以通过命令行完成，但是rqt 提供了一种更加用户友好的方式来操作ROS 2元素。

本教程介绍了ROS 2的核心概念，如节点（nodes）、主题（topics）和服务（services）。所有这些概念将在后续教程中详细阐述；目前，您只需设置工具并对其有所了解。


##### 1.安装turtlesim
```bash
sudo apt update

sudo apt install ros-humble-turtlesim


#检查是否安装了该软件包
ros2 pkg executables turtlesim

# 上述命令应返回turtlesim的可执行文件列表:
turtlesim draw_square
turtlesim mimic
turtlesim turtle_teleop_key
turtlesim turtlesim_node

```


##### 2.开始turtlesim
要启动turtlesim，请在终端中输入以下命令:
```bash
ros2 run turtlesim turtlesim_node
```
模拟器窗口应出现，并在中心显示一个随机的乌龟。
![[Pasted image 20251014210711.png]]

在终端下面的命令中，您将看到来自节点的消息：
```bash
# 节点信息
[INFO] [1760447223.878088046] [turtlesim]: Starting turtlesim with node name /turtlesim
[INFO] [1760447223.881540245] [turtlesim]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]

```
在那里您可以看到默认海龟的名称和它生成的坐标。

##### 3.使用turtlesim
打开一个新的终端并再次启动ROS 2。

现在您将运行一个新节点来控制第一个节点中的海龟：
```bash
ros2 run turtlesim turtle_teleop_key
```
此时，您应该已经打开了三个窗口：一个终端运行着``turtlesim_node``，一个终端运行着``turtle_teleop_key``，以及一个turtlesim窗口。调整这些窗口的位置，使您能够看到turtlesim窗口，但同时也要使运行``turtle_teleop_key``的终端处于活动状态，以便您可以控制turtlesim中的海龟。

使用键盘上的箭头键来控制海龟。它将在屏幕上移动，并使用其附加的“笔”来绘制它迄今为止所经过的路径。

您可以使用各自命令的``list``子命令查看节点及其关联的主题、服务和操作：
```bash
ros2 node list
ros2 topic list
ros2 service list
ros2 action list
```
在接下来的教程中，您将了解更多关于这些概念的内容。由于本教程的目标仅是对turtlesim进行一般概述，您将使用rqt调用一些turtlesim的服务并与``turtlesim_node``进行交互。


##### 4.安装rqt
```bash
sudo apt update

sudo apt install ~nros-humble-rqt*
```

要运行rqt：
```bash
rqt
```

##### 5.使用rqt
第一次运行rqt时，窗口将为空白。不要担心；只需在顶部的菜单栏中选择**插件** > **服务** > **服务调用器**。


**rqt可能需要一些时间来定位所有插件。如果你点击插件但看不到服务或任何其他选项，请关闭rqt并在终端中输入命令``rqt --force-discover``。**


![[Pasted image 20251014211540.png]]

请使用位于**service**下拉列表左侧的刷新按钮，以确保所有的turtlesim节点服务都可用。

点击**service**下拉列表，查看turtlesim的服务，并选择``/spawn``服务。

**5.1 尝试spawn服务
让我们使用rqt调用``/spawn``服务。从名称可以猜到，`/spawn`将在turtlesim窗口中创建另一只海龟。

通过在**表达式**列的空单引号之间双击，为新的海龟设置一个唯一的名称，比如``turtle2``。您可以看到这个表达式对应于**name**的值，并且是**string**类型的。

接下来，输入一些有效的坐标来生成新的海龟，比如``x = 1.0``和``y = 1.0``。

![[Pasted image 20251014211805.png]]


要生成``turtle2``，您需要点击rqt窗口右上方的**Call**按钮调用该服务。


如果服务调用成功，您应该会看到一个新的海龟（带有随机设计）在您输入的 **x** 和 **y** 坐标处生成。

![[Pasted image 20251014211900.png]]


如果您在rqt中刷新服务列表，您还会看到与新海龟相关的服务，例如 `/turtle2/...`，除了 `/turtle1/...`。


**5.2 尝试set_pen服务


现在让我们使用 `/set_pen` 服务为 `turtle1` 设置一个独特的画笔：
![[Pasted image 20251014212028.png]]

在**r**、**g**和**b**的取值范围为0至255之间时，设置了画笔``turtle1``的颜色，而**width**设置了线条的粗细。

要让``turtle1``画出明显的红色线条，将**r**的值改为255，将**width**的值改为5。更新数值后别忘了调用该服务。

如果返回到运行``turtle_teleop_key``的终端并按下箭头键，你会看到``turtle1``的画笔已经改变了。

![[Pasted image 20251014212406.png]]

你可能还注意到没有办法移动``turtle2``。这是因为``turtle2``没有对应的teleop节点。



##### 6. 重映射

您需要一个第二个teleop节点才能控制“turtle2”。然而，如果您尝试运行与之前相同的命令，您会注意到这个命令也控制“turtle1”。改变这种行为的方法是通过重映射“cmd_vel”话题。


在一个新的终端中，设置ROS 2环境并运行：
```bash
ros2 run turtlesim turtle_teleop_key --ros-args --remap turtle1/cmd_vel:=turtle2/cmd_vel
```
现在，当这个终端处于活动状态时，您可以移动“turtle2”，当另一个运行“turtle_teleop_key”的终端处于活动状态时，您可以移动“turtle1”。
![[Pasted image 20251014212659.png]]

##### 7. 关闭 turtlesim
要停止仿真，您可以在``turtlesim_node``终端中输入``Ctrl + C``，在``turtle_teleop_key``终端中输入``q``。

#### 3.理解节点

##### 背景
**ROS2中的两个节点**

ROS中的每个节点应该负责一个单一的模块化功能，例如控制轮子马达或发布来自激光测距仪的传感器数据。每个节点可以通过主题（topics）、服务（services）、动作（actions）或参数（parameters）与其他节点进行数据的发送和接收。

![[Nodes-TopicandService.gif]]一个完整的机器人系统由许多协同工作的节点组成。在ROS 2中，单个可执行文件（C++程序、Python程序等）可以包含一个或多个节点。

##### 1.ROS2 run
命令``ros2 run``会从一个软件包中启动一个可执行文件。
```bash
ros2 run <package_name> <executable_name>
```
要运行turtlesim，请打开一个新的终端，并输入以下命令：
```bash
ros2 run turtlesim turtlesim_node
```

turtlesim窗口将会打开。
这里，包名是[[#2.使用turtlesim、ros2和rqt]]，可执行文件名是 `turtlesim_node`。

然而，我们还不知道节点名称。您可以使用 `ros2 node list` 命令查找节点名称。

##### 2. ros2 node list

`ros2 node list` 命令将显示所有正在运行的节点的名称。当您想与节点进行交互或者系统运行多个节点时需要跟踪它们时，这特别有用。

在另一个终端中打开新的终端，同时 turtlesim 仍在运行，请输入以下命令：
```bash
ros2 node list
```
终端将返回节点名称：
```bash
/turtlesim
```
再打开一个新的终端，并使用以下命令启动 teleop 节点：
```bash
ros2 run turtlesim turtle_teleop_key

```

在这里，我们再次引用了“turtlesim”软件包，但这次我们的目标是名为“turtle_teleop_key”的可执行文件。

返回到之前运行``ros2 node list``的终端，再次运行该命令。你将会看到两个活动节点的名称：
```bash
/turtlesim
/teleop_turtle
```

##### 2.1 重映射
[重映射](https://design.ros2.org/articles/ros_command_line_arguments.html#name-remapping-rules) 允许你重新分配默认节点属性，如节点名称、话题名称、服务名称等，以自定义的值。在上一个教程中，你在``turtle_teleop_key``上使用了重映射来改变``cmd_vel``话题和目标节点为**turtle2**。

现在，让我们重新分配``/turtlesim``节点的名称。在一个新的终端中运行以下命令：
```bash
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
```

由于您再次调用``ros2 run``启动了turtlesim，将会打开另一个turtlesim窗口。但是，现在如果您返回到之前运行``ros2 node list``的终端，并再次运行该命令，您将看到三个节点名称：
```bash
/my_turtle
/turtlesim
/teleop_turtle
```

##### 3.ros2 节点信息

既然您知道了节点的名称，您可以使用以下命令访问有关它们的更多信息：
```bash
ros2 node info <node_name>
```
要查看您最新的节点``my_turtle``，请运行以下命令：
```bash
ros2 node info /my_turtle
```
`ros2 node info`返回订阅者、发布者、服务和动作的列表，即与该节点进行交互的ROS图连接。输出应该如下所示：
```bash
/my_turtle # 节点名称，代表一个具体的功能节点（此处是 turtlesim 中的一个海龟节点） 

Subscribers: # 订阅者列表（该节点会接收哪些话题的数据） 
	/parameter_events: rcl_interfaces/msg/ParameterEvent # 订阅参数事件话题，用于接收系统中参数变化的通知（如参数修改、新增等） 
	/turtle1/cmd_vel: geometry_msgs/msg/Twist # 订阅海龟运动控制话题，接收速度指令消息（包含线速度和角速度），用于控制海龟移动 
	
Publishers: # 发布者列表（该节点会发布哪些话题的数据） 
	/parameter_events: rcl_interfaces/msg/ParameterEvent # 发布参数事件话题，当节点自身参数发生变化时，向系统广播参数变化信息 
	/rosout: rcl_interfaces/msg/Log # 发布日志消息到 rosout 话题，用于输出节点运行中的日志（如调试信息、警告、错误等） 
	/turtle1/color_sensor: turtlesim/msg/Color # 发布海龟底部颜色传感器数据，包含当前位置的RGB颜色信息 
	/turtle1/pose: turtlesim/msg/Pose # 发布海龟当前位姿信息，包括x坐标、y坐标、朝向角度、线速度、角速度 
	
Service Servers: # 服务服务器列表（该节点提供哪些服务，可被其他节点调用） 
	/clear: std_srvs/srv/Empty # 提供清空画布服务，调用后清除海龟运动轨迹，无输入输出参数 
	/kill: turtlesim/srv/Kill # 提供删除海龟服务，输入海龟名称，删除指定海龟 
	/my_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters # 提供参数描述服务，返回节点参数的详细信息（如类型、范围等） 
	/my_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes # 提供获取参数类型服务，输入参数名称，返回对应参数的类型 
	/my_turtle/get_parameters: rcl_interfaces/srv/GetParameters # 提供获取参数值服务，输入参数名称，返回对应参数的当前值 
	/my_turtle/list_parameters: rcl_interfaces/srv/ListParameters # 提供参数列表服务，返回节点所有参数的名称列表 
	/my_turtle/set_parameters: rcl_interfaces/srv/SetParameters # 提供设置参数服务，输入参数名称和值，修改节点参数
	/my_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically # 提供原子化设置参数服务，确保多个参数的设置要么全部成功，要么全部失败（避免部分生效） 
	/reset: std_srvs/srv/Empty # 提供重置服务，调用后将海龟恢复到初始位置和状态，无输入输出参数 
	/spawn: turtlesim/srv/Spawn # 提供生成新海龟服务，输入位置、角度、名称等信息，在指定位置生成新海龟 
	/turtle1/set_pen: turtlesim/srv/SetPen # 提供设置画笔服务，输入颜色、线宽等参数，修改海龟运动轨迹的样式 
	/turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute # 提供绝对位置 teleport 服务，输入目标坐标和角度，将海龟直接移动到指定位置 
	/turtle1/teleport_relative: turtlesim/srv/TeleportRelative # 提供相对位置 teleport 服务，输入相对距离和角度，将海龟从当前位置相对移动 

Service Clients: # 服务客户端列表（该节点会调用哪些其他节点的服务） # 此处为空，表示该节点不主动调用其他服务 

Action Servers: # 动作服务器列表（该节点提供哪些动作服务，支持长时间任务和反馈） 
	/turtle1/rotate_absolute: turtlesim/action/RotateAbsolute # 提供绝对旋转动作服务，输入目标角度，控制海龟旋转到指定角度，过程中会反馈当前角度 
	
Action Clients: # 动作客户端列表（该节点会调用哪些其他节点的动作服务） # 此处为空，表示该节点不主动调用其他动作服务
```

现在尝试在``/teleop_turtle``节点上运行相同的命令，观察它的连接与``my_turtle``有何不同。
```bash
# 执行 `ros2 node info /teleop_turtle`命令的输出结果 
There are 2 nodes in the graph with the exact name "/teleop_turtle". 
# 提示：系统中存在2个同名节点 "/teleop_turtle"，当前仅显示其中一个的信息 
You are seeing information about only one of them. 

/teleop_turtle # 节点名称（海龟遥控节点，用于通过键盘等设备控制海龟运动） 

Subscribers: # 订阅者列表（该节点接收的话题） 
	/parameter_events: rcl_interfaces/msg/ParameterEvent # 订阅参数事件话题，用于接收系统中参数变化的通知（如其他节点参数修改） 
	
Publishers: # 发布者列表（该节点发布的话题） 
	/parameter_events: rcl_interfaces/msg/ParameterEvent # 发布参数事件话题，当自身参数变化时向系统广播 
	/rosout: rcl_interfaces/msg/Log # 发布日志消息到 rosout 话题，输出运行日志（调试、警告等） 
	/turtle1/cmd_vel: geometry_msgs/msg/Twist # 发布海龟速度控制指令，包含线速度和角速度，用于控制 /turtle1 节点的运动 

Service Servers: # 服务服务器列表（该节点提供的服务） 
	/teleop_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters # 提供参数描述服务，返回自身参数的详细信息（类型、范围等） 
	/teleop_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes # 提供获取参数类型服务，输入参数名，返回对应类型 
	/teleop_turtle/get_parameters: rcl_interfaces/srv/GetParameters # 提供获取参数值服务，输入参数名，返回当前值 
	/teleop_turtle/list_parameters: rcl_interfaces/srv/ListParameters # 提供参数列表服务，返回自身所有参数的名称 
	/teleop_turtle/set_parameters: rcl_interfaces/srv/SetParameters # 提供设置参数服务，修改自身参数值 
	/teleop_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically # 提供原子化设置参数服务，确保多个参数修改要么全成功，要么全失败 

Service Clients: # 服务客户端列表（该节点调用的其他服务） # 此处为空，表示该节点不主动调用其他服务 

Action Servers: # 动作服务器列表（该节点提供的动作服务） # 此处为空，表示该节点不提供动作服务 

Action Clients: # 动作客户端列表（该节点调用的其他动作服务） 
	/turtle1/rotate_absolute: turtlesim/action/RotateAbsolute # 调用海龟绝对旋转动作服务，向 /turtle1 节点发送旋转目标角度，实现精确转向控制
```

在即将进行的教程中，您将更多地了解ROS图连接概念，包括消息类型。



个人想法：ROS2的这个：

==Subscribers:  # 订阅者列表（该节点接收的话题） ：相当于编程中的对象的 “输入接口”，用于接收外部传来的数据（类似函数的 “输入参数”，但不是变量本身，而是获取数据的方式）。==

==Publishers:  # 发布者列表（该节点发布的话题）:相当于编程中的对象的 “输出接口”，用于向外部主动推送数据（类似对象的 “getter 方法” 或 “事件通知”，但更强调主动推送）。==

==Service Servers:  # 服务服务器列表（该节点提供的服务）：服务是 “请求 - 响应” 模式，和编程中的对象的方法完全对应：其他对象（客户端）调用服务时，服务服务器（当前对象）执行特定逻辑并返回结果。==

==Service Clients:  # 服务客户端列表（该节点调用的其他服务）：相当于编程中的对象所调用的其他对象的方法，服务客户端主动调用其他节点的服务，和 “对象 A 调用对象 B 的方法” 完全一致。==

==Action Servers:  # 动作服务器列表（该节点提供的动作服务）:**对象的 “耗时方法”，支持进度反馈和中途取消**，动作是比服务更复杂的交互模式，适用于需要长时间执行（如移动机器人导航 10 秒）、需要实时反馈（当前走了多少米）、允许中途取消的场景。==

==Action Clients:  # 动作客户端列表（该节点调用的其他动作服务）:**调用其他对象的 “耗时方法”，并处理其进度反馈和结果**，动作客户端主动发起一个长时间任务，过程中接收进度、处理结果或发送取消指令，对应 “对象 A 调用对象 B 的耗时方法，并实时获取进度 / 取消任务”。==



##### 总结
节点是ROS 2中的基本元素，在机器人系统中具有单一的模块化功能。

在本教程中，您利用了``turtlesim``包中创建的节点，通过运行``turtlesim_node``和``turtle_teleop_key``可执行文件。

你学会了如何使用 `ros2 node list` 命令来发现活动节点的名称，以及使用 `ros2 node info` 命令来内省单个节点。这些工具对于理解复杂的实际机器人系统中的数据流非常重要。


#### 4.理解话题


##### 背景
ROS 2将复杂系统分解为许多模块化节点。话题是ROS图中的重要组成部分，作为节点交换消息的总线。
![[Topic-SinglePublisherandSingleSubscriber.gif]]一个节点可以向任意数量的主题发布数据，并同时订阅任意数量的主题。主题是数据在节点之间以及系统不同部分之间传递的主要方式之一。


##### 1.设置
到目前为止，你应该已经熟悉如何启动turtlesim。

打开一个新的终端并运行：
```bash
ros2 run turtlesim turtlesim_node
```


打开另一个终端并运行：
```bash
ros2 run turtlesim turtle_teleop_key
```


回顾一下之前的教程[[#3.理解节点]]，这些节点的名称默认情况下是``/turtlesim``和``/teleop_turtle``。

##### 2.rqt_graph
在本教程中，我们将使用``rqt_graph``来可视化节点和话题的变化，以及它们之间的连接。

在 turtlesim 教程中，介绍了如何安装 rqt 以及其包括的所有插件，包括`rqt_graph`。

要运行 rqt_graph，请打开一个新的终端并输入以下命令：
```bash
rqt_graph
```
您也可以通过打开 `rqt` 并选择 **Plugins** > **Introspection** > **Node Graph** 来打开 rqt_graph。
![[Pasted image 20251014222945.png]]
您应该能够看到上述的节点和主题，以及图形周围的两个操作（暂时忽略它们）。如果将鼠标悬停在中心的主题上，您会看到像上面图片中那样的颜色高亮显示。

该图形描述了 `/turtlesim` 节点和 `/teleop_turtle` 节点之间如何通过一个主题进行通信。`/teleop_turtle` 节点将数据（您输入的按键操作来移动乌龟）发布到 `/turtle1/cmd_vel` 主题上，而 `/turtlesim` 节点订阅该主题以接收数据。

==**在 `rqt_graph` 中，圆形代表节点（Node），普通矩形代表话题（Topic），双竖线矩形代表服务（Service）。**==


rqt_graph的突出显示功能在检查具有许多以多种不同方式连接的节点和主题的更复杂系统时非常有帮助。

rqt_graph是一个图形化内省工具。现在我们将看一些用于内省主题的命令行工具。


##### 3.ros2 topic list
在新终端中运行``ros2 topic list``命令将返回系统中当前活动的所有主题的列表：
```bash
chuiyu@chuiyu:~$ ros2 topic list 
/parameter_events 
/rosout 
/turtle1/cmd_vel 
/turtle1/color_sensor 
/turtle1/pose 
/turtle2/cmd_vel 
/turtle2/color_sensor 
/turtle2/pose
```

`ros2 topic list -t` 将返回相同的主题列表，这次在括号中附加了主题类型：
```bash
chuiyu@chuiyu:~$ ros2 topic list -t 
/parameter_events [rcl_interfaces/msg/ParameterEvent] 
/rosout [rcl_interfaces/msg/Log] 
/turtle1/cmd_vel [geometry_msgs/msg/Twist] 
/turtle1/color_sensor [turtlesim/msg/Color] 
/turtle1/pose [turtlesim/msg/Pose] 
/turtle2/cmd_vel [geometry_msgs/msg/Twist] 
/turtle2/color_sensor [turtlesim/msg/Color] 
/turtle2/pose [turtlesim/msg/Pose]
```

特别是类型属性是节点了解它们正在通过主题传输相同信息的方式。

如果你想知道在 rqt_graph 中所有这些主题的位置，在 **隐藏：** 下取消选中所有复选框：

![[Pasted image 20251015075939.png]]

不过现在，保留这些选项的选择以避免混淆。

##### 4.ros2 topic echo
要查看在话题上发布的数据，请使用：
```bash
ros2 topic echo <topic_name>
```


由于我们知道 `/teleop_turtle` 在 `/turtle1/cmd_vel` 话题上向 `/turtlesim` 发布数据，让我们使用 `echo` 来检查该话题：
```bash
ros2 topic echo /turtle1/cmd_vel
```


刚开始，这个命令不会返回任何数据。这是因为它正在等待 `/teleop_turtle` 发布一些东西。

返回到运行``turtle_teleop_key``的终端，并使用箭头键来移动乌龟。同时观察运行``echo``的终端，你将会看到每次移动时发布的位置数据：
```bash
linear:
  x: 2.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0
  ---
```

现在返回到rqt_graph并取消选择**Debug**框。
![[Pasted image 20251015080158.png]]
`/_ros2cli_26646`是刚刚我们运行的`echo`命令创建的节点（数字可能不同）。现在你可以看到发布者正在通过`cmd_vel`主题发布数据，而两个订阅者正在订阅它。


##### 5.ros2 topic info
主题不仅可以是一对一的通信，还可以是一对多的、多对一的或多对多的通信。

另一种理解方式是运行：
```bash
ros2 topic info /turtle1/cmd_vel
```
将返回：
```bash
Type: geometry_msgs/msg/Twist
Publisher count: 1
Subscription count: 2
```

##### 6.ros2 interface show

节点使用消息在主题上发送数据。发布者和订阅者必须发送和接收相同类型的消息来进行通信。

在运行 `ros2 topic list -t` 后，我们可以看到之前提到的主题类型，它们告诉我们每个主题使用的消息类型。回想一下，`cmd_vel` 主题的类型是：
```bash
geometry_msgs/msg/Twist
```

这意味着在``geometry_msgs``包中有一个名为``Twist``的``msg``。

现在我们可以在此类型上运行``ros2 interface show <msg type>``以了解其详细信息。具体来说，消息期望的数据结构是什么样的。
```bash
ros2 interface show geometry_msgs/msg/Twist
```


对于上述消息类型，它产生以下结果：

```bash
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
这告诉你``/turtlesim``节点期望接收一个带有两个向量``linear``和``angular``的消息，每个向量都有三个元素。如果你回想一下我们使用``echo``命令看到``/teleop_turtle``传递给``/turtlesim``的数据，它具有相同的结构：

```bash
linear:
  x: 2.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0
  ---
```

##### 7.ros2 topic pub
现在您已经有了消息结构，您可以直接从命令行发布数据到一个话题上，使用以下命令：
```bash
ros2 topic pub <topic_name> <msg_type> '<args>'
```

`'<args>'` 参数是您将传递给话题的实际数据，其结构正如您在前一节中了解到的。

请注意，这个参数需要以 YAML 语法输入。请输入完整的命令，如下所示：

```bash
ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

`--once` 是一个可选参数，表示“发布一条消息后退出”。

您将在终端中看到以下输出：
```bash
publisher: beginning loop
publishing #1: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=2.0, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=1.8))
```

然后您将看到乌龟移动如下：
![[Pasted image 20251015080739.png]]

乌龟（以及通常用于模拟乌龟的真实机器人）需要持续不断地接收命令才能保持运动。因此，要让乌龟持续移动，您可以运行以下命令：
```bash
ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```
这里的区别是删除了``--once``选项，并增加了``--rate 1``选项，告诉``ros2 topic pub``以1 Hz的恒定速率发布命令。
![[Pasted image 20251015080824.png]]

你可以刷新rqt_graph来查看图形化的情况。你会看到``ros2 topic pub ...``节点（`/_ros2cli_30358`）正在``/turtle1/cmd_vel``话题上发布消息，这些消息被``ros2 topic echo ...``节点（`/_ros2cli_26646`）和``/turtlesim``节点接收。
![[Pasted image 20251015080838.png]]

最后，你可以在``pose``话题上运行``echo``命令，并重新检查rqt_graph：
```bash
ros2 topic echo /turtle1/pose
```

![[Pasted image 20251015080900.png]]

你会看到``/turtlesim``节点也在向``pose``话题发布消息，而新的``echo``节点已经订阅了该话题。



##### 8.ros2 topic hz

作为对该过程的最后一次内省，您可以使用以下命令查看数据发布的速率：
```bash
ros2 topic hz /turtle1/pose
```
它将返回``/turtlesim``节点向``pose``主题发布数据的速率。

```bash
average rate: 59.354
  min: 0.005s max: 0.027s std dev: 0.00284s window: 58
```
回想一下，您使用``ros2 topic pub --rate 1``将``turtle1/cmd_vel``的速率设置为稳定的1 Hz。如果您使用``turtle1/cmd_vel``而不是``turtle1/pose``运行上述命令，则会看到反映该速率的平均值。


##### 总结
节点通过主题发布信息，允许任意数量的其他节点订阅和访问该信息。在本教程中，您使用rqt_graph和命令行工具检查了几个节点之间的连接。现在您应该对数据在ROS 2系统中如何传输有一个很好的了解。




#### 5.了解服务
##### 背景
服务是ROS图中节点之间的另一种通信方法。服务基于调用-响应模型，而不是主题的发布-订阅模型。虽然主题允许节点订阅数据流并获得持续更新，但服务仅在客户端特定调用时提供数据。
![[Service-SingleServiceClient.gif]]
![[Service-MultipleServiceClient.gif]]

==简单来说：**ROS 服务是 “机器人版的请求 - 响应通信”，核心逻辑与 HTTP 一致，但为机器人内部通信做了优化（更高效、更易发现、强结构化）**。==
- ==当你需要 “让某个节点做一件事，并返回结果” 时（如查询、控制指令），用 ROS 服务；==
- ==当你需要 “持续获取某个数据的更新” 时（如实时图像、里程计），用 ROS 主题。==

==**话题和服务的区别**：==
- ==**话题**：“我发我的，你听你的”—— 适合持续数据流；==
- ==**服务**：“你问我答，有求必应”—— 适合单次指令 / 查询。==

##### 1.设置
启动两个 turtlesim 节点，`/turtlesim` 和 `/teleop_turtle`。

打开一个新的终端并运行：
```bash
ros2 run turtlesim turtlesim_node
```
打开另一个终端并运行：
```bash
ros2 run turtlesim turtle_teleop_key
```


##### 2.ros2 service list
在新终端中运行 `ros2 service list` 命令将返回当前系统中所有活动服务的列表：
```bash
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
```

你会发现两个节点都有相同的六个以``parameters``结尾的服务。ROS 2中几乎每个节点都有这些基础设施服务，参数就是构建在这些服务之上的。下一个教程将更多地介绍参数。在本教程中，将省略对参数服务的讨论。

现在，让我们专注于turtlesim特定的服务，`/clear`、`/kill`、`/reset`、`/spawn`、`/turtle1/set_pen`、`/turtle1/teleport_absolute`和`/turtle1/teleport_relative`。你可能还记得在 [[#2.使用turtlesim、ros2和rqt]]教程中使用rqt与其中一些服务进行交互。


##### 3.ros2服务类型
服务具有描述服务的请求和响应数据结构的类型。服务类型的定义类似于主题类型，但服务类型由请求部分和响应部分组成。

要找出服务的类型，请使用以下命令：
```bash
ros2 service type <service_name>
```

让我们来看一下 turtlesim 的 `/clear` 服务。在新的终端中，输入以下命令：
```bash
ros2 service type /clear
```

预期的输出应为：
```bash
std_srvs/srv/Empty
```

`Empty` 类型表示服务调用在发送请求时不发送任何数据，在接收响应时也不接收任何数据。


**3.1 ros2 service list -t

要同时查看所有活动服务的类型，可以将``--show-types``选项（缩写为``-t``）附加到``list``命令后面：
```bash
ros2 service list -t
```

将返回：
```bash
/clear [std_srvs/srv/Empty]
/kill [turtlesim/srv/Kill]
/reset [std_srvs/srv/Empty]
/spawn [turtlesim/srv/Spawn]
...
/turtle1/set_pen [turtlesim/srv/SetPen]
/turtle1/teleport_absolute [turtlesim/srv/TeleportAbsolute]
/turtle1/teleport_relative [turtlesim/srv/TeleportRelative]
...
```

##### 4.ros2 service find
如果您想找到特定类型的所有服务，请使用以下命令：
```bash
ros2 service find <type_name>
```

例如，您可以通过以下方式找到所有“Empty”类型的服务：

```bash
ros2 service find std_srvs/srv/Empty
```

将返回：
```bash
/clear
/reset
```

##### 5.ros2 interface show
您可以从命令行调用服务，但首先需要了解输入参数的结构。

```bash
ros2 interface show <type_name>
```

在“/clear”服务的类型“Empty”上尝试一下：

```bash
ros2 interface show std_srvs/srv/Empty
```

将返回：

```bash
---
```

`---` 分隔了请求结构（上方）和响应结构（下方）。但是，正如您之前学到的那样，`Empty` 类型不发送或接收任何数据。因此，它的结构自然为空白。

让我们来查看一个发送和接收数据的服务类型，比如 `/spawn`。通过 `ros2 service list -t` 的结果，我们知道 `/spawn` 的类型是 `turtlesim/srv/Spawn`。

要查看 `/spawn` 服务的请求和响应参数，请运行以下命令：

```bash
ros2 interface show turtlesim/srv/Spawn
```

将返回：

```bash
float32 x
float32 y
float32 theta
string name # Optional.  A unique name will be created and returned if this is empty
---
string name
```

`---` 行上方的信息告诉我们调用 `/spawn` 需要的参数。`x`、`y` 和 `theta` 决定了生成的乌龟的二维姿态，而 `name` 显然是可选的。

下划线以下的信息在这种情况下不是您需要了解的内容，但它可以帮助您理解调用返回的响应的数据类型。



##### 6.ros2 服务调用
既然您知道了什么是服务类型，如何找到服务的类型，以及如何找到该类型参数的结构，您可以使用以下命令调用服务：

```bash
ros2 service call <service_name> <service_type> <arguments>
```

`<arguments>` 部分是可选的。例如，您知道 `Empty` 类型的服务没有任何参数：

```bash
ros2 service call /clear std_srvs/srv/Empty
```

该命令将清除乌龟绘制的所有线条，使得乌龟仿真窗口变空白。
![[Pasted image 20251015095125.png]]

现在让我们通过调用``/spawn``并设置参数来生成一个新的乌龟。在从命令行进行服务调用时，输入的``<arguments>``需要使用YAML语法。

输入以下命令：
```bash
ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"`
```

您将会看到方法式的视图，了解正在发生的情况，然后获得服务的响应：
```bash
requester: making request: turtlesim.srv.Spawn_Request(x=2.0, y=2.0, theta=0.2, name='')

response:
turtlesim.srv.Spawn_Response(name='turtle2')
```


您的乌龟仿真窗口将立即更新，显示新生成的乌龟：
![[Pasted image 20251015095315.png]]

##### 总结
节点可以使用ROS 2中的服务进行通信。与话题不同，话题是一种单向通信模式，其中一个节点发布信息，可以由一个或多个订阅者消费。服务是一种请求/响应模式，其中客户端向提供服务的节点发出请求，服务处理请求并生成响应。

通常情况下，你不应该使用服务进行连续调用；更适合使用话题或者甚至是动作。

在本教程中，你使用命令行工具来识别、内省和调用服务。


#### 6.理解参数
##### 背景
参数是节点的配置值。您可以将参数视为节点设置。节点可以将参数存储为整数、浮点数、布尔值、字符串和列表。在 ROS 2 中，每个节点都维护其自己的参数。有关参数的更多背景信息，请参阅 [概念文档](http://fishros.org/doc/ros2/humble/Concepts/Basic/About-Parameters.html)。

##### 1.设置
启动两个 turtlesim 节点，`/turtlesim` 和 `/teleop_turtle`。

打开一个新的终端并运行：

```bash
ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行：

```bash
ros2 run turtlesim turtle_teleop_key
```

##### 2.ros2 param list

要查看属于你的节点的参数，打开一个新的终端并输入以下命令：

```bash
ros2 param list
```

您将看到节点命名空间``/teleop_turtle``和``/turtlesim``，然后是每个节点的参数：

```bash
/teleop_turtle:
  qos_overrides./parameter_events.publisher.depth
  qos_overrides./parameter_events.publisher.durability
  qos_overrides./parameter_events.publisher.history
  qos_overrides./parameter_events.publisher.reliability
  scale_angular
  scale_linear
  use_sim_time
/turtlesim:
  background_b
  background_g
  background_r
  qos_overrides./parameter_events.publisher.depth
  qos_overrides./parameter_events.publisher.durability
  qos_overrides./parameter_events.publisher.history
  qos_overrides./parameter_events.publisher.reliability
  use_sim_time
```

每个节点都有参数``use_sim_time``；它不是 turtlesim 特有的。

根据它们的名称，看起来``/turtlesim``的参数使用RGB颜色值来确定 turtlesim 窗口的背景色。

要确定参数的类型，您可以使用``ros2 param get``命令。

##### 3.ros2 param get
要显示参数的类型和当前值，请使用以下命令：

```bash
ros2 param get <node_name> <parameter_name>
```

让我们找出``/turtlesim``的参数``background_g``的当前值：

```bash
ros2 param get /turtlesim background_g
```

将返回该值：

```bash
Integer value is: 86
```

现在你知道 `background_g` 保存了一个整数值。

如果你对 `background_r` 和 `background_b` 运行相同的命令，你将分别得到值 `69` 和 `255`。

##### 4.ros2 param set

要在运行时更改参数的值，请使用以下命令：

```bash
ros2 param set <node_name> <parameter_name> <value>
```

让我们改变 `/turtlesim` 的背景颜色:

```bash
ros2 param set /turtlesim background_r 150
```

你的终端应该返回以下信息:

```bash
Set parameter successful
```

然后你的 turtlesim 窗口的背景颜色会改变:
![[Pasted image 20251015100156.png]]

使用 `set` 命令设置参数只会在当前会话中生效，而不是永久生效。然而，你可以保存你的设置，并在下次启动节点时重新加载。

##### 5.ros2 param dump

你可以使用以下命令查看节点的所有当前参数值：

```bash
ros2 param dump <node_name>
```
该命令默认将参数值打印到标准输出（stdout），但你也可以将参数值重定向到文件中以便以后使用。要将当前``/turtlesim``参数的配置保存到文件``turtlesim.yaml``中，请执行以下命令：

```bash
ros2 param dump /turtlesim > turtlesim.yaml
```

你会在当前工作目录中找到一个新的文件，如果你打开这个文件，你会看到以下内容：

```bash
/turtlesim:
  ros__parameters:
    background_b: 255
    background_g: 86
    background_r: 150
    qos_overrides:
      /parameter_events:
        publisher:
          depth: 1000
          durability: volatile
          history: keep_last
          reliability: reliable
    use_sim_time: false
```
将参数转储非常方便，如果您希望将来重新加载具有相同参数的节点。

##### 6.ros2 param load
您可以使用以下命令将参数从文件加载到当前运行的节点：

```bash
ros2 param load <node_name> <parameter_file>
```

要将使用``ros2 param dump``生成的``turtlesim.yaml``文件加载到``/turtlesim``节点的参数中，请输入以下命令：

```bash
ros2 param load /turtlesim turtlesim.yaml
```

终端将返回以下消息:

```bash
Set parameter background_b successful
Set parameter background_g successful
Set parameter background_r successful
Set parameter qos_overrides./parameter_events.publisher.depth failed: parameter 'qos_overrides./parameter_events.publisher.depth' cannot be set because it is read-only
Set parameter qos_overrides./parameter_events.publisher.durability failed: parameter 'qos_overrides./parameter_events.publisher.durability' cannot be set because it is read-only
Set parameter qos_overrides./parameter_events.publisher.history failed: parameter 'qos_overrides./parameter_events.publisher.history' cannot be set because it is read-only
Set parameter qos_overrides./parameter_events.publisher.reliability failed: parameter 'qos_overrides./parameter_events.publisher.reliability' cannot be set because it is read-only
Set parameter use_sim_time successful
```


注解
**只读参数只能在启动时进行修改，不能在之后进行修改，这就是为什么“qos_overrides”参数会有一些警告。**

##### 7.在节点启动时加载参数文件
要使用保存的参数值启动相同的节点，请使用以下命令：
```bash
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```
这是您启动turtlesim时经常使用的相同命令，只需添加``--ros-args``和``--params-file``标志，然后加上要加载的文件名。

停止正在运行的turtlesim节点，并尝试使用保存的参数重新加载它，使用命令：
```bash
ros2 run turtlesim turtlesim_node --ros-args --params-file turtlesim.yaml
```

turtlesim窗口应该像往常一样显示，但是背景将是您之前设置的紫色背景。


注解
**当在节点启动时使用参数文件时，包括只读参数在内的所有参数都将被更新**


##### 总结
节点具有参数来定义其默认配置值。您可以从命令行中获取和设置参数值。您还可以将参数设置保存到文件中，在将来的会话中重新加载。


#### 7.理解行为
##### 背景

行为是ROS 2中的一种通信类型，用于长时间运行的任务。它们由三个部分组成：目标（goal），反馈（feedback）和结果（result）。

行为是建立在主题（topics）和服务（services）之上的。它们的功能类似于服务，但行为可以被取消。与只返回单个响应的服务不同，行为还提供稳定的反馈。

动作使用客户端-服务器模型，类似于发布者-订阅者模型（在 [topics tutorial](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html) 中有描述）。一个"动作客户端"节点向一个"动作服务器"节点发送一个目标，服务器节点确认目标并返回一系列反馈和结果流。

![[Action-SingleActionClient.gif]]

##### 1.设置

启动两个 turtlesim 节点，`/turtlesim` 和 `/teleop_turtle`。

打开一个新的终端并运行：

```bash
ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行：

```bash
ros2 run turtlesim turtle_teleop_key
```


##### 2.使用动作
当你启动 `/teleop_turtle` 节点时，你将在终端中看到以下消息:

```bash
Use arrow keys to move the turtle.
Use G|B|V|C|D|E|R|T keys to rotate to absolute orientations. 'F' to cancel a rotation.
```

让我们关注第二行，它对应一个动作。（第一条指令对应之前在 [topics 教程](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html) 中讨论的 "cmd_vel" 话题。）

请注意，在美国的 QWERTY 键盘上，字母键 `G|B|V|C|D|E|R|T` 组成了围绕 `F` 键的“方框”（如果你没有使用 QWERTY 键盘，请查看 [此链接](https://upload.wikimedia.org/wikipedia/commons/d/da/KB_United_States.svg) 以便跟随）。每个键在 `F` 周围的位置对应 turtlesim 中的方向。例如，按下 `E` 将使乌龟的方向旋转到左上角。

请注意运行``/turtlesim``节点的终端。每次按下其中一个按键，都会向``/turtlesim``节点中的动作服务器发送一个目标。目标是将乌龟旋转到面向特定方向。当乌龟完成旋转时，应该显示一个传达目标结果的消息：
```bash
[INFO] [turtlesim]: Rotation goal completed successfully
```
按下``F``键将取消正在执行的目标。

尝试在乌龟完成旋转之前按下``C``键，然后再按下``F``键。在运行``/turtlesim``节点的终端中，你将看到以下消息：
```bash
[INFO] [turtlesim]: Rotation goal canceled
```
不仅客户端（你在teleop中输入的内容），而且服务器端也可以停止目标的执行。当服务器端选择停止处理目标时，称为“中止”目标。

在第一个旋转完成之前，请尝试按下 `D` 键，然后按下 `G` 键。在运行``/turtlesim``节点的终端中，您将看到以下消息：

```bash
[WARN] [turtlesim]: Rotation goal received before a previous goal finished. Aborting previous goal
```

这个动作服务器选择中止第一个目标，因为它收到了一个新的目标。它也可以选择其他操作，比如拒绝新目标或在第一个目标完成后执行第二个目标。不要假设每个动作服务器在收到新目标时都会选择中止当前目标。


##### 3.ros2节点信息
要查看一个节点提供的动作列表，例如这里的 `/turtlesim`，请打开一个新的终端并运行以下命令：
```bash
ros2 node info /turtlesim
```

它将返回``/turtlesim``的订阅者、发布者、服务、动作服务器和动作客户端的列表：
```bash
/turtlesim
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
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
  Service Clients:

  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:
```

请注意，`/turtlesim`中的`/turtle1/rotate_absolute`动作位于动作服务器下。这意味着`/turtlesim`对`/turtle1/rotate_absolute`动作进行响应并提供反馈。

`/teleop_turtle`节点的名称为`/turtle1/rotate_absolute`，位于动作客户端下，这意味着它发送该动作名称的目标。要查看，请运行：
```bash
ros2 node info /teleop_turtle
```
将返回：

```bash
/teleop_turtle
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Service Servers:
    /teleop_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /teleop_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /teleop_turtle/get_parameters: rcl_interfaces/srv/GetParameters
    /teleop_turtle/list_parameters: rcl_interfaces/srv/ListParameters
    /teleop_turtle/set_parameters: rcl_interfaces/srv/SetParameters
    /teleop_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:

  Action Clients:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
```


##### 4.ros2 action list
要识别ROS图中的所有动作，请运行以下命令：
```bash
ros2 action list
```

将返回：
```bash
/turtle1/rotate_absolute
```
目前ROS图中只有一个动作。它控制着乌龟的旋转，就像您之前看到的那样。您还已经知道，对于这个动作，有一个动作客户端（位于``/teleop_turtle``）和一个动作服务器（位于``/turtlesim``），可以使用``ros2 node info <node_name>``命令查看。


**4.1ros2 action list -t**

动作也有类型，类似于主题和服务。要找到``/turtle1/rotate_absolute``的类型，请运行以下命令：
```bash
ros2 action list -t
```

将返回：
```bash
/turtle1/rotate_absolute [turtlesim/action/RotateAbsolute]
```

在每个动作名称右侧的括号中（在这种情况下只有``/turtle1/rotate_absolute``），是动作类型``turtlesim/action/RotateAbsolute``。当你想要从命令行或代码中执行一个动作时，你将需要它。

##### 5.ros2 action info

你可以使用以下命令进一步检查``/turtle1/rotate_absolute``动作：
```bash
ros2 action info /turtle1/rotate_absolute
```
它将返回
```bash
Action: /turtle1/rotate_absolute
Action clients: 1
    /teleop_turtle
Action servers: 1
    /turtlesim
```
这告诉我们之前在每个节点上运行`ros2 node info`时学到的内容：`/teleop_turtle`节点具有一个动作客户端，而`/turtlesim`节点具有一个`/turtle1/rotate_absolute`动作的动作服务器。



##### 6.ros2 interface show
在发送或执行动作目标之前，您还需要了解动作类型的结构。

回想一下，在运行命令``ros2 action list -t``时，您识别了``/turtle1/rotate_absolute``的类型。在终端中输入以下带有动作类型的命令：
```bash
ros2 interface show turtlesim/action/RotateAbsolute
```

将返回：

```bash
# The desired heading in radians
float32 theta
---
# The angular displacement in radians to the starting position
float32 delta
---
# The remaining rotation in radians
float32 remaining
```

在第一个 `---` 之上的部分是目标请求的结构（数据类型和名称）。接下来的部分是结果的结构。最后一部分是反馈的结构。


##### 7.ros2 action send_goal

现在让我们通过命令行发送一个动作目标，使用以下语法：

```bash
ros2 action send_goal <action_name> <action_type> <values>
```

`<values>` 需要以 YAML 格式提供。

请密切关注turtlesim窗口，并在终端中输入以下命令：

```bash
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"
```

你应该会看到乌龟旋转，并在终端中看到以下消息：
```bash
Waiting for an action server to become available...
Sending goal:
   theta: 1.57

Goal accepted with ID: f8db8f44410849eaa93d3feb747dd444

Result:
  delta: -1.568000316619873

Goal finished with status: SUCCEEDED
```

所有目标都有一个唯一的ID，在返回消息中显示。你还可以看到名为``delta``的结果字段，它是相对于起始位置的位移。

要查看此目标的反馈，请在``ros2 action send_goal``命令中添加``--feedback``：
```bash
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: -1.57}" --feedback
```

终端将返回以下消息:
```bash
Sending goal:
   theta: -1.57

Goal accepted with ID: e6092c831f994afda92f0086f220da27

Feedback:
  remaining: -3.1268222332000732

Feedback:
  remaining: -3.1108222007751465

…

Result:
  delta: 3.1200008392333984

Goal finished with status: SUCCEEDED
```

您将继续接收反馈，直到完成目标。剩余弧度。


##### 总结
动作类似于服务，允许您执行长时间运行的任务，提供定期反馈，并可取消。

一个机器人系统可能会使用动作进行导航。一个动作目标可以告诉机器人前往一个位置。当机器人导航到该位置时，它可以沿途发送更新（即反馈），然后在到达目的地后发送最终结果消息。

Turtlesim具有一个动作服务器，动作客户端可以向其发送旋转乌龟的目标。在本教程中，您可以内省该动作``/turtle1/rotate_absolute``，以更好地了解动作是什么以及它们是如何工作的。



#### 8.使用rqt_console查看日志
##### 背景
`rqt_console` 是一个用于在ROS 2中检查日志消息的图形界面工具。通常，日志消息会显示在终端中。使用 `rqt_console`，您可以随时间收集这些消息，以更加仔细和有组织的方式查看它们，对它们进行过滤、保存，甚至可以重新加载保存的文件以在不同的时间进行检查。

节点使用日志来输出与事件和状态有关的消息，以各种方式进行。它们的内容通常是信息性的，为了用户的利益。


##### 1.设置
在新的终端窗口中使用以下命令启动 `rqt_console`：

```bash
ros2 run rqt_console rqt_console
```

`rqt_console` 窗口将会打开：
![[Pasted image 20251016111722.png]]

控制台的第一部分用于显示系统的日志消息。

在中间，您可以选择通过排除严重级别来过滤消息。您还可以使用右侧的加号按钮添加更多的排除过滤器。

底部的部分是用于突出显示包含您输入的字符串的消息。您也可以在此部分添加更多的过滤器。

现在在新终端中使用以下命令启动 `turtlesim` ：

```bash
ros2 run turtlesim turtlesim_node
```

##### 2.rqt_console上的2条消息

为了生成供``rqt_console``显示的日志消息，让乌龟撞到墙上。在新的终端中输入``ros2 topic pub``命令，如下所示：
```bash
ros2 topic pub -r 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.0}}"
```

由于上述命令以固定速率发布主题，乌龟会持续撞墙。在``rqt_console``中，你会看到相同的消息以``Warn``严重级别一遍又一遍地显示，如下所示：
![[Pasted image 20251016120357.png]]

在你运行``ros2 topic pub``命令的终端中按下``Ctrl+C``，停止乌龟撞墙。


##### 3.日志记录器级别
ROS 2的记录器级别按严重程度排序：
```bash
Fatal
Error
Warn
Info
Debug
```

关于每个级别表示的确切标准并没有精确的规定，但可以安全地假设：

- `Fatal`（致命）消息表明系统将终止以试图保护自身免受损害。
    
- `Error`（错误）消息表示存在重要问题，这些问题不一定会损坏系统，但会阻止其正常运行。
    
- `Warn` 消息表示意外活动或非理想结果，可能表示更深层问题，但不会直接影响功能。
    
- `Info` 消息表示事件和状态更新，作为系统正常运行的可视验证。
    
- `Debug` 消息详细描述系统执行的每个步骤。
    

默认级别是 `Info`。只会看到默认严重级别和更严重级别的消息。

通常，只有``Debug``级别的消息被隐藏，因为它们是比``Info``级别更轻微的级别。例如，如果您将默认级别设置为``Warn``，您只会看到`Warn`、`Error`和`Fatal`严重程度的消息。

**3.1 设置默认日志记录器级别**
您可以在首次运行``/turtlesim``节点时使用重映射设置默认日志记录器级别。在终端中输入以下命令：

```bash
ros2 run turtlesim turtlesim_node --ros-args --log-level WARN
```

现在您将不会看到上次启动``turtlesim``时在控制台上出现的初始``Info``级别的消息。这是因为``Info``消息的优先级低于新的默认严重程度``Warn``。


##### 总结
如果您需要仔细检查系统的日志消息，`rqt_console`可能非常有帮助。您可能出于各种原因想要检查日志消息，通常是为了找出出错的地方以及导致该问题的事件序列。



#### 9.启动节点
##### 背景
在大多数入门教程中，您每次运行新节点都需要打开新的终端。随着您创建更复杂的系统，并同时运行越来越多的节点，打开终端并重新输入配置详细信息变得繁琐。

启动文件允许您同时启动和配置包含 ROS 2 节点的多个可执行文件。

使用``ros2 launch``命令运行单个启动文件将一次性启动整个系统 - 所有节点及其配置。


##### 运行launch文件
打开一个新的终端并运行：
```bash
ros2 launch turtlesim multisim.launch.py
```

以下命令将运行下面的 Launch 文件：
```bash
# turtlesim/launch/multisim.launch.py

from launch import LaunchDescription
import launch_ros.actions

def generate_launch_description():
    return LaunchDescription([
        launch_ros.actions.Node(
            namespace= "turtlesim1", package='turtlesim', executable='turtlesim_node', output='screen'),
        launch_ros.actions.Node(
            namespace= "turtlesim2", package='turtlesim', executable='turtlesim_node', output='screen'),
    ])
```

上面的启动文件是用Python编写的，但你也可以使用XML和YAML来创建启动文件。你可以在 [使用 Python、XML 和 YAML 编写 ROS 2 启动文件](http://fishros.org/doc/ros2/humble/How-To-Guides/Launch-file-different-formats.html) 中查看这些不同的ROS 2启动文件格式的比较。
![[Pasted image 20251016131405.png]]

暂时不用担心启动文件的内容。你可以在 [ROS 2启动教程](http://fishros.org/doc/ros2/humble/Tutorials/Intermediate/Launch/Launch-Main.html) 中找到更多关于ROS 2启动的信息。


##### 总结
迄今为止，您所做的事情的重要性在于您使用一个命令运行了两个turtlesim节点。一旦您学会编写自己的launch文件，您就能够以类似的方式使用``ros2 launch``命令运行多个节点，并设置它们的配置。

有关ROS 2 launch文件的更多教程，请参阅：[主要的launch文件教程页面](http://fishros.org/doc/ros2/humble/Tutorials/Intermediate/Launch/Launch-Main.html)。



#### 10.录制和回放数据

暂时忽略，还用不上




