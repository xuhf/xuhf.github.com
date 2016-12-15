---
layout : post
title : "Java的ClassaLoader机制"
category : "java"
tags : [java]
published: false
---


```java
public class ClassLoaderDemo {

	public static void main(String[] args) {
		System.out.println(ClassLoader.getSystemClassLoader());
		System.out.println(ClassLoaderDemo.class.getClassLoader());
	}
}

// sun.misc.Launcher$AppClassLoader@1efde7ba
// sun.misc.Launcher$AppClassLoader@1efde7ba
```
