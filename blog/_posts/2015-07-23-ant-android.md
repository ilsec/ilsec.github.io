---
layout: post
title: ant android
author: ilsec
---

##ant 自动编译android工程

1. 生成ant配置文件build.xml

		android update project --path . --subprojects --target android-19

2. Android NDK build with ANT script

		<target name="-pre-build">
   		 <exec executable="${ndk.dir}/ndk-build" failonerror="true"/>
		</target>

		<target name="clean" depends="android_rules.clean">
    		<exec executable="${ndk.dir}/ndk-build" failonerror="true">
        	<arg value="clean"/>
    		</exec>
		</target>

*注要配置ndk.dir环境变量*

---

##参考链接

[http://stackoverflow.com/questions/16219238/how-to-use-ant-to-build-with-android](http://stackoverflow.com/questions/16219238/how-to-use-ant-to-build-with-android)

[http://stackoverflow.com/questions/7432449/android-ndk-build-with-ant-script](http://stackoverflow.com/questions/7432449/android-ndk-build-with-ant-script)

[http://stackoverflow.com/questions/24176846/android-studio-sdk-version-error-after-update](http://stackoverflow.com/questions/24176846/android-studio-sdk-version-error-after-update)