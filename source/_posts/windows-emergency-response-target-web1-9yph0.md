---
title: Windows应急响应靶机 - Web1
date: '2024-10-03 11:03:39'
updated: '2024-10-09 15:04:28'
description: Windows应急响应靶机 - Web1 题解
tags:
  - 教程
  - 网络安全
  - 应急响应
  - 打靶
  - Windows
categories:
  - 网络安全
permalink: /post/windows-emergency-response-target-web1-9yph0.html
comments: true
toc: true
cover: https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003131537-sxg887z.png
---

# 介绍

　　题目地址：

　　[前来挑战！应急响应靶机训练-Web1](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485154&idx=1&sn=2b27ca8af88ec5107138f3498e48f12a&chksm=c11a571ff66dde09e927da278fc8ac280d0f4a1a046e28eec41da72fc32df678af27f8fef01e&scene=21#wechat_redirect)

　　官方题解(WriteUp)

　　[应急响应靶机训练-Web1【题解】](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485188&idx=2&sn=54b8962a88b9da74a3db436f515b815b&chksm=c11a56f9f66ddfefd0ca7402617e4c5665033b0305a5f66a1434630b9795b09fa23fc63d3c12&scene=21#wechat_redirect)

　　‍

# 查询webshell

　　上传工具，D盾扫描到shell：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003131537-sxg887z.png)​

　　‍

　　找到木马文件:

　　在目录：www/content/plug

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003134741-93lipy8.png)​

　　这个木马内容，一眼顶针，鉴定为冰蝎木马。。。。

　　连接密码：rebeyond

# 查看日志

　　回到PHP study中，找到Apache的日志文件

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003131929-xh7i8yz.png)​

　　**找到黑客IP地址192.168.126.1**

　　发现有大量爆破行为，猜测存在弱口令

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132157-hhcamb5.png)​

　　使用Windows日志一键分析功能

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132244-c8r66fi.png)​

　　查看远程桌面登录成功的日志：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132301-rv758vf.png)​

　　**发现未知用户名hack168$**

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132547-zvyfjqm.png)​

# 挖矿木马

　　找到该用户文件夹位置，桌面上有个执行文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132655-pacd75e.png)​

　　将这个程序放到虚拟机运行一下，结果cpu飙升，鉴定为挖矿木马：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132905-pnjqs7b.png)​

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003132934-vlyi6xs.png)​

　　nmd，虚拟机都卡住了。。。。

　　这个程序的图标，也一眼鉴定为pyinstaller打包。

　　使用pyinstxtractor进行反编译 [github.com/extremecoders-re/pyinstxtractor](https://github.com/extremecoders-re/pyinstxtractor)

```powershell
python pyinstxtractor.py Kuang.exe
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003133856-kee1qty.png)​

　　得到pyc文件

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003134049-92qqid7.png)​

　　使用在线[pyc](https://toolkk.com/tools/pyc-decomplie)反编译工具，得到源码

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003134238-l5y1htf.png)​

　　得到矿池的域名：http://wakuang.zhigongshanfang.top

　　整理答案，提交就可以了。

# 漏洞修复

　　漏洞名称：emlog v2.2.0后台插件上传漏洞

　　黑客是通过这个漏洞上传webshell的，建议更新到最新版修复漏洞。

　　‍

# 总结

* 学习使用工具来排查webshell，节省时间
* 查看Apache的日志文件access.log
* 查看远程桌面登录成的日志（登录成功表示4624）
* 学习反编译pyinstxtractor打包的程序以及反编译pyc文件

　　‍
