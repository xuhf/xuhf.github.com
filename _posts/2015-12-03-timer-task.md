---
layout : post
title : "Java中实现定时任务的几种方式"
category : "java"
tags : [java]
---

##

突然之间，想总结下这些年在项目中遇到的任务调度方面的知识。

在项目中，我们总会遇到需要使用定时任务来处理的地方，比如：

1. 定时处理前一天的数据
2. 定时检查某种数据

以下将列举出我见过的定时任务的实现方式

## 线程的sleep

个人觉得比较古老的一种方式，这种主要是实现一个循环执行某种任务，主要实现方式如下：

```java
public static void thread() {
    SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    while (true) {
        Date date = new Date();
        System.out.println(format.format(date));
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

PS：我是真的见过这种实现。。。。。。

## 第三方组件

这里的第三方组件我用过的只有quartz了。

```java
public static void quartz() throws Exception {
    JobDetail job = JobBuilder.newJob(PrintTimeJob.class).withIdentity("job1", "group1").build();
    Trigger trigger = TriggerBuilder.newTrigger().withIdentity("trigger1", "group1").startNow()
            .withSchedule(SimpleScheduleBuilder.simpleSchedule().withIntervalInSeconds(5).repeatForever()).build();
    SchedulerFactory schedulerFactory = new StdSchedulerFactory();
    Scheduler scheduler = schedulerFactory.getScheduler();
    scheduler.scheduleJob(job, trigger);
    scheduler.start();
}

// 具体的定时任务需要实现Job接口
public static class PrintTimeJob implements Job {

    public void execute(JobExecutionContext context) throws JobExecutionException {
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = new Date();
        System.out.println("now is :" + format.format(date));
    }

}
```

需要的maven依赖

```xml
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
    <version>2.2.2</version>
</dependency>
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz-jobs</artifactId>
    <version>2.2.2</version>
</dependency>
```

单独使用quartz，扩展性比较强，比较适合定制需求高的项目。

## Spring quartz

正在整理

## Spring task

正在整理

## Linux的crontab

这种是我觉得最好的一种方式，可能没有那么优雅，但是实际用起来比较方便。

具体的实现方式就是真正的任务类和普通的类没有什么区别。

在这之前我们先做点准备工作

在`/opt/`目录下创建`app`目录

在`/opt/app`目录下创建`lib`和`log`目录

`/opt/app` 下我们用来存放执行定时程序的脚本

`/opt/app/lib` 下我们用来存放定时任务的依赖

`/opt/app/log` 下我们用来存放定时程序产生的日志文件

1, 创建一个task类

所谓的Task类，就仅仅是包含一个main方法的类。

```java
public class PrintTimeTask {

    public static void main(String[] args) {
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = new Date();
        System.out.println(format.format(date));
    }
}
```

2, 将需要执行的task项目打成一个jar包

在项目中，我们经常会依赖各种第三方组件的jar包.

我们在打包的时候可以借助maven的插件,将所有的依赖jar包都放在一个目录下.

```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-dependency-plugin</artifactId>
	<executions>
		<execution>
			<id>copy-dependencies</id>
			<phase>package</phase>
			<goals>
				<goal>copy-dependencies</goal>
			</goals>
			<configuration>
				<outputDirectory>${project.build.directory}/lib</outputDirectory>
				<overWriteReleases>false</overWriteReleases>
				<overWriteSnapshots>false</overWriteSnapshots>
				<overWriteIfNewer>true</overWriteIfNewer>
				<excludeArtifactIds>junit,serlvet-api</excludeArtifactIds>
			</configuration>
		</execution>
	</executions>
</plugin>
```

将定时程序构建的jar包`task.jar`和它的依赖包都放入到`/opt/app/lib`目录下

3, 编写一个shell脚本

printTimeTask.sh文件内容如下:

```sh
#!/bin/sh

###############################
#
# @author
#
# 在这里我们将task的所有依赖包都copy到/opt/app/lib目录下
#
###############################

CLASSPATH=`find /opt/app/lib -name "*.jar"|xargs|sed "s/ /:/g"`
export CLASSPATH=.:$CLASSPATH
export LANG="en_US.UTF-8"

date=`date +%Y%m%d`
log_dir=/opt/app/log/$date

if [ ! -d $log_dir ];then
    mkdir -p $log_dir
fi

# log file name
log_file_name=PrintTimeTask.log
class_name=com.vvkee.task.PrintTimeTask

pid=`ps -ef | grep $class_name | grep -v grep | awk '{print $2}'`

if [ -z $pid ];then
    java $class_name >> $log_dir/$log_file_name
else
    echo "$class_name process aleady exists, pid=$pid"
fi
```

赋予`printTimeTask.sh`执行脚本

```sh
chmod a+x printTimeTask.sh
```

将这个脚本放到`/opt/app`目录下

4, 将shell脚本加入到crontab中

每5分钟执行一次

```sh
*/5 * * * * /opt/app/printTimeTask.sh > /dev/null 2>&1
```
具体的crontab命令可以在网上找参考
