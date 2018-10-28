# ubuntu 16.04 LTS降级安装gcc
原文为[ubuntu 16.04 LTS 降级安装gcc 4.8](https://www.cnblogs.com/in4ight/p/6626708.html)

安装的Ubuntu自带gcc和g++为6.3.0版本，但是gcc 5和6在编译MXNet时有错误。因此降级到支持C++11的G++ 4.8之后版本。

**第一步：安装**

`sudo apt-get install gcc-4.9`

**第二步：设置默认版本**

查看当前默认版本：

`gcc --version`

查看已有的gcc版本，用于确认4.9是否安装成功：

`ls -l /usr/bin/gcc*`

将某个版本加入gcc后选中，最后的数字是优先级，将4.9设置为100.

`sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 100`

当为gcc如上设置过候选时，可以查看配置的多个gcc版本：

`sudo update-alternatives --config gcc`

<font color="FF0000">然后再按同样的方法安装、设置g++。</font>
