---
layout:     post
title:      "[ADB]Mac连接Android手机玩转跳一跳
subtitle:   "使用Mac控制手机自动跳一跳"
date:       2018-02-08 12:00:00
author:     "Stephen.Ri"
header-img: "img/Sublime-bg.jpg"
catalog: true
tags:
    - ADB
---


## 下载ADB

Android调试桥(adb)是一个开发工具，帮助安卓设备和个人计算机之间的通信。 这种通信大多是在USB电缆下进行，但是也支持Wi-Fi连接。 adb 还可被用来与电脑上运行的安卓模拟器交流通信。 adb 对于安卓开发来说就像一把“瑞士军刀”。

我们可以通过Brew安装ADB

`brew cask install android-platform-tools`

测试是否安装成功

`adb devices`

我们可以使用下面的命令模拟按压手机屏幕

`adb shell input swipe 200 300 200 300 1000`

前四个参数是两个位置，最后一个参数是时间，单位ms

## 连接手机

要使用Mac控制我们的安卓手机，首先需要在手机上打开相应的权限。

1. 启动开发者选项

2. 启动USB调试选项

3. 打开USB调试的安全设置（允许通过USB调试模拟点击）

这时，我们就可以通过ADB向我们的安卓手机发送命令啦。嗨起来～

## 跳一跳实现

要实现自动跳一跳，我们有三个主要步骤

1. 确定Player的位置

2. 确定Target的位置

3. 通过距离算出时间

#### 确定Player的位置

由于Player是不变的，这里，我们采用模式匹配的方法来确定Player的位置，python可以很方便的使用OpenCV库来进行图像匹配。

`player_loc = cv.matchTemplate(img, player, cv.TM_CCOEFF_NORMED)`

#### 确定Target的位置
跳一跳小游戏的界面干净整洁，街面上方没有什么乱七八糟的东西，
为了确定Target的位置，我们首先进行边缘检测来让图片更加简单。我们采用了Canny算法。
```
img_blur = cv.GaussianBlur(img,(5,5),0) 
img_canny = cv.Canny(img_blur, 1, 10)
```
处理之后的图像非常简洁，只剩下各个控件的轮廓。

接下来，我们就可以自上向下扫描，找到第一个不为0的点，即为Target的上边界，然后这个点下面会是一段空白，我们在这个点所在列向下扫描，找到第一个不为空白的点，就视为下边界。

#### 通过距离算出时间

通过上面两步我们可以算出两个点之间的距离。那么时间将是距离的一次函数。

