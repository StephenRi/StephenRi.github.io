---
layout:     post
title:      "[Learning]C++语法"
subtitle:   "C++中的一些语法使用"
date:       2017-11-16 12:00:00
author:     "Stephen.Ri"
header-img: "img/read-bg.jpg"
catalog: true
tags:
    - Learning
---

## extern 全局变量

在.h头文件中声明extern，如
`extern int tmp;`

在.cpp文件中定义变量，如
`int tmp = 1;`

## [unordered_map](http://en.cppreference.com/w/cpp/container/unordered_map)

`unordered_map` 与`map`类似，都是存储`key-value`的值，可以通过`key`快速检索到`value`。

不同的是`unordered_map`不会根据`key`的大小进行排序，即`map`中的元素是按照二叉搜索树存储的，而`unordered_map`是根据`key`的`hash`值判断元素是否相同。

所以，当需要内部元素自动排序时，使用`map`；不需要排序时，使用`unordered_map`提高查找效率。

**注意**，由于`unordered_map`内部采用`hash`函数实现，因此`find()`函数的**平均查找复杂度**为`O(1)`，**最差查找复杂度**为`O(n)`。

使用实例如下：

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
 
int main()
{
    // Create an unordered_map of three strings (that map to strings)
    std::unordered_map<std::string, std::string> u = {
        {"RED","#FF0000"},
        {"GREEN","#00FF00"},
        {"BLUE","#0000FF"}
    };
 
    // Iterate and print keys and values of unordered_map
    for( const auto& n : u ) {
        std::cout << "Key:[" << n.first << "] Value:[" << n.second << "]\n";
    }
 
    // Add two new entries to the unordered_map
    u["BLACK"] = "#000000";
    u["WHITE"] = "#FFFFFF";
 
    // Output values by key
    std::cout << "The HEX of color RED is:[" << u["RED"] << "]\n";
    std::cout << "The HEX of color BLACK is:[" << u["BLACK"] << "]\n";

    // find value by key
    auto search = u.find("RED");
    if(search != u.end()) {
        std::cout << "Found " << search->first << " " << search->second << '\n';
    }
    else {
        std::cout << "Not found\n";
    }
 
    return 0;
}
```


