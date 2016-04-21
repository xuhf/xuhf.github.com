---
layout : post
title : "HashMap和HashSet实现原理"
category : "java"
tags : [java]
---

## HashMap实现原理

HashMap是我们在开发过程中常用的一种数据结构

### 数据的存储结构

在HashMap中，个人觉得比较关键的数据结构就是以下三种：

`static final Entry<?,?>[] EMPTY_TABLE = {};`

`interface Entry<K,V> `

`static class Entry<K,V> implements Map.Entry<K,V>`

其中

`Entry<?,?>[] EMPTY_TABLE` 这个数组存储的是链表的头节点

`Entry<K,V>` 这种数据结构存储的是我们放入的数据

那么，HashMap是如何实现的呢？读一下源代码就会发现，HashMap的实现，就是`数组`+`链表`

数组就是`transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;`

链表就是`Entry`类啦。

我们先来看下`Entry`类的数据结构

```java
static class Entry<K,V> implements Map.Entry<K,V> {
  final K key;
  V value;
  // 指向下一个节点
  Entry<K,V> next;
  int hash;
}
```

next属性指向下一个节点。

### put方法实现

我们来看下put方法

```java
public V put(K key, V value) {
    if (table == EMPTY_TABLE) {
        //
        inflateTable(threshold);
    }
    if (key == null)
        // 存放key为null的数据
        return putForNullKey(value);
    // 对key值进行hash计标
    int hash = hash(key);
    // 确定key值所在数组中的下标
    int i = indexFor(hash, table.length);
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {
        Object k;
        // 如果相同的key两次put，那么用新值替换旧值
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }

    modCount++;
    addEntry(hash, key, value, i);
    return null;
}
```

### get方法实现

我们来看下get方法的实现

```java
public V get(Object key) {
    if (key == null)
        return getForNullKey();
    Entry<K,V> entry = getEntry(key);

    return null == entry ? null : entry.getValue();
}

final Entry<K,V> getEntry(Object key) {
    if (size == 0) {
        return null;
    }
    // 计算key的hash值
    int hash = (key == null) ? 0 : hash(key);
    // indexFor(hash, table.length) 这负责计算hash值所在数组的下标
    for (Entry<K,V> e = table[indexFor(hash, table.length)];
         e != null;
         e = e.next) {
        Object k;
        if (e.hash == hash &&
            ((k = e.key) == key || (key != null && key.equals(k))))
            return e;
    }
    return null;
}
```

### null值的处理

get

```java
if (size == 0) {
    return null;
}
for (Entry<K,V> e = table[0]; e != null; e = e.next) {
    if (e.key == null)
        return e.value;
}
return null;
```

put

```java
private V putForNullKey(V value) {
    for (Entry<K,V> e = table[0]; e != null; e = e.next) {
        if (e.key == null) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
    modCount++;
    addEntry(0, null, value, 0);
    return null;
}
```

### 容量扩充

默认的负载因子是`static final float DEFAULT_LOAD_FACTOR = 0.75f;`

```java
void resize(int newCapacity) {
    Entry[] oldTable = table;
    int oldCapacity = oldTable.length;
    if (oldCapacity == MAXIMUM_CAPACITY) {
        threshold = Integer.MAX_VALUE;
        return;
    }

    Entry[] newTable = new Entry[newCapacity];
    transfer(newTable, initHashSeedAsNeeded(newCapacity));
    table = newTable;
    threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
}
```

## HashSet实现原理

打开`HashSet`源码，就会发现，HashSet的就是使用HashMap实现的。

```java
public HashSet() {
    map = new HashMap<>();
}
```

在HashSet中使用HashMap中的key来保存我们的数据

value为` private static final Object PRESENT = new Object();`一个假值

```java
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
```

## 参考链接

<http://blog.csdn.net/vking_wang/article/details/14166593>

<http://www.importnew.com/7099.html>

<http://www.importnew.com/19208.html>
