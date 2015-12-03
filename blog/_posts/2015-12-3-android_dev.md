#ANDROID 开发

---
layout: post
title: android develop
author: ilsec
---

### XML命名空间之*XMLNS*解析

* 定义:
	
	XMLNS 是XML Namespaces的缩写，中文名称是XML（标准通用标记语言的子集）命名空间。
	
* 规则:

	使用的规则为
	
	首先定义命名空间xmlns:namespace-prefix="namespaceURI"。Android中xml中的使用是：xmlns:前缀=http://	schemas.android.com/apk/res/应用程序包路径；
	
	然后使用的时候按格式：namespace-prefix（前缀）：属性
	如果使用xmlns，则xmlns的定义必须放在最外层开始的的标记中
	当命名空间被定义之后，所有带有相同前缀的子元素都会与同一个命名空间相关联。避免XML解析器对xml解析时的发送名字冲突，这就是使用xmlns的必要性。
	
	当自定义的View有自己的属性的时候，就用到xmlns来定义一个命名空间。
	
* 例子:(重新定义了my这个命名空间)

		<?xml version="1.0" encoding="utf-8"?> 
    	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     	xmlns:my="http://schemas.android.com/apk/res/demo.view.my" 
     	android:orientation="vertical" 
     	android:layout_width="fill_parent" 
     	android:layout_height="fill_parent" 
     	> 
     	<demo.view.my.MyView 
     	android:layout_width="fill_parent" 
     	android:layout_height="wrap_content" 
     	my:textColor="#FFFFFFFF" 
     	my:textSize="22dp" 
     	/> 
    	</LinearLayout>
    	
### 设置背景色

1. 在*res/drawable*里定义xml，如下:

		<?xml version="1.0" encoding="utf-8"?>
		<shape xmlns:android="http://schemas.android.com/apk/res/android">
 		<gradient 
  		android:startColor="#FF000000"
		android:centerColor="#FF000000"
 		android:endColor="#FF777777"
  		android:angle="45"
 		/>
		</shape>
		
2. SHAPE使用：

	属性      | 含义
	---------| ----
	gradient | 渐变色
	solid    | 填充
	stroke   | 描边
	corners  | 圆角
	padding  | 内容离边界的距离
	
### ANDROID布局 LinearLayout、FrameLayout和AbsoulteLayout

* **LinearLayout（线性布局）**

* **FrameLayout（单帧布局）**

* **RelativeLayout（相对布局）**

* **AbsoluteLayout（绝对布局）**

* **TableLayout（表格布局）**