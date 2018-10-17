[原文链接](https://blog.csdn.net/qintaiwu/article/details/73741976) 
# apt-get软件管理工具
下面讲解，linux系统下如何进行软件的管理，包括软件的索引安装、更新、卸载删除、本地存储介质中软件的安装、系统升级等操作。
## 一、Linux软件包按封装类型分为３类：
1. Debian，其文件扩展名为".deb"
2. Red Hat，其文件扩展名为".rpm"
3. Tarball，其文件扩展名有".tar.gz"、".tar.bz2"和"TGZ"

Tarball是一种大量文件（类似zip文件）组成的单个档案大型文件集合，主要用于发布软件的源代码，用“tar”命令组合文件，用“gzip”命令压缩文件容量，解压时用“tar -xzf filename”命令解压，然后在执行安装操作。

## 二、软件仓库
软件仓库，顾名思义就是存放软件包的地方，指的是一个网站或者一个目录，Uvuntu  Linux系统下通过特定的命令就能完成软件包的索引、软件的更新、安装等操作，固定的仓库使得软件更为规范，安装操作步骤更为简单，不类似Windows下的软件，极为散乱，存在各种不安全性。

主要的软件仓库有：Main、Restricted、Universe、Multiverse这4个。

## 三、软件包的依赖关系
故名思意，下载的一个软件包时，需要依赖（从程序角度来说，这就是调用）别的软件或者某些函数来实现这个软件的功能。

apt-get命令维护软件时，会自动识别并下载相应的依赖软件。

## 软件维护操作
**１、安装软件**
`sudo apt-get install XXXX`
【提示】使用该命令安装软件时系统会自动安装存在依赖关系的软件包，以保证软件正常运行。

**２、更新软件**
`sudo apt-get update`：更新软件源索引
`sudo apt-get upgrade XXXX`：将软件升级到最新版本

**３、卸载软件**
１）`sudo apt-get remove XXXX`：卸载软件（删除软件包）
２）`sudo apt-get autoremove XXXX`：自动卸载软件但保留其配置文件
３）`sudo apt-get autoremove --purge XXXX`：自动卸载软件并删除其配置文件
【提示】１）一般用于卸载本地安装的软件，２／３）一般用于在线安装的软件

**４、重装同一软件**
`sudo apt-get --reinstall install XXXX`

补充：系统是如何知道软件源的呢？

原因是系统/etc/apt/sources.list配置文件定义了软件的发行源。







