---
layout:     post
title:      "[Sublime]Sublime使用技巧：Goto Anything"
subtitle:   "使用Goto Anything查找文件及内容"
date:       2017-12-11 12:00:00
author:     "Stephen.Ri"
header-img: "img/Sublime-bg.jpg"
catalog: true
tags:
    - Sublime
---

## Sublime的查找功能

Sublime的查找功能非常强大，如下图所示，左下角的6个按钮分别是：

1. 正则表达式查找

2. 大小写敏感

3. 全字匹配

4. Wrap：循环查看

5. 查找选中部分

6. 高亮匹配

 ![Sublime查找]({{site.baseurl}}/img/imgInBlog/goto1.png)

## 为什么要用Goto Anything

既然Find功能如此强大，为什么我们还需要Goto Anything呢？

因为Find里面没有**模糊匹配**，也就是说我们如果少一个下划线`_`，Find函数就不能返回正确的结果。这一点在阅读源码的过程中是非常痛苦的。

## Goto Anything

在Mac下，我们可以使用快捷键`Command + P`或者`Command + T`快速调出Goto Anything窗口。当然，也可以在菜单中打开`Menu --> Goto --> Goto Anything`。

打开后，我们直接输入内容将是查找文件。

 ![Sublime Goto]({{site.baseurl}}/img/imgInBlog/goto2.png)

而在文件后添加`#`则变成文件内查找内容，这一步是支持模糊匹配的，好棒棒。。

 ![Sublime Goto]({{site.baseurl}}/img/imgInBlog/goto3.png)

另外，在文件后添加`@`变成跳转到函数定义或者标题；在文件后添加`:`则变成跳转到文件内的第几行。
