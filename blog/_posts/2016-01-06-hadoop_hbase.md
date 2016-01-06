---
layout: post
title: hadoop hbase
author: ilsec
---

## 安装 HBase 集群

将 *hbase-0.98.16.1-hadoop1-bin.tar* 解压到 */usr/local/hbase*

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
        <value>hdfs://Master:60000</value>
    </property>

</configuration>
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
2. 启动zookeeper
3. 启动hbase

---

## 参考链接

[HBase分布式集群环境搭建](http://blog.csdn.net/huoyunshen88/article/details/9144039)

[手把手教你配置Hbase完全分布式环境](http://my.oschina.net/lanzp/blog/348116)

[Hbase集群安装配置](http://blog.csdn.net/chenxingzhen001/article/details/7756129)

[hadoop hbase维护问题总结](http://my.oschina.net/u/1169607/blog/347670)

[Hbase无法启动，报：Address already in use](http://my.oschina.net/hanzhankang/blog/130857)
