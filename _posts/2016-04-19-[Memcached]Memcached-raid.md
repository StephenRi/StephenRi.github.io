---
layout:     post
title:      "[Memcached]Memcached-raid"
subtitle:   " \"Memcached RDP\""
date:       2016-04-19 12:00:00
author:     "Stephen.Ri"
header-img: "img/CPU-bg.jpg"
catalog: true
tags:
    - memcached
---

## Memcached命令

存储命令
`set/add/replace <key> <flag> <exptime> <bytes>`
`<data block>`

读取命令
`get <key>`

状态命令
`stats`
`stats items`
`stats cachedump slab_id limit_num`


## 编译

要配置好cocytus， 首先需要安装基础的类库， 然后执行下面的代码：
`$ ./autogen.sh`
`$ ./configure --enable-cocytus`
`$ make`

如果类库不是按照默认路径安装的， 则需要执行下面的代码：
`$ ./autogen.sh`
`$ ./configure --enable-cocytus CPPFLAGS='-I/path/to/include' LDFLAGS='-L/path/to/libs'`
`$ make`

编译如果出现下面的错误
`cc1: warnings being treated as errors`
可以通过删除makefile里所有的下述语句来解决
`-Werror`

## 配置机器

要使cocytus能够在自己的服务器群上运行， 我们需要配置自己的机器ip和port， 这需要修改下面的文件
`cocytus/shard.conf`
把其中的ip地址修改为自己机器的ip即可， port可以选择默认的。

## 启动cocytus

要启动cocytus， 我们需要根据配置的机器来对启动参数进行调整
-X 组号
-x 节点号
-g 配置文件（shard.conf）
确定了参数， 可以用下面的命令启动
`$ ./memcached -m 64 -X 0 -x 0 -g shard.conf -t 1`

## 修改cocytus

要对cocytus进行测试， 我们可以对cocytus源代码进行一些修改， 以使cocytus更加方便我们测试
1. 修改const.h文件， 包括memsize大小和unitsize大小
2. 修改is_my_sharding函数， 这个函数用来判断接收到的数据是否属于自己

## 使用telnet进行测试

首先确保本机已经安装telnet, 输入命令进入telnet
`$ telnet 220.113.20.123 11211`
执行下面memcached命令
`set a 0 0 4`
`aaaa`

## YCSB测试

生成binding
`mvn -pl com.yahoo.ycsb:memraid-binding -am clean package`

在保证正确安装cocytus和YCSB后， 执行下面的命令
`$./bin/ycsb load memraid -s -P workloads/workloada > outputLoad.txt`
如果有参数， 使用-p添加， 例如
`-p "memcached.hosts=220.113.20.123:5001,220.113.20.124:5002,220.113.20.132:5003,220.113.20.133:5004"`

## RDP check

编码方案完成后， 需要进行以下检查

1. 一个数据盘丢失
2. 一个数据盘和一个行校验盘丢失(先后不同)
3. 一个数据盘和一个对角校验盘丢失(先后不同)
4. 两个数据盘丢失(leader ！= suber)
5. 向suber中set数据(以上各种情况)

6. 一个item包含多个unit
7. 多个item属于一个unit

>在Memcached-rdp中， 不修复校验盘
