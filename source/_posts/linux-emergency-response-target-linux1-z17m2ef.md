---
title: Linux应急响应靶机 -Linux1
date: '2024-10-03 14:06:28'
updated: '2024-10-07 09:41:08'
tags:
  - 网络安全
  - 应急响应
  - 分享
  - 打靶
  - 教程
  - Linux
permalink: /post/linux-emergency-response-target-linux1-z17m2ef.html
comments: true
toc: true
cover: https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/1f328ced5108c51f01d76964bedd7e59--3451917188.jpg
---

# 介绍

　　题目地址

　　[应急响应靶机-Linux(1)](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485600&idx=1&sn=c81d7b89dd15d6d46a2d82aae947e846&chksm=c11a595df66dd04b53a0703a21a201f23fea10aaa3cab4267b223a2de3e436ef88f60fcb6709&scene=21#wechat_redirect)

　　题解(WriteUp)

　　[应急响应靶机训练-Linux1题解](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485668&idx=1&sn=6ec157895b01fa377408873d63c01e8c&chksm=c11a5919f66dd00f0d96411431ed1a582bcb3e493b3d718890bf9b573e37d73c373ac122dffc&scene=21#wechat_redirect)

　　‍

　　**登陆虚拟机**

* 密码defend

　　‍

　　**目标：3个flag以及黑客的ip地址**

　　‍

# flag1

　　使用su 切换root账户，输入当前密码defend

　　查看历史命令：history

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003141355-hm71pza.png)​

　　拿到 flag{thisismybaby}

# flag2

　　发现似乎有人更改了/etc/rc.d/rc.local

　　而这个文件是配置开机脚本的，我们直接去看一下里面有什么

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003143110-b2ldywf.png)​

　　发现flag2

```powershell
flag{kfcvme50}
```

# flag3

　　当时再看/etc/passwd的时候，发现有redis服务

　　所以，查看一些redis的配置文件

```powershell
more /etc/redis.conf
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003143253-mcu9yoj.png)​

　　发现flag：

　　flag{P@ssW0rd_redis}

# 查看redis日志

　　看一眼，redis日志文件等级

　　cat /etc/redis.conf | grep loglevel

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003143547-o9cpf1h.png)​

　　发现为verbose,直接去找redis日志文件

　　verbose:包含很多不太有用的信息，但是不像debug级别那么混乱

　　查看一下连接日志

　　cat /var/log/redis/redis.log | grep Acc

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003143629-vlekjd1.png)​

　　找到IP地址192.168.75.129

# 提交答案

　　整理一下答案，3个flag，1个ip地址

```powershell
flag{thisismybaby}
flag{kfcvme50}
flag{P@ssW0rd_redis}
192.168.75.129
```

　　最后，提交题解系统

　　cd /home/defend/桌面/题解 && ./题解Script.sh

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241003144020-uoz2chf.png)​

# 总结

* 先切换root账号进行应急响应，以防权限不够
* 学习查看history历史命令
* 学习查看redis日志 /var/log/redis/redis.log

　　‍
