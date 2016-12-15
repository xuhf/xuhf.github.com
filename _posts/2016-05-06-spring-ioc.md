---
layout : post
title : "[Spring学习]Spring IOC 概述"
category : "Spring"
tags : [java,spring]
published: false
---

## 前言

用了Spring很久，但是对它却没有深入研究过，非常遗憾。

今天开始，一边看着Spring的源代码，一边看着书籍，慢慢的进入Spring的世界。

## 什么是IOC

依赖反转，我们也可以称之为依赖注入

在面向对象系统中，对象封装了数据和对数据的处理，对象的依赖关系常常体现在对数据和方法的依赖上。

在没有使用IOC之前，对象的创建及依赖关系需要我们自己去维护。

而在使用IOC之后，对象的创建和依赖关系可以交给IOC容器来维护。

这里的反转指的是责任的反转。

##
