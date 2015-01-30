---
layout: post
title: alicrackme review
author: ilsec
---

#说明

能力不够，所以未参加阿里的移动安全挑战赛，但是阻止不了我学习大牛的破解思路，这里就从大牛的破解思路学习android破解。

#参考链接

* [破解目标] (密码: 8k7a)

* [参考文章]

#破解过程

**AliCrackme_1**

图一：

![pic_1_1](http://f8.topit.me/8/06/49/112252130002e49068l.jpg)

图二：

![pic_1_2](http://fb.topit.me/b/9c/53/1122585776234539cbo.jpg)

解题思路：

1. JEB打开Alicrackme_1.apk，左边树选择MainActivity，右边窗口选择Assembly，按下tab键，选择Decompiled java，就会看到逆出来的java源码了。
	
2. 看图一可知：安装apk后看手机日志即可分析出密码，即爆破法。输入012345678，然后看日志，找出对应的数字即可。
	
3. 现在探究此算法的原理，看图一代码逻辑，如下：

code:

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

**AliCrackme_3**

**AliCrackme_4**

**AliCrackme_5**


[破解目标]: http://pan.baidu.com/s/1bns8LAJ

[参考文章]: http://bbs.pediy.com/showthread.php?t=197235