---
layout:     post
title:      "[Linux]使用except脚本设置SSH免密登陆"
subtitle:   "如何有效利用except脚本"
date:       2017-11-28 12:00:00
author:     "Stephen.Ri"
header-img: "img/Linux-bg.jpg"
catalog: true
tags:
    - Linux
---

## except脚本内容

```
#!/usr/bin/expect

set user 服务器账号
set host 服务器IP
set password 服务器密码
set port 服务器端口号

spawn ssh -Y $user@$host -p $port
set timeout 30
expect "*assword:*"
send "$password\r"
interact
expect eof

```

## except脚本解析

`#!/usr/bin/expect`  改行指明采用except脚本执行

`set user 服务器账号`  设置变量

`spawn ssh -Y $user@$host -p $port`  spawn是进入expect环境后才可以执行的expect内部命令，它主要的功能是给ssh运行进程加个壳，用来传递交互指令

`set timeout 30`  设置超时时间，单位为秒

`expect "*assword:*"`  except内部指令，意思是判断上次输出结果里是否包含指定字符串，如果有则立即返回，否则就等待一段时间后返回，这里等待时长就是上一步设置的时长  

`send "$password\r"`  执行交互动作，与手工输入密码的动作等效

`interact`  执行完成后保持交互状态，把控制权交给控制台

## 运行脚本

将该内容保存为一个文件`ssh-host`后，使用下述命令修改文件权限  
`chmod 777 ssh-host`  
然后就可以直接右键从终端打开。

## MacBook Terminal快捷方式

在Terminal打开Preference --> 描述文件 --> 复制一个描述文件 --> Shell --> 运行命令中输入  
`except pathParent/ssh-host`  
如下图所示

 ![这里写图片描述]({{site.baseurl}}/img/imgInBlog/ssh.png)