title: ubuntu_fonts
date: 2016-10-09 21:37:21
tags: ubuntu
---
>解决ubuntu下字体缺失问题
<!--more-->

# ubuntu 字体缺失问题解决

## 问题描述
以前没注意,今天打开ubuntu下wps发现汉字都是楷体...尽管上面显示的是宋体....
## 解决方案
从windows的 c:/windows/fonts 下拷贝如下字体
```
simfang.ttf 仿宋体
simhei.ttf 黑体
simkai.ttf 楷体
simsun.ttf 宋体和新宋体，原文件名simsun.ttc
tahoma.ttf tahoma字体
tahomabd.ttf tahoma字体的粗体形式
verdana.ttf verdana字体
verdanab.ttf verdana字体的粗体形式
verdanai.ttf verdana字体的斜体形式
verdanaz.ttf verdana字体的粗体＋斜体形式
```
到ubuntu的 /usr/share/fonts 目录下

再输入
```
fc-cache
```
更新字体缓存

## 参考
http://zhidao.baidu.com/link?url=ZnvwHHE10-0UZ8hPRvLczk0Pd8bY7tgiN3LR27aJaHinyOA0OGhesHv6maJxMqXtjhHGqglFwIh4SZY97XRCqs7gtB_7SvyRiMJEwq3Trby
