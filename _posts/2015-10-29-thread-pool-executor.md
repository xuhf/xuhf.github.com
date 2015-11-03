---
layout : post
title : "ThreadPoolExecutor运转机制"
category : "Java"
tags : [Java]
published: false
---


## 了解

构造函数

```java
  public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          RejectedExecutionHandler handler) {
    this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
         Executors.defaultThreadFactory(), handler);
}
```

参数解释

1. corePoolSize： 线程池维护线程的最少数量
2. maximumPoolSize：线程池维护线程的最大数量
3. keepAliveTime： 线程池维护线程所允许的空闲时间
4. unit： 线程池维护线程所允许的空闲时间的单位
5. workQueue： 线程池所使用的缓冲队列
6. handler： 线程池对拒绝任务的处理策略

## 运行机制

任务通过ex

## 代码示例



<http://blog.csdn.net/cutesource/article/details/6061229>
