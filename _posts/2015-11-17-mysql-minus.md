---
layout : post
title : "Mysql查询2个表的差集并修改差集的数据"
category : "mysql"
tags : [mysql]
---

## 需求

在做一个系统时，用户可以选择付款方式，付款方式包括支付宝和银行。

在用户表会记录用户的付款方式(`payment_code`)，对应到用户支付宝信息表或者用户银行信息表。

但是目前由于一个失误，造成用户表中的`payment_code`不正确，就是说，用户表中显示的`payment_code`为`BANK`。

但是他可能在用户银行信息表中不存在记录，用户支付宝信息表同理。

那么，我们就需要查出这部分用户，让他们去重新填写。

具体到mysql上，就是说要查到在用户表中`payment_code`等于`BANK`，但是却不在用户银行信息表中的用户。

## 建立测试表

```sql
-- 用户信息简表
drop table if EXISTS `test_user`;
create table test_user(
    id bigint primary key auto_increment,
    payment_code varchar(32)
);

-- 用户支付宝信息简表
drop table if EXISTS `test_user_alipay`;
create table test_user_alipay (
    id bigint primary key auto_increment,
    user_id bigint
);

-- 用户银行信息简表
drop table if EXISTS `test_user_bank`;
create table test_user_bank (
    id bigint primary key auto_increment,
    user_id bigint
);

-- 插入数据
insert into test_user(payment_code) values('BANK');
insert into test_user(payment_code) values('BANK');
insert into test_user(payment_code) values('BANK');
insert into test_user(payment_code) values('ALIPAY');
insert into test_user(payment_code) values('ALIPAY');
insert into test_user(payment_code) values('ALIPAY');

insert into test_user_bank(user_id) values(1);
insert into test_user_bank(user_id) values(2);
insert into test_user_alipay(user_id) values(5);
insert into test_user_alipay(user_id) values(6);
```

查看表中数据

```sql
select * from test_user
select * from test_user_bank;
select * from test_user_alipay;
```

就会发现：

用户4的支付方式为支付宝，但是却在用户支付宝信息表中不存在这个用户的记录。

用户3的支付方式为银行，但是却在用户银行信息表中不存在这个用户的记录。

## 查询用户表与支付信息表之间的差集

首先找到支付宝的

```sql
select u.* from test_user u
left join test_user_alipay a on a.user_id = u.id
where u.payment_code = 'ALIPAY'
and ISNULL(a.user_id);
```

找到银行的

```sql
select u.* from test_user u
left join test_user_bank a on a.user_id = u.id
where u.payment_code = 'BANK'
and ISNULL(a.user_id);
```

## 修复问题

修复支付方式为支付宝的用户付款方式为空

```sql
update test_user u left join test_user_alipay a on a.user_id = u.id
set u.payment_code = ''
where u.payment_code = 'ALIPAY'
and ISNULL(a.user_id);
```

修复支付方式为银行的用户付款方式为空

```sql
update test_user u left join test_user_bank a on a.user_id = u.id
set u.payment_code = ''
where u.payment_code = 'BANK'
and ISNULL(a.user_id);
```
