---
layout: post
title: "java并发讲义"
date: 2018-12-19 11:24
comments: true
tags: 
	- 笔记
---

#### 线程和进程

#### 基本线程类，怎样开启一个线程，线程状态以及系统调度

wait, notify 必须在synchronized块中调用

Thread类，Runnable接口，Callable接口

- 继承Thread类，重写该类的run()方法。

- 实现Runnable接口，并重写该接口的run()方法

- 使用Callable和Future接口创建线程。

Thread和Runnable的实现区别
<!--more-->

#### 获取线程中的异常

异常不能跨线程传播会main线程，所以必须在本地处理所有在任务内部产生的异常

UncaughtExceptionHandler

#### 线程的优先级，让步，后台线程，join()

#### 线程间数据的共享以及线程安全，共享受限资源

volatile，Synchronized，Lock，ReentrantLock(下数那边可以用这个来避免重复执行，允许你尝试获取但最终未获取锁，这样如果其他人已经获取了这个锁，那你就可以设定等待的超时时间，也可以决定离开去执行其它的事情)

如果一个域完全有synchronized方法或语句来保护，那就不必将其设置为是volatile的

原子类，AtomicInteger,AtomicLong,AtomicReference

其底层就是volatile和CAS 共同作用的结果：

- 首先使用了volatile 保证了内存可见性。

- 然后使用了CAS（compare-and-swap）算法 保证了原子性。

​      其中CAS算法的原理就是里面包含三个值：内存值A  预估值V  更新值 B  当且仅当 V == A 时，V = B; 否则，不会执行任何操作。

BlockingQueue

ConcurrentHashMap

线程的本地存储：ThreadLocal类

#### 线程池

ThreadPoolExecutor

无需显式的管理线程的生命周期

- 重复利用已存在并空闲的线程，从而减少线程对象的创建和线程消亡的开销

- 有效控制最大并发线程数，提高系统资源使用率，同时避免过多资源竞争

- 提供定时、定期、单线程、并发数控制等功能

- Future,Callable提供了更好的异步执行控制

Future.get(); Future.isDone();Future,cancel()…

创建线程池主要有三个静态方法供我们使用，由Executors来进行创建相应的线程池：

- newSingleThreadExecutor返回以个包含单线程的Executor,将多个任务交给此Exector时，这个线程处理完一个任务后接着处理下一个任务，若该线程出现异常，将会有一个新的线程来替代。

- newFixedThreadPool返回一个包含指定数目线程的线程池，如果任务数量多于线程数量，那么没有执行的任务必须等待，直到有任务完成为止。

- newCachedThreadPool根据用户的任务数创建相应的线程来处理，该线程池不会对线程数目加以限制，完全依赖于JVM能创建线程的数量，可能引起内存不足。

- newScheduledThreadPool创建一个至少有n个线程空间大小的线程池。此线程池支持定时以及周期性执行任务的需求。

corePoolSize和maximumPoolSize：

+ 如果当前运行的线程数小于corePoolSize，则创建新线程来处理新的任务对象，即使有其他空闲线程的存在。

+ 如果当前运行的线程数大于corePoolSize，小于maximumPoolSize时，优先向任务队列中添加任务，只有当任务队列满时，才创建新线程。

+ 如果corePoolSize等于maximumPoolSize，则意味着创建了一个固定大小的线程池。

代码演示：

Thread类，Runnable接口，Callable接口

Lock

线程池源码，几种java自带线程池的介绍