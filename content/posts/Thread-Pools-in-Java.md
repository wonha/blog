---
title: "Thread Pools in Java"
date: 2018-12-22T21:38:53+09:00
draft: false
tags: [Java, Multithreading, Thread Pool]
---

# Executor Framework

## Main Components
1. ExecutorService
    ```
    > Executor
        > ExecutorService
    ```
1. ThreadPoolExecutor
    ```
    > Executor
        > ExecutorService
            > AbstraceExecutorService
                > ThreadPoolExecutor
                    > execute()
                    > submit()
                    > invokeAny()
                    > invokeAll()
                    > shutdown() -- soft shutdown
                    > shutdownNow() -- a waiting ExecutorSErvice will cause the JVM to kee running
    ```
1. ScheduledThreadPoolExecutor
    ```
    > Executor
        > ExecutorService
            > AbstraceExecutorService
                > ThreadPoolExecutor
                    > ScheduledThreadPoolExecutor
    > Executor
        > ExecutorService
            > ScheduledExecutorService
                > ScheduledThreadPoolExecutor
    ```

# Fork/Join Pool Framework
This framework first 'fork' the given task, processing it, and then 'join'.  

## Main Components
1. ForkJoinPool
    ```
    > Executor
        > ExecutorService
            > AbstraceExecutorService
                > ForkJoinPool
    ```
1. ForkJoinWorkerThread
    ```
    > Thread
        > ForkJoinWorkerThread
    ```
Each of ForkJoinWorkerThread contains double-ended queue (deque), which stores forked tasks.  
By using this deque and Work Stealing Algorithm, Fork/Join Pool framework allows one thread to work for multiple (especially recursive) tasks.  

# Spring

1. ThreadPoolTaskExecutor 
    ```
    > Executor -- java.util.concurrent
        > TaskExecutor -- org.springframework.core.task
            > AsyncTaskExecutor
                > SchedulingTaskExecutor
                    > ThreadPoolTaskExecutor

    > ThreadFactory -- java.util.concurrent
    > CustomizableThreadCreator -- org.springframework.scheduling ...
        > CustomizableThreadFactory
            > ExecutorConfigurationSupport
                > ThreadPoolTaskExecutor


    > Executor -- java.util.concurrent
        > TaskExecutor -- org.springframework.core.task
            > AsyncTaskExecutor
                > AsyncListenableTaskExecutor
                    > ThreadPoolTaskExecutor
    ```

