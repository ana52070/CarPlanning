学习链接：[文档预览 - Gitee.com](https://gitee.com/gwmunan/ros2/wikis/pages?sort_id=9601241&doc_id=4855084)[http://fishros.org/doc/nav2](http://fishros.org/doc/nav2)

[http://fishros.org/doc/ros2/humble/](http://fishros.org/doc/ros2/humble/)
## 速通ROS2

### 初学者：客户端库

#### 1.使用colcon 构建软件包

##### 1.背景

`colcon`是对ROS构建工具`catkin_make`、`catkin_make_isolated`、`catkin_tools`和`ament_tools`的改进。有关colcon设计的更多信息，请参阅此文档.


##### 2.安装colcon

```bash
sudo apt install python3-colcon-common-extensions
```

##### 3.基础知识
ROS工作空间是一个具有特定结构的目录。通常有一个 `src` 子目录。在该子目录下是ROS软件包的源代码所在位置。通常该目录开始时为空。

colcon进行外部构建。默认情况下，它会创建以下目录作为 `src` 目录的同级目录：

- `build` 目录将用于存储中间文件。对于每个软件包，将在其中创建一个子文件夹，例如在该子文件夹中调用 CMake。
    
- `install` 目录是每个软件包将要安装到的位置。默认情况下，每个软件包将被安装到单独的子目录中。
    
- `log` 目录包含有关每个 colcon 调用的各种日志信息。
    

注解

与 catkin 相比，没有 `devel` 目录。
catkin是ROS1中的编译工具。


#####  4.创建一个工作空间

首先，创建一个目录（`ros2_ws`）来容纳我们的工作空间：


```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```

此时，工作空间中只包含一个空目录 `src`：

```bash
.
└── src

1 directory, 0 files
```


##### 5.添加一些源文件

将 [examples](https://github.com/ros2/examples) 存储库克隆到工作区的 `src` 目录中：

```bash
git clone https://github.com/ros2/examples src/examples -b humble
```

现在工作区应该有 ROS 2 示例的源代码：
```bash
.
└── src
    └── examples
        ├── CONTRIBUTING.md
        ├── LICENSE
        ├── rclcpp
        ├── rclpy
        └── README.md

4 directories, 3 files
```

##### 6.源目录

很重要的是，我们已经为现有的 ROS 2 安装环境提供了所需的构建依赖项，以便为示例软件包提供支持。这可以通过对二进制安装或源码安装提供的设置脚本进行实现，即使用另一个 colcon 工作区提供的设置脚本（参见 [Installation](http://fishros.org/doc/ros2/humble/Installation.html)）。我们称这个环境为 **源目录**。

我们的工作空间``ros2_ws``将作为现有ROS 2安装的**覆盖层**。一般而言，建议在计划迭代少量软件包时使用覆盖层，而不是将所有软件包放入同一个工作空间中。


##### 7.构建工作空间

**注意**
要在Windows上构建软件包，您需要处于Visual Studio环境中，请参阅 [构建ROS 2代码](http://fishros.org/doc/ros2/humble/Installation/Alternatives/Windows-Development-Setup.html#windows-dev-build-ros2) 获取更多详细信息。



在工作空间的根目录下运行``colcon build``命令。由于``ament_cmake``等构建类型不支持``devel``空间的概念，而且需要安装软件包，因此colcon支持选项``--symlink-install``。这允许通过更改``source``空间中的文件（例如Python文件或其他非编译资源）来更改已安装文件，以便实现更快的迭代。

```bash
colcon build --symlink-install
```

在构建完成后，我们应该能够看到 `build`、`install` 和 `log` 目录：
```bash
.
├── build
├── install
├── log
└── src

4 directories, 0 files
```


##### 8.运行测试

为了对刚刚构建的软件包运行测试，请执行以下操作：


```bash
colcon test
```

##### 9.加载环境变量

当colcon成功构建完成后，输出将位于``install``目录中。在您可以使用任何已安装的可执行文件或库之前，您需要将它们添加到您的路径和库路径中。colcon会在``install``目录中生成bash/bat文件来帮助设置环境。这些文件将会将所有所需的元素添加到您的路径和库路径中，并提供由软件包导出的任何bash或shell命令。

```bash
source install/setup.bash
```

##### 10.试用演示
在环境配置完成后，我们可以运行由colcon构建的可执行文件。让我们从示例中运行一个订阅者节点：

```bash
ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
```

在另一个终端中，让我们运行一个发布者节点（不要忘记配置脚本）：

```bash
ros2 run examples_rclcpp_minimal_publisher publisher_member_function
```

你应该能够看到发布者和订阅者的消息，并且消息编号会递增。



##### 11.创建你自己的包

```
colcon使用在`REP 149 <[https://www.ros.org/reps/rep-0149.html](https://www.ros.org/reps/rep-0149.html)>`__中定义的``package.xml``规范（也支持`format 2 <[https://www.ros.org/reps/rep-0140.html](https://www.ros.org/reps/rep-0140.html)>`__）。

colcon支持多种构建类型。推荐的构建类型是``ament_cmake``和``ament_python``。还支持纯``cmake``包。

[``](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html#id1)ament_python``构建的一个示例是`ament_index_python包 <[https://github.com/ament/ament_index/tree/humble/ament_index_python](https://github.com/ament/ament_index/tree/humble/ament_index_python)>`__，其中setup.py是构建的主要入口点。

诸如`demo_nodes_cpp <[https://github.com/ros2/demos/tree/humble/demo_nodes_cpp](https://github.com/ros2/demos/tree/humble/demo_nodes_cpp)>`__的包使用``ament_cmake``构建类型，并使用CMake作为构建工具。
```

为了方便起见，您可以使用``ros2 pkg create``工具基于模板创建一个新的包。

##### 12.设置 `colcon_cd


命令 `colcon_cd` 允许您快速将当前工作目录更改为包的目录。例如，`colcon_cd some_ros_package` 可快速将您导航到目录 `~/ros2_ws/src/some_ros_package`。

```bash
echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
echo "export _colcon_cd_root=/opt/ros/humble/" >> ~/.bashrc
```



根据您安装 `colcon_cd` 的方式和工作空间的位置，上述说明可能有所不同，请参考 [文档](https://colcon.readthedocs.io/en/released/user/installation.html#quick-directory-changes) 以获取更多详细信息。要在Linux和macOS上撤消此操作，请找到系统的shell启动脚本并删除附加的source和export命令。


##### 13.设置 `colcon` 的选项自动补全

如果已安装 `colcon-argcomplete` 包，则命令 `colcon` [支持命令补全](https://colcon.readthedocs.io/en/released/user/installation.html#enable-completion)，适用于 bash 和类似 bash 的 shell。
```bash
echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc
```

根据您安装 `colcon` 的方式和工作空间的位置，上述说明可能会有所不同，请参考 [文档](https://colcon.readthedocs.io/en/released/user/installation.html) 获取更多详细信息。要在 Linux 和 macOS 上撤消此操作，请找到您系统的 shell 启动脚本并删除附加的源命令。


##### 14.提示

- 如果您不想构建特定的软件包，请在目录中放置一个名为“COLCON_IGNORE”的空文件，它将不会被索引。
    
- 如果您想避免在CMake软件包中配置和构建测试，您可以传递参数：`--cmake-args -DBUILD_TESTING=0`。
    
- 如果您想从软件包中运行单个特定测试：
```bash
colcon test --packages-select YOUR_PKG_NAME --ctest-args -R YOUR_TEST_IN_PKG
```



#### 2.创建一个工作空间
##### 1.背景

工作空间是包含ROS 2软件包的目录。在使用ROS 2之前，需要在您计划使用的终端中使用ROS 2安装工作空间。这样可以使ROS 2的软件包在该终端中可用于使用。

您还可以选择使用"叠加层" - 一个次要的工作空间，您可以在其中添加新的软件包，而不会干扰您正在扩展或"下层"的现有ROS 2工作空间。您的下层必须包含叠加层中所有软件包的依赖关系。叠加层中的软件包将覆盖下层中的软件包。还可以有多个层次的下层和叠加层，每个连续的叠加层都使用其父级下层的软件包。


##### 2. 配置ROS 2环境

你的主要ROS 2安装将成为本教程的基础。 （请注意，基础不一定非得是主要的ROS 2安装。）

根据您安装ROS 2的方式（源码安装或二进制安装），以及您所在的平台，您的确切源命令会有所不同：
```bash
source /opt/ros/humble/setup.bash
```

如果这些命令对您不起作用，请参考您所遵循的:doc:[安装指南](http://fishros.org/doc/ros2/humble/Installation.html)。



##### 3. 创建一个新目录

最佳实践是为每个新的工作空间创建一个新的目录。目录名称无关紧要，但是有助于表明工作空间的目的。让我们选择目录名为``ros2_ws``，表示"开发工作空间"：

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```
另一个最佳实践是将工作空间中的任何包放置在``src``目录中。上述代码在``ros2_ws``内创建一个``src``目录，然后进入其中。

##### 4.克隆示例仓库

在克隆之前，请确保您仍在``ros2_ws/src``目录中。

在接下来的初学者开发者教程中，您将创建自己的软件包，但目前您将练习使用现有软件包组合一个工作空间。

如果您已经完成了[[ROS2学习日记1-CLI工具]]教程，您将熟悉 `turtlesim`，它是 [ros_tutorials](https://github.com/ros/ros_tutorials/) 中的一个软件包。

一个存储库可以有多个分支。您需要检出与您已安装的 ROS 2 发行版对应的分支。当您克隆这个存储库时，请添加 `-b` 参数，后跟该分支的名称。

在 `ros2_ws/src` 目录下运行以下命令：

```bash
git clone https://github.com/ros/ros_tutorials.git -b humble
```
现在在您的工作空间中克隆了 `ros_tutorials` 。`ros_tutorials` 仓库包含了 `turtlesim` 包，我们将在本教程的剩余部分中使用它。此仓库中的其他包没有构建，因为它们包含一个 `COLCON_IGNORE` 文件。

到目前为止，您已经在工作空间中添加了一个示例包，但它还不是一个完全功能的工作空间。您需要先解决依赖关系，然后再构建工作空间。


##### 5.解决依赖关系

在构建工作空间之前，您需要解决包的依赖关系。您可能已经拥有了所有的依赖项，但最佳实践是每次克隆时都检查依赖关系。您不希望在长时间等待后构建失败，然后才意识到您缺少依赖项。

从工作空间的根目录(`ros2_ws`)运行以下命令：

```bash
# cd if you're still in the ``src`` directory with the ``ros_tutorials`` clone
cd ..
rosdep install -i --from-path src --rosdistro humble -y
```

如果您从源代码或"fat"存档中在Linux上安装了ROS 2，则需要按照它们的安装说明使用rosdep命令。这里有 [从源代码安装的rosdep部分](http://fishros.org/doc/ros2/humble/Installation/Alternatives/Ubuntu-Development-Setup.html#linux-development-setup-install-dependencies-using-rosdep) 和 ["fat"存档的rosdep部分](http://fishros.org/doc/ros2/humble/Installation/Alternatives/Ubuntu-Install-Binary.html#linux-install-binary-install-missing-dependencies)。

如果您已经具备了所有的依赖项，控制台会返回：
```bash
#All required rosdeps installed successfully
```

软件包在package.xml文件中声明其依赖关系（您将在下一个教程中了解更多关于软件包的内容）。这个命令会遍历这些声明并安装缺失的依赖项。您可以在另一个教程（即将推出）中了解更多关于``rosdep``的信息。


##### 6.使用colcon构建工作空间

从工作空间的根目录（`ros2_ws`）开始，您可以使用以下命令构建您的软件包：
```bash
colcon build
```
控制台将返回以下消息：

```bash
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

``colcon build``的其他有用参数：

- `--packages-up-to` 构建所需的包及其所有依赖项，而不是整个工作空间（节省时间）
    
- `--symlink-install` 在您调整Python脚本时，无需每次重新构建
    
- `--event-handlers console_direct+` 在构建时显示控制台输出（否则可在``log``目录中找到）

构建完成后，在工作空间根目录（`~/ros2_ws`）中输入以下命令：

```bash
ls
```

您将看到 colcon 创建了新目录：

```bash
build  install  log  src
```

``install``目录是你的工作空间的设置文件所在的位置，你可以使用它来设置你的叠加层。


##### 7.设置叠加层

在设置叠加层之前，非常重要的一点是要打开一个新的终端，与构建工作空间的终端分开。在同一个终端中设置叠加层或在设置叠加层的终端中进行构建可能会导致复杂的问题。

在新的终端中，将你的主要ROS 2环境作为“underlay”来源化，这样你就可以在其“之上”构建叠加层：
```bash
source /opt/ros/humble/setup.bash
```

进入你的工作空间的根目录：

```bash
cd ~/ros2_ws
```

在根目录中，将你的叠加层源化：

```bash
source install/local_setup.bash
```

注解
只有在源中添加覆盖层的``local_setup``才会将覆盖层中的软件包添加到您的环境中。``setup``源同时包含覆盖层和创建它的底层，使您能够同时利用两个工作区。

因此，像您刚才做的那样，先对主要的ROS 2安装进行``setup``，然后再对``ros2_ws``覆盖层的``local_setup``进行源操作，与只对``ros2_ws``的``setup``进行源操作是相同的，因为后者已经包含了底层的环境。


现在，您可以从覆盖层中运行``turtlesim``软件包：

```bash
ros2 run turtlesim turtlesim_node
```

但是，您如何确定这是运行的覆盖层中的turtlesim，而不是主安装中的turtlesim呢？

让我们在覆盖层中修改turtlesim，这样你就可以看到效果了：

- 你可以在覆盖层中单独修改和重建软件包，而不影响底层。
    
- 覆盖层优先于底层。

##### 8.修改覆盖层

你可以通过编辑turtlesim窗口的标题栏来修改overlay中的`turtlesim`。要做到这一点，找到`~/ros2_ws/src/ros_tutorials/turtlesim/src`中的`turtle_frame.cpp`文件。用你喜欢的文本编辑器打开`turtle_frame.cpp`。

在第52行，你会看到函数`setWindowTitle("TurtleSim");`。将值`"TurtleSim"`更改为`"MyTurtleSim"`，然后保存文件。

返回到之前运行`colcon build`的第一个终端，并再次运行它。

返回到第二个终端（源化了overlay的终端），然后再次运行turtlesim:

```bash
ros2 run turtlesim turtlesim_node
```

![[Pasted image 20251019194404.png]]

尽管您之前在此终端中激活了主要的ROS 2环境，但是"ros2_ws"环境的叠加优先于底层的内容。

为了验证您的底层仍然完整，打开一个全新的终端并只激活您的ROS 2安装。再次运行turtlesim：

```bash
ros2 run turtlesim turtlesim_node
```

![[Pasted image 20251019194427.png]]

您可以看到叠加中的修改实际上没有对底层产生任何影响。

##### 9.总结

在本教程中，您将ROS 2主要发行版安装为底层，并通过克隆和构建包在新的工作空间中创建一个覆盖层。覆盖层被添加到路径中，并且优先于底层，正如您通过修改后的turtlesim所看到的。

建议在处理少量包时使用覆盖层，这样您就不必将所有内容放在同一个工作空间中，并且不必在每次迭代中重新构建一个庞大的工作空间。



#### 3.创建软件包
#### 1.什么是ROS 2包？

包是ROS 2代码的组织单元。如果您希望能够安装代码或与他人共享代码，那么您需要将其组织成一个包。通过包，您可以发布ROS 2工作并允许其他人轻松构建和使用它。

ROS 2中的包创建使用ament作为构建系统和colcon作为构建工具。您可以使用CMake或Python创建一个包，这两种方式得到官方支持，尽管还存在其他构建类型。



##### 2.什么构成了一个ROS 2软件包?

ROS 2的Python和CMake软件包各自有其最小要求的内容:

CMake

- ``CMakeLists.txt``文件，描述了如何构建软件包内的代码
    
- 包含该包的公共头文件的 `include/<package_name>` 目录
    
- 包含有关该包的元信息的 `package.xml` 文件
    
- 包含该包源代码的 `src` 目录


最简单的软件包的文件结构可能如下所示：

CMake
```bash
my_package/
     CMakeLists.txt
     include/my_package/
     package.xml
     src/
```


Python

- 包含有关该包的元信息的 `package.xml` 文件
    
- 用于标记该包的 `resource/<package_name>` 文件
    
- 当一个软件包包含可执行文件时，需要使用``setup.cfg``，以便``ros2 run``能够找到它们
    
- ``setup.py``包含了安装该软件包的指令
    
- `<package_name>` - 与软件包同名的目录，被ROS 2工具用于查找软件包，包含``__init__.py``

最简单的软件包的文件结构可能如下所示：

Python
```bash
my_package/
      package.xml
      resource/my_package
      setup.cfg
      setup.py
      my_package/
```



##### 3.工作空间中的3个软件包

一个工作空间可以包含任意多个软件包，每个软件包位于自己的文件夹中。您还可以在一个工作空间中拥有不同构建类型的软件包（如CMake、Python等）。但不能嵌套软件包。

最佳实践是在工作空间中创建一个``src``文件夹，并在其中创建您的软件包。这样可以保持工作空间的顶层“清洁”。

一个简单的工作空间可能如下所示：
```bash
workspace_folder/
    src/
      cpp_package_1/
          CMakeLists.txt
          include/cpp_package_1/
          package.xml
          src/

      py_package_1/
          package.xml
          resource/py_package_1
          setup.cfg
          setup.py
          py_package_1/
      ...
      cpp_package_n/
          CMakeLists.txt
          include/cpp_package_n/
          package.xml
          src/
```

##### 4.创建一个包

首先， [配置您的 ROS 2 安装环境](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)。

让我们使用您在 [先前教程](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html#new-directory) 中创建的工作空间 `ros2_ws` 来创建您的新软件包。

在运行软件包创建命令之前，请确保您位于 `src` 文件夹中。

```bash
cd ~/ros2_ws/src
```

在ROS 2中创建新包的命令语法如下：
```bash
# python
ros2 pkg create --build-type ament_python <package_name>


# Cmake
ros2 pkg create --build-type ament_cmake <package_name>
```


在本教程中，您将使用可选参数``--node-name``，它将在包中创建一个简单的Hello World类型的可执行文件。

在终端中输入以下命令：
```bash
# python
ros2 pkg create --build-type ament_python --node-name my_node my_package

#Cmake
ros2 pkg create --build-type ament_cmake --node-name my_node my_package
```

现在，您的工作空间的``src``目录中将有一个名为``my_package``的新文件夹。

运行该命令后，您的终端会返回以下消息：
```bash
# python
going to create a new package
package name: my_package
destination directory: /home/user/ros2_ws/src
package format: 3
version: 0.0.0
description: TODO: Package description
maintainer: ['<name> <email>']
licenses: ['TODO: License declaration']
build type: ament_python
dependencies: []
node_name: my_node
creating folder ./my_package
creating ./my_package/package.xml
creating source folder
creating folder ./my_package/my_package
creating ./my_package/setup.py
creating ./my_package/setup.cfg
creating folder ./my_package/resource
creating ./my_package/resource/my_package
creating ./my_package/my_package/__init__.py
creating folder ./my_package/test
creating ./my_package/test/test_copyright.py
creating ./my_package/test/test_flake8.py
creating ./my_package/test/test_pep257.py
creating ./my_package/my_package/my_node.py



# Cmake
going to create a new package
package name: my_package
destination directory: /home/user/ros2_ws/src
package format: 3
version: 0.0.0
description: TODO: Package description
maintainer: ['<name> <email>']
licenses: ['TODO: License declaration']
build type: ament_cmake
dependencies: []
node_name: my_node
creating folder ./my_package
creating ./my_package/package.xml
creating source and include folder
creating folder ./my_package/src
creating folder ./my_package/include/my_package
creating ./my_package/CMakeLists.txt
creating ./my_package/src/my_node.cpp

```
您可以看到为新软件包自动生成的文件。

##### 5.构建软件包

将软件包放入工作区尤其有价值，因为您可以通过在工作区根目录下运行``colcon build``一次性构建多个软件包。否则，您将需要逐个构建每个软件包。

返回工作区的根目录：
```bash
cd ~/ros2_ws
```


现在你可以构建你的软件包：
```bash
colcon build
```

从上个教程中你记得你的``ros2_ws``里也有``ros_tutorials``软件包。你可能已经注意到运行``colcon build``时还会构建``turtlesim``软件包。当你的工作区只有几个软件包时这没问题，但当有很多软件包时，``colcon build``会花费很长时间。

下次只构建``my_package``软件包，你可以运行：
```bash
colcon build --packages-select my_package
```

##### 6.导入设置文件

要使用您的新软件包和可执行文件，请首先打开一个新终端并导入您的主要ROS 2安装。

然后，在``ros2_ws``目录内运行以下命令以导入您的工作空间：
```bash
source install/local_setup.bash
```

现在，您的工作空间已添加到路径中，您将能够使用您的新软件包的可执行文件。

##### 7.使用软件包

要运行您使用``--node-name``参数在创建软件包时创建的可执行文件，请输入以下命令：

```bash
ros2 run my_package my_node
```
将在终端返回一条消息：

```bash
# python
Hi from my_package.




# Cmake
hello world my_package package

```

##### 8.检查软件包内容

在 `ros2_ws/src/my_package` 内，您将看到 `ros2 pkg create` 自动创建的文件和文件夹：

```bash
# Python
my_package  package.xml  resource  setup.cfg  setup.py  test

`my_node.py` 位于 `my_package` 目录中。这是您以后将放置所有自定义 Python 节点的位置。


# Cmake
CMakeLists.txt  include  package.xml  src
`my_node.cpp` 位于 `src` 目录中。这是您以后将放置所有自定义 C++ 节点的位置。

```


##### 9.自定义 package.xml

略



##### 10.总结

你创建了一个用于组织代码并方便他人使用的包。

您的软件包已自动填充所需文件，然后您使用 colcon 构建它，以便在本地环境中使用其可执行文件。



#### 编写一个简单的发布者和订阅者（C++）


##### 1.背景

[节点](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html) 是在 ROS 图中进行通信的可执行进程。在本教程中，节点将以字符串消息的形式相互传递信息，通过一个 [主题](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html)。这里使用的示例是一个简单的 "talker" 和 "listener" 系统；一个节点发布数据，另一个节点订阅该主题以接收数据。

这些示例中使用的代码可以在 [这里](https://github.com/ros2/examples/tree/humble/rclcpp/topics) 找到。


##### 2.创建一个包

在一个新的终端中 [初始化你的 ROS 2 安装](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)，这样 `ros2` 命令才能正常工作。

进入在 [上一个教程](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html#new-directory) 中创建的 `ros2_ws` 目录。

记住，包应该创建在 `src` 目录中，而不是工作空间的根目录。所以，进入 `ros2_ws/src` 目录，并运行包创建命令：

```bash
ros2 pkg create --build-type ament_cmake cpp_pubsub
```

终端会返回一条消息，确认已创建名为 `cpp_pubsub` 的包及其所有必要的文件和文件夹。

进入 `ros2_ws/src/cpp_pubsub/src` 目录。请注意，这是任何 CMake 包中包含可执行文件的源文件所在的目录。

```bash
wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/humble/rclcpp/topics/minimal_publisher/member_function.cpp
```
现在会有一个名为``publisher_member_function.cpp``的新文件。使用您喜欢的文本编辑器打开该文件。
```Cpp
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;

/* This example creates a subclass of Node and uses std::bind() to register a
* member function as a callback from the timer. */

class MinimalPublisher : public rclcpp::Node
{
  public:
    MinimalPublisher()
    : Node("minimal_publisher"), count_(0)
    {
      publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
      timer_ = this->create_wall_timer(
      500ms, std::bind(&MinimalPublisher::timer_callback, this));
    }

  private:
    void timer_callback()
    {
      auto message = std_msgs::msg::String();
      message.data = "Hello, world! " + std::to_string(count_++);
      RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
      publisher_->publish(message);
    }
    rclcpp::TimerBase::SharedPtr timer_;
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
    size_t count_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```




**2.1 检查代码**

代码的开头包含了您将要使用的标准C++头文件。标准C++头文件之后是``rclcpp/rclcpp.hpp``的包含部分，它允许您使用ROS 2系统中最常见的组件。最后是``std_msgs/msg/string.hpp``，它包含了您将用于发布数据的内置消息类型。
```Cpp
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;
```

这些行表示节点的依赖关系。请记住，依赖关系必须添加到``package.xml``和``CMakeLists.txt``中，在下一节中您将进行这些操作。


下一行通过从``rclcpp::Node``继承来创建节点类``MinimalPublisher``。代码中的每个``this``都是指向该节点。
```Cpp
class MinimalPublisher : public rclcpp::Node
```

公共构造函数将节点命名为``minimal_publisher``，并将``count_``初始化为0。在构造函数内部，使用``String``消息类型、主题名称``topic``以及必要的队列大小来初始化发布者。接下来，初始化了``timer_``，它导致``timer_callback``函数每秒执行两次。
```Cpp
public:
  MinimalPublisher()
  : Node("minimal_publisher"), count_(0)
  {
    publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
    timer_ = this->create_wall_timer(
    500ms, std::bind(&MinimalPublisher::timer_callback, this));
  }
```
``timer_callback``函数是设置消息数据并实际发布消息的地方。``RCLCPP_INFO``宏确保每个发布的消息都会打印到控制台。
```cpp
private:
  void timer_callback()
  {
    auto message = std_msgs::msg::String();
    message.data = "Hello, world! " + std::to_string(count_++);
    RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
    publisher_->publish(message);
  }
```

最后是定时器、发布者和计数器字段的声明。
```cpp
rclcpp::TimerBase::SharedPtr timer_;
rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
size_t count_;
```

下面是``MinimalPublisher``类的定义，接着是``main``函数，其中节点实际执行。``rclcpp::init``用于初始化ROS 2，``rclcpp::spin``则开始处理来自节点的数据，包括定时器的回调函数。
```cpp
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```

**2.2 添加依赖项**

返回到``ros2_ws/src/cpp_pubsub``目录，这是``CMakeLists.txt``和``package.xml``文件所在的目录。这些文件已经为您创建好。

用文本编辑器打开``package.xml``文件。

如在:doc:``` 之前的教程 <./Creating-Your-First-ROS2-Package>`中提到的，确保填写``<description>` ```, ``<maintainer>``和``<license>``标签:
```xml
<description>Examples of minimal publisher/subscriber using rclcpp</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```
在``ament_cmake``构建工具依赖项之后添加一个新行，并粘贴以下与节点的包含语句对应的依赖项:
```xml
<depend>rclcpp</depend>
<depend>std_msgs</depend>
```

这将在构建和执行代码时声明该包需要``rclcpp``和``std_msgs``.

确保保存文件.

**2.3 CMakeLists.txt**

现在打开 `CMakeLists.txt` 文件。在现有的依赖项 `find_package(ament_cmake REQUIRED)` 下方，添加以下行：

```make
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```
然后，添加可执行文件并将其命名为 `talker`，这样你就可以使用 `ros2 run` 运行你的节点：
```make
add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)
```

最后，添加 `install(TARGETS...)` 部分，以便 `ros2 run` 可以找到你的可执行文件：
```make
install(TARGETS
  talker
  DESTINATION lib/${PROJECT_NAME})
```

你可以通过删除一些不必要的部分和注释来简化你的``CMakeLists.txt``，使其看起来像这样：
```make
cmake_minimum_required(VERSION 3.5)
project(cpp_pubsub)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

install(TARGETS
  talker
  DESTINATION lib/${PROJECT_NAME})

ament_package()
```

现在您可以构建您的软件包，加载本地设置文件并运行它，但是让我们首先创建订阅者节点，这样您就可以看到整个系统的运行情况。


##### 3.编写订阅者节点

返回到 `ros2_ws/src/cpp_pubsub/src` 目录，创建下一个节点。在终端中输入以下代码：


```bash
wget -O subscriber_member_function.cpp https://raw.githubusercontent.com/ros2/examples/humble/rclcpp/topics/minimal_subscriber/member_function.cpp
```

现在在控制台输入 `ls` 将会返回：

```bash
publisher_member_function.cpp  subscriber_member_function.cpp
```

使用文本编辑器打开 `subscriber_member_function.cpp`。
```cpp
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"
using std::placeholders::_1;

class MinimalSubscriber : public rclcpp::Node
{
  public:
    MinimalSubscriber()
    : Node("minimal_subscriber")
    {
      subscription_ = this->create_subscription<std_msgs::msg::String>(
      "topic", 10, std::bind(&MinimalSubscriber::topic_callback, this, _1));
    }

  private:
    void topic_callback(const std_msgs::msg::String & msg) const
    {
      RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg.data.c_str());
    }
    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalSubscriber>());
  rclcpp::shutdown();
  return 0;
}
```


**3.1 检查代码**

订阅者节点的代码与发布者几乎完全相同。现在节点被命名为 `minimal_subscriber`，构造函数使用节点的 `create_subscription` 类来执行回调函数。

没有定时器，因为订阅者只会在数据被发布到 `topic` 主题时做出响应。
```cpp
public:
  MinimalSubscriber()
  : Node("minimal_subscriber")
  {
    subscription_ = this->create_subscription<std_msgs::msg::String>(
    "topic", 10, std::bind(&MinimalSubscriber::topic_callback, this, _1));
  }
```

回顾一下 [主题教程](http://fishros.org/doc/ros2/humble/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html) 中提到，发布者和订阅者使用的主题名称和消息类型必须匹配，才能使它们进行通信。

`topic_callback` 函数接收通过主题发布的字符串消息数据，并使用 `RCLCPP_INFO` 宏将其简单地写入控制台。

这个类中仅有一个字段声明，即订阅。
```cpp
private:
  void topic_callback(const std_msgs::msg::String & msg) const
  {
    RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg.data.c_str());
  }
  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
```

`main` 函数完全相同，只是现在它旋转 `MinimalSubscriber` 节点。对于发布者节点，旋转意味着启动计时器，而对于订阅者节点，它只是准备在消息到来时接收消息。

由于该节点与发布者节点具有相同的依赖关系，因此在``package.xml``中没有新内容可添加。

**3.2 CMakeLists.txt**

重新打开``CMakeLists.txt``，在发布者的条目下面添加订阅者节点的可执行文件和目标。

```make
add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})
```

确保保存文件，然后您的发布/订阅系统就准备好了。


##### 4.构建和运行

很可能您已经在您的ROS 2系统中安装了``rclcpp``和``std_msgs``软件包。在构建之前，最好在您的工作空间根目录（`ros2_ws`）中运行``rosdep``来检查是否存在缺失的依赖项：

```bash
rosdep install -i --from-path src --rosdistro humble -y
```

仍然在您的工作空间根目录（`ros2_ws`）中，构建您的新软件包：


```bash
colcon build --packages-select cpp_pubsub
```

打开一个新的终端，导航到 `ros2_ws`，并加载设置文件：

```bash
. install/setup.bash
```

现在运行对话节点：

```bash
ros2 run cpp_pubsub talker
```

终端应该会每0.5秒发布一条信息消息，如下所示：

```bash
[INFO] [minimal_publisher]: Publishing: "Hello World: 0"
[INFO] [minimal_publisher]: Publishing: "Hello World: 1"
[INFO] [minimal_publisher]: Publishing: "Hello World: 2"
[INFO] [minimal_publisher]: Publishing: "Hello World: 3"
[INFO] [minimal_publisher]: Publishing: "Hello World: 4"
```

打开另一个终端，在``ros2_ws``内部再次source设置文件，然后启动监听节点：

```bash
ros2 run cpp_pubsub listener
```

监听器将从发布者当前的消息计数开始，将消息打印到控制台，就像这样：

```bash
[INFO] [minimal_subscriber]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber]: I heard: "Hello World: 14"
```

在每个终端中输入``Ctrl+C``来停止节点的旋转。

##### 5.总结

你创建了两个节点，用于在一个话题上发布和订阅数据。在编译和运行之前，你将它们的依赖项和可执行文件添加到了软件包配置文件中。