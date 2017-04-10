---
layout: post
title: sdk设计指南
author: ilsec
---

## SDK 简介
olo
借用百度百科的解释，sdk就是软件开发工具包，软件开发工具包（外语首字母缩写：SDK、外语全称：Software Development Kit）一般都是一些软件工程师为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件时的开发工具的集合。

总体来说，SDK是以 动态库，静态库，源码，framework 的形式存在，并提供文档与API接口方便他人使用的工具集。

## SDK 的元素

在SDK使用阶段，如果有SDK迭代，通常都会引起产品版本的更新，为了尽可能减少SDK的无必要的迭代，我们需要分析SDK的元素都有哪些。

一般情况下，SDK的元素有如下几点：

> SDK的存在形式，有源码方式的，静态库方式的，动态库方式的，还有framework方式的，各有利弊。

> 外部调用接口，简称API，尽量设计的简洁明了。先做到好用，再做到实用。参数一大堆，接口一大堆，事倍功半。不如将复杂参数简单化，接口数量优化，给人点清新感。

> 内部组成结构：日志记录，异常处理，协议，敏感数据的保护，权限管理等，单说异常处理，直接与SDK的稳定性相关，这个很重要。

> 功能元素与业务逻辑元素

## SDK的开发流程图

![](https://github.com/ilsec/ilsec.github.io/blob/master/images/blogs/2016122301.png?raw=true)