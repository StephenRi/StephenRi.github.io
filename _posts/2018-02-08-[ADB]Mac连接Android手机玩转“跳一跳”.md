---
layout:     post
title:      "[ADB]Mac连接Android手机玩转跳一跳"
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

## 优化方向

优化我觉得主要在确定Target位置方面。

1. 部分Target存在中心小白点，这种可以先进行模式匹配得到小白点的位置。

2. 有些Target的上表面突然比较复杂，这种一般是一个菱形，不难看出它的边界从上到下是先变宽后变窄的，我们可以找到最宽处作为他的横坐标。再找中心作纵坐标。

3. 时间-距离的函数，可以通过最小二乘法拟合一条最佳曲线。

总之，代码比较简单，只是实现了最基础的功能，可以优化的地方还有很多。

## 代码

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import os
import cv2 as cv
import numpy as np
import time
import random

def get_player_loc(img, player):
    player_loc = cv.matchTemplate(img, player, cv.TM_CCOEFF_NORMED)
    player_loc_min_val, player_loc_max_val, player_loc_min_loc, player_loc_max_loc = cv.minMaxLoc(player_loc)
    player_loc_center = (player_loc_max_loc[0] + 29, player_loc_max_loc[1] + 130)
    return player_loc_max_loc, player_loc_center


def get_target_loc(img, player_loc_max_loc):
    #Canny edge detection
    img_blur = cv.GaussianBlur(img,(5,5),0) 
    img_canny = cv.Canny(img_blur, 1, 10)
    img_width, img_height = img_canny.shape

    #sometimes, the player is higher than target, so erase the player from the img
    for i in range(player_loc_max_loc[1], player_loc_max_loc[1] + 147):
            for j in range(player_loc_max_loc[0], player_loc_max_loc[0] + 58):
                img_canny[i][j] = 0

    #get the upper boundary of target
    #scan the gray from y = 400 to bottom, the first y that makes img(x, y) != 0 is the upper boundary
    y_up = np.nonzero([max(row) for row in img_canny[400:]])[0][0] + 400
    x_up = int(np.mean(np.nonzero(img_canny[y_up])))

    #get the lower boundary of target
    #scan from y = y_up + 50 to bottom, the first y that makes img(x_up, y) != 0 is the lower boundary 
    y_low = y_up + 60
    for row in range(y_up + 10, y_up + 200):
        if img_canny[row, x_up] != 0:
            y_low = row
            break

    #get the center of target
    target_loc = (x_up, (y_up + y_low) // 2)
    return target_loc
    

def press_screen(player_loc_center, target_loc):
    #compute the distance
    distance = ((target_loc[0] - player_loc_center[0]) ** 2 + (target_loc[1] - player_loc_center[1]) ** 2) ** 0.5
    press_time = int(100 + distance * 2)
    print distance

    #random the press positon
    rand = random.randint(200,500)
    cmd = ("adb shell input swipe %i %i %i %i " + str(press_time)) % (rand, rand, rand, rand)
    os.system(cmd)



def main():
    round = 0
    while True:
        print round

        #get the screencap
        cmd = "adb shell screencap -p /sdcard/screen.png; wait; adb pull /sdcard/screen.png"
        os.system(cmd)
        img = cv.imread('screen.png', 0)
        player = cv.imread('player.png', 0)

        #get the location of player
        player_loc_max_loc, player_loc_center = get_player_loc(img, player)
        print "player_loc: ", player_loc_center

        #get the location of target
        target_loc = get_target_loc(img, player_loc_max_loc)
        print "target_loc: ", target_loc

        #compute the distance and press the screen
        press_screen(player_loc_center, target_loc)

        time.sleep(1.5)

if __name__ == "__main__":
    main()

```