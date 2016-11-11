# 配置ROS

1. 配置Ubuntu软件仓库

   配置Ubuntu软件仓库（repositories）以允许 "restricted"、"universe" 和 "multiverse"这三种安装模式。 

   [配置指南](https://help.ubuntu.com/community/Repositories/Ubuntu)

2. 添加source.list

   配置电脑使其能够安装来自packages.ros.org的软件包。__ROS Jade **仅** 支持Trusty (14.04)、Utopic (14.10) 和 Vivid (15.04)。__

   ```
   sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
   ```

3. 添加keys

   ```
   sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net --recv-key 0xB01FA116
   ```

   执行结果（先前已经添加过）

   ![pic1](http://p1.bpimg.com/567571/ac3dccd7c8f30d10.png)

4. 安装

   首先，确保Debian软件包索引是最新的：

   ```
   sudo apt-get update
   ```

   ![](http://i1.piimg.com/567571/f285249d86822940.png)

   ROS中有很多各种函数库和工具，这里使用桌面完整版安装：（推荐）包含ROS、[rqt](http://wiki.ros.org/rqt)、[rviz](http://wiki.ros.org/rviz)、通用机器人函数库、2D/3D仿真器、导航以及2D/3D感知功能。

   ```
   sudo apt-get install ros-jade-desktop-full
   ```
   执行结果（先前已经安装过）

   ![](http://i1.piimg.com/567571/a237073c0d8d9cf0.png)

5. 初始化rosdep

   在开始使用ROS之前还需要初始化rosdep。rosdep可以在需要编译某些源码的时候为其安装一些系统依赖，同时也是某些ROS核心功能组件所必需用到的工具。

   ```
   sudo rosdep init
   rosdep update
   ```

6. 环境配置

   如果每次打开一个新的终端时ROS环境变量都能够自动配置好（即添加到bash会话中），那将会方便很多：

   ```
   echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```

   如果安装有多个ROS版本, ~/.bashrc 必须只能 source 你当前使用版本所对应的 setup.bash。

   如果只想改变当前终端下的环境变量，可以执行以下命令：

   ```
   source /opt/ros/jade/setup.bash
   ```

7. 安装rosinstall

   [rosinstall](http://wiki.ros.org/rosinstall) 是ROS中一个独立分开的常用命令行工具，它可以使你通过一条命令就可以给某个ROS软件包下载很多源码树。

   要在ubuntu上安装这个工具，需运行：

   ```
   sudo apt-get install python-rosinstall
   ```

   执行结果（先前已经安装过）

   ![pic2](http://p1.bpimg.com/567571/f4e7418e64aa0f81.png)

8. Build farm状态

   所安装的各种软件包都是通过[ROS build farm](http://jenkins.ros.org/)来编译构建的。可以在[这里](http://www.ros.org/debbuild/jade.html)查看每个软件包的编译状态。

9. 测试

   现在要想测试安装是否成功请进入[ROS 教程](http://wiki.ros.org/cn/ROS/Tutorials)。

   ​

*关于如何查看Ubuntu版本:`cat /etc/issue`

![version](http://p1.bpimg.com/567571/7a19ff53909d3a0a.png)