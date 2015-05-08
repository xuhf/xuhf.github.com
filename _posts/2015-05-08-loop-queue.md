---
layout : post
title : "[数据结构笔记]队列-循环队列"
category : "数据结构"
keywords : 循环队列, java实现
tags : [数据结构, 算法]
published: false
---

## 前言

### 什么是循环队列?

我们把队列的这种头尾相接的顺序存储结构称为循环队列。

### 如何判断队列是空还是满？

首先定义2个指针，

front 指向队头元素

rear 指向队尾元素的下一个位置

当队列为空时，front ＝ rear

当队列为满时，我们修改其条件，保留一个元素空间。也就是说，当队列满时，数组中还有一个空闲单元。

通用的计算队列长度的公式为：

```
(rear - front + maxSize) % maxSize
```

## java实现

我自己使用java代码进行了循环队列的实现，如果问题，欢迎留言指正。

```java

import java.util.Arrays;

/**
 * 循环队列
 *
 * @author xuhf
 *
 */
public class LoopQueue {

  Object[] data;
  int front;
  int rear;

  public LoopQueue(int maxSize) {
    // 初始化队列
    data = new Object[maxSize];
    front = 0;
    rear = 0;
  }

  public int length() {
    // 返回当前队列的元素个数，也就是队列的当前长度
    return (this.rear - this.front + data.length) % data.length;
  }

  public void en(Object d) {
    // 若队列未满，则插入元素d为队列的新的队尾元素
    if ((this.rear + 1) % this.data.length == this.front) {
      // 队列已经满了
      throw new RuntimeException("queue 满");
    }
    data[this.rear] = d;
    this.rear = (this.rear + 1) % data.length;
  }

  public Object de() {
    if (this.rear == this.front) {
      return null;
    }
    Object e = data[this.front];
    data[this.front] = null;
    this.front = (this.front + 1) % data.length;
    return e;
  }

  public void print() {
    System.out.println(Arrays.toString(data));
  }

  public static void main(String[] args) {
    // LoopQueue queue = new LoopQueue(5);
    // queue.en(1);
    // queue.print();
    // System.out.println(queue.front + " " + queue.rear);
    // queue.en(2);
    // queue.print();
    // System.out.println(queue.front + " " + queue.rear);
    // Object e = queue.de();
    // System.out.println(e);
    // queue.print();
    // System.out.println(queue.front + " " + queue.rear);
    // queue.en(3);
    // queue.en(4);
    // queue.en(5);
    // queue.print();
    // System.out.println(queue.front + " " + queue.rear);
    // queue.en(6);
    // queue.print();
    // System.out.println(queue.front + " " + queue.rear);
    // 第二种情况
    // LoopQueue queue = new LoopQueue(5);
    // queue.en(1);
    // queue.en(2);
    // queue.en(3);
    // queue.en(4);
    // queue.print();
    // System.out.println(queue.front + " " + queue.rear);
    // queue.en(5);

    LoopQueue queue = new LoopQueue(5);
    queue.en(1);
    queue.en(2);
    queue.en(3);
    queue.en(4);
    queue.print();
    System.out.println(queue.front + " " + queue.rear);
    Object e = queue.de();
    System.out.println(e);
    queue.print();
    System.out.println(queue.front + " " + queue.rear);
    queue.en(5);
    queue.print();
    System.out.println(queue.front + " " + queue.rear);
    Object e1 = queue.de();
    System.out.println(e1);
    queue.print();
    System.out.println(queue.front + " " + queue.rear);
    queue.en(6);
    queue.print();
    System.out.println(queue.front + " " + queue.rear);
  }
}
```
