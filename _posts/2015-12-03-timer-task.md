---
layout : post
title : "Java中实现定时任务的几种方式"
category : "java"
tags : [java]
published: false
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

## Spring task

## Linux的crontab

这种是我觉得最好的一种方式，可能没有那么优雅，但是实际用起来比较方便。

具体的实现方式就是真正的任务类和普通的类没有什么区别。

1, 创建一个task类

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

在项目中，我们经常会依赖各种第三方组件的jar包。

3, 编写一个shell脚本

4, 将shell脚本加入到crontab中
