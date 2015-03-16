---
layout: post
title: osx_yosemite_usb
author: ilsec
---
## os x yosemite 的USB安装 

1. 选区8GB以上容量的U盘，在OS X系统下格式化为1个分区，格式为Mac OS 扩
展（日志式），并在选项里选择GUID分区表。

2. 在appstore中下载 *OS X Yosemite*

3. 执行如下命令:

`sudo /Applications/Install\ OS\ X\
Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/u
盘名字 --applicationpath /Applications/Install\ OS\ X\ Yosemite.app
--nointeraction`

4. 安装方式就是开机按住`Option`键，选择U盘安装。

5. 参考链接 [http://www.iplaysoft.com/osx-yosemite-usb-install-drive.html](http://www.iplaysoft.com/osx-yosemite-usb-install-drive.html)
