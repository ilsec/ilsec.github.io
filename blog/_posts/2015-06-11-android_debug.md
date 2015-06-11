---

layout: post

title: android debug

author: ilsec

---

##bugreport

会打印完整的系统日志

	adb bugreport > xxx.log

##strace

很有用的APP调用跟踪

	strace -ff -F -tt -s 200 -o /data/data/debug/strace.log -p [pid]

##其它命令

* vmstat
* df
* netcfg
* logcat
* dumpsys
* top
* local.prop (log.tag.SQLiteStatements=VERBOSE log.tag.SQLiteTime=VERBOSE)	
* 

##参考

[http://www.cnblogs.com/qianxudetianxia/archive/2012/05/14/2497073.html](http://www.cnblogs.com/qianxudetianxia/archive/2012/05/14/2497073.html)

[http://blog.csdn.net/loughsky/article/details/3276482](http://blog.csdn.net/loughsky/article/details/3276482)