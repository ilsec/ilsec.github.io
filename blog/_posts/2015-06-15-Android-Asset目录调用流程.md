---

layout: post

title: Android Asset

author: ilsec

---

##Android读取Assert目录

1. *Context.getAssets().open(“sample.txt”)*

2. *WebView.loadUrl(“file:///android_asset/sample.html”)*

##调用流程

**AssetManager.open**

[frameworks/base/core/java/android/content/res/AssetManager.java](http://code.metager.de/source/xref/android/1.6/frameworks/base/core/java/android/content/res/AssetManager.java)


**openAsset**

[frameworks/base/core/jni/android_util_AssetManager.cpp](http://code.metager.de/source/xref/android/1.6/frameworks/base/core/jni/android_util_AssetManager.cpp)

**Asset\* a = am->open(fileName8, (Asset::AccessMode)mode)**

[frameworks/base/libs/utils/AssetManager.cpp](http://code.metager.de/source/xref/android/1.6/frameworks/base/libs/utils/AssetManager.cpp)

**getEntryInfo**

[frameworks/base/libs/utils/ZipFileRO.cpp](http://code.metager.de/source/xref/android/1.6/frameworks/base/libs/utils/ZipFileRO.cpp)


