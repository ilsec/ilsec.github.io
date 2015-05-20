---
layout: post
title: c++宏应用
author: ilsec
---

##使用宏表达式

说明：

这种方式优点是将函数的代码整合成一句宏，增强代码可读性。

	
表达式如下：

	#define MACRO_TEST(VALUE, EXP) \
	 		int VALUE = EXP;

调用方式如下：

 	int v = 0; //这个定义一定要放到全局定义，不然会导致重复定义。
	MACRO_TEST(v, 2 + 8);
	printf("MACRO_TEST %d\n", v);

##使用宏函数

说明：
宏与函数的主要区别在于，宏在代码里调用，编译后是展开成代码的，提高了执行效率也提高了代码体积。函数在代码里调用只是跳转到相应的地址，增加的是堆栈空间，所以效率没宏函数高。宏函数主要用在调用频率低的情况。

宏函数如下：

	#define MACRO_TEST(TYPE, PARAM) ({             \
	TYPE _rc = -1;                                  \
  	do{                                             \
    	if (PARAM == 1){                            \
    	  _rc = PARAM;                              \
    	}                                           \
  	}while(0);                                      \
  	_rc;})

调用方式如下:

	int r = MACRO_TEST(int, 1);
