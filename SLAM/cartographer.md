# 配置cartographer

##### 需要安装3个软件包，ceres solver、cartographer和cartographer_ros

**0.安装所有依赖项**

> sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev

安装结果(最后部分)

![](http://ww4.sinaimg.cn/large/0060lm7Tgw1f9l7wlvr5kj30gl03bdhm.jpg)

**1.首先安装ceres solver，选择的版本是1.11（路径随意）**

* `git clone https://github.com/hitcm/ceres-solver-1.11.0.git`

  ![](http://ww2.sinaimg.cn/large/0060lm7Tgw1f9l86qrg0sj30hc03h3zy.jpg)

* `cd ceres-solver-1.11.0/build`  &  `cmake ..`(截图只包含后面的操作)

  图片失效（未备份）

* `make –j`

  ![](http://i1.piimg.com/4851/92b5fc3ecb581606.png)

* `sudo make install`

   ![4](cartographer_截图\4.png)

**2.然后安装 cartographer（路径随意）**

* `git clone https://github.com/hitcm/cartographer.git`

   ![111](cartographer_截图\111.png)

* `cd cartographer/build` & `cmake .. -G Ninja`

   ![222](cartographer_截图\222.png)

* `ninja`

  图片失效（未备份）

* `ninja test`

  ![](http://i1.piimg.com/4851/eba7702a392ded6d.png)

  ……………………………………………………省略中间部分……………………………………………………

  ![](http://i1.piimg.com/4851/dcdacf9ec97a4a28.png)

* `sudo ninja install`

  ![](http://i1.piimg.com/4851/a103fb3ea2d34ad1.png)

**3.安装cartographer_ros**

`git clone https://github.com/hitcm/cartographer_ros.git`

下载到catkin_ws下面的src文件夹下面，然后到catkin_ws下面运行catkin_make即可。

catkin_ws的创建和初始化步骤：

```
# Install wstool and rosdep.
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build

# Create a new workspace in 'catkin_ws'.
mkdir catkin_ws
cd catkin_ws
wstool init src

# Merge the cartographer_ros.rosinstall file and fetch code for dependencies.
wstool merge -t src https://raw.githubusercontent.com/googlecartographer/cartographer_ros/master/cartographer_ros.rosinstall
wstool update -t src

# Install deb dependencies.
rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y

# Build and install.
catkin_make_isolated --install --use-ninja
source install_isolated/setup.bash
```

上述步骤`catkin_make_isolated --install --use-ninja`命令要执行成功，需要翻墙，也可通过换源解决：

* 打开下面这个文件，修改git下载的地址

  > build_isolated/ceres_solver/install/ceres_src-prefix/tmp/ceres_src-gitclone.cmake

* 黑体部分修改成：
  ```
  while(error_code AND number_of_tries LESS 3)
  execute_process(
  COMMAND "/usr/bin/git" clone "https://github.com/ceres-solver/ceres-solver" "ceres_src"
  WORKING_DIRECTORY
  ```
* 修改好后从新运行该步骤命令，成功执行后结果如下图

   ![](http://yotuku.cn/link?url=Ey3K82RlM&tk_plan=free&tk_storage=tietuku&tk_vuid=a577832b-efdb-4ad2-8b19-e290d50f6e07&tk_time=2016111121)


**4.数据下载测试（运行google提供的demo，显示google采集到的一个房间的地图）**

*# Download the 2D backpack example bag.*
  ```
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag
  ```

*# Launch the 2D backpack demo.*

```
roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
```

运行结果：

   ![](http://yotuku.cn/link?url=NkGLL2Rxf&tk_plan=free&tk_storage=tietuku&tk_vuid=a577832b-efdb-4ad2-8b19-e290d50f6e07&tk_time=2016111121)

*#Download the 3D backpack example bag.*

```
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_3d/cartographer_3d_deutsches_museum.bag
```

*# Launch the 3D backpack demo.*

```
roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=${HOME}/Downloads/cartographer_3d_deutsches_museum.bag
```
__*由于系统内存分配不足，导致无法将该测试数据下载完整再进行测试。__
