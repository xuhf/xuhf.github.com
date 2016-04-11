---
layout : post
title : "Oracle 存储过程入门"
category : "Oracle"
tags : [Oracle,存储过程]
published: false
---

##　什么是存储过程？

`存储过程`（Stored Procedure）是在大型数据库系统中，一组为了完成特定功能的SQL语句集。

存储在数据库中，经过第一次编译后再次调用不需要再次编译，用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数）来执行它。

也就是说，我们可以将我们的商业逻辑和业务逻辑使用存储过程存储在Oracle中。

## 基本语法

```sql
CREATE OR REPLACE PROCEDURE TEST_PROC(V_ID IN VARCHAR2, V_NAME OUT VARCHAR2)
AS
BEGIN
  SELECT NAME INTO V_NAME FROM T_EMP WHERE V_ID = V_ID;
END TEST_PROC;
```
