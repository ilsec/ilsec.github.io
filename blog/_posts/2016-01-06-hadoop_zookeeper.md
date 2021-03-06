---
layout: post
title: hadoop zookeeper
author: ilsec
---

## 下载 ZooKeeper

[zookeeper-3.5.1-alpha.tar.gz](http://apache.fayea.com/zookeeper/zookeeper-3.5.1-alpha/zookeeper-3.5.1-alpha.tar.gz)

解压到目录 */usr/local/zookeeper/*

```
tar -xf zookeeper-3.5.1-alpha.tar
sudo mv zookeeper-3.5.1-alpha.tar /usr/local/zookeeper/
```

## 配置环境变量

打开 *~/.bashrc*

```
export ZOOKEEPER_HOME=/usr/local/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin:$ZOOKEEPER_HOME/conf
```

## 配置ZooKeeper

修改 *$ZOOKEEPER_HOME/conf/zoo.cfg*

```
dataDir=/usr/local/zookeeper/data # 不能用环境变量
dataLogDir=/usr/local/zookeeper/log

server.1=Master:2888:3888
server.2=Slave1:2888:3888
server.3=Slave2:2888:3888
```

将 *zoo.cfg* 拷贝到 *Slave1* , *Slave2*

```
scp zoo.cfg hadoop@Slave1:/user/local/zookeeper/conf/
scp zoo.cfg hadoop@Slave2:/user/local/zookeeper/conf/
```

在 *$ZOOKEEPER_HOME/data* 新建 **myid**

```
# Master
1

# Slave1
2

# Slave2
3
```

分别在集群中启动

```
zkServer.sh start
```

## 简单操作

四字命令

```
echo ruok | nc Master 2181
echo conf | nc Master 2181
```

连接到ZOOKEEPER服务

```
zkCli.sh -server Master:2181
```


## 参考链接
---

[Hadoop+HBase+ZooKeeper三者关系与安装配置](http://edu.dataguru.cn/thread-241488-1-1.html)

[ZooKeeper-3.3.4集群安装配置](http://blog.csdn.net/shirdrn/article/details/7183503)

[zookeeper-3.5.1-alpha](http://zookeeper.apache.org/doc/r3.5.1-alpha/zookeeperStarted.html)

[基于ZooKeeper大规模集群配置系统概述](http://www.cnblogs.com/cobbliu/archive/2011/11/04/2389009.html)

[ZOOKEEPER的作用](http://blog.sina.com.cn/s/blog_7c5a82970102uy1y.html)
