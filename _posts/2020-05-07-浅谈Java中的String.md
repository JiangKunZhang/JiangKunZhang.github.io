---
layout:     post
title:      浅谈Java中的String
subtitle:   浅谈Java中的String
date:       2020-05-07
author:     JiangKun
header-img: img/post-bg-miui6.jpg
catalog: true
tags:
    - String
---

**1.创建字符串**

```java
String str = "hello";
System.out.println(str);
String str2 = new String("abcdef");
System.out.println(str2);
char[] val = {'a','b','c','d','e','f'};
String str3 = new String(val);
System.out.println(str3);
```
内存中的布局：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195647638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
对于下面的代码：

```java
String str1 = "hello";
String str2 = str1;
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195833263.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
如果修改str1 那么str2是否会发生变化呢？

```java
String str1 = "hello";
String str2 = str1;
System.out.println(str1);
System.out.println(str2);
str1 = "abc";
System.out.println(str1);
System.out.println(str2);
```

结果显然不是
原因如下：通过str2访问的还是原来的字符串，只不过让str1重新指向另一个字符串
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507200418811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**2.字符串比较相等**

这两个是相等的，因为常量在编译的时候会被运算。
```java
String str1 = "hello";
String str2 = "hel"+"lo";
System.out.println(str1 == str2);//true
```
这两个是不相等的
```java
String str1 = "hello";
String str3 = new String("hel")+"lo";
System.out.println(str3 == str1);//false
```
内存布局如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507201034810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**使用`equals()`方法
直接使用`==`是比较两个引用是否指向同一个对象**

**3.字符串常量池**
通常有两种方法进行实例化

 （1）. **直接赋值**
 （2）. **采用构造方法**

**直接赋值**
如果常量池当中有这个字符串，就不开辟新的空间，直接进行引用

```java
String str1 = "hello";
String str3 = "hello";
System.out.println(str1 == str3);//true
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195149600.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**采用构造方法**
采用构造方法会开辟两块内存空间，比较浪费空间

```java
String str1 = "hello";
String str2 = new String("hello");
System.out.println(str1 == str2);//false
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195454432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**我们可以使用`String`的`intern()`方法手动把String对象加入到字符串常量池中**

```java
String str2 = new String("hello").intern();
String str1 = "hello";
System.out.println(str1 == str2);//比较的是引用
```
这两种情况是一样的
```java
String str1 = "hello";
String str2 = new String("hello").intern();
System.out.println(str1 == str2);//比较的是引用
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195603928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
如果使用`intern()`方法，从现象上看，**手动入池**
当前的这个字符串在常量池当中是否存在？
**如果存在**，把常量池当中的引用  赋值给当前的引用类型。
**如果不存在**，就是指`Scanner`输入的时候，在常量池生成一个引用 然后给引用类型
**不管什么情况，常量池只放一份**

**4.理解字符串不可变**

```java
String str = "hello" ;
str = str + " world" ;
str += "!!!" ;
System.out.println(str);
```
这段代码的输出看起来是正常的，表面上好像是修改了字符串，其实不是，内存是这样的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507201416318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
```java
for (int i = 0; i < 1_0000; i++) {
    //不敢这样拼接字符串   StringBuffer  StringBuilder
    str+=1;
}
```
这种代码会产生大量的临时对象，效率比较低
**1.创建字符串**

```java
String str = "hello";
System.out.println(str);
String str2 = new String("abcdef");
System.out.println(str2);
char[] val = {'a','b','c','d','e','f'};
String str3 = new String(val);
System.out.println(str3);
```
内存中的布局：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195647638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
对于下面的代码：

```java
String str1 = "hello";
String str2 = str1;
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195833263.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
如果修改str1 那么str2是否会发生变化呢？

```java
String str1 = "hello";
String str2 = str1;
System.out.println(str1);
System.out.println(str2);
str1 = "abc";
System.out.println(str1);
System.out.println(str2);
```

结果显然不是
原因如下：通过str2访问的还是原来的字符串，只不过让str1重新指向另一个字符串
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507200418811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**2.字符串比较相等**

这两个是相等的，因为常量在编译的时候会被运算。
```java
String str1 = "hello";
String str2 = "hel"+"lo";
System.out.println(str1 == str2);//true
```
这两个是不相等的
```java
String str1 = "hello";
String str3 = new String("hel")+"lo";
System.out.println(str3 == str1);//false
```
内存布局如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507201034810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**使用`equals()`方法
直接使用`==`是比较两个引用是否指向同一个对象**

**3.字符串常量池**
通常有两种方法进行实例化

 （1）. **直接赋值**
 （2）. **采用构造方法**

**直接赋值**
如果常量池当中有这个字符串，就不开辟新的空间，直接进行引用

```java
String str1 = "hello";
String str3 = "hello";
System.out.println(str1 == str3);//true
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195149600.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**采用构造方法**
采用构造方法会开辟两块内存空间，比较浪费空间

```java
String str1 = "hello";
String str2 = new String("hello");
System.out.println(str1 == str2);//false
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195454432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
**我们可以使用`String`的`intern()`方法手动把String对象加入到字符串常量池中**

```java
String str2 = new String("hello").intern();
String str1 = "hello";
System.out.println(str1 == str2);//比较的是引用
```
这两种情况是一样的
```java
String str1 = "hello";
String str2 = new String("hello").intern();
System.out.println(str1 == str2);//比较的是引用
```
内存布局如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507195603928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
如果使用`intern()`方法，从现象上看，**手动入池**
当前的这个字符串在常量池当中是否存在？
**如果存在**，把常量池当中的引用  赋值给当前的引用类型。
**如果不存在**，就是指`Scanner`输入的时候，在常量池生成一个引用 然后给引用类型
**不管什么情况，常量池只放一份**

**4.理解字符串不可变**

```java
String str = "hello" ;
str = str + " world" ;
str += "!!!" ;
System.out.println(str);
```
这段代码的输出看起来是正常的，表面上好像是修改了字符串，其实不是，内存是这样的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200507201416318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
```java
for (int i = 0; i < 1_0000; i++) {
    //不敢这样拼接字符串   StringBuffer  StringBuilder
    str+=1;
}
```
这种代码会产生大量的临时对象，效率比较低
