---
title: Windows应急响应靶机-近源OS1
date: '2024-10-04 18:27:30'
updated: '2024-10-09 14:35:01'
description: Windows应急响应靶机-近源OS1 题解
tags:
  - Windows
  - 分享
  - 应急响应
  - 打靶
  - 教程
  - 网络安全
categories:
  - 网络安全
permalink: /post/windows-emergency-response-targetproximal-os1-19yxkq.html
comments: true
toc: true
cover: https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009105855-x072lr9.png
---

# 介绍

　　官方题解：[https://mp.weixin.qq.com/s/LcyuWWkNEnqDO94r_3wRlA](https://mp.weixin.qq.com/s/LcyuWWkNEnqDO94r_3wRlA)

## 挑战内容

　　前景需要：小王从某安全大厂被优化掉后，来到了某私立小学当起了计算机老师。某一天上课的时候，发现鼠标在自己动弹，又发现除了某台电脑，其他电脑连不上网络。感觉肯定有学生捣乱，于是开启了应急。

　　1.攻击者的外网IP地址

　　2.攻击者的内网跳板IP地址

　　3.攻击者使用的限速软件的md5大写

　　4.攻击者的后门md5大写

　　5.攻击者留下的flag

　　‍

　　‍

## 相关账户密码

　　Administrator

　　zgsf@2024

# 解题

## 攻击者的外网IP地址

　　先使用本机的rdp连接虚拟机

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009105303-1c0zsuh.png)​

　　将这几个doc文件全部丢入微步沙箱分析，

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009105716-8858zu8.png)​

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009105855-x072lr9.png)​

　　存在宏病毒，ip直接暴露出来：8.219.200.130

　　‍

## 攻击者的内网跳板IP地址

　　桌面有个phpstudy修复工具，点开属性一看，tm是个快捷文件，我第一眼都没看出来，根据该快捷方式指向：/lnk/test.bat

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009110029-suucsrd.png)​

　　tm的在桌面上找半天没找，搞隐藏是吧，选择文件夹选项，选中显示隐藏的文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009110821-zrp9x75.png)​

　　成功获取到内网跳板ip：192.168.20.129:801

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009110935-y1180te.png)​

　　‍

## 攻击者使用的限速软件的md5大写

　　然后是限速软件，靶场取自真实环境，真实环境中，一个普通用户怎么去劫持整个局域网网速呢？？

　　答案：ARP劫持

　　用啥劫持的呢？

　　翻一翻C盘文件就知道了

　　地址

```
C:\PerfLogs\666\666\777\666\666\666\666\666\666\666\666\666\666\666
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009123555-bagxc9f.png)​

　　‍

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009123630-gmxwcyi.png)​

　　也就是这玩意

　　我们直接算hash就可以了

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009123806-hg5h9q6.png)​

　　2A5D8838BDB4D404EC632318C94ADC96

　　‍

## 攻击者的后门md5大写

　　发现shift后门

　　连按5次shift系统会运行粘滞键

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009123927-s70beks.png)​

　　位置在

```
C:\Windows\System32\sethc.exe
```

　　​​

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009124100-kuhe2fg.png)​

　　计算md5：58A3FF82A1AFF927809C529EB1385DA1

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009124151-jijav11.png)​

　　‍

　　‍

　　‍

## 攻击者留下的flag

　　上一步的shift后门，执行之后就显示出flag了：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009123927-s70beks.png)​

　　flag:flag{zgsf@shift666}

　　‍

　　提交答案：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241009125011-azcmlw8.png)​

# 总结

* 对于这种老机子，先拿本机rdp远程连接之后再操作
* 一定要注意桌面上的文件，看是否是lnk文件或者其他可疑文件
* 学习发现隐藏目录
* 学习shift后门
* 一般通过ARP劫持来限制网速

　　‍
