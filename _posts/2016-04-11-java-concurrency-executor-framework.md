---
layout: post
title: Executor框架
category: Java
---

* content
{:toc}

## `Executor`框架简介

### 两级调度模型

在`HotSpot VM`的线程模型中, Java线程被一对一映射为本地操作系统中的线程. Java线程启动时就会创建一个本地操作系统线程. 当Java线程终止时, 操作系统线程也会被回收.

两级调度模型分别如下:

* 在上层, Java多线程程序通常把应用分解为多个任务, 然后使用用户级的调度器(`Executor`框架)将这些任务映射为固定数量的线程
* 在下层, 操作系统内核将这些线程映射到硬件处理器上

![java-concurrency-executor-model]({{ site.url }}/assets/java/java-concurrency-executor-model.png)

### 结构与成员

`Executor`的结构主要由3部分组成如下:

* **任务** : 任务的表示通过实现`Runnalbe`接口和`Callable`接口
* **任务的执行** : 任务执行机制的核心接口`Executor`, 以及继承了`Executor`的`ExecutorService`接口. Executor框架有两个关键类实现了`ExecutorService`接口
	* `ThreadPoolExecutor`
	* `ScheduledThreadPoolExecutor`
* **异步计算的结果** : 包括`Future`和实现`Future`接口的`FutureTask`类

`Executor`框架的类与接口如下:

![java-concurrency-executor-framework-package]({{ site.url }}/assets/java/java-concurrency-executor-framework-package.png)

`Executor`框架的使用示意图如下:

![java-concurrency-executor-usage]({{ site.url }}/assets/java/java-concurrency-executor-usage.png)

`Executor`框架的成员包括:

* `ThreadPoolExecutor` : 通常使用`Executors`工厂类来创建
	* `SingleThreadPool` : 适用于需要保证各个任务顺序执行, 并且在任意时间点, 不会有多个线程是活动的场景
	* `FixedThreadPool` : 适用于为了满足资源管理的需求, 而限制当前线程数量的应用场景. 适用于负载较重的服务器
	* `CachedThreadPool` : 适用于执行很多短期异步任务的小程序, 或者负载较轻的服务器
* `ScheduledThreadPoolExecutor` : 通常使用`Executors`工厂类来创建
	* `ScheduledThreadPool` : 适用于需要多个后台线程周期执行任务, 同时为了满足资源管理需求而限制后台线程数量的场景
	* `SingleScheduledThreadPool` : 适用于需要单个后台线程周期执行任务, 同时需要保证顺序执行各个任务的场景
* `Future` : 当我们把`Rnnable`和`Callable`的实现类`submit`给`ThreadPoolExecutor`或`ScheduledThreadPoolExecutor`时, 会返回一个`FutureTask`对象, 表示异步计算的结果
* `Runnalbe和Callable` : `Runnable`表示不会返回结果的任务, `Callable`表示可以返回结果的任务. `Executors`可以把`Runnable`包装为`Callable`

## `ThreadPoolExecutor`详解

Executors框架的核心类是ThreadPoolExecutor, 它是线程池的实现类, 下面是参数的主要说明:

| 参数 | 说明 |
|--------|--------|
|`corePoolSize`|核心线程池的大小|
|`maximumPoolSize`|最大线程池的大小|
|`BlockingQueue`|保存任务的工作对列|
|`RejectedExecutionHandler`|当任务无法执行时的处理机制|

### `FixedThreadPool`详解

`FixedThreadPool`的创建代码如下:

	public static ExecutorService newSingleThreadPool( int nThreads ) {
        return new ThreadPoolExecutor( nThreads, nThreads, 
                                        0L, TimeUnit.MILLISECONDS, 
                                        new LinkedBlockingQueue<Runnable>() );

`FixedThreadPool`的运行示意图如下:

![java-concurrency-fixed-thread-pool-execution]({{ site.url }}/assets/java/java-concurrency-fixed-thread-pool-execution.png)

`LinkedBlockingQueue`是基于链表实现的, 所以该任务队列是一个无界队列, 因此

* 线程池中的线程数量不会超过`corePoolSize`
* `maximumPoolSize`是一个无效参数
* `keepAliveTime`是一个无效参数
* 不会拒绝任务(也就是不会调用`RejectedExecutionHandler.rejectedExecution`)

### `SingleThreadPool`详解

`SingleThreadPool`的创建代码如下:

    public static ExecutorService newFixedThreadPool() {
        return new ThreadPoolExecutor( 1, 1, 
                                        0L, TimeUnit.MILLISECONDS, 
                                        new LinkedBlockingQueue<Runnable>() );

`SingleThreadPool`与`FixedThreadPool`基本上一样, 只是`corePoolSize`的大小为1, 使用的队列也是无界队列, 所以具有`FixedThreadPool`一样的性质

`SingleThreadPool`的运行示意图如下:

![java-concurrency-single-thread-pool-execution]({{ site.url }}/assets/java/java-concurrency-single-thread-pool-execution.png)

### `CachedThreadPool`详解

`CachedThreadPool`的创建代码如下:

    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor( 0, Integer.MAX_VALUE, 
                                        60L, TimeUnit.SECONDS, 
                                        new SynchronousQueue<Runnable>() );

`CachedThreadPool`的运行示意图如下:

![java-concurrency-cached-thread-pool-execution]({{ site.url }}/assets/java/java-concurrency-cached-thread-pool-execution.png)

`CachedThreadPool`的`corePoolSize`被设置为0, 也就是`corePool`为空; `maximumPoolSize`被设置为`Integer.MAX_VALUE`, 也就是`maximumPool`是无界的.这里把`keepAliveTime`设置为`60s`, 意味着空闲线程等待新任务的最长时间为60秒, 空闲线程超过60秒就会被终止.

由于`maximumPool`是无界的, 如果主线程提交任务的速度高于`maximumPool`中线程处理任务的速度, 那么就会不断创建新线程, 极端情况下, 主机的CPU和内存资源将会被耗尽.

## `ScheduledThreadPoolExecutor`详解

`ScheduledThreadPoolExecutor`继承自`ThreadPoolExecutor`. **它主要用于在给定的延迟之后运行任务, 或者定期执行任务**.

### 运行机制

`ScheduledThreadPoolExecutor`的运行示意图如下:

![java-concurrency-scheduled-thread-pool-execution]({{ site.url }}/assets/java/java-concurrency-scheduled-thread-pool-execution.png)

### 实现

`ScheduledThreadPoolExecutor`的任务执行步骤如下:

![java-concurrency-scheduled-thread-pool-execution-steps]({{ site.url }}/assets/java/java-concurrency-scheduled-thread-pool-execution-steps.png)

步骤说明如下:

1. 线程从`DelayQueue`中获取已到期的`ScheduledFutureTask`(`DelayQueue.take()`). 到期任务是指`ScheduledFutureTask`的`time`大于等于当前时间
2. 执行`ScheduledFutureTask`
3. 修改`ScheduledFutureTask`的`time`变量为下次将要执行的时间
4. 把`ScheduledFutureTask`返回`DelayQueue`中

`DelayQueue.take()`的执行步骤如下:

![java-concurrency-scheduled-thread-pool-execution-delay-queue-take]({{ site.url }}/assets/java/java-concurrency-scheduled-thread-pool-execution-delay-queue-take.png)

1. 获取`Lock`
2. 获取周期任务

	2.1. 如果`PriorityQueue`为空, 当前线程到`Condition`中等待

	2.2. 如果`PriorityQueue`的头元素的`time`时间比当前时间大, 当前线程到`Condition`中等待到`time`时间

	2.3.1 获取`PriorityQueue`的头元素

	2.3.2 如果`PriorityQueue`不为空, 则唤醒`Condition`中等待的所有线程

3. 释放`Lock`

`DelayQueue.offer()`的执行步骤如下:

![java-concurrency-scheduled-thread-pool-execution-delay-queue-offer]({{ site.url }}/assets/java/java-concurrency-scheduled-thread-pool-execution-delay-queue-offer.png)

1. 获取`Lock`
2. 添加任务

	2.1. 向`PriorityQueue`添加任务

	2.2. 如果添加到`PriorityQueue`的是头元素, 唤醒在`Condition`中等待的所有线程

3. 释放`Lock`

## `FutureTask`详解

### 简介

`FutureTask`代表异步计算的结果, 实现了`Runnable`接口和`Future`接口.

`FutureTask`有3种状态, 如下图所示:

![java-concurrency-future-task-status]({{ site.url }}/assets/java/java-concurrency-future-task-status.png)

### 使用

`FutureTask`的使用方法有`get()`和`cancel()`, 如下图所示:

![java-concurrency-future-task-get-and-cancel]({{ site.url }}/assets/java/java-concurrency-future-task-get-and-cancel.png)



### 实现

`FutureTask`的实现基于`AbstractQueuedSynchronizer`(简称`AQS`). `AQS`是一个同步框架, 它提供通用机制来原子性管理同步状态, 阻塞, 唤醒线程, 以及维护被阻塞线程的队列.

基于`AQS`实现的同步器包括: `ReentrantLock`, `Semaphore`, `ReentrantReadAndWriteLock`, `CoundDownLatch`, `FutureTask`

每一个基于`AQS`实现的同步器都会包含两种类型的操作, 如下:

* 至少一个`acquire`操作. 这个操作阻塞调用线程, 除非知道`AQS`的状态允许这个线程继续执行. `FutureTask`的`acquire`操作为`get()`.
* 至少一个`release`操作, 这个操作改变`AQS`的状态, 改变后的状态可允许一个或多个阻塞线程被解除阻塞. `FutureTask`的`release`操作为`run()`和`cancel(...)`.

下面是`FutureTask`的设计示意图:

![java-concurrency-future-task-implementation]({{ site.url }}/assets/java/java-concurrency-future-task-implementation.png)

`FutureTask`的级联唤醒示意图:

![java-concurrency-future-task-awake-chain]({{ site.url }}/assets/java/java-concurrency-future-task-awake-chain.png)
