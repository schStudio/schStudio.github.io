---
layout: post
title: Java中的线程池
category: Java
---

* content
{:toc}

## 线程池简介

Java中的线程池是一个**并发框架**, 几乎所有需要**异步或并发执行任务**的程序都可以使用线程池.

使用线程池的好处有三个:

* **降低资源消耗** : 线程的创建与销毁需要消耗资源, 而线程池中的线程是已创建好的并可以重复利用
* **提高响应速度** : 当有任务交给线程池时, 线程池利用已有的线程立刻执行任务, 不需要创建新线程
* **提高线程的可管理性** : 线程是稀缺资源, 如果无限制地创建, 不仅会消耗系统资源, 还会降低系统的稳定性, 使用线程池可以进行统一分配, 调优, 监控

- - -

## 线程池的实现原理

### 线程池的工作原理

![java-concurrency-thread-pool]({{ site.url }}/assets/java/java-concurrency-thread-pool.jpg)

![java-concurrency-thread-pool-executor]({{ site.url }}/assets/java/java-concurrency-thread-pool-executor.jpg)

`ThreadPoolExecutor`的`execute()`方法执行步骤如下:

1. 如果当前线程数量小于`corePoolSize`, 则创建线程(获取全局锁)并执行, 否则进入步骤2
2. 如果当前执行队列`Blocking Queue`未满, 则加入执行队列, 否则进入步骤3
3. 如果`maximumPool`未满, 则创建线程(获取全局锁)并执行, 否则进入步骤4
4. 任务被拒绝, 并调用`RejectedExecutionHandler.rejectedExecution()`方法

ThreadPoolExecutor的任务执行步骤如此设计的思路, **是为了在执行`execute()`方法时, 尽可能地避免获取全局锁**

在ThreadPoolExecutor完成预热之后(`corePoolSize`已满, 任务开始加入`BlockingQueue`), 几乎所有的`execute()`方法调用都是执行步骤2, 而步骤2不需要获取全局锁.

### 线程池的源码分析

线程池的`execute()`方法源码如下:

    public void execute( Runnable command ) {
        if( command == null )
            throw new NullPointerException();
        //                              **************步骤1**************
        if( poolSize >= corePoolSize || !addIfUnderCorePoolSize(command) ) {
            //                         **********步骤2**********
            if( runState == RUNNING && workQueue.offer(command) ) {
                if( runState != RUNNING || poolSize == 0 )
                    ensureQueuedTaskHandled( command );
            //         ***************步骤3****************
            } else if( !addIfUnderMaximumPoolSize(command) ) {
            //  ******步骤4*******
                reject( command );
            }
        }
    }

工作线程`Worker`的`run()`方法源码如下:

    public void run() {
        try {
            Runnable task = firstTask;
            firstTask = null;
            while( task != null || (task == getTask()) != null ) {
                runTask( task );
                task = null;
            }
        } finally {
            workerDone( this );
        }
    }

- - -

## 线程池的使用

### 线程池的创建

	ThreadPoolExecutor threadpool = new ThreadPoolExecutor( corePoolSize, maximumPoolSize, 
    keepAliveTime, milliseconds, runnalbeTaskQueue, handler );

* `corePoolSize` : 线程池核心线程数量
* `maximumPoolSize` : 线程池最大线程数量
* `keepAliveTime` : 线程池的工作线程空闲后, 保持存活的时间
* `milliseconds` : `keepAliveTime`的时间单位
* `runnableTaskQueue` : 保存等待执行任务的阻塞队列
* `handler` : 饱和策略, 任务无法执行时的处理机制

`runnalbeTaskQueue`可选择的阻塞队列有以下:

* `ArrayBlockingQueue` : 基于数组的有界队列, 按照FIFO进行任务调度
* `LinkedBlockingQueue` : 基于链表的阻塞队列, 按照FIFO进行任务调度, 吞吐量通常高于`ArrayBlockingQueue`
* `SynchronousQueue` : 不存储元素的阻塞队列, 每个插入操作必须等另一个线程的移除操作, 否则插入操作一直处于阻塞状态, 吞吐量通常高于`LinkedBlockingQueue`
* `PriorityBlockingQueue` : 具有优先级的无限阻塞队列

Java线程池框架提供了4种饱和策略:

* `AbortPolicy` : 抛出异常
* `CallerRunsPolicy` : 还给调用者线程执行任务
* `DiscardOldestPolicy` : 丢弃队列最近的一个任务, 并执行当前任务
* `DiscardPolicy` : 不处理

### 线程池的任务执行

有两个方法向线程池提交任务, 分别是`execute`和`submit`

* `execute` : 用于执行不需要返回值的任务

        threadpools.execute( new Runnable() {
                                @Override
                                public void run() {
                                    //...
                                });

* `submit` : 用于执行需要返回值的任务

		Future<Object> future = executor.submit( hasReturnValueTask );
        try {
        	Object obj = future.get();
		} catch( InterruptedException e ) {
        	// 处理中断异常
		} catch( ExecutionException e ) {
        	// 处理无法执行任务异常
		} finally {
	        // 关闭线程池
	        executors.shutdown();
		}

> `submit`可以设置计时器, 就算任务没有完成也会返回

### 线程池的关闭

有两个方法关闭线程池, 分别是`shutdown`和`shutdownNow`. 它们的工作原理都是遍历线程池中的工作线程, 然后逐个调用线程的`interrupt`方法来中断线程, 所以无法响应中断的任务可能永远无法终止.

* `shutdown` : 将线程池的状态设置为SHUTDOWN状态, 然后中断所有没有正在执行的任务
* `shutdownNow` : 将线程池的状态设置为STOP状态, 然后尝试停止所有的正在执行或暂停执行的任务, 并返回等待执行任务的列表

只要调用了这两个方法中的任何一个, `isShutdown`方法就会返回`true`.

当所有的任务都关闭后, 才表示线程池关闭成功, `isTerminated`方法会返回`true`.

通常调用`shutdown`来关闭线程池, 如果任务不一定要执行完, 可以调用`shutdownNow`.

### 合理地配置线程池

想要合理配置线程池, 需要考虑以下的因素:

* 任务的性质 : CPU密集型任务, IO密集型任务, 混合型任务
* 任务的优先级 : 高, 中, 低
* 任务的执行时间 : 长, 中, 短
* 任务的依赖性 : 是否依赖其它系统资源, 例如数据库连接

性质不同的任务可以用不同规模的线程池分开处理:

* CPU密集型任务应配置尽可能小的线程, 如配置N<sub>cpu</sub>+1个线程的线程池
* IO密集型任务应配置尽可能大的线程, 如配置2*N<sub>cpu</sub>个线程的线程池
* 混合型任务可以将任务拆开为CPU密集型任务和IO密集型任务, 如果两个任务执行时间相差不大; 如果相差很大, 那么没必要将任务拆开, 直接应用CPU密集型任务或IO密集型任务
* 优先级不同的任务可以使用优先级队列`PriorityBlockingQueue`来处理
* 执行时间不同的任务可以使用不同规模的线程池处理, 也可以使用优先级队列, 让执行时间短的任务先执行
* 依赖数据库连接的任务, 因为要提交SQL查询任务, 需要时间等待返回结果, 那么CPU空闲时间比较多, 应该设置尽量多的线程, 让CPU忙起来

> 建议使用有界队列. 有界队列能增加系统俄稳定性和预警能力

### 线程池的监控

如果在系统中大量使用线程池, 则有必要对线程池进行监控, 方便在出现问题时, 可以根据线程池的使用状况快速定位问题. 可以通过线程池提供的参数进行监控, 在监控线程池的时候可以使用以下属性:

* `taskCount` : 线程池需要执行的任务总数量
* `completedTaskCount` : 线程池在运行过程中已经完成的线程数量
* `largestPoolSize` : 线程池中曾经创建过的最大线程数量
* `getPoolSize` : 线程池中线程的数量
* `getActiveCount` : 获取活动的线程数量

另外, 还可以通过扩展线程池进行监控. 可以通过继承线程池来自定义, 重写线程池的`beforeExecute`, `afterExecute`, `terminated`这几个方法, 可以在任务执行前, 执行后, 线程池关闭前执行一些代码来进行监控. 例如, 监控任务的平均执行时间, 最大执行时间, 最小执行时间. 这几个方法在线程池中都是空方法.

	protected void beforeExecute( Thread t, Runnable r ) {}
    protected void afterExecute( Runnable r, Throwable t ) {}
    protected void terminated() {}