title: hadoop-start
date: 2016-08-30 20:16:57
tags: hadoop
---
>hadoop安装以及开发环境搭建
<!--more-->

# hadoop start:

## 安装ssh，配置ssh无密码登陆
```
sudo apt-get install openssh-server
```

ssh localhost可以连接本地,不过要输入密码
```
exit                           # 退出刚才的 ssh localhost
cd ~/.ssh/                     # 若没有该目录，请先执行一次ssh localhost
ssh-keygen -t rsa              # 会有提示，都按回车就可以
cat ./id_rsa.pub >> ./authorized_keys  # 加入授权
```
再用ssh localhost即可直接登陆

## 安装java：
略过

## 安装hadoop：

官网下载hadoop

将下载来的文件夹放入/usr/local/中,并将名字改为hadoop

设定hadoop-env.sh(Java 安装路径)
进入hadoop目录，打开etc/hadoop/目录下到hadoop-env.sh，添加以下信息:
```
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk (视你机器的java安装路而定)
export HADOOP_HOME=/usr/local/hadoop

```
使其生效
```
~$ source /usr/local/hadoop/conf/hadoop-env.sh
```
可以在当前用户目录下的.bashrc文件中引入hadoop的bin和sbin目录，方便调用hadoop命令：
```
export PATH=$PATH:/usr/local/hadoop/bin:/usr/local/hadoop/sbin
```



至此，单机安装成功

可以hadoop version查看是否安装成功

## 用intellJ跑Hadoop:The Definitive Guide里面的例子

参考：

http://jingpin.jikexueyuan.com/article/55098.html

这篇文章其他问题都不大。。
但是edit configurations的时候


“application的名字我们这里取MaxTemperature，然后main class需要输入org.apache.hadoop.util.RunJar，再点击program arguments，填写参数：
其中，第一个参数之前在project structure中填写的jar文件路径，第二个参数是输入文件的目录，第二个参数是输出文件的路径”

这里估计是hadoop没有配置的时候这样填。。。
在我的配置里：
main class里面直接填MaxTemperature即可
program argument无需填写jar文件路径什么的

直接填该例子需要的两个参数，一个是input/，一个是output/即可

## 参考文章

* http://blog.csdn.net/hitwengqi/article/details/8008203
* http://dblab.xmu.edu.cn/blog/install-hadoop/
* http://jingpin.jikexueyuan.com/article/55098.html
