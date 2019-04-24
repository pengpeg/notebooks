# linux使用systemd实现程序自启动
参考：
1.[systemctl介绍](https://www.cnblogs.com/lxjshuju/p/7183689.html) 
2.[最简明扼要的 Systemd 教程，只需十分钟](https://blog.csdn.net/weixin_37766296/article/details/80192633) 
3.[关于利用systemctl添加自定义系统服务问题](https://www.jianshu.com/p/c5e4953f6fa8) 

## 指令介绍
1.` systemctl list-unit-files`
列出linux系统上所有单元（类似Windows的服务）
2.`systemctl list-unit-files --type=service`
列出service类单元
3.`systemctl list-unit-files --type=target`
列出target类单元
4.`systemctl status gdm.service`
列出单元状态信息
5.服务操作指令
<table>
<tr><th>使服务自动启动</th><th>`systemctl enable httpd.service`</th></tr>
<tr><th>使服务不自动启动</th><th>`systemctl disable httpd.service`</th></tr>
<tr><th>启动某服务</th><th>`systemctl start httpd.service`</th></tr>
<tr><th>停止某服务</th><th>`systemctl stop httpd.service`</th></tr>
<tr><th>重启某服务</th><th>`systemctl restart httpd.service`</th></tr>
</table>

## 利用systemctl添加自定义系统服务

## 示例
参照参考3