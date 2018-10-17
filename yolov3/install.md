# Installing Darknet
Darknet非常容易安装，只有两个可选的依赖库：

- `OpenCV`如果你想要支持多种图片类型（以及直接显示检测结果图片）。
- `CUDA`如果你想要使用GPU运算。
两个依赖库都是可选的。

## 安装基础系统
首先从[git仓库](https://github.com/pjreddie/darknet)clone Darknet。可以通过一下命令完成：
```
git clone https://github.com/pjreddie/darknet.git
cd darknet
make
``` 
如果上述命令起作用了，你应该会看到一些飞快刷新的编译信息。
如果编译成功，执行命令：
```
./darknet
```
你应该看到如下输出结果：
```
usage: ./darknet <function>
```
在[这里](https://pjreddie.com/darknet/) 你可以了解可以使用darknet做哪些很酷的事情。

# 编译依赖CUDA的darknet
首先安装CUDA。（没给安装细节，需个人自己去解决。）
一旦CUDA安装成功，修改darknet根目录的`Makefile`文件的第一行：
```
GPU=1
```
现在你可以使用`make`命令编译这个项目。默认情况下使用第0张显卡。可以在命令中通过`-i <index>`指定使用那张显卡，比如：
```
./darknet -i 1 imagenet test cfg/alexnet.cfg alexnet.weights
```
当你使用CUDA编译但想使用CPU计算，你可以使用`-nogpu`来使用CPU：
```
./darknet -nogpu imagenet test cfg/alexnet.cfg alexnet.weights
```
# 编译依赖OpenCV的darknet
默认情况下，Darknet使用`stb_image.h`加载图片。如果你想支持更广泛的图片格式可以使用`OpenCV`代替。`OpenCV`也可以使我们无需将图片保存到硬盘中而直接显示图片和检测。
首先安装OpenCV。如果你从源代码安装将会花费更长时间且更复杂，因此试着找一个包来安装吧。
然后修改文件`Makefile`的第二行：
```
OPENCV=1
```
首先使用`make`重新编译。然后使用`imtest`测试图片加载和显示：
```
./darknet imtest data/eagle.jpg
```

-----

## CUDA环境变量设置
1. 安装玩CUDA后设置环境变量：
方法一：
在终端执行下面命令（只对当前终端有效，新终端需重新设置）
```
export PATH=/usr/local/cuda-8.0/bin:$PATH
#export LIBRARY_PATH=/usr/local/cuda-8.0/lib64：$LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64：$LD_LIBRARY_PATH
```
方法二：
将上述命令写到`~/.bashrc`文件中，此时对当前用户有效。（对已经打开的终端无效，需打开新的终端。或使用source命令执行这个文件中命令）
方法三：
将上述命令写到`/etc/profile`中，对所有用户有效。（１.使用source执行此文件使其立即生效。２.不推荐这种方法。）
**三个环境变量说明**
`PATH`：可执行程序的查找路径
`LIBRARY_PATH`：编译程序时动态链接库查找路径（一般通过在gcc命令中使用-L指定，所以一般不必设置这个变量）
`LD_LIBRARY_PATH`：运行程序时动态链接库查找路径



