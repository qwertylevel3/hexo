title: win32_concole
date: 2016-09-13 19:59:41
tags: c++
---
>非控制台项目中开启控制台并重定向
<!--more-->

# 在非控制台项目中开启控制台

```
AllocConsole();						//打开控制台窗口以显示调试信息
SetConsoleTitleA("console");			//设置标题

freopen("CONIN$", "r+t", stdin); // 重定向 STDIN
freopen("CONOUT$", "w+t", stdout); // 重定向STDOUT
```

***
参考文献:
* http://blog.163.com/gjx_12358@126/blog/static/89536345201372811227318/
