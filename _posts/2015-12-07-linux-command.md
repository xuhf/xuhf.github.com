---
layout : post
title : "Linux使用小技巧/小命令集合"
category : "linux"
tags : [linux]
---

## 文件编码转换

```sh
iconv -t utf-8 -f gb2312 -c a.txt > b.txt
```

-f  原编码
-t  目标编码
-c 忽略无法转换的字符

## 
