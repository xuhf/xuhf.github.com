---
layout : post
title : " mysqldump忽略锁表"
category : "mysql"
tags : [mysql]
---

## 问题

在进行线上数据的导出时，遇见如下错误。

```sh
mysqldump: Got error: 1044: Access denied for user 'user'@'ip' to database 'databasename' when doing LOCK TABLES
```

原因是由于在执行mysqldump时不能锁住你要导出的表。

## 解决办法

在导出时增加`--skip-lock-tables`参数。

重新执行

```sh
mysqldump -uroot -p -h hostip database table1 table2 > ~/export.sql
```
