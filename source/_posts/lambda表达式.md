---
title: lambda表达式的使用
---
# 1.概述
## 1.1 为什么学？
- 能够看的懂公司的代码
- 大数据下处理集合效率高
- 代码可读性高
- 消灭嵌套地狱

**传统的方式**
```java
    List<Person> list = new ArrayList<>();
    Person person = new Person(1,"faith",25,"衢州");
    Person person1 = new Person(2,"evil",25,"杭州");
    Person person2 = new Person(3,"acid",25,"杭州");
    Person person3 = new Person(4,"taylor",25,"温州");
    list.add(person1);
    list.add(person);
    list.add(person2);
    list.add(person3);
    //查询id为1的
    //传统方式
    ArrayList<Person> plist = new ArrayList<>();
    for (Person p:
         list) {
        if (p.getId() == 1){
        plist.add(p);
        }else {
            continue;
        }
    }
```
***使用lambda表达式***
```java
List<Person> collect = list.stream().filter(p -> p.getId() == 1).collect(Collectors.toList());
```
## 1.2  函数式编程
### 1.2.1 概念
面向对象思想需要关注用什么对象完成什么事情。而函数式编程思想就类似与我们数学中的函数。它主要关注的是对数据进行了什么操作
### 1.2.2
- 代码简洁 开发快速
- 接近自然语言，易于理解
- 易于并发编程
# 2.Lambda表达式
## 2.1 概述
lambda表达式是jdk8中的一个语法糖，它可以对某些匿名内部类的写法进行简化。他是函数式编程思想的一个重要体现。让我们不用关注是什么对象。而是更关注对数据进行了什么操作。
## 2.2 核心原则
> 可推导 可省略

**例一**
我们在创建线程并启动时可以使用匿名内部类写法
```java
    new Thread(new Runnable() {
        @Override
        public void run() {
            System.out.println("hello");
        }
    });
```