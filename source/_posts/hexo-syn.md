title: hexo_syn
date: 2016-10-06 22:43:58
tags: hexo
---
>hexo博客文件通过git在不同电脑上同步
<!--more-->

# hexo同步
>本来hexo博客一直是用坚果云在不同电脑上同步的,但是最近坚果云总是出现文件锁定的问题,折腾了很久未果.而hexo文件夹下是有一个.gitignore文件的,这似乎隐隐暗示这什么......所以干脆就用github同步得了

## 建立远程库,添加关联

在最新的hexo目录下
```
git init
```

再在github上新建一个git远程仓库,添加本地仓库的关联

中间遇到一个问题,当
```
git add
```
的时候,玩命的提示

```
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）
  （提交或丢弃子模组中未跟踪或修改的内容）

	修改：     themes/freemind (修改的内容)

```
这是因为这个主题是用git clone下来的,说白了,也是一个git 仓库...

可以cd到该目录下,删掉.git文件夹,接触git的控制

然后就可以了

剩下的步骤和一般的git远程库操作一样

## blog的同步

每次写之前首先
```
git pull
```
更新一下

写完之后再
```
git add .
git ci -m "更新内容"
git push origin master
```

***
后来又发现一个问题,上传之后发现theme下面为空文件夹,推测是git的问题,尽管git status查看是正常的

可以用
```
git rm -r --cached .
```

删除git当前的缓存,重新上传一下即可
