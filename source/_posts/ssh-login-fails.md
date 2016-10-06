title: ssh_login_fails
date: 2016-08-31 16:50:54
tags: ubuntu
---
>rsa ssh login fails with "sign_and_send_pubkey: signing failed: agent
  refused operation" error的解决办法
<!--more-->

## 问题描述
安装hadoop的时候设置了ssh的无密码登陆，今天重新折腾了hexo，git，似乎是因为又添加了ssh key的原因，当ssh localhost的时候玩命的提示：
```
sign_and_send_pubkey: signing failed: agent refused operation" error
```

后来找到了解决方法：

```
ssh-add
```

这样ssh localhost是解决了，不过hexo提示没有权限（没有了ssh key）
其实正确的ssh-keygen多个key的时候不要一直回车，需要设置不同的名字
然后用ssh-add命令添加key
具体可以参考：
http://www.cnblogs.com/fanyong/p/3962455.html


## 参考文献
× https://www.mail-archive.com/touch-packages@lists.launchpad.net/msg161008.html
* http://www.cnblogs.com/fanyong/p/3962455.html
