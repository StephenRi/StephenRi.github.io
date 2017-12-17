---
layout:     post
title:      "[Alfred]打造You-Get插件"
subtitle:   "给Alfred做一个You-Get插件，实现一键下载网页视频"
date:       2017-12-07 12:00:00
author:     "Stephen.Ri"
header-img: "img/Alfred-bg.jpg"
catalog: true
tags:
    - Alfred
---

## 安装You-Get

Alfred只是调用一些程序，因此要实现一键下载网页视频，我们需要首先下载You-Get插件。

打开终端，输入以下命令

`brew install you-get`

当然如果没有安装brew的需要先安装brew。

## 安装ffmpeg

ffmpeg是一个合并视频的软件，现在一些网站都是将视频分段的，使用ffmpeg可以将这些分段的视频合并起来，而Alfred可以自动调用ffmpeg，非常方便。

`brew install ffmpeg`

**这两个软件的下载可能会下载一些依赖软件，请耐心等待。**

## 写AppleScript脚本

由于要用到一些Mac的交互信息，因此用AppleScript脚本非常方便。脚本内容如下：

 ![AppleScript脚本文件]({{site.baseurl}}/img/imgInBlog/youget1.png)

这里采用了**直接调用**Terminal的方式，当然也可以采用**后台调用**Terminal，但是后台调用的一个缺点就是放进Alfred之后，下载过程中Alfred不可用。

## 给Alfred添加Workflow

如下图所示创建流程，将AppleScript脚本内容拷贝到里面即可。

 ![Workflow流程图]({{site.baseurl}}/img/imgInBlog/youget2.png)

 感兴趣的童鞋，可以直接下载[You-Get-Download.alfredworkflow](https://github.com/StephenRi/myAlfredWorkflow/blob/master/You-Get-Download.alfredworkflow)