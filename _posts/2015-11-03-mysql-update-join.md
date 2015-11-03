---
layout : post
title : "Mysql跨表更新，update join修改数据"
category : "mysql"
tags : [mysql]
---

## 需求描述

存在两张表，member和video，member与video是一对多的关系。video中记录着member的id和status。

目前的数据中video中member_status和member表中的status不一致，需要使用sql语句将其修改一致。

## 初始化数据

member表

```sql
CREATE TABLE `member` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) DEFAULT NULL,
  `status` bigint(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8
```

video表

```sql
CREATE TABLE `video` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `member_id` bigint(20) DEFAULT NULL,
  `member_status` bigint(20) DEFAULT NULL,
  `name` varchar(32) DEFAULT NULL,
  `status` bigint(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8
```

插入数据

```sql
-- member数据
insert into member(name,status) values('java',0);
insert into member(name,status) values('vvkee',1);
insert into member(name,status) values('oracle',1);
insert into member(name,status) values('mysql',0);

-- video数据
insert into video(member_id,member_status,name,status) values(5,1,'m1-video1',1);
insert into video(member_id,member_status,name,status) values(5,0,'m2-video2',0);
insert into video(member_id,member_status,name,status) values(5,1,'m3-video3',1);
insert into video(member_id,member_status,name,status) values(6,1,'m4-video4',0);
insert into video(member_id,member_status,name,status) values(6,1,'m5-video5',1);
insert into video(member_id,member_status,name,status) values(7,0,'m6-video6',1);
insert into video(member_id,member_status,name,status) values(7,0,'m7-video7',0);
insert into video(member_id,member_status,name,status) values(8,1,'m8-video8',1);
insert into video(member_id,member_status,name,status) values(8,0,'m9-video9',1);
```

## 问题解决

我们知道，可以使用join将表中数据全部查询出来，例如

```sql
select m.*,v.* from video v
join member m on v.member_id = m.id
```

那么我也就可以使用join将表中数据修改

```sql
update video v join member m on v.member_id = m.id
	set v.member_status = m.status;
```
