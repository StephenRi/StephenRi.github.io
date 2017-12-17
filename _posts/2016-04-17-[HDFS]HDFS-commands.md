---
layout:     post
title:      "[HDFS]HDFS"
subtitle:   " \"HDFS记录\""
date:       2016-04-17 12:00:00
author:     "Stephen.Ri"
header-img: "img/Bigdata-bg.jpg"
catalog: true
tags:
    - HDFS
---

## HDFS常用命令  

查看磁盘: `df -hl`  
查看大文件: `find . -type f -size +800M  -print0 | xargs -0 du -h`  
更新: `cdhdfs & svn up`  
添加文件: `svn add 3.mkv`  
提交: `svn commit -m "null"`  
编译Hadoop: `bin/ant`  
启动Hadoop: `bin/start-all.sh`  
检查Java进程: `bin/jps`  
关闭安全模式: `bin/hadoop dfsadmin -safemode leave`  
启动raidNode: `bin/start-raidnode.sh`  
创建文件夹: `bin/hadoop dfs -mkdir mylrc`  
上传文件: `bin/hadoop dfs -put ./3.mkv /mylrc/3.mkv`  
下载文件: `bin/hadoop dfs -get /mylrc/3.mkv ./3.mkv`  
编码文件: `bin/hadoop raidshell -raidFile /mylrc/3.mkv /parity lrc`  
查看文件: `bin/hadoop fsck /mylrc/3.mkv -blocks -files -locations`  
删除文件: `find cluster-data -name "*blk_-4064361108693255849_50073*"`  
解码文件: `bin/hadoop raidshell -recoverBlocks /mylrc/3.mkv`



