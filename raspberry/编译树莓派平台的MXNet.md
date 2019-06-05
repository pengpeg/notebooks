# 树莓派安装MXNet

## 环境介绍
1）Respberry Pi 3 Model B+ @RS </br>
2) MXNet 1.3.0 </br>

## 介绍
MXNet支持基于Debian的Rabbian ARM操作系统，因此你可以在树莓派设备上运行MXNet。 </br>
本文介绍怎样给树莓派编译MXNet，安装绑定Python的库。 </br>
两种方式编译：1）在本地机器做dockerized交叉编译；2）在树莓派上本地编译。 </br>
编译完的MXNet库需要占用200MB的RAM，加载大的模型可能占用超过1GB的RAM。因此我们推荐在树莓派3或至少1GB RAM、4GB存储的等价设备上运行MXNet。

## 交叉编译（实验）（Cross compilation build（Experimental））

## 本地编译（Native Build）
安装MXNet需要两步： </br>
1. 从MXNet C++源码构建共享库。 </br>
2. 为MXNet安装支持的专用语言包。 </br>

### 第一步：构建共享库
在树莓派的Raspbian系统上，你需要一下依赖： </br>
* Git（为了从Github上获取代码）
* libblas（为了线性代数操作）
* libopencv（为了计算机视觉操作。如果你想要节省RAM和存储空间，此项是可选的） </br>
* 一个支持C++ 11的C++编译器。此C++编译器负责编译和构建MXNet源代码。支持的编译器包括如下： </br>
  - G++（4.8或更近）。确保使用gcc4而不是5或6，因为这些有bugs存在 </br>
  - Clang（3.9-6） </br>
在任意目录下使用如下指令安装这些依赖： </br>
<code>

sudo apt-get update

sudo apt-get -y install git cmake ninja-build build-essential g++-4.9 c++-4.9 liblapack* libblas* libopencv* libopenblas* python3-dev virtualenv

</code>
克隆MXNet源代码： </br>
<code>
git clone https://github.com/apache/incubator-mxnet.git --recursive

git clone --recursive https://github.com/apache/incubator-mxnet.git

cd incubator-mxnet

</code>

构建：
<code>

mkdir -p build && cd build

cmake -DUSE_SSE=OFF -DUSE_CUDA=OFF -DUSE_OPENCV=ON -DUSE_OPENMP=ON -DUSE_MKL_IF_AVAILABLE=OFF -DUSE_SIGNAL_HANDLER=ON -DCMAKE_BUILD_TYPE=Release -GNinja ..

ninja -j$(nproc)

</code>

某些编译单元要求内存接近1GB，因此推荐如下设置交换分区并谨慎增加构建时工作数(-j)。

执行这些命令开始构建过程，编译过程会花两个小时，并在构建目录中生成文件`libmxnet.so`。

如果遇到编译器被关闭的错误，这可能是超出内存（尤其在低于1GB RAM的树莓派1，2，0），通过编辑文件`/etc/dphys-swapfile`将`CONF_SWAPSIZE=100`改为`CONF_SWAPSIZE=1024`增加交换文件的size来矫正这个问题，然后执行：

<code>
sudo /etc/init.d/dphys-swapfile stop

sudo /etc/init.d/dphys-swapfile start

free -m # to verify the swapfile size has been increased
</code>

### 第二步：安装MXNet Python Bindings
在MXNet目录中执行：
<code>
cd python

pip install --upgrade pip

pip install -e
</code>

`-e`是可选的，意思如果你编译了源文件，安装时这些改变会反映出来。

现在可以在树莓派上运行MXNet。通过[Real-time Object Detection with MXNet On The Raspberry Pi](http://mxnet.incubator.apache.org/tutorials/embedded/wine_detector.html)开始。