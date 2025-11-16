**硬件设备**：鲁班猫4（RK3588）、USB to TTL（串口模块）、5V4A电源
**软件工具**：MobaXterm
**参考链接：
[Ubuntu 22.04安装ROS2全攻略（手把手避坑版）_ubuntu22.04安装ros2-CSDN博客](https://blog.csdn.net/INosdfgs/article/details/147634901)
[1. ROS安装 — 快速使用手册—基于LubanCat-H618系列板卡 文档](https://doc.embedfire.com/linux/h618/quick_start/zh/latest/lubancat_allwinner_software_hardware/software/ros/ros.html)
系统版本：Ubuntu 22.04

---
# 开机启动板卡
1. 将板卡的调试串口与串口模块使用杜邦线相接
![[Pasted image 20251016111411.png]]
```
#线对线

板子 ----- 串口线
GND ------ GND
TXD ------ RXD
RXD ------ TXD
```
2. 串口模块插上电脑，打开`MobaXterm`软件，点击*Seesion*，在弹出的Session settings窗口选择Serial。
   随后选择正确端口号（可通过电脑的设备管理器查看），设置波特率为1500000，关闭流控，具体流程如图：
![[Pasted image 20251016164725.png]]
3. 板卡Type-C接口接上5V4A电源，绿灯闪烁，则正常开机
![[Pasted image 20251016165553.png]]
4. 显示以下界面，等待启动完成，显示*lubancat login*：时则可进行串口登录：
![[Pasted image 20251016165901.png]]
5. 先输入用户名（区分大小写）后按下回车键，然后再输入密码(输入密码是不会有文字提示的，所以需要注意中文输入法和大写)后按下回车键
```
用户  --- 用户名 -- 密码
超级用户 - root -- root
普通用户 - cat  -- temppwd
```
   如果用户名和密码都正确就会进入终端
![[Pasted image 20251016170656.png]]

---
# 准备工作
1. 检查wifi连接
```
ping baidu.com    # 有回复则有网络，反之没有
ifconfig    # 检查是否有ip没有则没连接网络
```
2. 连接WiFi
```
nmcli device wifi list    # 检查可用的WiFi列表
sudo nmcli device wifi connect "你的WiFi名" password "你的密码"    # 连接WiFi
```
3. 重复第一步检查网络情况
4. 确认系统版本`lsb_release -a`（本教程使用Ubuntu 22.04）
5. ~~换源操作（提升下载速度）:~~
```
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
```
# 正式安装
==使用鱼香ROS的一键安装脚本：==
```
wget http://fishros.com/install -O fishros && . fishros
```
安装过程会有中文提示，根据自身需求选择进行安装



1. ~~添加ROS2仓库:~~
```
sudo apt update && sudo apt install curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
2. ~~安装核心组件~~
```
sudo apt update
sudo apt install ros-humble-desktop
```

---
# ~~安装过程问题~~
~~1.无法获得锁 /var/lib/dpkg/lock-frontend...~~
~~![[Pasted image 20251016190555.png]]~~
~~解决方法：[【Linux】解决缓存锁问题：无法获得锁 /var/lib/dpkg/lock-frontend_正在等待缓存锁:无法获得锁-CSDN博客](https://blog.csdn.net/naihe_fish/article/details/134084444)~~

~~---~~
2. ~~下列软件包有未满足的依赖关系...~~
~~![[Pasted image 20251016195903.png]]~~
~~问题分析:~~
~~ROS 需要官方的通用库，但系统安装了厂商（Rockchip (瑞芯微)）的专用库，两者版本不兼容，导致软件包依赖关系冲突~~
~~要解决这个问题，您需要将系统中的 `ffmpeg` 相关库从 Rockchip 的专用版本切换回 Ubuntu 的官方版本。这可能会让您失去一些硬件加速功能，但这是安装 ROS 的必要步骤。~~
> [!NOTE] ~~解决方法：~~
> ~~1. 列出系统中的软件源文件：`ls /etc/apt/sources.list.d/`~~
> 	~~找到看到一个名字里可能包含 `rockchip`、`rk` 或者类似字样的 `.list` 文件。~~
> 	~~![[Pasted image 20251016201138.png]]~~
> ~~2. 禁用该软件源（加上了 `.bak` 后缀使其失效）：~~
> ~~```~~
> ~~sudo mv /etc/apt/sources.list.d/liujianfeng1994-ubuntu-rockchip-multimedia-jammy.list /etc/apt/sources.list.d/liujianfeng1994-ubuntu-rockchip-multimedia-jammy.list.bak~~
> ~~```~~
> ~~3. 再次尝试安装~~
> ~~```~~
> ~~sudo apt update~~
> ~~sudo apt install ros-humble-desktop-full~~
> ~~```~~

