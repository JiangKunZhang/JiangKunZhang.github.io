---
layout:     post
title:      Synchronized
subtitle:   Synchronized
date:       2020-06-21
author:     JiangKun
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - Synchronized
---

synchronized的底层是使用操作系统的mutex lock实现的。<br>
当线程释放锁时，JMM会把该线程对应的工作内存中的共享变量刷新到主内存中<br>
当线程获取锁时，JMM会把该线程对应的本地内存置为无效。从而使得被监视器保护的临界区代码必须从主内存中读取共享变量<br>
synchronized用的锁是存在Java对象头里的。<br>
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200620204232801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
synchronized同步快对同一条线程来说是可重入的，不会出现自己把自己锁死的问题；<br>
同步块在已进入的线程执行完之前，会阻塞后面其他线程的进入。

**语法：**
1. 修饰方法定义 同步方法
2. 修饰代码块     同步代码块
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200620211354292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

凡是 Java 的对象中，都自带着一把锁
锁被称为：1.同步锁  2 内部锁  3.对象锁  4.监视器锁（monitor lock）

<font color = "red">Synchronized 最主要是实现原子性 实际上也可以保证一定的可见性和重排序 </font>

**理解Synchronized保证原子性**
-
 - 1. <font color = "red">只要锁不放开，自己被切换下来也没用，别的线程抢到CPU 也没用</font>
 - 2. <font color = "red">抢不到锁，1.放弃 CPU 2. 放弃抢 CPU 的资格（因为抢不到锁，抢到 CPU 也没用，干脆就别抢了）
进入BLOCKED （专门位synchronized设计的一种状态）</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200620210331642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
 1. 释放锁并不意味着释放CPU
2. 释放锁之后也可以保证公平性，让先等待的线程请求到锁	
     也可以不保证公平性，我只管释放，把所有的线程都释放成就绪态，大家再去抢

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020062021121374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
<font color = "red">Synchronized 修饰的同步代码块</font>是起到什么作用的。<font color = "red">让多个线程之间产生同步</font>
怎么产生同步效果 <font color = "red">通过锁！</font><br>

<font color = "red">Synchronized 修饰方法</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020062021162014.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200620214114695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200620212033335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

**理解Synchronized 保证可见性**
-
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200620212936300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
中间一段位置是临界区 保障原子性，前提是线程之间得互斥 得互相同步
可以局部保证可见性

 - <font color = "red">请求锁成功的一瞬间，JVM 规定 刷新线程的工作内存，保证以后读到的数据都是最新的</font>
 - <font color = "red">在释放锁的一瞬间，JVM 规定 必须把线程的工作内存的数据，刷新回主内存，保证其他线程读到你最新的修改</font>
 - <font color = "red">中间的时候不做可见性保证</font>

**理解Synchronized 达到代码重排序**
-

 <font color = "red">A / B / C 内部随便重排序</font>

 - 但 A 的代码不能重排序到 B 内部 或者 C 内部

 - C 的代码不能重排序到 A 内部 或者 C 内部

 - C 的代码不能重排序到 A 内部 或者 B 内部

三段内部各自可以重排序

**Synchronized 语法演示**
```java
/**
 * 演示 synchronized 关键字的语法
 */
public class SynchronizedSyntaxDemo {
    /**
     * 修饰方法
     */
    public synchronized void method() {
    }

    public static synchronized void staticMethod() {
    }

    /**
     * 修饰代码块
     */
    public void otherMethod() {
        // 括号里跟着指向对象的引用，引用不能是 null
        // 只要是引用就行，没有任何要求
        Object o = new Object();
        synchronized (o) {
        }

        synchronized (this) {
        }

        // 反射中，指向当前类对象
        synchronized (SynchronizedSyntaxDemo.class) {
        }
    }
}
```
