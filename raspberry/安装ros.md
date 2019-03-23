
## 树莓派zero w安装ros
1.安装raspbian系统（增加交换分区大小）
2.安装ros官方在树莓派上编译安装的方法安装ros
3.首先给系统换源（用的中科大的）
4.换ros的源
将指令：
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
改为：
```
sudo sh -c 'echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
或之后直接修改`ros-latest.list`文件
5.获取核心包编译时，选择**Desktop**，防止缺少包。指令：
```
$ rosinstall_generator desktop --rosdistro kinetic --deps --wet-only --tar > kinetic-desktop-wet.rosinstall
$ wstool init src kinetic-desktop-wet.rosinstall
```
6.编译时因为zero内存小，采用`-j1`（`-j2`会出内存溢出错）。指令：
```
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/kinetic -j1
```
7.编译中在编译opencv时出现`opencv_contrib/xfeatures2d/src/boostdesc.cpp:673:20: fatal error: boostdesc_bgm.i: No such file or directory`。
解决办法：来到cpp文件所在目录，在其上级目录中cmake目录中有说明下载什么文件，CMakeList文件显示`.i`文件下载到二进制目录下面，而ros的编译好的文件在catkin_ws的build下，所以去`ros_catkin_ws/build_isolated/opencv3/install/downloads/xfeatures2d`会发现缺失的`.i`文件。然后全部复制到cpp文件所在的目录，即`~/ros_catkin_ws/src/opencv3/opencv_contrib/xfeatures2d/src`。
注：因为网络原因，会导致`.i`不全，单独下载缺少的文件。指令：
```
curl https://raw.githubusercontent.com/opencv/opencv_3rdparty/fccf7cd6a4b12079f73bbfb21745f9babcd4eb1d/vgg_generated_120.i > vgg_generated_120.i
```
（上面指令可以参考cmake修改）
8.出现找不到cuda.h，仍是因为下载文件不全，目前方法在opencv3的cmake文件夹中取消cuda选项（因为反正不用cuda）。
9.经历漫长编译安装后，配置完路径。使用rospack find查看sensor_msgs，cv_bridge都存在。

## 树莓派3B+安装ros
1.目前Ubuntu mate对3B+兼容问题，因此使用raspbian系统编译安装ros，但选择编译的时recomm的基础版导致却一些包。
2.当时的方法是安装的ros官方给的安装好ros的Ubuntu系统，造成mxnet也要重新编译。
3.现在可以同树莓派zero w一样，编译Desktop版。
