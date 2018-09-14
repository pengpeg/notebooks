参考：

[win10下安装Ubuntu16.04双系统](https://blog.csdn.net/s717597589/article/details/79117112/)

[搭建双系统win10+ubuntu17.10](https://www.cnblogs.com/jiu0821/p/8413849.html)

[安装Ubuntu 16.04时卡住的那些坑](https://blog.csdn.net/Dod_Jdi/article/details/78635126)

安装过程：
1. 在Ubuntu官网找到历史版本
2. 下载Ubuntu 16.04.5 Desktop (64-bit)
3. 在Ubuntu官网的下载页面（不是历史版本页面）最下端有官方安装文档
4. 使用Ubuntu中文网推荐的Universal USB Installer制作系统U盘（英文官网推荐Rufus制作系统U盘）
5. U盘启动
6. 方向键将焦点移到install上
7. 按e键，将splash --- 改为splash nomodeset（因为英伟达显卡驱动的问题），否则会画面卡住不动
8. 按F10保存退出
9. 接下来进入正常安装流程
10. 分区说明：只有交换分区为主分区，其他设为逻辑分区，启动引导为efi系统分区（/boot是传统引导分区，不在单独分区），/usr为软件安装目录和/home目录单独分区，/作为系统分区分配30GB
11. 最好联网选择安装更新和第三方软件，不然装完系统再装各种软件（比如flash插件）会很烦
12. 重启完成安装
13. 选择启动Ubuntu系统，按下e键，继续设置spalsh nomodeset（英伟达显卡驱动还没装呢）
14. 安装英伟达驱动
15. 法一：软件和更新——>附加驱动——>安装英伟达驱动