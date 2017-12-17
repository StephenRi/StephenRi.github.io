---
layout:     post
title:      "[Linux]Linux常用命令"
subtitle:   " \"CentOS command\""
date:       2016-04-19 12:00:00
author:     "Stephen.Ri"
header-img: "img/Linux-bg.jpg"
catalog: true
tags:
    - Linux
---

# 硬件
查看端口占用:   
  `netstat -ntlp`  
查看磁盘使用情况:  
  `df -hl`

# 文件
查找文件  
  `find folder_name -name "*file_name*"`  
查找大文件  
  `find . -type f -size +800M -print0 | xargs -0 du -h`  
修改配置文件  
  `vim .bashrc`  

# GDB调试
进入GDB  
`$ gdb test`  
查看源码  
`(gdb) l line_num`  
添加断点  
`(gdb) b line_num`  
运行代码  
`(gdb) r`  
单步运行  
`(gdb) n`  
继续运行， 直到断点  
`(gdb) c`  
显示变量  
`(gdb) p var_name`  
观察变量  
`(gdb) watch var_name`  
设置参数  
`(gdb) set args arg_1 arg_2`  
