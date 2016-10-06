title: hexo-start
date: 2015-06-11 16:23:39
tags: hexo
---
>利用免费的hexo，github搭建一个个人主页

<!--more-->

# hexo博客的搭建笔记
***

## 安装node.js
官网下载一路next即可

## 安装git
这个算是程序猿必备工具，就跟吃饭要用筷子一样，这个也不废话了

## 添加ssh
本来这一步也没什么问题，不过遇到一个坑，而且网上很多网友也遇上了，故在此记录

+ 生成ssh：

```
ssh-keygen -t rsa -C "你的邮箱"
```

+ 继续：

```
ssh-agent -s
```

+ 继续：

```
ssh-add ~/.ssh/id_rsa
```

这一步有可能会提示“Could not open a connection to your authentication agent”错误，网上有的解法并不可行，这里给出官方解法（还是官方文档靠谱）
+ 输入：

```
eval $(ssh-agent -s)
```

+ 最后：

```
ssh-add ~/.ssh/id_rsa
```

## 安装hexo
执行如下命令安装hexo
```
npm install -g hexo
```
然后cd到你要初始化的地方初始化

```
hexo init
```

## 生成静态页面

```
hexo generate
```

## 本地启动

```
hexo server
```

启动后浏览器输入http://localhost:4000就可以看到效果了

## 部署到github
到此为止，还是只能在本地看到生成的博客，可以将博客放到github免费提供的页面上，当然也可以自己买一个域名

先在github上新建一个repository，**名字一定要为"名字.github.io"**

打开 hexo 目录下的_config.yml(vim，notepad++都行),这个是个控制文件，有一些比较重要的信息，比如标题，描述，作者等等。找到：

```
deploy:
  	type:
```
一般就在文件最后，将其改为：

```
deploy:
type: git
repository: git@github.com:用户名/名字.github.io.git
branch: master
```
保存退出，执行：

```
npm install hexo-deployer-git --save
```

这个不安装，deploy以后出现命令显示ERROR Deployer not found : github

```
hexo generate
hexo deploy
```
放到github页面上

到此为止，就可以在 http://名字.github.io上看到结果了

##关于写东西
首先：
```
hexo new 文章名
```

然后就可以在hexo\source\_posts里看到新建的一个"文章名.md"，接着用一般的markdown语法编辑器就可以写了，关于markdown语法，百度一下十分钟就能学会，快的很。另外windows下可以用markdownpad来写，即时渲染，很好用。。。

写完了以后：
```
hexo generate
hexo deploy
```

##关于theme

网上找一个喜欢的theme,git命令安装，然后修改_config.yml,找到

```
theme:
```

改为

```
theme: 主题名
```
即可

##关于摘要
在摘要与正文之间用

```
<!--more-->
```

分割即可

##关于多说
可以利用多说添加评论，因为我用的是pacman主题，在多说注册后，修改\hexo\themes\pacman里的_config.yml文件，找到:

```
duoshuo:
enable: false  ## duoshuo.com
short_name:    ## duoshuo short name.
```
修改为：

```
duoshuo:
enable: true  ## duoshuo.com
short_name: 多说注册名字   ## duoshuo short name.
```
即可

##关于七牛图床
可以用七牛提供的免费图床服务，把图片放在七牛上，这样网页加载起来快一点。。。

先在七牛注册一个号(这里是邀请链接[_(:з」∠)_](https://portal.qiniu.com/signup?code=3lhqv6ntfhdn6))，然后弄一个空间，把图片上传上去，引用图片的时候用该图片的外链地址即可，很方便

##其他
+ _config.yml里所有"xxxx: yyyy"这个冒号后面要接一个空格
+ 主题都在hexo/themes/下，要更改theme修改_config.yml即可，快的很。。。
+ npm安装慢的话，用淘宝的源：

```
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"

```
然后用
```
cnpm install -g hexo
```
来安装
