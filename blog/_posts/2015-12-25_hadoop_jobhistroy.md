---
layout: post
title: hadoop jobhistroy
author: ilsec
---

## 介绍

Hadoop自带了一个历史服务器，可以通过历史服务器查看已经运行完的Mapreduce作业记录，系统日志，stdout输出日志等信息。

## [Configurations for MapReduce JobHistory Server](http://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-common/ClusterSetup.html)

### 配置 mapred-site.xml

**mapreduce.jobhistory.address**配置

```
<property>
    <name>mapreduce.jobhistory.address</name>
    <value>Master:10020</value>
</property>

```
**mapreduce.jobhistory.webapp.address**配置

```
<property>
    <name>mapreduce.jobhistory.webapp.address</name>
    <value>Master:19888</value>
</property>
```

**mapreduce.jobhistory.intermediate-done-dir**配置

NOTE:
> 正在运行的Hadoop作业记录.

```
<property>
    <name>mapreduce.jobhistory.intermediate-done-dir</name>
    <value>/mr-history/tmp</value>
</property>

```

**mapreduce.jobhistory.done-dir**配置

NOTE:
> 存放已经运行完的Hadoop作业记录.

```
<property>
    <name>mapreduce.jobhistory.done-dir</name>
    <value>/mr-history/done</value>
</property>
```

### 配置 yarn-site.xml

**yarn.log-aggregation-enable**

NOTE:
> 若未配置，则出现 *Aggregation may not enable* 错误

```
<property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
</property>
```

**yarn.nodemanager.remote-app-log-dir**

NOTE:
> 若未配置，则出现 *Aggregation may not be complete* 错误

```
<property>
    <name>yarn.nodemanager.remote-app-log-dir</name>
    <value>logs</value>
</property>
```

------

参考链接：

[hadoop doc](http://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-common/ClusterSetup.html)

[Hadoop jobhistory历史服务器介绍](http://www.aboutyun.com/thread-8150-1-1.html)

[Log aggregation in Yarn
](http://grokbase.com/t/hadoop/user/129asewydw/log-aggregation-in-yarn)
