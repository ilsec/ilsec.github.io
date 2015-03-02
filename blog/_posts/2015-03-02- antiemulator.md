---
layout: post
title: Anti Emulator
author: ilsec
---

##说明

* Goldfish

	检测*ro.hardware*,若为*goldfish*，则可判断为虚拟机运行。

	Goldfish是一个虚拟cpu，是一种ARM处理器。Android模拟器通过运行它来运行arm926t指令集（arm926t属于armv5构架）。它的核心内容存放在：arch/arm/mach-goldfish。

* brand

	检测Build.BRAND,如果这句代码在基于ARM CPU的模拟器中运行，其值为”generic”。


##参考链接：

链接一: [检测android模拟器的方法]

链接二:[Android反调试之 AntiEmulator 检测安卓模拟器]

[检测android模拟器的方法]:http://blog.163.com/lyzaily@126/blog/static/4243883720132755797

[Android反调试之 AntiEmulator 检测安卓模拟器]:http://0nly3nd.sinaapp.com/?p=368



