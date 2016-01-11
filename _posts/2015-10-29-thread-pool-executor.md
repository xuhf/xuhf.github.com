---
layout : post
title : "ThreadPoolExecutor入门"
category : "Java"
tags : [Java]
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

## 代码示例

```java
public class Demo {

    private static Log logger = LogFactory.getLog(Demo.class);

    // 休眠时间
    private static int taskSleepTime = 200;

    // 队列最大个数
    private static int maxTaskQueueSize = 10;

    public static void main(String[] args) {
        // 构造一个线程池
        ThreadPoolExecutor threadPool = new ThreadPoolExecutor(5, 10, 10, TimeUnit.SECONDS,
                new ArrayBlockingQueue<Runnable>(maxTaskQueueSize), new ThreadPoolExecutor.DiscardOldestPolicy());

        try {
            for (int i = 0; i < 100;) {
                // 获取当前任务队列大小
                int currentQueueSize = threadPool.getQueue().size();
                logger.info("currentQueueSize =  " + currentQueueSize);
                System.out.println("currentQueueSize = " + currentQueueSize);
                System.out.println("no loop now i is " + i);
                if (currentQueueSize < maxTaskQueueSize) {
                    System.out.println("now i is " + i);
                    threadPool.execute(new ThreadPoolMessageSendTask(i));
                    i++;
                } else {
                    System.out.println("Thread is sleep");
                    Thread.sleep(taskSleepTime);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
            threadPool.shutdown();
        }

        // 等待所有任务执行完成
        while (threadPool.getActiveCount() > 0) {
            logger.debug("active thread count =  " + threadPool.getActiveCount());
            try {
                Thread.sleep(taskSleepTime);
            } catch (InterruptedException e) {
                logger.error("主线程休眠失败:" + e.getMessage(), e);
            }
        }

        // 任务执行完毕，关闭线程池
        threadPool.shutdown();
    }

    static class ThreadPoolMessageSendTask implements Runnable, Serializable {

        private static final long serialVersionUID = 7668409698027485662L;

        private int message;

        ThreadPoolMessageSendTask(int message) {
            this.message = message;
        }

        @Override
        public void run() {
            System.out.println("params is " + message);
            try {
                Thread.sleep(5000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

    }
}
```

参考链接：<http://blog.csdn.net/cutesource/article/details/6061229>
