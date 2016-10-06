title: atom_plug_in
date: 2016-09-13 16:17:58
tags: atom
---

>atom插件安装慢
<!--more-->

#解决atom插件安装慢

参考：https://segmentfault.com/q/1010000004125896

如下操作：

* 打开上面的目录 C:\Users\...\.atom\packages
* 打开命令提示符，切换路径于此
* 将atom插件直接 git 下来, 在插件介绍也一般都有Github下载地址
* 命令提示符 cd 进插件目录， npm install 等待插件依赖安装完成即可
* 重启atom
