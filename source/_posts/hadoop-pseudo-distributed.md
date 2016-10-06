title: hadoop_pseudo_distributed
date: 2016-09-05 17:55:27
tags: hadoop
---
>hadoop伪分布式
<!--more-->

# hadoop伪分布式

## 环境安装
[hadoop环境以及单机模式搭建](http://qwertylevel3.github.io/2016/08/30/hadoop-start/#more)

## 修改配置文件
### 修改/usr/local/hadoop/etc/hadoop/下的core-site.xml
将
```
<configuration>
</configuration>
```
修改为：
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
### 修改/usr/local/hadoop/etc/hadoop/下的hdfs-site.xml：
同理，修改：
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/data</value>
    </property>
</configuration>
```

## namenode格式化
输入：

```
./bin/hdfs namenode -format
```
## 启动守护进程
```
start-dfs.sh
```
## 查看是否启动
可以通过
```
jps
```
命令查看是否启动成功，也可以通过访问http://localhost:50070 查看信息

## 停止守护进程
```
stop-dfs.sh
```
## 其他
如果namenode可以格式化，但是启动守护进程的时候却提示
```
Error: JAVA_HOME is not set and could not be found.
```
这时候可以查看/usr/local/hadoop/etc/hadoop/hadoop-env.sh文件
里面有一句：
```
export JAVA_HOME=${JAVA_HOME}
```
本来这句意思是调用JAVA_HOME环境变量，但是不知为何没用。。。。
需要手动指定JAVA_HOME目录
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
```
具体目录由实际javaHome目录确定

## 参考
* http://dblab.xmu.edu.cn/blog/install-hadoop/
* http://stackoverflow.com/questions/21533725/hadoop-2-2-0-fails-running-start-dfs-sh-with-error-java-home-is-not-set-and-cou
