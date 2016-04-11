---
layout : post
title : "Java的重载与重写"
category : "java"
tags : [java,重写,重载]
---

## 什么是Java的重载与重写

在朋友圈看到这样一个问题：

基类里有方法 `void a()`;

子类中有方法 `int a()`;

这2个方法是重载还是重写？

现在我们先不说明答案，先来学习下什么是重写与重载。

## Java的重写

1，在子类中可以根据需要对从基类中继承来的方法进行重写，是子类与父类的一种多态性体现

2，重写的方法和被重写的方法必须具相同的`方法名称`，`参数列表`和`返回类型`

3，重写方法不能使用比被重写的方法更严格的访问权限

```java

class P {

  public void a() {
    System.out.println("Parent");
  }
}

class C extends P {
  public void a() {
    System.out.println("Child");
  }
}
```

## Java的重载

Java允许在一个类中，多个方法有用相同的名字，但是必须有不同的参数列表，这就是重载。

重载的特点：方法名称相同，但是参数列表不同（注意：不允许参数列表相同，但是返回值不同的情况）

最典型的方法重载：

```java

class P {

  private int a(int b) {
    return 0;
  }

  private int a(int b , int c) {
    return 0;
  }
}
```

## 重写与重载的区别

1，重载和覆盖的方法名称都相同，但重载要求参数列表不同，而覆盖要求参数列表完全相同。

2，重载对于方法访问权限没有限制，而覆盖则对访问权限使用有限制

## 最上面问题的答案

答案就是：和重载和重写没一毛钱关系，子类编译错误。

## 参考连接

<http://liujinpan75.iteye.com/blog/495562>

<http://www.cnblogs.com/Gaojiecai/p/4001041.html>
