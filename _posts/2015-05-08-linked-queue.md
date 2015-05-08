---
layout : post
title : "[数据结构笔记]队列-队列的链式存储结构"
category : "数据结构"
keywords : 链队列, java实现
tags : [数据结构, 算法]
---

## 前言

队列的链式存储结构，其实就是线性表的单链表，只不过它只能尾进头出，我们把他简称为链队列。

## java实现

使用java代码实现队列的链式存储结构，如果问题，欢迎留言指正。

```java

/**
 * 队列的链式存储结构
 *
 * @author xuhf
 *
 */
public class LinkedQueue {

  public static class Node {
    Object data;
    Node next;
  }

  Node front;
  Node rear;

  public void en(Node node) {
    if (rear == null) {
      // 队列为空
      rear = node;
      front = node;
    } else {
      Node n = rear;
      n.next = node;
      rear = node;
    }
  }

  public Node de() {
    if (front == null) {
      return null;
    }
    Node n = front;
    Node next = n.next;
    front = next;
    return n;
  }

  public int length() {
    if (front == null) {
      return 0;
    }
    Node n = front;
    int i = 0;
    while (n != null) {
      i++;
      System.out.println("data is :" + n.data);
      n = n.next;
    }
    return i;
  }

  public static void main(String[] args) {
    LinkedQueue linkedQueue = new LinkedQueue();
    System.out.println(linkedQueue.length());
    Node n1 = new Node();
    n1.data = 1;
    linkedQueue.en(n1);
    Node n2 = new Node();
    n2.data = 2;
    linkedQueue.en(n2);
    System.out.println("length is " + linkedQueue.length());
    Node n = linkedQueue.de();
    System.out.println(n.data);
    System.out.println("length is " + linkedQueue.length());
  }
}
```
