---
layout: post
title: alicrackme review
author: ilsec
---

##说明

能力不够，所以未参加阿里的移动安全挑战赛，但是阻止不了我学习大牛的破解思路，这里就从大牛的破解思路学习android破解。

##参考链接

* [破解目标] (密码: 8k7a)

* [参考文章]

##破解过程

**AliCrackme_1**

图一：

![pic_1_1](http://f8.topit.me/8/06/49/112252130002e49068l.jpg)

图二：

![pic_1_2](http://fb.topit.me/b/9c/53/1122585776234539cbo.jpg)

解题思路：

1. JEB打开Alicrackme_1.apk，左边树选择MainActivity，右边窗口选择Assembly，按下tab键，选择Decompiled java，就会看到逆出来的java源码了。
	
2. 看图一可知：安装apk后看手机日志即可分析出密码，即爆破法。输入012345678，然后看日志，找出对应的数字即可。
	
3. 现在探究此算法的原理，看图一代码逻辑，如下：

code

	1. String v2 = MainActivity.bytesToAliSmsCode(v5, v3.getBytes("utf-8"));
       
	2. if(v4 == null || (v4.equals("")) || !v4.equals(v2))

	3.
	private static String bytesToAliSmsCode(String table, byte[] data) {
        StringBuilder v1 = new StringBuilder();
        int v0;
        for(v0 = 0; v0 < data.length; ++v0) {
            v1.append(table.charAt(data[v0] & 255));
        }

        return v1.toString();
    }

*从上面代码可以看出，`table.charAt(data[v0] & 255)`将密码以ascii码表对应的值作为索引，比如1在ascii码表里对应49，`v1.append`将索引(49)对应的密码表项拼接起来就是加密后的密码了。*

总结：

1. 这道题把日志放开了，说明这道题的目的是测试这种密码保护方式的强度。

2. 密码加密是经过两个表进行加密的，一次是ascii表，一次是自定义的表。

3. 密码与自定义表存贮在图片里，达到隐藏效果。


**AliCrackme_2**

先介绍下调试apk的两种方式：

第一种方式：
>
1. 设备必须开启dbg，一般用模拟器。
2. adb shell am start -D -n 包名/类名 启动app，会看到app界面有等待调试。
3. 启动android_server
4. 启动ida，附加到app上，下好断点，运行app。
5. 启动DDMS，查看app调试端口号，一般为8700,然后执行命令：jdb -connect com.sun.jdi.SocketAttach:hostname=127.0.0.1,port=8700
6. jdb调试必须开ddms，很奇怪啊。
7. ida调试开始。
8. 注意：调试多线程断点有时候命中不了，需要单步在线程之前。调试观察lr返回值位置。

第二种方式：
>
1. 启动apk
2. kill -19 进程pid 暂停进程 可以查看cat/proc/pid/status 进程状态
3. ida 附加，前期我们知道调试器附加，则会被kill掉，所以我们此时在kill下断点。
4. kill -18 进程pid 恢复进程 可以查看cat/proc/pid/status 进程状态
5. ida 运行，会断点会断到kill，然后查看lr，回溯到调用函数，静态分析此调用函数，会回溯到循环校验调试器函数，此时可以处理kill，也可以处理校验函数。
6. http://www.sanwho.com/671.html

解体思路：

图一：

![pic_2_1](http://f5.topit.me/5/d5/63/11252682993f763d55o.jpg)

图二：

![pic_2_2](http://id.topit.me/d/7e/dc/1125268301cd6dc7edo.jpg)

图三：

![pic_2_3](http://i1.topit.me/1/ff/04/1125268303b6c04ff1o.jpg)

图四：

![pic_2_4](http://ia.topit.me/a/e1/ac/1125268305fabace1ao.jpg)

图五：

![pic_2_5](http://fc.topit.me/c/ee/0b/1125268506a6e0beeco.jpg)

图六：

![pic_2_6](http://id.topit.me/d/62/fb/1125268523ecffb62do.jpg)

1. 按第一种方式保证在apk加载so之前启动调试器，防止so中反调试功能起作用。
2. 我们知道，反调试的方式一般是检测`/proc/pid/status`里的*status*与*TracerPid*的状态，所以要在*open*与*fopen*上下断点，这里要注意的是多线程情况，若只在fopen上下断点，多线程检测若用open则断点不会命中。
3. 接下来就是单步调试了，在每次进入*open*的时候检测r0的值是否打开目标SO，如图2-2。这时就是程序加载so的过程。
4. 接下来继续跟踪open，知道r0的值为`/proc/pid/status`，这时就到了反调试线程中的关键代码块。如图2-3.
5. 找到了第4步的关键函数偏移，然后静态(就是ida静态打开so，非调试。)定位到此函数，查找静态中此函数的位置，然后查找调用此函数的位置，如图2-4。
6. 然后继续动态跟踪，就会知道循环判断代码块，如图2-5。
7. 此时，之要修改blx代码为nop就可以防止反调试了。
8. 如图2-6所示，也可以直接在kill函数下断点，当跳转到kill时回溯到调用位置，屏蔽调用kill的代码，也可以防止进程退出。

总结：

这道题考的是通用的反调试方式，不过将直接调用的系统函数比如*kill*以`dlsym(handle, "kill")`的方式引用，这样的作用是静态分析的时候不会出现*kill*引用，增加静态分析难度。

对于自动化屏蔽反调试工具，可以这样做：
首先以so作为载体。
先用inotify监控/proc/pid/status，来判断是否需要屏蔽反调试。
然后hook open，fopen，fgets，ptrace等函数，将返回值都做修改。
最后修改目标apk的smali让此so优先加载，然后将此so打包到目标apk。

**AliCrackme_3**

**AliCrackme_4**

**AliCrackme_5**


[破解目标]: http://pan.baidu.com/s/1bns8LAJ

[参考文章]: http://bbs.pediy.com/showthread.php?t=197235
