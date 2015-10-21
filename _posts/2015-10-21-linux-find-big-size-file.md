---
layout : post
title : "Linux下如何查找大文件或文件夹"
category : "Linux"
tags : [linux,find]
---

## 为什么要查找大文件？

在一些情况下，我们的服务器可能会被各种各样的日志文件、垃圾文件充满。

造成磁盘空间过小，当我们想要完成某一项工作的时候，突然发现，咦，磁盘空间竟然不够了。

这个时候，我们需要将大文件查找出来，或者进行备份，或者进行删除。

## 查看磁盘空间

Linux上如何查看磁盘空间？

```sh
vvkee:~ vvkee$ df -h
Filesystem      Size   Used  Avail Capacity  iused    ifree %iused  Mounted on
/dev/disk1     233Gi   64Gi  168Gi    28% 16827669 44157673   28%   /
devfs          180Ki  180Ki    0Bi   100%      623        0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0        0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0        0  100%   /home
```

## 如何查找大文件

```sh
$ find . -type f -size +1024M
./package/Illustrator_16_LS3.dmg
./package/Microsoft Office 2016 v15.11.2.dmg
./package/OfficePreview.pkg
./package/ubuntukylin-14.04.2-desktop-amd64.iso
```

可以配合例如`xargs`命里来进行查看这些文件的大小，以及对这些文件的排序。

## 如何查找Linux下的大目录

 ```
$ du -h --max-depth=2 ##Linux下可用
$ du -h -d 2 ##Mac下可用
 ```
