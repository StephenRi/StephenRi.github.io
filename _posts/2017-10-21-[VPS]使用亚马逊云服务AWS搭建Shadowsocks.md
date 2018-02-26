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
```
通过上面的步骤，我们安装好了shadowsocks服务端，加下来就是启动该服务。

首先建立配置文件
```
$ cd /etc
$ mkdir shadowsocks
$ cd shadowsocks
$ vim ss.json
```
在ss.json文件中输入如下内容
```
{
    "server":"0.0.0.0",
    "server_port":8888,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"your_password",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false,
    "workers": 1
}
```
然后保存文件并退出，使用如下命令控制ss服务器端
启动：`ssserver -c /etc/shadowsocks/ss.json -d start`
停止：`ssserver -c /etc/shadowsocks/ss.json -d stop`

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

5.开启ipv6功能
=====================
通过开启ipv6功能，使用校园网就可以免费上外网而不走流量啦。

a. 登陆AWS控制台，进入VPC控制台，在您的**VPC**选项卡中选中你EC2实例所绑定的VPC编辑 -> 添加IPv6CIDR，关掉弹出层。
b. 选择**子网**并选中你的EC2实例对应的子网（亦可给全部子网进行相同操作）并编辑IPv6CIDR -> 添加IPv6CIDR.(IPv6CIDR后两位可以自定义,默认即可)，关掉弹出层。
c. 选择**路由表**，选择路由点编辑 -> 添加其他路由，第一个空填::/0，第二个空选一个网关，默认即可，点击**保存**。
d. 回到EC2控制台，给EC2实例添加一个IPv6地址。添加IP的地方在操作 -> 联网 -> 管理IP地址里 点一下分配让他自动分配就行。

部分系统需要登陆进去使用DHCP自动获取一下。
`dhclient -6`

通过上面的步骤，我们的实例已经可以通过ipv6的方式访问了，最后还需要设置一下shadowsocks服务器端。
```
$ cd /etc/shadowsocks
$ vim ss1.json
```
在ss.json文件中输入如下内容
```
{
    "server":"your_ipv6_addr",
    "server_port":8886,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"your_password",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false,
    "workers": 1
}
```
然后保存文件并退出，使用如下命令控制ss服务器端
启动：`ssserver -c /etc/shadowsocks/ss1.json -d start --pid-file ss1.pid`
停止：`ssserver -c /etc/shadowsocks/ss1.json -d stop --pid-file ss1.pid`
这里因为开启了多个ss进程，要指定一下pid-file，否则会冲突，显示已经启动。（可能是这个原因吧）