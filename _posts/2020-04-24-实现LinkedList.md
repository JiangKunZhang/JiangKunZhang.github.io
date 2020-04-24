---
layout:     post
title:      实现LinkedList
subtitle:   实现LinkedList
date:       2020-04-24
author:     JiangKun
header-img: img/post-bg-keybord.jpg
catalog: true
tags:
    - LinkedList
---

我们知道LinkedList的实现是用链表实现,那我们不妨来手动实现一下LinkedList
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424145203327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

先把链表的一个节点定义出来
```java
class Node {
    int data;
    Node next;

    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```
接着我们来实现一些接口

> 	//头插法
> 
> 	public void addFirst(int data) {}

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424150105898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

在头插之前呢，先判断整个链表是不是空的，也就是this.head == null如果是这样，那就直接把头节点的引用改为此节点，否则进行头插的时候，先让要插入的节点的next指向头节点，然后把头节点变为此节点

> 	//尾插法
> 
> 	public void addLast(int data) {}

在尾插之前呢，先判断整个链表是不是空的，也就是this.head == null如果是这样，那就直接把头节点的引用改为此节点，否则进行尾插的时候，要找到最后一个节点，然后把此节点链在后面

>	//查找index位置的前一个节点
> 
>    public Node searchIndex(int index) {} 

查找index的前一个节点，那只需要从头节点移动index-1次就找到了，同时也要对index进行合法性判断

> //任意位置插入，第一个数据节点为0号下标
> 
>    public void addIndex(int index,int data) {} 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424150012665.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

任意位置插入的时候，**如果这个index = 0，那么就是头插；如果index = this.size()，那么就是尾插；否则就要找到index的前一个结点进行插入**，让当前节点的next指向perv.next；然后prev.next=此节点

> //链表的长度
> 
>    public int size() {}

链表长度就没啥好说的，直接遍历一遍，节点为null就结束

> //查找是否包含关键字key在单链表中
> 
>    public boolean contains(int key) {}

contains也是将链表遍历，如果node.data == key;返回true，否则返回false

> //查找关键字key的前驱
> 
>    public Node findPrev(int key) {} 

定义一个prev节点从头开始遍历，循环的时候，条件是prev.next != null，因为最后一个节点的next是null，循环体就是比对prev.next.data == key，如果等于，返回prev，循环结束后还没有找到就return false

>  //删除第一次出现关键字为key的节点
> 
>    public void romove(int key) {} 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424150354655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)
如果链表为空就说明里面没有元素，那就更不可能删除其中任意一个元素；如果要删除的是头节点，那就不用找先驱；如果不是头节点，找到key的先驱，然后prev.next就是要删除的del节点，让prev.next = del.next,就完成删除了

> //删除所有节点为key的节点
> 
>    public void romoveAllKey(int key) {} 

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020042415340758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5na3VuMDMzMQ==,size_16,color_FFFFFF,t_70)

首先我们要明确一个思路，如果要删除的节点是头节点，那我们不能先删除头节点，如果先删除头节点，要是头节点后面紧跟了一个还需要删除的，那就出bug了，**所以我们先把除了头节点以外的删除干净，然后再看要不要删除头节点**。删除后面的节点时，定义两个节点，prev,cur(代表要删除的节点),初始化的时候，prev指向头节点，Node prev= this.head; cur指向头结点的下一个，Node cur = this.head.next; 看cur.data等不等于key，如果等于key，进行删除prev.next = cur.next; 然后cur后移，防止是连续的要删除 cur = cur.next; cur为空就删除了除头节点之外的。执行完之后，判断头结点。
> //打印单链表
> 
>    public void display() {}

遍历单链表，输出每一个节点的data即可

> //释放内存
> 
>    public void clear(){}

当一个对象没有人引用的时候，就会被jvm回收掉，所以我们把this.head = null;没有人引用头节点，那头节点就被回收了；回收了以后，头节点的下一个节点也没有人引用，也被回收掉；以此类推......整个链表被clear

下面是具体的**实现代码**：

```java
package MyLinkedList;

/**
 * @author PineappleSnow
 * @version 1.0
 * @date 2020/4/24 13:17
 */

class Node {
    int data;
    Node next;

    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}
public class MyLinkedList {
    //保存头结点的引用
    public Node head = null;

    //头插法
    public void addFirst(int data) {
        Node node = new Node(data);
        //头为空说明这是空链表
        if (this.head == null) {
            this.head = node;
            return;
        }
        node.next = this.head;
        this.head = node;
    }

    //尾插法
    public void addLast(int data) {
        Node node = new Node(data);
        //头为空说明这是空链表
        if (this.head == null) {
            this.head = node;
            return;
        }
        //找到最后一个节点
        Node cur = this.head;
        while (cur.next != null) {
            cur = cur.next;
        }
        cur.next = node;
    }

    //查找index位置的前一个节点
    public Node searchIndex(int index) {
        //对index合法性进行判断
        if (index < 0 || index > this.size()) {
            throw new RuntimeException("index位置非法");
        }
        Node cur = this.head;
        while ((index-1) != 0) {//走index-1次
            cur = cur.next;
            index--;
        }
        return cur;
    }

    //任意位置插入，第一个数据节点为0号下标
    public void addIndex(int index,int data) {
        if (index == 0) {
            addFirst(data);
            return;
        }
        if (index == this.size()) {
            addLast(data);
            return;
        }
        Node node = new Node(data);
        //先找到  index位置的前一个节点的地址
        Node prev = searchIndex(index);
        //开始插入
        node.next = prev.next;
        prev.next = node;
    }

    //链表的长度
    public int size() {
        int count = 0;
        Node cur = this.head;
        while (cur != null) {
            cur = cur.next;
            count++;
        }
        return count;
    }

    //查找是否包含关键字key在单链表中
    public boolean contains(int key) {
        Node cur = this.head;
        while (cur != null) {
            if(cur.data == key) {
                return true;
            }
            cur = cur.next;
        }
        return false;
    }

    //查找关键字key的前驱
    public Node findPrev(int key) {
        Node prev = this.head;
        while (prev.next != null) {
            if (prev.next.data == key) {
                return prev;
            }else{
                prev = prev.next;
            }
        }
        return null;
    }

    //删除第一次出现关键字为key的节点
    public void romove(int key) {
        //单链表为空，不用删除了
        if (this.head == null) {
            return ;
        }
        //删除的是不是头节点
        if (this.head.data == key) {
            this.head = this.head.next;
            return;
        }
        //找到先驱节点
        Node prev = findPrev(key);
        if (prev == null) {
            System.out.println("你要删除的节点就不存在！");
            return;
        }
        //开始删除
        Node del = prev.next;//del是要删除的节点
        prev.next = del.next;
    }

    //删除所有节点为key的节点
    public void romoveAllKey(int key) {
        Node prev = this.head;
        Node cur = prev.next;//代表要删除的节点
        //先不考虑头节点是不是要删除的
        while (cur != null) {
            //等于key就删除
            if (cur.data == key) {
                prev.next = cur.next;//删除
                cur = cur.next;//cur后移
            }else {//不等于
                prev = cur;//prev跳到cur这里
                cur = cur.next;//cur后移
            }
        }
        //最后考虑头节点是不是要删除的
        if(this.head.data == key) {
            this.head = this.head.next;
        }
    }

    //打印单链表
    public void display() {
        Node node = this.head;
        while (node != null) {
            System.out.print(node.data + " ");
            node = node.next;
        }
        System.out.println();
    }

    //释放内存
    public void clear() {
        this.head = null;
    }
}

```
