---
layout: post
title: sdk设计指南
author: ilsec
---

## SDK 简介

借用百度百科的解释，sdk就是软件开发工具包，软件开发工具包（外语首字母缩写：SDK、外语全称：Software Development Kit）一般都是一些软件工程师为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件时的开发工具的集合。

总体来说，SDK是以 动态库，静态库，源码，framework 的形式存在，并提供文档与API接口方便他人使用的工具集。

## SDK 的元素

在SDK使用阶段，一般情况下SDK是不能变动的，一旦有变动，将会引起使用SDK的程序产生版本更新。
