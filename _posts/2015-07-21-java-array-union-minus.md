---
layout : post
title : "java比较两个数组的并集、差集与交集"
category : "java"
keywords : java,array
---

## 前言

在开发工程中，需要比较两个数组的差集与交集。

## 代码实现

### 数组并集

```java
/**
 * 并集
 *
 * @param a
 * @param b
 * @return
 */
public static String[] union(String[] a, String[] b) {
    // 利用Set的特性
    Set<String> set = new HashSet<String>();
    for (String s : a) {
        set.add(s);
    }
    for (String s : b) {
        set.add(s);
    }
    return set.toArray(new String[] {});
}
```

### 数组交集

```java
/**
 * 交集
 *
 * @param a
 * @param b
 * @return
 */
public static String[] intersect(String[] a, String[] b) {
    List<String> resultList = new ArrayList<String>();
    List<String> bList = Arrays.asList(b);
    for (String s : a) {
        if (bList.contains(s)) {
            resultList.add(s);
        }
    }
    return resultList.toArray(new String[] {});
}
```

### 数组差集

```java
/**
 * 差集
 *
 * @param a
 * @param b
 * @return
 */
public static String[] minus(String[] a, String[] b) {
    List<String> resultList = new ArrayList<String>();
    List<String> aList = Arrays.asList(a);
    List<String> bList = Arrays.asList(b);
    // 在a中不在b中
    for (String s : a) {
        if (!bList.contains(s)) {
            resultList.add(s);
        }
    }
    // 在b中不在a中
    for (String s : b) {
        if (!aList.contains(s)) {
            resultList.add(s);
        }
    }
    return resultList.toArray(new String[] {});
}
```
