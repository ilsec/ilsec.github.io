---

layout: post

title: unity mono

author: ilsec

---

##[unity mono](https://github.com/Unity-Technologies/mono)

		git clone https://github.com/Unity-Technologies/mono.git

##编译环境

**ubuntu**

##编译步骤

1. 配置环境:**android-ndk-r9** x86版本的，设置环境变量**ANDROID\_NDK_ROOT**。

2. mono源码目录下的**external/buildscripts/build_runtime_android.sh**放到根目录，一会用这个脚本编译。

3. 编译时会出现```perl -w```错误，所以这里手动编译**external/android_krait_signal_handler**,将编译生成的**libkrait-signal-handler.a**文件放到**android-ndk-r9/platforms/android-9/arch-arm/usr/lib**目录下。将**build_runtime_android.sh**中对应的语句注释掉。

4. 编译过程中若出现*libstdc++.so.6: cannot open shared obj*，则需要执行安装命令

```sudo apt-get install lib32stdc++6```	

5. 编译过程中若出现bison未找到，则安装bison

```sudo apt-get install bison```
