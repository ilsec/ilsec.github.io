---
layout: post
title: andird逆向分析工具集
author: ilsec
---

##smali.jar 教程##

说明：
此工具主要功能是将smali文件编译成dex。

命令行：

	java -jar smali.jar [options] [--] [<smali-file>|folder]*


##bksmali.jar 教程##

说明：
此工具主要功能是将dex文件反编译成smali。

命令行：

	java -jar baksmali.jar [options] <dex-file>
	java -jar baksmali.jar -d ./system/framework classes.dex

##jd-gui.jar ##

说明：
此工具主要功能是可视化反编译jar为java。

命令行：

	java -jar jd-gui.jar

##参考链接##

[http://jd.benow.ca/](http://jd.benow.ca/)

[http://bbs.pediy.com/showthread.php?t=200230](http://bbs.pediy.com/showthread.php?t=200230)