---
layout: post
title: hadoop install (集群配置)
author: ilsec
---

# hadoop集群

Google的数据中心使用廉价的Linux PC机组成集群，在上面运行各种应用。即使是分布式开发的新手也可以迅速使用Google的基础设施。核心组件是3个：

**GFS（Google File System)**

一个分布式文件系统，隐藏下层负载均衡，冗余复制等细节，对上层程序提供一个统一的文件系统API接口。Google根据自己的需求对它进行了特别优化，包括：超大文件的访问，读操作比例远超过写操作，PC机极易发生故障造成节点失效等。GFS把文件分成64MB的块，分布在集群的机器上，使用Linux的文件系统存放。同时每块文件至少有3份以上的冗余。中心是一个Master节点，根据文件索引，找寻文件块。详见Google的工程师发布的GFS论文。

**MapReduce**

Google发现大多数分布式运算可以抽象为MapReduce操作。Map是把输入Input分解成中间的Key/Value对，Reduce把Key/Value合成最终输出Output。这两个函数由程序员提供给系统，下层设施把Map和Reduce操作分布在集群上运行，并把结果存储在GFS上。

**BigTable**

一个大型的分布式数据库，这个数据库不是关系式的数据库。像它的名字一样，就是一个巨大的表格，用来存储结构化的数据。

# 安装环境

1. 简单集群架构是：一台虚拟机作为Master，一台作为Slave1。

2. 按[hadoop伪集群安装教程](http://ilsec.github.io/blog/2015/12/10/haoop_install.html) 配置好一台虚拟机作为Master。

3. 大致流程如下

```
* 选定一台机器作为 Master
* 在 Master 主机上配置hadoop用户、安装SSH server、安装Java环境
* 在 Master 主机上安装Hadoop，并完成配置
* 在其他主机上配置hadoop用户、安装SSH server、安装Java环境
* 将 Master 主机上的Hadoop目录复制到其他主机上
* 开启、使用 Hadoop
```

# 配置集群

确定Master主机hadoop用户与Slave1主机hadoop用户在同一Group。

## Master 配置

修改*/etc/hostname*,目的是在网络中通过主机名访问。

```
sudo emacs -nw /etc/hostname

Ubuntu -> Master
```

修改*/etc/hosts*,确保*Master*与*Slave1*在网络中相互可见。

```
127.0.0.1       localhost
192.168.1.121   Master
192.168.1.122   Slave1
```

SSH无密码登陆节点

```
cd ~/.ssh

// 生成ssh公私钥对
ssh-keygen -t rsa

// Master无密码登录Master本机的ssh
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

// Master公钥传输到Slave1节点，以使Master可ssh登录Slave1,控制Slave1。
scp ~/.ssh/id_rsa.pub hadoop@Slave1:/home/hadoop/

// Slave1机器上配置Master的公钥
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys

// 测试
ssh Slave1
```

配置Slave

将原来 localhost 删除，把所有Slave的主机名写上，每行一个。例如我只有一个 Slave节点，那么该文件中就只有一行内容： Slave1。

```
cd /usr/local/hadoop/etc/hadoop

emacs slaves

localhost -> Slave1

```

配置core-site.xml,参数可参考[Hadoop参数汇总](http://segmentfault.com/a/1190000000709725)

```
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://Master:9000</value>
</property>
<property>
    <name>hadoop.tmp.dir</name>
    <value>file:/usr/local/hadoop/tmp</value>
    <description>Abase for other temporary directories.</description>
</property>

```

文件hdfs-site.xml，因为只有一个Slave，所以dfs.replication的值设为1。

```
<property>
    <name>dfs.namenode.secondary.http-address</name>
    <value>Master:50090</value>
</property>
<property>
    <name>dfs.namenode.name.dir</name>
    <value>file:/usr/local/hadoop/tmp/dfs/name</value>
</property>
<property>
    <name>dfs.datanode.data.dir</name>
    <value>file:/usr/local/hadoop/tmp/dfs/data</value>
</property>
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
```

文件mapred-site.xml，这个文件不存在，首先需要从模板中复制一份

```
cp mapred-site.xml.template mapred-site.xml

<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
```

文件yarn-site.xml配置

```
<property>
    <name>yarn.resourcemanager.hostname</name>
    <value>Master</value>
</property>
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
```

将Master的配置复制到Slave1上

```
cd /usr/local
rm -r ./hadoop/tmp  # 删除 Hadoop 临时文件
sudo tar -zcf ./hadoop.tar.gz ./hadoop
scp ./hadoop.tar.gz Slave1:/home/hadoop

//在Slave1上执行
sudo tar -zxf ~/hadoop.tar.gz -C /usr/local
sudo chown -R hadoop:hadoop /usr/local/hadoop
```

启动Master上的hadoop服务

```
cd /usr/local/hadoop/
bin/hdfs namenode -format       # 首次运行需要执行初始化，后面不再需要
sbin/start-dfs.sh
sbin/start-yarn.sh
```

通过命令jps可以查看各个节点所启动的进程

```
可以看到Master节点启动了NameNode、SecondrryNameNode、ResourceManager进程
Slave节点则启动了DataNode和NodeManager进程
```

## Slave1 配置

1. 修改*/etc/hostname*,目的是在网络中通过主机名访问。

```
sudo emacs -nw /etc/hostname

Ubuntu -> Slave1
```

2. 修改*/etc/hosts*,确保*Master*与*Slave1*在网络中相互可见。

```
127.0.0.1       localhost
192.168.1.121   Master
192.168.1.122   Slave1
```

----

参考链接：

[Hadoop集群安装配置教程_Hadoop2.6.0/Ubuntu 14.04](http://www.powerxing.com/install-hadoop-cluster/)
