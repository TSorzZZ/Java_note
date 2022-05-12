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

ReentrantLock(可重入锁，Lock接口的实现类，默认为非公平锁)：

公平锁：严格遵循先来后到

非公平锁：可以插队

**Synchronized和Lock的区别？**

1、Synchronized 内置的Java关键字，Lock是一个Java类
2、Synchronized无法判断获取锁的状态，Lock 可以判断是否获取到了锁
3、Synchronized 会自动释放锁，lock 必须要手动释放锁!如果不释放锁，**死锁**
4、Synchronized线程1(获得锁，阻塞)、线程2(等待，傻傻的等） ; Lock锁就不一定会等待下去;
5、Synchronized 可重入锁，不可以中断的，非公平;Lock，可重入锁，可判断锁，默认非公平(可以自己设置）;
6、Synchronized适合锁少量的代码同步问题，Lock适合锁大量的同步代码!

虚假唤醒问题：

1 wait是会释放锁的，如果写if的话，再次唤醒该线程时不会重新进行等待条件的判断，

如果两个线程一起被唤醒，就会无视等待条件导致程序执行出错，所以用while可以使他继续判断。

### 2.1、生产者消费者问题

**Synchronized版：**

```java
/*
synchronized 加锁格式
1 判断是否满足等待条件
2 执行业务
3 唤醒
*/
public synchronized void produce() throws InterruptedException {
        while(num != 0){        //写if的话会出现虚假唤醒问题
            this.wait();		//wait会释放锁
        }
        num++;
        System.out.println(Thread.currentThread().getName()+"+"+num);
        notifyAll();	//唤醒其他所有线程
    }
```

**Lock版：**

非精准版

```java
class Resource2 {
    private int num = 0;

    Lock lock = new ReentrantLock();			//锁声明
    Condition condition = lock.newCondition();	//监视器

	public void produce() throws InterruptedException {
        lock.lock();

        try {
            while(num != 0){        
                condition.await();	//等待，会释放锁
            }
            num++;
            System.out.println(Thread.currentThread().getName()+"+"+num);
            condition.signalAll();	//唤醒所有监视器
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } finally {
            lock.unlock();	//解锁
        }

    }
}
```

精准唤醒版

```java
class Data{
    private int num = 0;

    Lock lock = new ReentrantLock();
    Condition condition1 = lock.newCondition();//多个监视器
    Condition condition2 = lock.newCondition();
    Condition condition3 = lock.newCondition();

    public void A() throws InterruptedException{
        lock.lock();
        try {
            while(num != 0){
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName()+"+"+num);
            num = 1;
            condition2.signal();	//单独唤醒监视器2 达成精准唤醒
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```



### 2.2、八锁问题

>1：问题引入：线程的执行顺序和执行时间有关吗？
>
>发短信线程  和打电话线程 为例，
>
>答：一次只能有一个线程访问该类的方法，**锁的对象是方法的调用者**，**两个方法共用同一个锁，谁先拿到锁谁先执行。**

>2：问题引入：增加了一个普通方法后，先执行哪个？
>
>普通方法不是同步方法，不受锁的影响。

>3： **同步方法变为静态方法时，锁是针对类模板的锁，static方法随类初始化而创建，所以锁的是class模板类而不是类的对象**

```java
public static synchronized void send(){
    System.out.println("发短信");
}
```



## 3、集合类不安全

### 3.1、ArrayList线程不安全

<font color="red">java.util.ConcurrentModificationException      并发修改异常</font>

modCount和expectedModCount不相等时报错

解决方法例子

解决1 ：Vector的add和remove被synchronized修饰，线程安全

解决2： Collections里的synchronizedList

JUC解决3：CopyOnWriteArrayList<>()  juc中的写入时复制(COW)的List    transient volatile，效率比synchronized高，读写分离思想

```java
public class ListTest {
    public static void main(String[] args) {
        //List<String> list = new ArrayList<>();
        List<String> list = new Vector<>();	//解决1 Vector的add和remove被synchronized修饰，线程安全
		List<String> list = new Collections.synchronizedList(new ArrayList<>());	//解决2 Collections里的synchronizedList
        List<String> list = new CopyOnWriteArrayList<>(); //解决3 juc中的写入时复制(COW)的List    transient volatile，效率比synchronized高，读写分离思想
        
            for (int i = 1; i < 51; i++) {
                new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
                },String.valueOf(i)).start();
            }

    }
}

```

