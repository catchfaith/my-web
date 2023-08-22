---
title: 数据结构入门
date: 2023-08-22 16:54:05
categories: Java
---

## 数据结构回顾


## 1.1说到数据结构先谈谈什么是数据

1.数据:是描述客观事物的符号,是计算机中可操控的对象,能被计算机识别.并输入给计算机处理的符号集合.数据不光光包含整型,实型等数值类型,还包含音频,视频图像等等非数值类型.
2.数据元素:是组成数据的,有一定的基本单位 好比面向编程里面的一个个实体类 Person Bird
3.数据项:一个数据元素可以是若干个数据项组成,比如上面说的人有脚手鼻子眼睛换成编程中类的属性
4.数据对象:是性质相同的数据元素的集合,是数据的子集,好比人都有姓名,生日,性别相同的数据项

## 1.2那什么是结构呢?

大话数据结构里说:数据结构就是相互之间存在一种或者多种特定关系的数据元素的集合
我的认为就是用于储存很多数据元素的集合 好比java当中的数组或者list
并且把数据结构分成逻辑结构和物理结构

### 1.2.1首先说说这个逻辑结构

逻辑结构:**是指数据对象中数据元素之间的相互关系.** 分为一下四种
1.集合结构:集合结构中的数据元素除了同属于一个集合外,它们之间没有其他关系.
我的理解就是一个集合里塞了很多数据元素 比如一个广场上有3个人 他们彼此都不认识,他们仅仅就是在同一个广场上而已
![Test](/images/data1-1.png)
2.线性结构:线性结构中的数据元素是一一对应的关系
![Test](/images/data1-2.png)
3.树形结构:树形结构数据元素存在一种一对多的关系
![Test](/images/data1-3.png)
4.图形结构:图形结构中的元素是多对多的
![Test](/images/data1-4.png)

### 1.2.1再来说说这个物理结构
物理结构:是指数据的逻辑结构在计算机中的存储形式

数据元素的存储结构形式有两种:顺序存储和链式存储

1.顺序存储结构:是把数据元素存在放在连续的存储单元中,从编程语言的角度上就是数组,当你使用java
```java
int[] arr = new int[4];
```
如上面的代码new了一个长度为4的int类型的数据 这个时候内存里就会为这个数据开辟4个连续的内存地址

2.链式存储结构:是把数据元素存放在任意的存储单位里,存储单元可以是连续的可以是不连续的,数据元素的存储关系并不能反应其逻辑关系，所以需要用一个指针存放数据元素的地址



## 2. 线性表==>零个或者多个数据元素的有限序列
### 2.1 线性表的顺序存储结构
线性表的顺序存储结构，指的是用一段地址连续的存储单位一次存储线性表的数据元素
要理解的话可以参考数据的原理

### 2.2 线性表的链式存储结构
真如1.2.1所说的链式存储结构 存储地址不是连续的 他们通过一个指针来标记他的前后
这里可以分成 单链表，静态链表，循环链表，双向链表
#### 2.3 单链表==>
```java 
        //1.链表是以节点的方式进行存储的
        //头指针就是 130指向a1
        //地址   data域     next域
        //110   a2         150
        //120   a5         null
        //130   a1         110
        //140   a4         120
        //150   a3         140
        //2.每个节点包含data域，next域 指向下一个节点
        //3.如上发现链表的各个节点不一定是连续存放的
        //4.链表分带头节点的链表和没有头节点的链表，根据实际的需求来确定
```
使用java实现单链表的基本功能
```java
package com.faith.datastruct;

/**
 * @description:
 * @author:faith
 * @time:2023/8/1516:54
 */
public class LinkListTest {
    public static void main(String[] args) {
        /*Node linkList = new Node();
        linkList.add(1);
        linkList.add(5);
        linkList.add(2);
        linkList.add(8);
        linkList.showNode();// 1 5 2 8
        System.out.println("链表个数：" + linkList.Count());
        //删除第四个节点
        linkList.removeIndex(3);
        linkList.showNode();// 1 5 2
        System.out.println(linkList.Contains(3));
        //头插法 尾插就是默认的add
        linkList.headAdd(6);
        //sout 6 1 5 2
        linkList.showNode();*/
        LinkList list = new LinkList();
        list.add(1);
        list.add(1);
        list.add(1);
        list.showNode();
        System.out.println("=========");
        LinkList list1 = new LinkList();
        list1.add(2);
        list1.add(2);
        list1.add(2);
        list1.showNode();
        System.out.println("=========");
        list.mergeNode(list1);
        list.showNode();
/*        linkList.add(1);
        linkList.add(1);
        linkList.add(1);*/
/*        Node linkList1 = new Node();
        linkList1.add(2);
        linkList1.add(2);
        linkList1.add(2);
        linkList.showNode();*/
        //System.out.println("=========");
        //linkList1.showNode();
        //Node node = linkList.mergeNode(linkList1);
        //node.showNode();
    }
}
//线性表分为顺序存储结构（地址连续）
//还有链式存储结构（地址不连续 每个节点指向下个节点）
//手写单链表
//这里的data我就单独使用一个int 好了 实际中其实这里可以是个实体对象
//Node可以理解为节点的意思
class LinkList{
    //这里为什么要多加一个head头节点呢 原来我直接写在了node里面 但是发现不加static 会一直构造node造成内存溢出 加了static 因为是唯一的 所以你new其他的linklist 他内部存储的都是指向这个node
    public Node head = new Node();
    //新增节点
    public void add(int value){
        //将node节点的最后一个值指向new Node(value)对象
        //新建一个node 地址指向head 引用类型都是在栈存储了堆的内存地址
        //如果不新建的花 遍历的时候指向下个节点 head = head.next
        //等到get的时候 发现head变成了最后一个节点
        Node temp = head;
        //遍历
        while (true){
            if (temp.next == null){
                temp.next = new Node(value);
                return;
            }
            //当前遍历节点指向下个节点
            temp = temp.next;
        }
    }

    //打印链表
    public void showNode(){
        //输出当前的单链表
        //因为需要遍历改变遍历节点位置 那么这里的话依然需要temp
        //这里为什么要使用head.next??
        //因为在遍历的时候需要把当前指向位置往后移动 temp = temp.next;
        //while中的判断条件肯定是temp.next 输出一下就break 但是temp第一次 head有个头节点 他的next不是null 会输出 我们要把开始定在头节点的下一个
        Node temp = head.next;
        if (temp == null){
            System.out.println("链表为null");
            return;
        }
        while (true){
            //如果temp == null break
            if (temp == null){
                break;
            }
            //sout
            System.out.printf("val=%d,hashCode=",temp.data);
            //输出一下temp的内存地址 看是不是有顺序的
            System.out.println(temp.hashCode());
            //指向下一个
            temp = temp.next;
        }
    }

    //删除
    public void removeIndex(int index){
        // 0 1 2 3 4 5 6
        //test 01 delete 0
        //test 02 delete 1
        // delete 5 index
        if (index < 0 || index > Count()){
            System.out.println("请输入正确的下标");
        }
        //删除第一个元素
        if (index == 0 ){
            head.next = head.next.next;
            //head 0  //1 2 3 4 5 6
        }
        int i = 0;
        Node temp = head;
        while (temp.next != null){
            if (i != index){
                temp = temp.next;
                i++;
            }
            else {
                temp.next = temp.next.next;
            }
        }
    }

    //获取单链表的长度
    public int Count(){
        int count =0;
        Node temp = head.next;
        while (temp!=null){
            count++;
            temp = temp.next;
        }
        return count;
    }

    //查看链表中是否包含元素
    public boolean Contains(int key){
        Node temp = head.next;
        while (temp != null){
            if (temp.data == key){
                return true;
            }
            temp = temp.next;
        }
        return false;
    }

    //头插
    public void headAdd(int val){
        Node temp = head;
        if (temp.next == null){
            head.next = new Node(val);
        }else {
            Node ins = new Node(val);
            ins.next = head.next;
            head.next = ins;
        }
    }

    //清空
    public void clear(){
        head.next = null;
    }

    public Node gethead(){
        return head;
    }
    //合并链表
    public void mergeNode(LinkList list){
        //head最后的指向指向list.next
        if (head.next == null){
            head.next = list.gethead().next;
        }
        Node temp = head.next;
        while (true){
            if (temp.next == null){
                temp.next = list.gethead().next;
                break;
            }
            temp = temp.next;
        }
    }
}
class Node{
     public int data;
     public Node next;
     //构造函数 给头节点一个默认值
     public Node(){
         data = 0;
     }
     //add的时候需要newNode把value值放进去
     public Node(int val){
         data = val;
     }

}
```
#### 2.4 静态链表
像C语言他有了指针这个东西，使得它可以非常容易的操纵内存中的地址和数据，后续的java c#虽然没有指针，但是一样有对象引用的概念，从一定角度也是实现了指针的功能
但是像一些早期的语言 他没有指针如何实现单链表呢
其实这个可以用数组的概念来实现 
首先我们让数组的元素有两个数据域组成，cur和data，data就用来防止数据元素的数据，cur用来存放它下一个指向值得下标，cur就好比单链表中得next指针
我们把这种用数组表示的叫做静态链表，这种描述方法还有起名叫游标实现法
![Test](/images/staticLinked01.png)
如图上
数组的第一个和最后一个属于是备用链表
第一个存储的是没有使用的数组元素的开始位置（比如数组长度10但是只塞了3个 那么这三个存储就是从静态数链下标为1开始存储 第一个存储的就是下标4 从4之后还有6个位置 减去最后一个还有5个位置）
最后一个存储的是数组存储数据的开始下标也就是从1开始存储 最后一个存储的cur就是1

#### 2.5 循环链表 
可以理解为单链表最后的节点的next指向第一个节点，这种头尾衔接起来的就可以称为循环链表
#### 2.6 双链表 
这个和前面的单链表的区别就是 双链表不仅仅指向它后面的节点还指向了它前面的节点


### 2.7 队列和栈
先来说说栈吧
栈（stack） 是限定仅在表尾进行插入和删除的线性表，先进后出是他的特点
![Test](/images/stack01.png)

这里插一个中缀表达式合后缀表达式
实例 
```java 
//中缀表达式==>后缀表达式
//9+(3-1)*3+10/2
//to 9 3 1 - 3 * + 10 2 / +
```
中缀表达式也就是我们平时看的懂得表达式 9 + (3 + 1) * 3 + 10 / 2
后缀表达式就是计算机能懂得表达式 9 3 1 - 3 * + 10 2 / +
```java 
        //先来看看后缀表达式如何运算
        //9 3 1 - 3 * + 10 2 / +
        //第一个9 入栈 3 入栈 1 入栈
        //遇到-运算符 出 3 1 -运算 = 2 入栈 此时栈内是 9 2
        // 3 入栈 ==》 9 2 3
        //遇到* 运算符 出栈 3 2 *运算 = 6 入栈 此时栈内 9 6
        //遇到+ 运算符 出 9 6 +运算 = 15 入栈 此时栈内 15
        //10 入栈 2 入栈 此时栈内 15 10 2
        //遇到/运算符 10 /2 运算到5 入栈 15 5
        //遇到+运算符 15 5 出栈 15 + 5 = 20 入栈
```
再来看看这两个东西怎么转换？？
规则
从左到右边 是数字就输出 是符号就入栈 当括号闭合得时候 输出括号内得符号 当栈内加入一个符号他前面得符号优先级比自己高得时候 把该符号前面到栈顶得符号出栈输出

```java
        //9+(3-1)*3+10/2
        //第一次遇到9 输出 9
        //遇到+ 入栈 +
        //遇到（ 入栈 + （
        //遇到3 输出 9 3
        //遇到- 入栈 ==> + （ -
        //遇到1 输出 9 3 1
        //遇到） 入栈 ==> + （ - )
        //当()闭合 从栈顶开始出栈输出 ( - ) 这里排除了括号 所以输出 - 栈内目前只有 +
        //控制台目前输出的内容==> 9 3 1 -
        //遇到了* 入栈 目前栈内 + *
        //遇到3 输出 9 3 1 - 3
        //遇到 + 入栈观察 + * + 上一个优先级比入栈的+ 高 所以全部出栈 输出 栈内目前只有+
        //控制台目前输出的内容==> 9 3 1 - 3 * +
        //栈内目前只有+ 注意这里的+号不是之前栈顶的+运算符 （就是表达式9后面的+） 现在栈内的+ 式3+10的那个3 因为遇到之前的运算符优先级比自己高把之前的都输出了
        //遇到10 输出 9 3 1 - 3 * + 10
        //遇到/ 入栈 + /
        //遇到2 输出 9 3 1 - 3 * + 10 2
        //最后输出 / +  ==> 9 3 1  - 3 * + 10 2 / +
```

再来说说队列
队列（queue）是只允许在一端进行插入操作 而在另一边进行删除操作得线性表
队列是一种先进先出得线性表允许插入的为一头是队尾 允许删除的为另外一头是队头
假设队列是q={a1,a2,a3,a4} 那么队头是a1 队尾是a4 因为a4后面a5 才是往里头插入的
## 3 串
串是由零个或者多个字符组成的有限序列


## 4 树
### 4.1、为什么需要用到树形结构
#### 4.1.1、数组存储方式的分析 
- 优点：通过下标方式访问元素，速度快。对于有序数组，还可以使用**二分查找**提高检索速度
- 缺点：如果要检索具体某个值，或者插入值（按照一定顺序）会整体移动，效率低
#### 4.1.2、链式存储方式的分析
- 优点：在一定程度上对数组存储方式有优化（比如：插入一个数值节点，只需要插入节点，链接到链表中即可，删除效率也很好）
- 缺点：在进行检索时，效率仍然较低，比如遍历的时候需要从头遍历到查到为止
#### 4.1.3、树存储方式的分析
能提高数据存储，读取效率，比如利用二叉排序树。既可以保证数据的检索速度，同时也可以保证数据的插入，删除，修改速度。
树的常用术语
![Test](/images/tree01.png)
### 4.2 二叉树
1.每个节点最多只能有两个子节点的形式叫二叉树，二叉树的子节点又分成左节点和右节点
2.如果二叉树的所有叶子节点都在最后一层，并且节点的总数=2 ^ n - 1 ，n为层数，则我们可以称为满二叉树。
3.如果该二叉树的所有叶子节点都在最后一层后者倒数第二层，而且最后一层的叶子节点都在左边延续，倒数第二层的叶子节点在右边延续，我们可以称之为完全二叉树
4.前序遍历：先输出父节点，再遍历左子树和右子树
5.中序遍历：先遍历左子树，再输出父节点，再遍历右子树
6.后序遍历：先左树，再右树再父节点
这里可以看父节点树的位置来判断

使用CSharp 对于这里的各种顺序的遍历 
``` C# 
// See https://aka.ms/new-console-template for more information

namespace faithApp
{
    public class Program
    {
        public  static void Main(string[] args)
        {
           //1. 创建二叉树
           //2.前序遍历
           //3.先输出当前节点
           //4.如果左子节点不为null 就递归继续前序遍历
           //如果右节点不为null，递归继续前序遍历
           /*
            * 4.前序遍历：先输出父节点，再遍历左子树和右子树
            5.中序遍历：先遍历左子树，再输出父节点，再遍历右子树
            6.后序遍历：先左树，再右树再父节点
            */
           /*        1
            *      2    3
            *           5  4
            * 前：1 2 3 5 4 
            * 中：2 1 5 3 4
            * 后：2 5 4 3 1
            */
           Node node = new Node(1);
           Node node1 = new Node(2);
           Node node2 = new Node(3);
           Node node3 = new Node(4);
           Node node4 = new Node(5);
           node.leftNode = node1;
           node.rightNode = node2;
           node2.leftNode = node4;
           node2.rightNode = node3;
           Tree tree = new Tree();
           tree.node = node;
           //前序
           Console.WriteLine("前序遍历");
           tree.frontDisplay();
           Console.WriteLine("中序遍历");
           tree.midDisplay();
           Console.WriteLine("后序遍历");
           tree.behindDisplay();
           Console.ReadKey();
        }
    }

    class Tree
    {
        public Node node { get; set; }
        //前序
        public void frontDisplay()
        {
            if (this.node != null)
            {
                this.node.frontDisplay();
            }
            else
            {
                Console.WriteLine("二叉树为null");
            }
        }
        //中序
        public void midDisplay()
        {
            if (this.node != null)
            {
                this.node.midDisplay();
            }
            else
            {
                Console.WriteLine("二叉树为null");
            }
        }
        //后序
        public void behindDisplay()
        {
            if (this.node != null)
            {
                this.node.behindDisplay();
            }
            else
            {
                Console.WriteLine("二叉树为null");
            }
        }
        
    }
    class  Node
    {
        public Node(int val)
        {
            this.val = val;
        }
        public int val { get; set; }
        public Node leftNode { get; set; }
        public Node rightNode { get; set; }

        public override string ToString()
        {
            return $"节点权为：{val}";
        }

        //前序
        public void frontDisplay()
        {
            //4.前序遍历：先输出父节点，再遍历左子树和右子树
            //先输出当前节点
            Console.WriteLine(this.ToString());
            if (this.leftNode != null)
            {
                this.leftNode.frontDisplay();
            }

            if (this.rightNode != null)
            {
                this.rightNode.frontDisplay();
            }
        }
        //中序
        public void midDisplay()
        {
            //中序遍历：先遍历左子树，再输出父节点，再遍历右子树
            if (this.leftNode != null)
            {
                this.leftNode.midDisplay();
            }
            Console.WriteLine(this.ToString());
            if (this.rightNode != null)
            {
                this.rightNode.midDisplay();
            }
        }
        //后序
        public void behindDisplay()
        {
            //后序遍历：先左树，再右树再父节点
            if (this.leftNode != null)
            {
                this.leftNode.behindDisplay();
            }
            
            if (this.rightNode != null)
            {
                this.rightNode.behindDisplay();
            }
            
            Console.WriteLine(this.ToString());
        }
    }
}
```

