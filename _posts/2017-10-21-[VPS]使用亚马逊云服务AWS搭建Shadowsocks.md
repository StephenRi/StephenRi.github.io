---
layout:     post
title:      "[WALL]使用亚马逊云服务AWS搭建Shadowsocks"
subtitle:   "如何用AWS走出WALL看世界"
date:       2017-10-21 12:00:00
author:     "Stephen.Ri"
header-img: "img/wall-bg.jpg"
catalog: true
tags:
    - WALL
---

1.注册AWS
========

AWS：https://amazonaws-china.com

常规注册操作。需要注意的是AWS的注册需要使用**信用卡**。

完成后的界面如下：

![这里写图片描述]({{site.baseurl}}/img/imgInBlog/aws1.png)

点击左上角服务，选择**EC2**，点击右上角，选择**亚太地区（东京）**。然后点击**启动实例**。

![这里写图片描述]({{site.baseurl}}/img/imgInBlog/aws2.png)

选择**ubuntu系统**，下一步点击**审核和启动**，**启动**。

在密钥下拉框选择**创建新密钥**，名称随意，如myawskey，然后**下载密钥对**（myawskey.pem）。

2.远程连接VPS
==========

在密钥对文件`myawskey.pem`所在目录下打开终端。
终端输入：
```
$ chmod 400 myawskey.pem
$ ssh -i myawskey.pem ubuntu@yourip
```

3.在VPS上安装Shadowsocks服务器端
========================

```
$ sudo -s
$ apt-get update
$ apt-get install python-pip
$ pip install shadowsocks
$ pip install --upgrade pip
$ sudo ssserver -p 8388 -k mypassword -m rc4-md5 --user nobody -d start
```
最后一行代码开启Shadowsocks服务器，-p为端口，-k为密码，-m为加密方式。

**开启AWS入站端口**：在**启动实例**后，把滚动条滚到最右边，点击**安全组**下的`launch-wizard-1`。

![这里写图片描述]({{site.baseurl}}/img/imgInBlog/aws3.png)

点击**操作**，**编辑入站规则**。

![这里写图片描述]({{site.baseurl}}/img/imgInBlog/aws4.png)

点击**添加规则**，**端口**为8388，**来源**为任何，点击**保存**。

![这里写图片描述]({{site.baseurl}}/img/imgInBlog/aws5.png)

4.本机安装Shadowsocks客户端
=====================

下载并安装Shadowsocks。

Mac版：https://github.com/shadowsocks/ShadowsocksX-NG
Windows版：https://github.com/shadowsocks/shadowsocks-windows
Android版：https://github.com/shadowsocks/shadowsocks-android
iOS版：本人使用的是`Shadowrocket`

在**服务器配置**配置外网ip，端口号，密码，加密方式即可。
