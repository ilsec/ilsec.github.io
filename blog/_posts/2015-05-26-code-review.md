---
layout: post
title: code review
author: ilsec
---

##介绍

阅读代码犹如读书，总是会受益匪浅。

代码的艺术，在于将有规律的逻辑总结为最简洁明了的逻辑。

比如：

	if ((flags & ~(RTLD_NOW|RTLD_LAZY|RTLD_LOCAL|RTLD_GLOBAL|RTLD_NODELETE|RTLD_NOLOAD)) != 0) {
	DL_ERR("invalid flags to dlopen: %x", flags);
	return nullptr;
	}

所以在这里，总结几种阅读源码的工具，方便学习。

