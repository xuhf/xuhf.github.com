---
layout : post
title : "Mac下安装Lua 5.3"
category : "Mac"
tags : [Mac,lua]
---

## install

1, 首先确定你没有装过lua，使用`lua -v`查看lua的版本

```sh
vvkee:lua-5.3.0 vvkee$ lua -v
Lua 5.3.0  Copyright (C) 1994-2015 Lua.org, PUC-Rio
```
如果出现以上这样，就表示你安装过lua了。

2，去lua的官网下载最新的安装包，目前最新版本为Lua 5.3

下载地址：<http://www.lua.org/download.html>

3, 将下载好的lua安装包lua-5.3.0.tar.gz移动到`/usr/local`目录,并解压。

```sh
sudo mv lua-5.3.0.tar.gz /usr/local/
cd /usr/local/
sudo tar -zxvf lua-5.3.0.tar.gz
```

4，进入文件夹进行编译

```sh
cd lua-5.3.0/
sudo make macosx
sudo make test
sudo make install
```

5，查看lua是否安装正确

```sh
lua -v ## 查看lua版本
which lua ## 查看lua的安装目录
```
