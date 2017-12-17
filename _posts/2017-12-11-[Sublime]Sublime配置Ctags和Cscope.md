---
layout:     post
title:      "[Sublime]Sublime配置Ctags和Cscope"
subtitle:   "使用Ctags和Cscope实现函数跳转和查看函数调用"
date:       2017-12-11 12:00:00
author:     "Stephen.Ri"
header-img: "img/Sublime-bg.jpg"
catalog: true
tags:
    - Sublime
---

## 什么是Ctags和Cscope

Ctags最先是用来生成C代码的索引（tags）文件，后来扩展成可以生成各类语言的tags。

Ctags工具实现了很强大的函数跳转功能。

Cscope是另一个C语言的浏览工具，通过Cscope可以很方便地找到某个函数或变量被定义和被调用的位置等信息。

## 安装Ctags和Cscope

Ctags和Cscope在Mac的安装可以方便地借助于brew进行。

打开终端，输入以下命令

`brew install ctags`  
`brew install cscope`

当然如果没有安装brew的需要先安装brew。

安装完成后，可以使用下面的命令查看安装结果

`brew list`

 ![安装结果]({{site.baseurl}}/img/imgInBlog/ctags1.png)

## 安装sublime插件

打开sublime，Command + Shift + P，选择Package Control：install Package。

搜索并安装Ctags和Cscope。

 ![安装结果]({{site.baseurl}}/img/imgInBlog/ctags2.png)

## 构建索引

使用sublime右键项目文件夹，分别选择下面两个选项

`Ctags: Rebuild tags`  
`Cscope: Rebuild database`

 ![构建索引]({{site.baseurl}}/img/imgInBlog/ctags3.png)

## 使用插件

构建完索引之后，在打开的文件中，右键想要查询的关键词即可出现下面的选项

`Navigate to Definition`    跳转到函数定义
`Cscope: Look up symbol`    查看符号、函数调用等

 ![插件使用]({{site.baseurl}}/img/imgInBlog/ctags4.png)

## 使用Command + R实现函数快速跳转

在Sublime中，我们可以使用`Command + R`组合键实现函数的快速跳转，在Markdown中还可以在标题中快速跳转。

 ![函数跳转]({{site.baseurl}}/img/imgInBlog/ctags5.png)