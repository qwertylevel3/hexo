title: hadoop_vmware_distributed
date: 2016-09-06 19:52:53
tags: hadoop
---
>hadoop虚拟机分布式环境搭建
<!--more-->

# hadoop分布式环境搭建(虚拟机)

## 环境

* cpu:Intel® Core™ i5-3230M CPU @ 2.60GHz × 4
* 内存:12G
* 系统:Ubuntu 16.04.1 LTS 64 位

## 安装vmware
vmware官网下载workstation for linux安装即可(网上注册码居然到处都是...)，本文用到的版本为12

## 设置虚拟机
创建一个新的ubuntu虚拟机(当前设置的是1G内存，单核cpu，20G磁盘空间),命名为master,
操作系统还是用的Ubuntu 16.04.1 LTS 64 位,
[安装java，hadoop，创建一个hadoop用户，配置ssh无用户登陆](http://qwertylevel3.github.io/2016/08/30/hadoop-start/#more)，最后复制2个master虚拟机，命名为node0，node1

## 修改网络配置
参阅网上的有些教程，需要另外配置网络使得各个虚拟机联通。不过当前vmware虚拟机设置后各个虚拟机是可以ping通的，在这里就无需另外配置网络联通了。

可以通过：
```
ifconfig
```
命令查看网络情况

在3台虚拟机中查看ip,再这里，3台虚拟机ip分别为
* master:172.16.148.128
* node0:172.16.148.130
* node1:172.16.148.131

### 修改hosts文件
关于hostname和hosts文件，可以参考[这篇文章](http://liuleijsjx.iteye.com/blog/427900).
在这里，修改/etc/hosts：
```
127.0.0.1	localhost
127.0.1.1	ubuntu
172.16.148.130	node0
172.16.148.131	node1
172.16.148.128	master

```
这样就为这几个ip设置了几个‘别名’，方便后面设置

### 配置ssh
master的无密码登陆已经设置完成，需要将公钥提供给node0，node1这两个子节点

首先将公钥添加到authorized_keys中
```
sudo cat id_rsa.pub >> authorized_keys
```

然后将authorized_keys提供给子节点，(这里假设node子节点中已经有名为hadoop的用户)
这里用到了一个scp的远程cp命令，可以参考[这个](http://www.cnblogs.com/hitwtx/archive/2011/11/16/2251254.html)

```
sudo scp authorized_keys hadoop@node0:~/.ssh       
sudo scp authorized_keys hadoop@node1:~/.ssh
```
如果成功，可以通过
```
ssh node0
```
查看是否可以无密码登陆

## 配置hadoop
假定hadoop安装在/usr/local/下，需要修改/usr/local/hadoop/etc/hadoop/下的几个文件：

* core-site.xml
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://master:9000</value>
    </property>
</configuration>
```
* hdfs-site.xml
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/usr/local/hadoop/hdfs/name</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/usr/local/hadoop/hdfs/data</value>
    </property>
</configuration>
```
* mapred-site.xml(这里目录下有一个mapred-site.xml.templeate文件，不知是做什么的)
```
<configuration>
	<property>
		<name>mapred.jop.tracker</name>
		<value>master:9001</value>
	</property>
</configuration>
```
* slaves
```
node0
node1
```

然后这几个文件在子节点中也弄一份（其实也可以在一台虚拟机上设置好，然后复制这个虚拟机）

## 启动
格式化namenode节点
```
hdfs namenode -format
```
启动
```
start-dfs.sh
```
同样可以通过
```
jps
```
命令来查看hdfs，在这里，master中：
```
5362 sun.tools.jps.Jps
5064 NameNode
5247 SecondaryNameNode
```
node0中：
```
4867 DataNode
5020 sun.tools.jps.Jps
```

可以：
```
stop-dfs.sh
```
来停止

***

## 参考文献
* http://www.weixuehao.com/archives/577
* http://liuleijsjx.iteye.com/blog/427900
* http://www.cnblogs.com/hitwtx/archive/2011/11/16/2251254.html
