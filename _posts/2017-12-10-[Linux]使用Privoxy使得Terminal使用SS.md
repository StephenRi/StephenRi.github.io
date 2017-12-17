---
layout:     post
title:      "[Linux]使用Privoxy使得Terminal使用SS"
subtitle:   "给Terminal设置代理"
date:       2017-12-10 11:00:00
author:     "Stephen.Ri"
header-img: "img/Linux-bg.jpg"
catalog: true
tags:
    - Linux
---

## 安装Privoxy

privoxy有将socks代理转为http代理的功能。我们首先在Terminal中安装privoxy

打开终端，输入以下命令

`brew install privoxy`

当然如果没有安装brew的需要先安装brew。

## 查看ShadowSock端口

打开ShadowSock -> 偏好设置 -> 高级

 ![Shadowsock设置]({{site.baseurl}}/img/imgInBlog/privoxy1.png)

如图所示，即sock5地址为`127.0.0.1:1086`

## 修改Privoxy配置

打开配置文件

`vim /usr/local/etc/privoxy/config`

在文件末尾添加如下两行

`listen-address 0.0.0.0:8118`  
`forward-socks5 / localhost:1086 .`

注意后面有空格和.

## 启动Privoxy

`brew services start privoxy`

当然如果要关闭Privoxy，可以使用下面的命令

`brew services stop privoxy`

## 使用http代理

Privoxy讲ShadowSock的sock5代理转化为http代理，而我们需要使用下面的命令使得Terminal使用该http代理。

`vim ~/.bashrc`

添加如下内容

`export http_proxy='http://localhost:8118'`  
`export https_proxy='http://localhost:8118'`

 ![Shadowsock设置]({{site.baseurl}}/img/imgInBlog/privoxy2.png)

如果要临时使用代理的话，可以直接在Terminal输入上面的两条，而不需要在`.bashrc`中添加

## 使用Alfred制作Privoxy开关

思路比较简单，相当于给Terminal设置快捷键

 ![Shadowsock设置]({{site.baseurl}}/img/imgInBlog/privoxy3.png)
