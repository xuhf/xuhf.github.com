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

## 提取文件名/目录名

`basename` 从路径中提取出文件名
`dirname` 从路径中提取出目录名

```sh
file=/opt/app/test.sh
dir_name=/opt/app/

basename $file
# test.sh
dirname $file
# /opt/app
dirname $dir_name
# /opt
```
