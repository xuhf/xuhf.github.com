---
layout : post
title : "使用cronolog分割tomcat的日志文件"
category : "Linux"
tags : [tools]
---

## 安装

1，<a target='_blank' href='http://download.csdn.net/detail/xuhf_1988/9233029'>下载cronolog</a>

2，安装

```sh
cd cronolog-1.6.2/
./configure
sudo make
sudo make install
```

安装完成后，使用`which cronolog`查看，显示出

```sh
/usr/local/sbin/cronolog
```

即为安装成功


## 使用cronolog

1，本人使用的tomcat版本

进入tomcat的bin目录，使用`./version.sh`进行查看，信息如下：

```
Using CATALINA_BASE:   /opt/apache-tomcat
Using CATALINA_HOME:   /opt/apache-tomcat
Using CATALINA_TMPDIR: /opt/apache-tomcat/temp
Using JRE_HOME:        /usr/java/default
Using CLASSPATH:       /opt/apache-tomcat/bin/bootstrap.jar:/opt/apache-tomcat/bin/tomcat-juli.jar
Server version: Apache Tomcat/7.0.62
Server built:   May 7 2015 17:14:55 UTC
Server number:  7.0.62.0
OS Name:        Linux
OS Version:     2.6.18-308.el5
Architecture:   i386
JVM Version:    1.7.0_55-b13
JVM Vendor:     Oracle Corporation
```

2，需要修改的位置

编辑tomcat的bin目录下的catalina.sh脚本，需要修改的位置如下：

删除

```sh
# 370行
touch "$CATALINA_OUT"
```

将

```sh
# 383，384行
org.apache.catalina.startup.Bootstrap "$@" start \
>> "$CATALINA_OUT" 2>&1 "&"
# 392，393行
org.apache.catalina.startup.Bootstrap "$@" start \
>> "$CATALINA_OUT" 2>&1 "&"
```
替换为：

```sh
# 需要注意，这没有折行，是一行
org.apache.catalina.startup.Bootstrap "$@" start  2>&1 \ | /usr/local/sbin/cronolog "$CATALINA_BASE"/logs/catalina.%Y-%m-%d.out >> /dev/null &
```

重新启动tomcat，进入logs目录，就可以看到类似`catalina.2015-11-04.out`这样的文件了。

到此，使用cronolog分割tomcat的日志文件就全部结束了，本人亲测可用。
