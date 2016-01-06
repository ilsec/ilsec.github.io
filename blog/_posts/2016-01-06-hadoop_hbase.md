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
