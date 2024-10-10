---
title: 应急响应靶机训练-挖矿
date: '2024-10-10 00:14:26'
updated: '2024-10-10 15:19:45'
tags:
  - 教程
  - 分享
  - 网络安全
  - 应急响应
  - 打靶
  - Windows
categories:
  - 网络安全
permalink: /post/emergency-response-target-machine-trainingmining-fgqqu.html
comments: true
toc: true
---

# 介绍

## 挑战内容

　　前景需要：机房运维小陈，下班后发现还有工作没完成，然后上机器越用越卡，请你帮他看看原因。

　　挑战题解：

　　攻击者的IP地址

　　攻击者开始攻击的时间

　　攻击者攻击的端口

　　挖矿程序的md5

　　后门脚本的md5

　　矿池地址

　　钱包地址

　　攻击者是如何攻击进入的

　　靶机地址：

　　[[hvv训练]应急响应靶机训练-挖矿事件](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247487458&idx=1&sn=4b9ec1450f0634e16de69af88e0a8fad&chksm=c11a5e1ff66dd709d6afd9c7026abd781d07ca7f8dbdc001984aa788dfe1fd40d40d285d9f3f&scene=21#wechat_redirect)

　　题解地址：

　　[[题解]应急响应靶机训练-挖矿事件](http://mp.weixin.qq.com/s?__biz=MzkxMTUwOTY1MA==&mid=2247487690&idx=1&sn=c4f26ba7807af51c18d8899726081937&chksm=c11a4137f66dc8212b3fb72eb47a6546a67c3a3460d4bd6e49a76e23c4341fb1df3d12ea6dc7&scene=21#wechat_redirect)

　　‍

## 账号密码

　　administrator/zgsf@123

# 解题

　　登录机器之后，先rdp远程登录

　　登录上去，卡的要死，直接定位到这个程序位置，一眼挖矿,cpu 都100%了

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010110254-sm7civ9.png)​

　　按照题目要求计算一下挖矿程序XMRig.exe 的hash：A79D49F425F95E70DDF0C68C18ABC564

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010130512-w2pzkas.png)​

　　做着做着，tmd，cmd窗口自己蹦出来了，艹，还在执行什么东西：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010112242-hutps30.png)​

　　一眼什么启动项，计划任务啥的，根据命令直接得知 矿池地址：c3pool.org

　　找到这个挖矿服务，把这个服务关了：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010112726-d9ene3m.png)​

　　由于现在还不知道这个挖矿脚本在哪，所以上传火绒剑，找一下文件这个脚本文件地址：有一个未知启动项

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010130747-p12xkq9.png)​

　　在启动项里发现了这个文件，发现就是这个挖矿脚本文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010130856-luxmy5h.png)​

　　题目要求计算这个挖矿脚本的hash：8414900F4C896964497C2CF6552EC4B9

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010131032-9rpe4ws.png)​

　　矿池地址：c3pool.org

　　打开这gb挖矿程序配置文件一看，钱包地址（也就是这个配置文件中user）：

```go
4APXVhukGNiR5kqqVC7jwiVaa5jDxUgPohEtAyuRS1uyeL6K1LkkBy9SKx5W1M7gYyNneusud6A8hKjJCtVbeoFARuQTu4Y
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010110441-3dvbjx5.png)​

　　已知条件，是一台运维的电脑，那么说明，不回去跑任何业务，打开桌面上的表格看看

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010131513-g67kcxs.png)​

　　密码都是统一的，说明可能是攻击者在拿到一台的密码后进行的密码喷洒，所以攻击者可能就是一直拿密码爆破本机的3389端口远程服务。

　　右键双击Windows日志一键分析，上传至服务器

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010131544-iq7lx9p.png)​

　　查看登录失败日志，果然发现暴力破解痕迹，并记录第一次开始时间2024-05-21 20:25:22

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010131749-5381cfo.png)​

　　攻击者IP地址192.168.115.131

　　所以攻击者确实通过暴力破解远程rdp服务来进入的。

　　最后解题：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20241010132127-df8fe03.png)​

# 总结

* 深刻学习了**一次对挖矿进行应急响应的处理**
* 火绒剑是个不错的工具，用来排查异常的服务很好用
* 记录一下 运维的电脑一般不会跑业务程序的

　　‍
