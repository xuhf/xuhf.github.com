---
layout : post
title : "Mysql函数分割字符串"
category : "mysql"
tags : [mysql]
---

## 字符串拆分统计元素个数

```sql
DELIMITER $$
CREATE FUNCTION `func_get_split_string_total`(
f_string varchar(1000),f_delimiter varchar(5)
) RETURNS int(11)
BEGIN
  return 1+(length(f_string) - length(replace(f_string,f_delimiter,'')));
END$$
DELIMITER ;
```

## 返回按照分隔符进行拆分后某一个位置的元素

```sql
DELIMITER $$
CREATE FUNCTION `func_get_split_string`(
f_string varchar(1000),f_delimiter varchar(5),f_order int) RETURNS varchar(255) CHARSET utf8
BEGIN
  declare result varchar(255) default '';
  set result = reverse(substring_index(reverse(substring_index(f_string,f_delimiter,f_order)),f_delimiter,1));
  return result;
END$$
DELIMITER ;
```

## 使用

首先使用`func_get_split_string_total`函数统计元素个数

在使用`func_get_split_string`函数获取某一个位置的元素