---
layout: article
title: hexo配置https试水
date: 2018-04-13 13:18:51
tags: https
---

### 配置https

#### 基本信息

HTTPS（全称：Hyper Text Transfer Protocol over Secure Socket Layer），是以安全为目标的HTTP通道，简单讲是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。 它是一个URI scheme（抽象标识符体系），句法类同http:体系。用于安全的HTTP数据传输。https:URL表明它使用了HTTP，但HTTPS存在不同于HTTP的默认端口及一个加密/身份验证层（在HTTP与TCP之间）

> 必须先要有ssl证书才可以配置https链接

> 因为用的是腾讯云服务器，所以证书相应在腾讯云购买

> 下载好证书之后，解压完得出四个个文件

> 分别对应与`IIS`,`Apache`,`nginx`,'tomcat'服务器

> 因为自身的服务器是搭建了nginx的所以查看nginx服务器内容

> 可以得到两个文件`1_www.szfavor.cn_bundle.crt`和`2_www.szfavor.cn.key`

> 1_www.szfavor.cn_bundle.crt文件包括两段证书代码"------BEGIN CERTIFICATE-----"和"-----END CERTIFICATE-----"

> 2_www.szfavor.cn.key 文件包括一段私钥代码“-----BEGIN RSA PRIVATE KEY-----”和“-----END RSA PRIVATE KEY-----”。

#### 证书安装

> 首先你得安装nginx服务器

> 参考菜鸟教程中nginx的安装和配置(http://www.runoob.com/linux/negin-install-setup.html)

> 将上述两个文件上传到与nginx.conf同一文件夹的地方，我上传的地址是/etc/nginx/

> 然后更新nginx.conf文件

> 原先的配置

```

```
> 更新后的配置

```

```