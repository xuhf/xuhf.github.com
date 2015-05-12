---
layout : post
title : "[数据结构笔记]栈的基础知识"
category : "数据结构"
keywords : 栈, 栈的基础
tags : [数据结构]
---

## 栈的定义

栈（stack）是限定仅在表尾进行插入和删除操作的线性表

允许插入和删除的一端称为栈顶（top），另一端称为栈底（bottom）。

栈又称为后进先出（Last In First Out）的线性表，简称LIFO结构

栈的插入操作，叫做进栈，也成压栈、入栈。

栈的删除操作，也叫出栈。

## 栈的抽象数据类型

```
ADT 栈（stack）
Data
  同线性表，元素具有相同的类型，相邻元素具有前驱和后继关系。
Operation
  // 初始化操作，建立一个空栈
  // 销毁栈
  // 清空栈
  Push 若栈存在，插入新元素e到栈中并称为栈顶元素
  Pop 删除栈中的栈顶元素，并用e返回其值
endADT
```

## 栈的顺序存储结构及实现

使用数组实现顺序栈，使用下标为0的一端作为栈底。

Java实现

```java
import java.util.Arrays;

public class ArrayStack {

  int maxSize = 10;

  Object[] data;

  int top = -1;

  public ArrayStack() {
    data = new Object[maxSize];
  }

  public void push(Object d) {
    if (top == maxSize - 1) {
      throw new RuntimeException("stack is full");
    }
    top++;
    data[top] = d;
    System.out.println(Arrays.toString(data));
  }

  public Object pop() {
    if (top < 0) {
      throw new RuntimeException("stack is empty");
    }
    Object d = data[top];
    data[top] = null;
    top--;
    System.out.println(Arrays.toString(data));
    return d;
  }

  public static void main(String[] args) {
    ArrayStack as = new ArrayStack();
    as.push(1);
    Object d = as.pop();
    System.out.println(d);
  }
}
```

## 栈的链式存储结构及实现

使用链表实现，将栈顶放在单链表的头部。

java实现

```java
public class LinkedStack {

  Node top;

  public static class Node {
    Object data;
    Node next;
  }

  int count;

  public LinkedStack() {

  }

  public void push(Object d) {
    Node n = new Node();
    n.data = d;
    n.next = top;
    top = n;
    count++;
  }

  public Object pop() {
    // 空栈
    if (top == null) {
      throw new RuntimeException("stack is empty");
    }
    Node n = top;
    top = n.next;
    count--;
    return n.data;
  }

  public static void main(String[] args) {
    LinkedStack ls = new LinkedStack();
    ls.push(1);
    System.out.println(ls.top.data);
    ls.push(2);
    System.out.println(ls.top.data);
    System.out.println("count is " + ls.count);
    Object d = ls.pop();
    System.out.println("d is " + d);
    System.out.println("top is " + ls.top.data);
  }
}
```

## 栈的应用

### 递归

递归的定义：把一个直接调用自己或者通过一系列的调用语句间接调用自己的函数，称为递归函数。

每个递归定义必须至少有一个条件，满足时递归不再进行，即不能引用自身而是返回值退出。

借用书上的小例子:

如果兔子在出生2个月后，就有繁殖能力，一对兔子每个月能生出一对小兔子，

假设所有兔子不死，那么两年后可以繁殖多少对兔子。

```java
public static void fbi() {
  int month = 24;
  for (int i = 1; i <= month; i++) {
    System.out.println(fbi(i));
  }
}

private static int fbi(final int i) {
  if (i < 2) {
    return i == 0 ? 0 : 1;
  }
  return fbi(i - 1) + fbi(i - 2);
}
```
