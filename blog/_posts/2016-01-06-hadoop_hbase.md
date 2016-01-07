---
layout: post
title: hadoop hbase
author: ilsec
---

## 安装 HBase 集群

将 *hbase-0.98.16.1-hadoop2-bin.tar* 解压到 */usr/local/hbase*

```
要注意  hadoop2 与 hadoop1 对应的 hbase，不然 hmaster会闪退
```

修改环境变量 *~/.bashrc*

```
export HBASE_HOME=/usr/local/hbase
export PATH=PATH:$HBASE_HOME/bin
```

修改配置文件 *$HBASE_HOME/conf/hbase-site.xml*

```
#hbase-site.xml

<configuration>
    <property>
        <name>hbase.rootdir</name>r
        <value>hdfs://Master:9000/hbase</value>
    </property>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>Master,Slave1,Slave2</value>
    </property>

    <property>
        <name>hbase.master</name>
        <value>Master:60000</value>
    </property>

</configuration>
```

修改配置文件 *$HBASE_HOME/conf/hbase-env.sh*

```
export HBASE_PID_DIR=/usr/local/hbase/pids
export JAVA_HOME=...
export HBASE_MANAGES_ZK=false  
```

修改配置文件 *$HBASE_HOME/conf/regionservers*

```
Slave1
Slave2
```

将配置复制到集群中

```
scp ~/.bashrc hadoop@Slave1:~/.bashrc

scp -r /usr/local/hbase/ hadoop@Slave1:/usr/local/hbase

scp ~/.bashrc hadoop@Slave2:~/.bashrc

scp -r /usr/local/hbase/ hadoop@Slave2:/usr/local/hbase

```

## 启动hbase

1. 启动hadoop
2. 启动zookeeper (前提是：export HBASE_MANAGES_ZK=false,不然需要修改conf/)
3. 启动hbase

## hbase 几本操作

删除表

```
// 先禁用t1，再删除t1
disable 't1'
drop 't1'
```



---

## 参考链接

[HBase分布式集群环境搭建](http://blog.csdn.net/huoyunshen88/article/details/9144039)

[手把手教你配置Hbase完全分布式环境](http://my.oschina.net/lanzp/blog/348116)

[Hbase集群安装配置](http://blog.csdn.net/chenxingzhen001/article/details/7756129)

[hadoop hbase维护问题总结](http://my.oschina.net/u/1169607/blog/347670)

[Hbase无法启动，报：Address already in use](http://my.oschina.net/hanzhankang/blog/130857)

[HBase：HMaster启动后自动关闭](http://blog.chinaunix.net/xmlrpc.php?r=blog/article&id=4008535&uid=26275986)

[hbase Hmaster 进程启动后一会消失的问题解决过程](http://f.dataguru.cn/thread-246372-2-1.html)

[全分布式下安装hbase](http://www.dataguru.cn/article-2674-1.html)

[HBase教程](http://www.yiibai.com/hbase/hbase_create_data.html)
