title: cpp_unit_test_vs2015
date: 2016-09-13 15:57:10
tags: c++
---
>vs2015单元测试
<!--more-->

#vs2015单元测试搭建

参考：http://www.cnblogs.com/xiaoyongwu/p/5289964.html

最后如果提示
```
precompiled object not linked in;image may not run
```
注意在附加依赖项中添加：
```
../test/Debug/stdafx.obj
```
