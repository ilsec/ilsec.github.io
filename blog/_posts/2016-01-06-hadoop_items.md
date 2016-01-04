---
layout: post
title: hadoop items
author: ilsec
---

## 添加节点

修改**hosts**与**hostname**

```
# /etc/hosts
172.16.210.155 Master
172.16.210.156 Slave1
172.16.210.157 Slave2

# /etc/hostname
Slave2
```
添加Master的公钥，使Master可以ssh登陆Slaves。

修改**$hadoop_env/etc/hadoop/hdfs-site.xml**

```
dfs.replication ==>> 2 # 现在有两个Slave.
```

修改**$hadoop_env/etc/hadoop/slaves**

```
Slave1
Slave2
```

将配置同步

```
scp $hadoop_env/etc/hadoop/*.xml hadoop@Slave1:$hadoop_env/etc/hadoop/
scp $hadoop_env/etc/hadoop/*.xml hadoop@Slave2:$hadoop_env/etc/hadoop/
```

删除Slave2的tmp 与 Log

```
rm -rf $hadoop_env/tmp/*
rm -rf $hadoop_env/logs/*
```

启动 **datanode** 与 **tasktracker**

```
hadoop-daemon.sh start datanode
hadoop-daemon.sh start tasktracker
```

均衡节点数据

```
start-balancer.sh
```

## 删除节点

修改**$hadoop_env/etc/hadoop/hdfs-site.xml**

```
<property>
    <name>dfs.hosts.exclude</name>
    <value>/soft/hadoop/conf/excludes</value>
</property>
```

修改**mapred-site.xml**

```
<property>
   <name>mapred.hosts.exclude</name>
   <value>/soft/hadoop/conf/excludes</value>
   <final>true</final>
</property>
```

新建**excludes**文件，添加要删除的hostname

删除节点命令

```
hadoop mradmin -refreshNodes
hadoop dfsadmin -refreshNodes
hadoop dfsadmin -report
```

## 参考链接
----

[添加和删除hadoop集群中的节点](http://www.cnblogs.com/tommyli/p/3418273.html)
