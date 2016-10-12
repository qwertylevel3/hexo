title: windows磁盘清理
date: 2016-10-12 20:45:38
tags: windows
---
>windows7 c盘清理和空间释放
<!--more-->

# windows7 C盘清理

## 问题描述
现在用的电脑是120ssd+1T机械盘，ssd放windows7系统。但是现在系统盘莫名只剩下8G可用了。。。遂决定清理一下

## 工具
首先介绍一个比较好的磁盘空间检测工具：SpaceSniffer

有点像ubuntu下面的那个磁盘检测工具，可以快速看出哪个文件占用了较多空间

* 官网：http://www.uderzo.it/main_products/space_sniffer/

* 下载：http://www.uderzo.it/main_products/space_sniffer/download.html

## 检测
检测出几个比较大的文件和文件夹
* hiberfil.sys
>这个是windows休眠用的文件，当windows休眠的时候会将当前内存写入这个文件中，开机了再重新写回（所以这个文件基本和内存一样大），当然我用的是ssd，内存12G，休眠这个功能基本用不上。。。
管理员运行cmd，输入以下代码关闭休眠功能，同时也删除这个文件
```
powercfg -h off
```


* pagefile.sys
>这个是windows的虚拟内存文件，如果电脑内存不够用了，就用这个顶上。当然12G内存如果不是玩某些丧心病狂的游戏怎么也够用了。。。可以用 计算机->属性->高级系统设置->高级->性能->设置->高级->虚拟内存 来修改虚拟内存的大小

* user/XXX/appData/local/temp
>这个是应用程序的temp文件。。。顾名思义。。。删了就好。。。

***

## 参考
* http://www.jb51.net/os/other/41268.html
* http://jingyan.baidu.com/article/ac6a9a5e6269492b653eacf0.html
* http://jingyan.baidu.com/article/f3ad7d0fc0992e09c2345b51.html
