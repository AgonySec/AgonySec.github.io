---
title: Linux应急响应靶机 -Linux2
date: '2024-10-03 19:29:10'
updated: '2024-10-09 23:56:42'
description: 前景需要：看监控的时候发现webshell告警，领导让你上机检查你可以救救安服仔吗！！
tags:
  - Linux
  - 分享
  - 应急响应
  - 打靶
  - 教程
  - 网络安全
categories:
  - 网络安全
permalink: /post/linux-emergency-response-target-linux2-x00hf.html
cover: https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009095806-4dety9p.png
comments: true
toc: true
---
# 介绍

　　**题目地址：**

　　[应急响应靶机-Linux(2)](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485625&idx=1&sn=ec58f917314da2fee442a94bce81ce9d&chksm=c11a5944f66dd0526a43531f5afdaec58e39dea79df0fbcf413269cb9d5ce54fb1fd1077176a&scene=21#wechat_redirect)

　　**题解(WriteUp)：**

　　[应急响应靶机训练-Linux2题解](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247485704&idx=1&sn=2fbcaae9954ac422052903b9de0343ea&chksm=c11a58f5f66dd1e37953b92ad464b3794a2e8019ad30ce8d3b4342ab725dbc4399330d038cfd&scene=21#wechat_redirect)

　　‍

　　**挑战内容：**

　　前景需要：看监控的时候发现webshell告警，领导让你上机检查你可以救救安服仔吗！！

　　1,提交攻击者IP

　　2,提交攻击者修改的管理员密码(明文)

　　3,提交第一次Webshell的连接URL(http://xxx.xxx.xxx.xx/abcdefg?abcdefg只需要提交abcdefg?abcdefg)

　　3,提交Webshell连接密码

　　4,提交数据包的flag1

　　5,提交攻击者使用的后续上传的木马文件名称

　　6,提交攻击者隐藏的flag2

　　7,提交攻击者隐藏的flag3

　　‍

　　**相关账户密码：**

　　root/Inch@957821.

# 解题

　　**注意：这个靶机有问题，里面字符是乱码，导致无法正常启动宝塔，故后面的实验也做不了，所以wp是直接从官方wp拷贝过来的**

　　登录宝塔，输入bt，然后修改面板密码：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004125744-5qmmh9l.png)​

　　‍

## 提交攻击者IP

　　答案：192.168.20.1

　　查看宝塔日志即可![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004125817-xcifwnl.png)​

　　‍

## 提交攻击者修改的管理员密码(明文)

　　答案：Network@2020

　　查看数据库表（x2\_user）

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004125858-iofdije.png)​

　　发现passwod加密在代码中寻找加密逻辑并进行逆推

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009095747-qor2e7u.png)​

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009095755-c1olawe.png)​

## 提交第一次Webshell的连接URL

　　答案：index.php?user-app-register

　　登录后台后发现木马写在了注册协议，注册协议的路由为user-app-register

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009095806-4dety9p.png)​

　　‍

## 提交Webshell连接密码

　　答案：Network2020

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009095816-66umvnu.png)​

## 提交数据包的flag1

　　答案：flag1{Network@\_2020\_Hack}

　　下载root下的数据包

　　追流可以发现flag1

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004130312-s8q88sd.png)​

　　‍

　　‍

## 提交攻击者使用的后续上传的木马文件名称

　　答案：version2.php

　　经典的冰蝎Webshell特征

​![图片](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/network-asset-640-20241007152544-czhq5d7.png)​

## 提交攻击者隐藏的flag2

　　答案：flag{bL5Frin6JVwVw7tJBdqXlHCMVpAenXI9In9}

　　在/www/wwwroot/127.0.0.1/.api/alinotify.php

　　‍

​![图片](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/network-asset-640-20241007152544-embkzsg.webp)​

## 提交攻击者隐藏的flag3

　　答案：flag{5LourqoFt5d2zyOVUoVPJbOmeVmoKgcy6OZ}

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241004131335-vo3tk33.png)​

　　‍

　　​`env`​ 命令用于显示或设置环境变量

# 总结

* 熟悉查看宝塔日志
* 学习根据 加密代码 逆推 解密逻辑
* 学习使用wireshark 来查看流量数据包

　　‍
