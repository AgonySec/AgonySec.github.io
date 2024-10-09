---
title: Windows应急响应靶机 - Web3
date: '2024-10-04 13:15:06'
updated: '2024-10-09 23:36:44'
tags:
  - Windows
  - 分享
  - 打靶
  - 应急响应
  - 教程
  - 网络安全
permalink: /post/windows-emergency-response-target-web3-1motjj.html
comments: true
toc: true
---

![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/3d05c59d86ea52290cd9053919476cf8--2885164731-20241007102956-wx5j531.jpg)

# 介绍

　　题目地址

　　[前来挑战！应急响应靶机训练-Web3](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485467&idx=1&sn=d1e6c82ed181c4baee4874f858832a06&chksm=c11a59e6f66dd0f03202f22fa8bde5aedaf86c995f45dfa83f6d92da356b2b39d9e3a28e1071&scene=21#wechat_redirect)

　　题解(WriteUp)

　　[应急响应靶机训练-Web3题解](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485579&idx=1&sn=ab5f66071aa726dade5a9d6d41878e47&chksm=c11a5976f66dd060372d6453e5b20cda24d471aef46708e91dfc65d018487afaf169a1e8e22d&scene=21#wechat_redirect)

　　‍

　　**挑战内容**

　　前景需要：小苕在省护值守中，在灵机一动情况下把设备停掉了，甲方问：为什么要停设备？小苕说：我第六感告诉我，这机器可能被黑了。

　　这是他的服务器，请你找出以下内容作为通关条件：

1. 攻击者的两个IP地址
2. 隐藏用户名称
3. 黑客遗留下的flag【3个】

　　本虚拟机的考点不在隐藏用户以及ip地址，仔细找找把。

　　‍

　　**相关账户密码：**

　　Windows:administrator/xj@123456

# 解题

　　‍

## 隐藏用户

　　net user 没有发现其他用户

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004152008-7mewmc4.png)​

　　如何又去控制面板上寻找，无果，最后再用户目录下发现了这个隐藏用户：hack6618$

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004152113-7g6v9ux.png)​

　　‍

　　‍

## flag

### flag1

　　在隐藏用户的下载目录下，存在一个system.bat文件，里面发现了一个flag{888666abc}

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004152435-65ym5co.png)​

　　‍

　　‍

### flag2

　　登录mysql，使用数据库zblog123，查询表 zbp_member ，发现攻击者登录的

　　ip ：192.168.75.130 和flag{H@Ck@sec}

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004153720-glu56py.png)​

　　‍

### flag3

　　cmd输入：taskschd.msc

　　打开计划任务，发现了最后一个flag{zgsfsys@sec}

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004155738-bknak06.png)​

　　于此同时计划任务触发的程序正是之前先一步找到的system.bat文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004155827-308sh5p.png)​

　　所以按照顺序来说，应该先找计划任务得到线索，慢慢得出答案！😂

　　‍

## 攻击者的2个ip

　　分析apache的日志文件

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004155235-y1lx3c5.png)​

　　分析得出两个IP地址

　　192.168.75.129

　　192.168.75.130

　　‍

　　**最后提交答案完成练习。**

# 总结

* 熟悉应急响应操作步骤
* 加强了日志分析能力
* 先排查 隐藏用户-启动项-计划任务-服务，后面在排查其他的
* 要注意排查数据库的内容

　　‍
