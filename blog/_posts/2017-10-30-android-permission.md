---
layout: post
title: android permission
author: ilsec
---

# Android 权限

[权限列表](https://developer.android.com/reference/android/Manifest.permission.html)

---

## Android 权限使用

在**AndroidManifest**根节点添加:

```
<manifest ...>
...
<uses-permission android:name="所需要的权限"/>
...
<application
...
</application>
</manifest>
```

若未申请权限会报**Permission Denial**错误:

```
Caused by: java.lang.SecurityException: Permission Denial: reading com.androintacts.ContactsProvider2 uri content://contacts/data/phones from pid=23763, uid=10036 requires android.permission.READ_CONTACTS
```

## Android 自定义权限

### Android 自定义权限格式

```
<permission android:description=”string resource”
　　　android:icon=”drawable resource”
　　　android:label=”string resource”
　　　android:name=”string”
　　　android:permissionGroup=”string”
　　　android:protectionLevel=[“normal” | “dangerous” |
　　　　　　　　　　　　“signature” | “signatureOrSystem”]
/>
```


* android:description：对权限的描述，比lable更加的详细，介绍该权限的相关使用情况，比如当用户被询问是否给其他应用该权限时。注意描述应该使用的是string资源，而不是直接使用string串。

* android:icon：用来标识该权限的一个图标。

* android:label：权限的一个给用户展示的简短描述。方便的来说，这个可以直接使用一个string字串来表示，但是如果要发布的话，还是应该使用string资源来表示。

* android:name：权限的唯一名字，由于独立性，一般都是使用包名加权限名，该属性是必须的，其他的可选，未写的系统会指定默认值。

* android:permissionGroup： 权限所属权限组的名称，并且需要在这个或其他应用中使用<permission-group>标签提前声明该名称，如果没有声明，该权限就不属于该组。

* android:protectionLevel：权限的等级

    > 关于android:protectionLevel：权限的等级

    > normal  低风险权限，只要申请了就可以使用（在AndroidManifest.xml中添加<uses-permission>标签），安装时不需要用户确认；

    > dangerous  高风险权限，安装时需要用户的确认才可使用；

    > signature  只有当申请权限的应用程序的数字签名与声明此权限的应用程序的数字签名相同时（如果是申请系统权限，则需要与系统签名相同），才能将权限授给它；

    > signatureOrSystem  签名相同，或者申请权限的应用为系统应用（在system image中），与signature类似，只是增加了rom中自带的app的声明 ，尽量不要使用该选项，因为signature已经适合绝大部分的情况。

例子

```
<permission
        android:name="personprovider.permission.read"
        android:description="@string/personprovider_permission_read_desstring"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/personprovider_permission_read_labestring"
        android:protectionLevel="normal"/>
    <permission
        android:name="personprovider.permission.write"
        android:description="@string/personprovider_permission_write_desstring"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/personprovider_permission_write_labestring"
        android:protectionLevel="normal"/>
```

### Android 自定义权限组

```
<permission-group android:description="string resource"
            android:icon="drawable resource"
            android:label="string resource"
            android:name="string"/>
```

* android:description  这个属性用于给权限组定义一个用户可读的说明性文本。这个文本应该比标签更长、更详细。这个属性必须要引用一个字符串资源，跟label属性不一样，它不能够使用原生的字符串。

* android:icon  这个属性定义了一个代表权限的图标。这个属性要使用包含图片定义的可绘制资源来定义。

* android:label  这个属性给权限组定义了一个用户可读的名称。为了开发方便，在开发时，可以直接使用原生的字符串来设置这个属性。但是，当应用程序正式发布时，应该使用字符串资源来设置，以便能够像用户界面中其他的字符串一样能够被本地化。

* android:name 这个属性定义了权限组的名称，它是能够分配给<permission>元素的permissionGroup属性的名称。

例子

```
<permission-group
        android:name="personprovider.permission-group"
        android:description="@string/personprovider_permission_group_desstring"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/personprovider_permission_group_labelstring"/>

    <permission
        android:name="personprovider.permission.read"
        android:description="@string/personprovider_permission_read_desstring"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/personprovider_permission_read_labestring"
        android:permissionGroup="personprovider.permission-group"
        android:protectionLevel="normal"/>
    <permission
        android:name="personprovider.permission.write"
        android:description="@string/personprovider_permission_write_desstring"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/personprovider_permission_write_labestring"
        android:permissionGroup="personprovider.permission-group"
        android:protectionLevel="normal"/>
```

### 使用自定义权限

申请权限

```
<uses-permission android:name="personprovider.permission.read"/>
   <uses-permission android:name="personprovider.permission.write"/>
```

在宿主上声明需要申请访问权限

```
<provider
    android:name=".PersonProvider"
    android:authorities="com.whoislcj.testsqlite.personprovider"
    android:enabled="true"
    android:exported="true"
    android:readPermission="personprovider.permission..read"
    android:writePermission="personprovider.permission..write"/>
```

## Android 6.0 之动态权限

在Android6.0上，对于危险权限而言，仅仅在**AndroidManifest**上申请，已经无法获取到权限，而且还会引起崩溃，这就出现了动态权限申请。
之所以会有与用户进行交互的动态权限，大概是为了让用户了解APP所需的侵犯用户隐私的权限有哪些，是否允许APP去侵犯。

Android6.0规定的危险权限有下面这些：

|Permission Group|Permissions|
|:---|:---|
|CALENDAR|READ_CALENDAR|
||WRITE_CALENDAR|
||CAMERA|
|CAMERA|CAMERA|
|CONTACTS|READ_CONTACTS|
||WRITE_CONTACTS|
||GET_ACCOUNTS|
|LOCATION|ACCESS_FINE_LOCATION|
||ACCESS_COARSE_LOCATION|
||RECORD_AUDIO|
|PHONE|READ_PHONE_STATE|
||CALL_PHONE|
||READ_CALL_LOG|
||WRITE_CALL_LOG|
||ADD_VOICEMAIL|
||USE_SIP|
||PROCESS_OUTGOING_CALLS|
|SENSORS|BODY_SENSORS|
|SMS|SEND_SMS|
||RECEIVE_SMS|
||READ_SMS|
||RECEIVE_WAP_PUSH|
||RECEIVE_MMS|
|STORAGE|READ_EXTERNAL_STORAGE|
||WRITE_EXTERNAL_STORAGE|


### 动态权限的申请方式

```
ActivityCompat.requestPermissions(final @NonNull Activity activity,
            final @NonNull String[] permissions, final int requestCode)
```

# 参考
[Android权限管理之Permission权限机制及使用](http://www.cnblogs.com/whoislcj/p/6072718.html)
