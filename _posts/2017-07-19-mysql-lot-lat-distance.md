---
layout : post
title : "Mysql计算2个坐标点的距离"
category : "mysql"
tags : [mysql]
---

## 代码

```sql
DELIMITER $$
DROP FUNCTION
IF EXISTS `fun_distance`$$
CREATE FUNCTION `fun_distance`(lat1 float,lng1 float,lat2 float,lng2 float) RETURNS float
BEGIN
DECLARE dis float DEFAULT 0.0;
set dis=round(6378.138*2*asin(sqrt(pow(sin( (lat1*pi()/180-lat2*pi()/180)/2),2)+cos(lat1*pi()/180)*cos(lat2*pi()/180)* pow(sin( (lng1*pi()/180-lng2*pi()/180)/2),2)))*1000);
RETURN dis;
END$$
DELIMITER ;
```