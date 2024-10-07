---
title: Windows应急响应靶机 - Web2
date: '2024-10-03 15:08:51'
updated: '2024-10-07 10:18:05'
description: Windows应急响应靶机-web2 题解
tags:
  - Windows
  - 分享
  - 应急响应
  - 打靶
  - 教程
  - 网络安全
categories:
  - 网络安全
permalink: /post/windows-emergency-response-target-web2-2ibdlc.html
comments: true
toc: true
cover: https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/1fb652b75f7d9a40773798719a00dd01--782050166.jpg
---

# 介绍

　　**题目地址**

　　[前来挑战！应急响应靶机训练-Web2](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485230&idx=1&sn=7a6ff6376caf2a2268a468e78901b6d0&chksm=c11a56d3f66ddfc505fa9faf68077bb4679e31919b8cfefe505e8a93a0bcd086f44904e7530f&scene=21#wechat_redirect)

　　**官方题解(WriteUp)**

　　[应急响应靶机训练-Web2【题解】](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485236&idx=1&sn=d6d301a864a3243bc8692ccfe1e77815&chksm=c11a56c9f66ddfdf5a74386ab9ae388cc91406b04f9e5eb9848853d20c584503a6ee2bf798d5&scene=21#wechat_redirect)

　　**挑战内容：**

　　1.攻击者的IP地址（两个）？

　　2.攻击者的webshell文件名？

　　3.攻击者的webshell密码？

　　4.攻击者的伪QQ号？

　　5.攻击者的伪服务器IP地址？

　　6.攻击者的服务器端口？

　　7.攻击者是如何入侵的（选择题）？

　　8.攻击者的隐藏用户名？

　　‍

　　**相关账户密码**

　　用户:administrator

　　密码:Zgsf@qq.com

# 解题

## 排查webshell

　　打开虚拟机输入密码，启动靶机内所有服务

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003151939-tzxnl41.png)​

　　先上传工具箱，使用D盾扫描到后门：system.php

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003152256-t5hyu1w.png)​

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003152404-416wc6x.png)​

　　后门连接密码是：hack6618

## 查看日志

　　发现攻击者进行目录扫描，接着就到访问木马这一步了

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003161342-j0a803t.png)​

　　攻击ip：192.168.126.135

　　没有找到上传webshell的特征，继续查看服务器存在ftp，所以接着去看ftp日志

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003163814-g1w72oo.png)​

　　发现在ftp进行了webshell上传，在此之前攻击者进行了ftp账号爆破，攻击ip还是：192.168.126.135

　　‍

　　‍

## 寻找隐藏用户

　　查看远程桌面登录成功日志：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003164230-xmfct8d.png)​

　　发现未知用户 hack887 ，登录ip为：192.168.126.129

　　查看用户桌面有2个文件

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003162538-9tl7nz2.png)​

　　删除用户失败：

```powershell
net user hack887$ /del
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003185914-p3kdz8w.png)​

　　控制面板寻找账户并未发现

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003185949-vw7vwol.png)​

　　猜测该用户为克隆administrator隐藏用户，注册表寻找该用户

　　找到隐藏账户hack887\$

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003190157-ge5na7c.png)​

　　‍

　　‍

## 追溯到攻击者遗留文件

　　继续寻找，查看到文档有qq文件，qq账号是：777888999321

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003162835-0luaq30.png)​

　　继续查看qq传输的文件，发现传输了frp文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003162919-ddxjgz5.png)​

　　根据frp的配置文件发现了攻击者的服务端ip：256.256.66.88 ,端口为65536

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003163006-a11qcu5.png)​

　　‍

　　最后整理信息：

　　IP1:192.168.126.135

　　IP2:192.168.126.129

　　伪QQID:777888999321

　　伪服务器地址：256.256.66.88

　　伪端口：65536

　　隐藏用户名：hack887\$

　　webshell密码：hack6618

　　webshell名称：system.php

　　‍

　　打开解题.exe，提交答案：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003190611-6sin725.png)​

　　解题成功！

# 总结

* 学习查看ftp日志
* 学习如何排查隐藏用户
* 学习如何追溯黑客攻击行为

　　‍
