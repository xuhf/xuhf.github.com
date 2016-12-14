---
layout : post
title : " 单例模式"
category : "设计模式"
tags : [java]
---

## 单例模式的定义

一个类有且仅有一个实例，并且自行实例化向整个系统提供

## 饿汉模式

特点是线程安全

```java
public class Singleton {

    private Singleton() {
    }

    private static Singleton instance = new Singleton();

    public static Singleton getInstance() {
        return instance;
    }
}
```

## 懒汉模式

延迟加载，线程不安全

```java
public class Singleton2 {

    private Singleton2() {
        System.out.println("Singleton2 init .........");
    }

    private static Singleton2 instance = null;

    public static Singleton2 getInstance() {
        if (instance == null) {
            instance = new Singleton2();
        }
        return instance;
    }
}
```

## 静态内部类

```java
public class Singleton3 {

    private Singleton3() {

    }

    public static Singleton3 getInstance() {
        Singleton3 instance = Singleton.instance;
        return instance;
    }

    private static class Singleton {
        private static Singleton3 instance = new Singleton3();
    }
}
```