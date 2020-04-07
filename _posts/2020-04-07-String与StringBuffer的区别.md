---
layout:     post
title:      String与StringBuffer的区别
subtitle:   String与StringBuffer的区别
date:       2020-04-07
author:     JiangKun
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - String
    - StringBuffer
---

**String与StringBuffer的区别**
简单地说，就是一个变量和常量的关系.StringBuffer对象的内容可以修改;而字符串对象一旦产生后就不可以被修改，重新赋值其实是两个对象

**StringBuffer内部实现方式和字符串不同**

StringBuffer的在进行字符串处理时，不生成新的对象，在内存使用上要优于串类。所以在实际使用时，如果经常需要对一个字符串进行修改，例如插入，删除等操作，使用StringBuffer要更加适合一些。

**举一个简单的例子：**

> StringBuffer  sbf=new StringBuffer();
for(int =0;i<100;i++){
sbf.append(i);
}

上边的代码的效率很高，因为只创建了一个StringBuffer对象，而下边的代码的效率很低，因为创建了101个对象

> String  str=newString();
for(int i=0;i<100;i++){
str=str+i;
}

而在某些特别情况下， String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuffer 快的：

```java
 String S1 = "This is only a" + " simple" + " test";  
 ```
 ```java
 StringBuffer Sb= new StringBuilder("This is only a").append(" simple").append(" test");
```

 你会很惊讶的发现，生成 String S1 对象的速度简直太快了，而这个时候 StringBuffer 居然速度上根本一点都不占优势。其实这是 JVM 的一个把戏，在 JVM 眼里，这个

```java
 String S1 = "This is only a" + " simple" + "test";
```

 其实就是：

```java
 String S1 = "This is only a simple test";
```

 所以当然不需要太多的时间了。但大家这里要注意的是，如果你的字符串是来自另外的 String 对象的话，速度就没那么快了，譬如：

```java
String S2 = "This is only a";
String S3 = " simple";
String S4 = " test";
String S1 = S2 +S3 + S4;
```

这时候 JVM 会规规矩矩的按照原来的方式去做

