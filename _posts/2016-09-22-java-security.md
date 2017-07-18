---
layout : post
title : "Java加密算法总结"
category : "java"
tags : [security]
published: false
---

在我们进行系统设计与开发时,或多多少的都需要设计到加密

个人认为比较典型的加密场景

1. 用户的密码加密
2. api中对参数的加密与解密

针对以上两种加密场景,也恰好的符合了我们对于加密算法的2种认知,即不可逆加密与对称式加密

对于用户的密码,我们要使用不可逆加密算法进行加密,保证用户的安全

对于api中参数的加密,我们就需要使用一个可逆算法,将加密后的数据解密回来,方便我们处理.

## 对称式加密算法

## 不可逆加密

<http://www.iteye.com/topic/1122076/>
<http://security.group.iteye.com/group/wiki/1710-one-way-encryption-algorithm>
