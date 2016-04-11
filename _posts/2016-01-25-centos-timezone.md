---
layout : post
title : "CentOS6.5 时区设置"
category : "linux"
tags : [linux]
published: false
---

## 时区选择

输入以下命令选择时区

```
tzselect
```

5)Asia

9)china

1)east China - Beijing, Guangdong, Shanghai, etc.

1)Yes

## 替换Centos系统时区文件

```
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

## 验证

```
date
```
