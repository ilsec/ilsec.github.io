---
layout: post
title: hadoop install
author: ilsec
---

##环境

1. 实验系统选择ubuntu比较好。
2. hadoop版本选择最新的.

##hadoop部署

hadoop安装的用户名最好是haoop。

####创建hadoop用户

	sudo useradd -m hadoop -s /bin/bash

####修改密码

	sudo passwd hadoop

####添加管理员权限

	sudo adduser hadoop sudo

####切换用户到*hadoop*

####安装SSH Server

hadoop集群是靠ssh管理的。

	sudo apt-get install openssh-server
	
添加无密码登录

	ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
	
	cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
	
	ssh localhost

####安装Java环境

ubuntu 可选择OpenJDK，如果hadoop版本是2.7＋，选择jdk6会报错，应该选jdk7

安装jdk7

	sudo apt-get install openjdk-7-jre openjdk-7-jdk

配置环境变量

	sudo emacs -nw ~/.bashrc
	
	export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
	
	source ~/.bashrc
	
	echo $JAVA_HOME

####安装hadoop2
hadoop2 增加了新特性。

下载hadoop2

	wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-2.7.1/hadoop-2.7.1.tar.gz 
	
解压安装hadoop

	sudo tar -zxvf ./hadoop-2.7.1.tar.gz -C /usr/local
	cd /usr/local
	sudo mv ./hadoop-2.7.1/ ./hadoop
	sudo chown -R hadoop:hadoop ./hadoop
	
测试hadoop

	cd ./hadoop
	./bin/hadoop
	
配置hadoop环境变量

	sudo emacs -nw ~/.bashrc
	export HADOOP_HOME=/usr/local/hadoop/bin:/usr/local/hadoop/sbin
	export $PATH:$HADOOP_HOME
	
配置hadoop/etc/hadoop/Hadoop-env.sh

	export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64

配置hadoop/etc/hadoop/core-site.xml

	<configuration>
    	<property>
        	<name>fs.default.name</name>
        	<value>hdfs://localhost:9000</value>
        	<description>test.</description>
    	</property>
	</configuration>
	
配置hadoop/etc/hadoop/hdfs-site.xml

	<configuration>
    	<property>
        	<name>dfs.replication</name>
        	<value>1</value>
    	</property>
    </configuration>
    
配置hadoop/etc/hadoop/mapred-site.xml

	<configuration>
    	<property>
        	<name>mapred.job.tracker</name>
        	<value>localhost:9001</value>
    	</property>
    </configuration>
    
namenode的格式化操作:

	hdfs namenode -format
	
开启NameNode与DataNode守护进程

	start-dfs.sh
	
配置hdfs，设置伪分布实例

	hdfs dfs -mkdir -p /user/hadoop
	hdfs dfs -mkdir input
	hdfs -put $HADOOP_HOME/etc/hadoop/*.xml input
	hdfs dfs -ls input
	
hdfs删除目录操作

	hdfs dfs -rm -r /user/hadoop/output
	
关闭hadoop

	stop-dfs.sh


---
##参考链接

[http://hadoop.apache.org](http://hadoop.apache.org/releases.html)

[Hadoop安装教程_单机/伪分布式配置_Hadoop2.6.0/Ubuntu14.04](http://www.powerxing.com/install-hadoop/)