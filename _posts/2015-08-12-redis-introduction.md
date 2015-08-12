---
layout : post
title : "redis入门"
category : "NoSQL"
keywords : redis,NoSQL
---

## 安装

首先去redis官网下载redis安装包

我下载的是 redis-3.0.3.tar.gz

```
tar zxvf redis-3.0.3.tar.gz
cd redis-3.0.3
make
```

## redis的数据类型

### string 类型

string类型是redis的最基本类型。

相关的基本命令如下

```sh
127.0.0.1:6379> set test vvkee
OK
127.0.0.1:6379> get test
"vvkee"
```

## list 类型

redis的list类型其实就是一个每个子元素都是string类型的双向链表。

我们可以通过push，pop操作从链表的头部或者尾部添加删除元素。这使得list即可以用作栈，也可以用作队列。

相关的基本命令如下

```sh
127.0.0.1:6379> lpush list aaa bbb
(integer) 2
127.0.0.1:6379> llen list
(integer) 2
127.0.0.1:6379> lrange list 0 1
1) "bbb"
2) "aaa"
127.0.0.1:6379> lpop list
"bbb"
127.0.0.1:6379> rpush list ccc
(integer) 2
127.0.0.1:6379> lrange list 0 2
1) "aaa"
2) "ccc"
127.0.0.1:6379> rpop list
"ccc"
```

## set 类型

redis的set是string类型的无序集合。

相关的基本命令如下

```sh
127.0.0.1:6379> sadd set vvkee // 添加一个 string 元素到,key 对应的 set 集合中,成功返回 1,如果元素已经在集合中 返回 0
(integer) 1
127.0.0.1:6379> sadd set vvkee
(integer) 0
127.0.0.1:6379> sadd set vvkee.com
(integer) 1
127.0.0.1:6379> smembers set // 返回 key 对应 set 的所有元素,结果是无序的
1) "vvkee"
2) "vvkee.com"
127.0.0.1:6379> scard set // 返回 set 的元素个数,如果 set 是空或者 key 不存在返回 0
(integer) 2
127.0.0.1:6379> sadd set2 vvkee
(integer) 1
127.0.0.1:6379> sadd set2 hello
(integer) 1
127.0.0.1:6379> sinter set set2 // 返回所有给定 key 的交集
1) "vvkee"
127.0.0.1:6379> sunion set set2 // 返回所有给定 key 的并集
1) "vvkee"
2) "vvkee.com"
3) "hello"
```
## sorted set 类型

和 set 一样,sorted set 也是 string 类型元素的集合,不同的是每个元素都会关联一个 double 类型 的 score。

相关的基本命令如下

```sh
127.0.0.1:6379> zadd sset 10 vvkee // 添加元素到集合,元素在集合中存在则更新对应 score
(integer) 1
127.0.0.1:6379> zadd sset 9 vvkee.com
(integer) 1
127.0.0.1:6379> zadd sset 1 www.vvkee.com
(integer) 1
127.0.0.1:6379> zadd sset 5 www.isharec.com
(integer) 1
127.0.0.1:6379> zcard sset // 返回集合中元素个数
(integer) 4
127.0.0.1:6379> zrank sset www.vvkee.com // 返回指定元素在集合中的排名(下标),集合中元素是按 score 从小到大排序的
(integer) 0
127.0.0.1:6379> zrank sset vvkee.com
(integer) 2
127.0.0.1:6379> zrange sset 0 4 //类似 lrange 操作从集合中去指定区间的元素。返回的是有序结果
1) "www.vvkee.com"
2) "www.isharec.com"
3) "vvkee.com"
4) "vvkee"
127.0.0.1:6379> zscore sset vvkee // 返回给定元素对应的 score
"10"
127.0.0.1:6379> zrem sset www.vvkee.com // 删除指定元素,1 表示成功,如果元素不存在返回 0
(integer) 1
127.0.0.1:6379> zrange sset 0 4
1) "www.isharec.com"
2) "vvkee.com"
3) "vvkee"
```

## hash 类型

redis的hash是一个string类型的field和value的映射表。

相关的基本命令如下

```
127.0.0.1:6379> hset hash name vvkee // 设置 hash field 为指定值,如果 key 不存在,则先创建
(integer) 1
127.0.0.1:6379> hset hash age 28
(integer) 1
127.0.0.1:6379> hset hash host vvkee.com
(integer) 1
127.0.0.1:6379> hget hash name // 获取指定的 hash field
"vvkee"
127.0.0.1:6379> hmget hash name age // 获取全部指定的 hash filed
1) "vvkee"
2) "28"
127.0.0.1:6379> hlen hash // 返回指定 hash 的 field 数量
(integer) 3
127.0.0.1:6379> hkeys hash // 返回 hash 的所有 field
1) "name"
2) "age"
3) "host"
127.0.0.1:6379> hvals hash // 返回 hash 的所有 value
1) "vvkee"
2) "28"
3) "vvkee.com"
127.0.0.1:6379> hgetall hash // 返回 hash 的所有 filed 和 value
1) "name"
2) "vvkee"
3) "age"
4) "28"
5) "host"
6) "vvkee.com"
127.0.0.1:6379> hdel hash host // 删除指定的 hash field
(integer) 1
127.0.0.1:6379> hgetall hash
1) "name"
2) "vvkee"
3) "age"
4) "28"
```
## 参考

<www.redis.io>

<www.redis.cn>
