title: ubuntu_boot_script
date: 2016-08-31 21:40:01
tags: ubuntu
---
>ubuntu开机脚本
<!--more-->

# ubuntu 开机脚本配置

ubuntu 中开机脚本为/etc/rc.local
在exit 0之前添加需要运行的脚本语句

注意：
默认环境为！/bin/sh -e
其中-e的意思为失败则退出
另外，有些命令可能无法识别，比如source命令
可以将其改为bash即可（不过有些source之后由于仅仅再当前shell中起效，当这个脚本退出后也许就失效了）
