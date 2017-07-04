---
layout : post
title : "SpringBoot使用TaskExecutor进行多任务配置"
category : "java"
tags : [springboot]
---

## 问题分析

在SpringBoot的使用过程中，我们难免会使用到定时任务，在定义定时任务时，我们大多会使用`@Shedule`。

那么问题来了，在一个任务类中，定义2个任务`A`和`B`，使用`@Shedule`注解启用定时任务，

如果A任务在执行的过程中，B的周期到来了，那么B是阻塞，还是跟A并发执行？

针对这个问题，我们进行一个实验。

定义了一个任务类Task，如下：

```java
import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class Task {

    @Scheduled(fixedDelay = 1000)
    public void printTime() {
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println("task one " + format.format(new Date()));
    }

    @Scheduled(fixedDelay = 1000)
    public void printTimeSleep() throws InterruptedException {
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println("===================----> task two " + format.format(new Date()));
        Thread.sleep(5000);
    }

}
```

在执行定时任务的时候就会发现，这2个任务并不是并行模式，而是阻塞模式的。

在任务启动后，任务的执行结果如下：

```
task one 2017-07-04 13:26:10
===================----> task two 2017-07-04 13:26:11
task one 2017-07-04 13:26:16
===================----> task two 2017-07-04 13:26:17
task one 2017-07-04 13:26:22
===================----> task two 2017-07-04 13:26:23
task one 2017-07-04 13:26:28
......
```

这显然不符合我的要求，我想要结果是2个任务并行，互不影响，通过在网上查资料并实验。

发现这个时候我们需要用到TaskExecutor。

## TaskExecutor的配置

新建`applicationContext.xml`

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:task="http://www.springframework.org/schema/task"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/task
        http://www.springframework.org/schema/task/spring-task-3.0.xsd">

    <!-- Enables the Spring Task @Scheduled programming model -->
    <task:executor id="executor" pool-size="5" />
    <task:scheduler id="scheduler" pool-size="10" />
    <task:annotation-driven executor="executor"
        scheduler="scheduler" />

</beans>
```

新建配置类`XmlImportingConfiguration.java`

```
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ImportResource;

@Configuration
@ImportResource("applicaitonContext.xml")
public class XmlImportingConfiguration {

}

```

重新启动SpringBoot，就会发现，我们的任务可以以并行模式运行了。

运行结果如下：

```
task one 2017-07-04 13:39:20
task one 2017-07-04 13:39:21
task one 2017-07-04 13:39:22
task one 2017-07-04 13:39:23
===================----> task two 2017-07-04 13:39:24
task one 2017-07-04 13:39:24
task one 2017-07-04 13:39:25
task one 2017-07-04 13:39:26
task one 2017-07-04 13:39:27
task one 2017-07-04 13:39:28
task one 2017-07-04 13:39:29
===================----> task two 2017-07-04 13:39:30
task one 2017-07-04 13:39:30
task one 2017-07-04 13:39:31
task one 2017-07-04 13:39:32
task one 2017-07-04 13:39:33
task one 2017-07-04 13:39:34
task one 2017-07-04 13:39:35
===================----> task two 2017-07-04 13:39:36
task one 2017-07-04 13:39:36
......
```