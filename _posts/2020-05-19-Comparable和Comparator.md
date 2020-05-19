---
layout:     post
title:      Comparable和Comparator
subtitle:   Comparable和Comparator
date:       2020-05-07
author:     JiangKun
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - Comparable
    - Comparator
---


Java当中如何实现比较自定义类型的大小呢？
在Java当中，自定义类型不可以通过`> < =`来比较，一般有3种方式来实现：
**Comparable、Comparator、equals()**
今天先学习Comparable Comparator
| Comparable | Comparator |
|--|--|
|一般用于定义类的时候，实现该接口  | 一般用于定义某个比较器的同时实现该接口 |
| 注意重写方法：compareTo |  注意重写方法：compare|
|  优点：可以实现自定义类的比较方式| 优点：可以根据不同的需求，进行不同的排序方式 |
|  缺点：排序方式固定，不够灵活| 缺点：必须自己建一个类进行排序 |
**Comparable接口**
通过实现Comparable接口，重写CompareTo方法
```java
class Student implements Comparable<Student>{
    public String name;
    public int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }


    @Override
    public int compareTo(Student o) {
        return this.age - o.age;
        //根据年龄比较
        //return this.name.compareTo(o.name);
    }
}


public class TestComparable {
    public static void main(String[] args) {
        Student student1 = new Student("zhangsan",18);
        Student student2 = new Student("lisi",28);
        Student student3 = new Student("wangwu",38);
        if (student1.compareTo(student2) > 0) {
            System.out.println("student1的年龄大于student2的年龄");
        }else if (student1.compareTo(student2) < 0) {
            System.out.println("student1的年龄小于student2的年龄");
        }else {
            System.out.println("student1的年龄等于student2的年龄");
        }
    }
}
```
如果此时需要比较姓名的话，就需要重新实现CompareTo方法，**同时这也是使用Comparable这种方法的弊端，一旦在类中写死，就只能秀给类內部，不够灵活**

**Comparator接口**
这个接口也被叫做**比较器**，先看具体的实现代码：

```java
import java.util.Arrays;
import java.util.Comparator;

class Person {
    public String name;
    public int age;
    public int score;

    public Person(String name, int age, int score) {
        this.name = name;
        this.age = age;
        this.score = score;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", score=" + score +
                '}';
    }
}

class AgeComparator implements Comparator<Person> {

    @Override
    public int compare(Person o1, Person o2) {
        //从小到大排序
        return o1.age - o2.age;
    }
}

class ScoreComparator implements Comparator<Person> {

    @Override
    public int compare(Person o1, Person o2) {
        return o1.score - o2.score;
    }
}

class NameComparator implements Comparator<Person> {

    @Override
    public int compare(Person o1, Person o2) {
        return o1.name.compareTo(o2.name);
    }
}

public class TestComparator {

    //高级用法：使用Arrays.sort()方法传入比较器
    public static void main(String[] args) {
        Person person1 = new Person("aaa",18,80);
        Person person2 = new Person("bbb",28,70);
        Person person3 = new Person("ccc",38,60);
        Person[] people = new Person[3];
        people[0] = person1;
        people[1] = person2;
        people[2] = person3;
        ScoreComparator scoreComparator = new ScoreComparator();
        Arrays.sort(people,scoreComparator);
        System.out.println(Arrays.toString(people));
    }

    public static void main1(String[] args) {
        Person person1 = new Person("aaa",18,80);
        Person person2 = new Person("bbb",28,70);
        Person person3 = new Person("ccc",38,60);

		//使用比较器进行比较
        AgeComparator ageComparator = new AgeComparator();
        System.out.println("根据年龄比较");
        System.out.println(ageComparator.compare(person1,person2));
        ScoreComparator scoreComparator = new ScoreComparator();
        System.out.println("根据分数比较");
        System.out.println(scoreComparator.compare(person2,person3));
        NameComparator nameComparator = new NameComparator();
        System.out.println("根据姓名比较");
        System.out.println(nameComparator.compare(person3,person1));
    }
}

```
直接定义一个类**实现Comparator接口并重写compare()方法**，想比较什么就直接定义，非常的灵活
同时`Comparator`还可以作为参数传递给`Arrays.sort()`方法，来规定他的比较方式

```java
	//高级用法：使用Arrays.sort()方法传入比较器
    public static void main(String[] args) {
        Person person1 = new Person("aaa",18,80);
        Person person2 = new Person("bbb",28,70);
        Person person3 = new Person("ccc",38,60);
        Person[] people = new Person[3];
        people[0] = person1;
        people[1] = person2;
        people[2] = person3;
        ScoreComparator scoreComparator = new ScoreComparator();
        Arrays.sort(people,scoreComparator);
        System.out.println(Arrays.toString(people));
    }
```
**小结**
Java在做自定义类型的比较的时候，通过实现这两个接口来完成，具体的细节看需求
**Comparable**

 1. 一般用于定义类的时候，实现该接口
 2. 注意重写方法：compareTo
 3. 优点：可以实现自定义类的比较方式
 4. 缺点：排序方式固定，不够灵活

**Comparator**

 1. 一般用于定义某个比较器的同时实现该接口
 2. 注意重写方法：compare
 3. 优点：可以根据不同的需求，进行不同的排序方式
 4. 缺点：必须自己建一个类进行排序


