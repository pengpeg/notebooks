# 在Ubuntu16.04中使用eclipse开发C程序
<div>

</div>
## 工程相对目录问题
1.在使用eclipse运行或调试程序时其当前目录为**当前项目目录**，例如：
工程目录：eclipse-workspace/DLFCompare/
要读取的文件路径：eclipse-workspace/DLFCompare/img/001.jpg
则代码中读取此文件的相对路径为：`img/001.jpg`
而在终端执行时则相对的是可执行程序所在的目录

## eclipse调试时依赖的运行时库设置
1.在visual stdio中，动态库可以和.exe文件放在一起或设置环境变量。而eclipse及时将共享库和生成的可执行放在一起也无法找到共享库，在~/.bashrc中设置的`LD_LIBRARY_PATH`也无法找到。
解决方法：运行->调试配置(或在调试图标按钮右边下标点出)->环境->新建
（变量）LD_LIBRARY_PATH
（值）/home/chen/Github/darknet
将环境追加到本机环境（没试过第二个选项）。

## eclipse编译时包含文件和依赖库路径设置
1.选定要编译的项目后，项目->属性->C/C++构建->设置->工具设置
　- GNU C编译器->包含：设置包含文件路径
　- GNU C连接器->库：设置依赖库

## eclipse测试共享库工程
1.编写完库工程后，可以新建一个可执行程序工程，按照正常添加依赖库的方法为可执行程序工程添加include,lib和设置运行、调试配置的变量（参见‘eclipse调试时依赖的运行时库设置’）。
2.然后调试可执行程序工程就可以直接跟踪进库工程，debug库工程的代码。
