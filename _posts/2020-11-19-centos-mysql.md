---
layout : post
title : "CentOS6.9下安装MySQL5.7"
category : "CentOS"
tags : [centos,mysql]
---

# MySQL安装（yum方式）

## 卸载CentOS自带MySQL

```sh
service mysqld stop
rpm -qa | grep -i mysql
rpm -e mysql组件名称 --nodeps
```

## 下载rpm文件

```sh
wget https://dev.mysql.com/get/mysql80-community-release-el6-2.noarch.rpm
rpm -Uvh mysql80-community-release-el6-2.noarch.rpm
```

## 选择安装版本
在mysql-community.repo文件里面定义要安装的版本。

因为我们要安装MySQL5.7，所以把5.7的enabled由0改为1,同时将8.0的由1改为0。（默认是安装最新版8.0）。

```sh
vim /etc/yum.repos.d/mysql-community.repo
```

将**[mysql57-community]**下的enabled由0修改为1。
将**[mysql80-community]**下的enabled由1修改为0。

执行以下命令检查是否已启用和禁用了正确的子存储库：

```sh
yum repolist enabled | grep mysql
```

## 安装MySQL

执行以下命令安装（安装时间较长）：

```sh
yum install mysql-community-server
```

## MySQL启动

```sh
service mysqld start
```

## 修改MySQL密码

mysql在初始化的时候会生成临时密码，我们通过日志中，将临时密码找到。

```sh
grep 'temporary password' /var/log/mysqld.log 
```

在`A temporary password is generated for root@localhost:`后面的就是默认密码，我们将默认密码进行复制。

登录MySQL

```sh
mysql -uroot -p
```

输入默认密码进行登录。登录后，使用如下命令修改密码：

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root123456!';
```

MySQL服务器装好以后，默认只能在localhost上登录，如果你要是从另外的IP地址登录，即使是本机登录也会出现问题。

```mysql
grant all privileges on *.* to 'root'@'%' identified by 'Root123456!' with grant option;
FLUSH PRIVILEGES; 
```

## 配置MySQL

修改/etc/my.cnf 配置文件，主要是为了修改字符编码，在修改完成后，将MySQL重新启动。

```sh
vim /etc/my.cnf
```

在配置文件中，在[mysqld]下新增如下：

```
character_set_server=utf8
```

在文件最后增加如下配置：

```
[client]
default-character-set=utf8
```

最后重启MySQL。

```sh
service mysqld restart
```

重新登录数据库，执行如下命令：

```mysql
SHOW VARIABLES LIKE 'character%';
```

我们可以看到，字符编码已经修改为utf8。

## 设置开启自启

```sh
chkconfig --add mysqld
chkconfig mysqld on
```



# MySQL安装rpm包方式

**需要注意，这些命令全部在root用户下执行。**

##  卸载CentOS自带MySQL

```sh
service mysqld stop
rpm -qa | grep -i mysql
rpm -e mysql组件名称 --nodeps
```

## 获取与上传安装包

安装包可由以下地址下载。

```
https://downloads.mysql.com/archives/community/
```

或执行以下命令下载。

```
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.18-1.el6.x86_64.rpm-bundle.tar
```

将`mysql-5.7.18-1.el6.x86_64.rpm-bundle.tar`上传到服务器，并进行解压。

```sh
cd /app/soft
mkdir -p /app/soft/mysql
tar -xvf mysql-5.7.18-1.el6.x86_64.rpm-bundle.tar -C /app/soft/mysql
```

## 安装MySQL

```sh
rpm -ivh mysql-community-common-5.7.18-1.el6.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.18-1.el6.x86_64.rpm
rpm -ivh mysql-community-client-5.7.18-1.el6.x86_64.rpm
rpm -ivh mysql-community-server-5.7.18-1.el6.x86_64.rpm
```

## MySQL启动

```sh
service mysqld start
```

## 修改MySQL密码

mysql在初始化的时候会生成临时密码，我们通过日志中，将临时密码找到。

```sh
grep 'temporary password' /var/log/mysqld.log 
```

在`A temporary password is generated for root@localhost:`后面的就是默认密码，我们将默认密码进行复制。

登录MySQL

```sh
mysql -uroot -p
```

输入默认密码进行登录。登录后，使用如下命令修改密码：

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root123456!';
```

MySQL服务器装好以后，默认只能在localhost上登录，如果你要是从另外的IP地址登录，即使是本机登录也会出现问题。

```mysql
grant all privileges on *.* to 'root'@'%' identified by 'Root123456!' with grant option;
FLUSH PRIVILEGES; 
```

## 配置MySQL

修改/etc/my.cnf 配置文件，主要是为了修改字符编码，在修改完成后，将MySQL重新启动。

```sh
vim /etc/my.cnf
```

在配置文件中，在[mysqld]下新增如下：

```
character_set_server=utf8
```

在文件最后增加如下配置：

```
[client]
default-character-set=utf8
```

最后重启MySQL。

```sh
service mysqld restart
```

重新登录数据库，执行如下命令：

```mysql
SHOW VARIABLES LIKE 'character%';
```

我们可以看到，字符编码已经修改为utf8。

## 设置开启自启

```sh
chkconfig --add mysqld
chkconfig mysqld on
```