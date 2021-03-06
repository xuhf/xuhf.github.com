---
layout : post
title : "[数据结构笔记]线性表（一）"
category : "数据结构"
tags : [数据结构]
---

## 什么是线性表

线性表是零个或多个数据元素的有限序列。

## 线性表的抽象数据类型

```
ADT 线性表List
Data
  线性表的数据对象集合为｛a1,a2,....,an｝,每个元素的类型均为DataType。其中，除第一个元素a1外，每一个元素有且只有一个直接前驱元素，除了最后一个an外，每一个元素有且只有一个直接后续元素。数据元素之间的关系是一对一的关系。
Operation
  // 初始化
  // 判断线性表是否为空
  // 清空线性表
  // 返回某个位置的元素
  // 判断是否存在某个元素，返回其位置
  // 插入元素
  // 删除元素
  // 放回线性表的元素个数
endADT
```

## 线性表的顺序存储结构

线性表的顺序存储结构，指的是用一段地址连续的存储单元依次存储线性表的数据元素。

Java里的ArrayList就是使用数组来实现的顺序存储。

顺序存储结构的优缺点

优点

不需要为表中元素之间的逻辑关系而增加额外的存储空间

可以快速地存取表中任一位置的元素

缺点

插入和删除需要移动大量的元素

当线性表长度变化较大时，难以确定存储空间的容量

造成存储空间"碎片"

## 线性表的链式存储结构

为了表示每个数据元素ai 与其直接后继数据元素ai+1之间的逻辑关系，对数据元素ai来说，除了存储其本身的信息之外，还需存储一个指示其直接后继的信息（即直接后继的存储位置）。我们把存储数据元素信息的域称为数据域，把存储直接后继位置的域称为指针域，指针域中存储的信息称作指针或链。这两部分信息组成数据元素ai的存储映像，称为结点（Node）。

n个结点（ai的存储映像）链结成一个链表，即为线性表(a1,a2,...,an)的链式存储结构，因为此链表的每个结点中只包含一个指针域，所以叫做单链表。

链表中第一个结点的位置叫做头指针。

用java语言来描述一个结点。

```java
public class Node {
  Object data;
  Node next;
}
```

由上面的代码我们知道，结点由存放数据元素的数据域和存放后继结点地址的指针域组成。

java中的LinkedList就是使用链式存储结构实现。

## 单链表结构与顺序存储结构对比

如果线性表查找多，而插入删除少，适宜采用顺序存储结构。如果插入删除比较多，适宜采用单链表结构。

当线性表中的元素个数变化较大时或者根本不知道有多大时，最好采用单链表结构。
