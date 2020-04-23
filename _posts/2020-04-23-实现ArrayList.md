---
layout:     post
title:      实现ArrayList
subtitle:   实现ArrayList
date:       2020-04-23
author:     JiangKun
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - ArrayList
---

我们知道ArrayList的底层是用数组实现的，那我们不妨来手动实现一下ArrayLlist

首先定义ArrayList所需要的成员变量，用一个int类型的数组，元素的个数（用了多长空间）以及初始容量（先设置为10）

```java
 	public int[] elem;//数组
 	public int usedSize;//有效的数据个数
 	public static final int intCapacity = 10;//初始容量
```
然后定义无参构造方法，初始化一个长度为10的数组，并且当前ArrayList里没有一个元素

```java
	public MyArrayList() {
        this.elem = new int[intCapacity];
        this.usedSize = 0;
    }
```
这样就把初始化的工作完成了，接着来实现ArrayList的一些接口


> 	//增加元素 	
> public void add(int pos,int data) {}
> 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200423155302693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
在增加元素的时候，先判断数组是否为满，如果是满的就需要进行扩容，再判断pos的位置是否合法，如果pos < 0 || pos > this.usedSize就说明pos不合法，此时不能添加，一切条件都允许添加之后，进行挪数据，往后挪一个，this.elem[i+1] = this.elem[i]当pos位置空出来，this.elem[pos] = value；添加完成后别忘记usedSize++;

>  	//打印顺序表
>     public void display() {}
   
打印顺序表就没什么好说的，遍历整个顺序表
>    //判断顺序表当中是否包含某个元素
>     public boolean contains(int toFind) {}

判断顺序表当中是否包含某个元素，也是遍历顺序表，找到返回true,否则返回false
> //查找某个元素对应的位置
>     public int search(int toFind) {}

查找某个元素对应的位置,也是遍历顺序表，找到返回下标,否则返回-1
>   //获取pos位置的元素
>     public int getPos(int pos) {}

获取pos位置的元素的时候，如果顺序表为空，那么pos位置上肯定没有元素，就不用找了；如果不为空，判断pos的位置是否合法，如果pos < 0 || pos >= this.usedSize那么pos就不合法，注意这里和add有点不同，当插入的时候可以等于usedSize
> //给pos位置的元素设置为value
>     public void setPos(int pos,int value) {}

给pos位置的元素设置为value前提是pos存在，如果pos < 0 || pos >= this.usedSize那么pos就不合法，否则进行替换 this.elem[pos] = value
 
> //获取顺序表长度
>     public int size(){}

直接返回this.usedSize

>    //删除第一次出现的关键字toRomove
>     public void romove(int toRomove) {}

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200423161941978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

删除关键字的时候先要拿到关键字的index,如果这个关键字不存在，就不用删除了，否则，将后面的元素每次往前挪一个，从this.usedSize - 1直到index，删除后别忘记this.usedSIze - 1

> //清空顺序表
> public void clear(){}

直接让this.usedSize = 0;

下面是完整代码：

```java
import java.util.Arrays;

import javax.management.RuntimeErrorException;

/**
 * @author PineappleSnow
 * @version 1.0
 * @date 2020/4/19 15:56
 */
public class MyArrayList {

    public int[] elem;//数组
    public int usedSize;//有效的数据个数
    public static final int intCapacity = 10;//初始容量

    public MyArrayList() {
        this.elem = new int[intCapacity];
        this.usedSize = 0;
    }

    //判断是否为满
    private boolean isFull() {
        if(usedSize == this.elem.length) {
            return true;
        }
        return false;

        //return this.usedSize == this.elem.length;
    }

    //增加元素
    public void add(int pos,int data) {
        //扩容
        if(isFull()) {
            this.elem = Arrays.copyOf(this.elem,2*this.elem.length);
        }
        //判断下标
        if(pos < 0 || pos > this.usedSize) {
            //System.out.println("Index Error!");
            return;
        }
        //往后移
        int i = this.usedSize - 1;
        while (i >= pos) {
            this.elem[i+1] = this.elem[i];
            i--;
        }
        this.elem[pos] = data;
        this.usedSize++;
    }

    //打印顺序表
    public void display() {
        for (int i = 0;i < this.usedSize;i++) {
            System.out.print(this.elem[i] + " ");
        }
        System.out.println();
        //System.out.println(Arrays.toString(this.elem));
    }

    //判断顺序表当中是否包含某个元素
    public boolean contains(int toFind) {
        for (int i = 0;i < this.usedSize;i++) {
            if (toFind == this.elem[i]) {
                return true;
            }
        }
        return false;
    }

    //查找某个元素对应的位置
    public int search(int toFind) {
        for (int i = 0;i < this.usedSize;i++) {
            if (toFind == this.elem[i]) {
                return i;
            }
        }
        return -1;
    }

    //获取pos位置的元素
    public int getPos(int pos) {
        if (this.usedSize == 0) {
            throw new RuntimeException("顺序表为空");
        }
        if (pos < 0 || pos >= this.usedSize) {
            throw new RuntimeException("pos非法");
        }
        return this.elem[pos];
    }

    //给pos位置的元素设置为value
    public void setPos(int pos,int value) {
        if (pos < 0 || pos >= this.usedSize) {
            throw new RuntimeException("index非法");
        }
        this.elem[pos] = value;

    }

    //获取顺序表长度
    public int size() {
        return this.usedSize;
    }

    //删除第一次出现的关键字toRomove
    public void romove(int toRomove) {
        int index = search(toRomove);
        if (-1 == index) {
            System.out.println("没有需要删除的数字!");
            return;
        }
        for (int i = index;i < this.usedSize - 1;i++) {
            this.elem[i] = this.elem[i+1];
        }
        this.usedSize--;
    }

        //清空顺序表
        public void clear() {
        this.usedSize = 0;
    }
}
```
