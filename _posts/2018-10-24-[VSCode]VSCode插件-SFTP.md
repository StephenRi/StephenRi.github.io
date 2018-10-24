---
layout:     post
title:      "[VSCode]VSCode插件-SFTP"
subtitle:   "使用SFTP插件直接对远端文件进行修改"
date:       2018-10-24 16:00:00
author:     "Stephen.Ri"
header-img: "img/Sublime-bg.jpg"
catalog: true
tags:
    - VSCode
---

## SFTP插件有什么用

SFTP原意是Secure File Transfer Protocol（安全文件传输协议）。

而借助VSCode的SFTP插件，用户可以做到**直接在VSCode上像操作本地文件一样对远端文件进行修改**，而不再需要繁杂的上传，下载过程。美滋滋！！

## 安装SFTP插件

直接在VSCode插件库搜索并安装SFTP，认准作者liximomo。

## 配置SFTP插件

按下**command + shift + p** 或者 **F1** 打开VSCode命令面板，输入 **SFTP: Config**，将会创建一个 sftp.json 文件。

按照自己的服务器进行配置，配置说明如下：

```
{
    "name": "configName1",                  //这份配置的名字
    "protocol": "sftp",                     //协议
    "host": "192.168.1.211",                //服务器IP
    "port": 9036,                           //端口
    "username": "Stephen",                  //用户名
    "password": "123456",                   //密码
    "remotePath": "/home/me/project",       //服务器上你想要操作的目录
    "context": "‎⁨project1",                  //本机相对路径
    "uploadOnSave": false,                  //为true则一旦保存文件，就会同步到远端
    "ignore": [
        ".vscode",
        ".git",
        ".DS_Store"
    ],
    "watcher": {
        "files": "**/*",
        "autoUpload": false,
        "autoDelete": false
    }
},
{
    "name": "configName2"                   //多台服务器或者多个目录时这么用
}
```

## 使用SFTP插件

如果上面配置了 **uploadOnSave** 为 **true**，则每次保存就会同步到远端；否则需要使用命令面板手动同步，或者在要同步的文件上右键进行同步。