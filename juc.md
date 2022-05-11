# JUC并发编程详解

## 1、基础知识

 **什么是JUC：**Java.util.concurrent包，用于并发编程。

**进程：**一个程序，系统进行资源分配和调度的基本单位。

**线程：**操作系统能够进行运算调度的最小单位，一个进程往往可以包含多个线程。

Java默认有几个线程?最少2个，mian、GC

对于Java而言调用多线程的方法:Thread、Runnable（静态代理）、Callable（静态代理）

**java真的可以开启线程嘛？**(面试题)
不行，因为开启线程需要调用start0()，这是个本地方法，使用的是底层c++。

**线程有几种状态？**

```java
public enum State {
   //新生
    NEW,
    //运行
RUNNABLE,
    //阻塞
    BLOCKED,
    //无限期等待
    WAITING,
    //限时等待
    TIMED_WAITING,
    //终止
    TERMINATED;
}
```

**wait和sleep的区别？**

1、来自不同的类
wait    -> object
sleep  -> Thread

2、关于锁的释放
wait 会释放锁，sleep睡觉了，抱着锁睡觉，不会释放!
3、使用的范围不同
wait：必须在同步代码块。
sleep：可在任何地方睡。
#即有synchronized修饰符修饰的语句块，被该关键词修饰的语句块，将加上内置锁。实现同步。
例：synchronized(Object o ){}
4、是否需要捕获异常，两者都需要捕获异常
wait：需要捕获异常（中断异常）
sleep：必须要捕获异常

## 2、Lock锁

线程就是一个单独的资源类，没有任何的附属操作

**Synchronized和Lock的区别？**



ReentrantLock(可重入锁，Lock接口的实现类，默认为非公平锁)：

公平锁：严格遵循先来后到

非公平锁：可以插队
