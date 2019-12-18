

# 进程和线程



## 什么是进程？

​		进程（Process），是指计算机中正在运行的程序，在当代多数操作系统中，进程本身不是基本运行单位，而是线程的容器。程序本身只是指令、数据及其组织形式的描述，进程才是程序（那些指令和数据）的真正运行实例。一个程序运行时必定会产生一个进程，若干进程也有可能与同一个程序有关系。

​		简单来说，当你在使用Word写文档的同时，你还和好友聊着QQ并且还在使用着迅雷下载电影。每个软件都在后台开着一个进程。



## 什么是线程？

​		线程是一个轻量级（lightweight）的进程，在一个进程中可以有多个线程（至少有一个），每个线程可以并行执行不同的任务。线程是最小的执行单元，它可以共享进程中的资源。进程好比一个工厂，而线程则是工厂里面的工人，没有工厂工人便没有地方工作（线程运行），当工人数量足够多时工厂的效率便会提高，当然并非工人越多越好，工人数量过多反而会导致效率降低。



## 什么是多线程？

​		Java 对多线程编程提供了内置的支持。 一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。在上面举的工厂与工人的例子中也说明了这一点。正确的使用多线程能够减小资源开销，同时也能帮助程序员编写高效率的程序以达到充分利用CPU的目的。



## 多线程有什么用？

​		上面已经提到，使用多线程能够减小资源开销与充分利用CPU。利用多线程的方式去同时完成几件事情而不互相干扰，当需要处理大量IO操作或处理的情况需要消耗大量的时间时则可以使用多线程提高程序的运行效率。不过缺点则是，如果滥用多线程则会造成死锁，以及影响系统的性能。越多的线程占用的内存空间则越多。



# 主线程

在Java中，main线程便是主线程（Main Thread）。

当一个Java程序启动后，main线程便开始执行，它的重要性主要体现在以下两点：

- 它是产生其他子线程的线程
- 通常它是在最后完成执行，因为它需要执行各种关闭操作

每次的程序运行中至少启动两个线程，一个main线程，一个垃圾收回收线程。因为每次运行时都会启动一个Java虚拟机（JVM），而JVM会启动一个垃圾回收线程。



# 如何创建线程

在Java中，创建线程有两种简单的方法（JDK 1.0）：
- 实现Runnable接口（推荐）
- 继承Thread类



## 实现Runnable接口

首先，创建一个实现Runnable接口的类，并实现其中的`run()`方法。Runnable接口非常的简单，它只包含一个`run()`方法。

```java
public class ThreadDemo {

    public static void main(String[] args) {
        
        // 实例化工作类
        Worker worker = new Worker();
        // 实例化线程类，并将工作类传递给线程类
        Thread thread = new Thread(worker);
        
        System.out.println("开始执行线程"); 
        // 开始执行线程
        thread.start();
        System.out.println("不等待线程执行完毕，继续向下执行");
        
        // 也可通过以下方式来创建并运行一个线程
        // new Thread(new Worker()).start();
    }
}

// 创建一个实现Runnable接口的类
class Worker implements Runnable {
	
    // 重写run()方法，run方法的方法体就是线程的执行体
    @Override
    public void run() {
        System.out.println("实现Runnable接口创建的线程, Running ...");
    }
}
```

运行结果：

```
开始执行线程
不等待线程执行完毕，继续向下执行
实现Runnable接口创建的线程, Running ...
```

**注意：**在执行一个线程时，应该调用`start()`方法，而不是`run()`方法。调用`run()`方法时，只会被当作一个普通的方法执行，程序依旧按照顺序执行；调用`start()`方法时，才会被当作一个真正的多线程程序执行。




## 继承Thread类

通过继承Thread类来创建线程其实和实现Runnable接口是类似的，都需要重写`run()`方法。

```java
public class ThreadDemo {

    public static void main(String[] args) {

        System.out.println("开始执行线程");
        // 直接实例化线程类并执行
        new Worker().start();
        System.out.println("不等待线程执行完毕，继续向下执行");

    }
}

class Worker extends Thread {

    @Override
    public void run() {
        System.out.println("继承Thread类创建的线程, Running ...");
    }
}
```
运行结果：

```
开始执行线程
不等待线程执行完毕，继续向下执行
继承Thread类创建的线程, Running ...
```

```java
public class Thread implements Runnable {
    /****/
}
```

从Thread类的源码中我们也可以看到Thread类实现了Runnable接口。



# Runnable 相较 Thread 的好处

1. Runnable更适合相同的程序代码去处理同一个资源（即资源共享）
2. Runnable可以避免Java的单根继承的限制
3. Runnable增加程序的健壮性，代码可以被多个线程共享，代码和数据独立