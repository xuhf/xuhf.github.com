---
layout : post
title : "Spring4与Quartz2 实现任务的定时调度"
description: "spring4集成quartz实现任务的定时调度"
category : "java"
tags : ["spring","quartz"]
---

## 需求

最近在做一个任务，需要定时执行，并且将定时任务单独提出来，使用jar包的形式。

## 解决

定义任务执行的Bean

```java
public class PaymentTask {

  private static final Logger logger = LoggerFactory
      .getLogger(PaymentTask.class);

  public void execute() {
    logger.info("PaymentTask is starting...");
    System.out.println("PaymentTask execute");
  }
}
```

定义启动启动定时任务的主方法，当Spring容器启动的时候，定时任务就会启动。

```java
@SuppressWarnings("resource")
public static void main(String[] args) {
  // 启动spring容器
  logger.info("Main spring 容器启动");
  @SuppressWarnings("unused")
  ApplicationContext context = new ClassPathXmlApplicationContext(
      "spring.xml");
}
```

配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

  <!-- 定义要执行的任务bean -->
  <bean id="demoTask" class="你定义的任务类的包名+类名，例如cn.xuhaifei.task.DemoTask">
  </bean>

  <!-- 定义调用对象和调用对象的方法 -->
  <bean id="demoTaskDetail"
    class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
    <!-- 调用的类 -->
    <property name="targetObject" ref="demoTask" />
    <!-- 调用类中的方法 -->
    <property name="targetMethod" value="execute" />
    <!-- 定时调度不允许并发 -->
    <property name="concurrent" value="false" />
  </bean>

  <!-- 定义触发时间 -->
  <bean id="demoTaskTrigger"
    class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">
    <property name="jobDetail" ref="demoTaskDetail" />
    <!-- cron表达式 --><!-- 每5秒执行一次 -->
    <property name="cronExpression" value="*/5 * * * * ?" />
  </bean>

  <!-- 总管理类 如果将lazy-init='false'那么容器启动就会执行调度程序 -->
  <bean lazy-init="false" autowire="no"
    class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
    <property name="triggers">
      <list>
        <ref bean="demoTaskTrigger" />
      </list>
    </property>
  </bean>
</beans>
```

### 版本

Spring4.0.2 + quartz2.2.1

## 问题解决

1，Caused by: java.lang.IncompatibleClassChangeError: class org.springframework.scheduling.quartz.CronTriggerBean has interface org.quartz.CronTrigger as super class

定义Trigger时，请使用 org.springframework.scheduling.quartz.CronTriggerFactoryBean
参考博客[http://blog.csdn.net/shootyou/article/details/8175754](http://blog.csdn.net/shootyou/article/details/8175754)

## 参考

<http://ju.outofmemory.cn/entry/108512>
