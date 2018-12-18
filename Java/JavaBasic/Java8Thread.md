## [Java 8 并发入门 [Thread]](#top) :maple_leaf: <b id="top"></b> 
`这可是相当好玩呢！ 线程一个高级程序员的基操`

- [x] :maple_leaf: [`基本的概念`](#know)
- [x] :maple_leaf: [`Java多线程`](#thread)
- [X] :maple_leaf: [`Thread 类`](#use)
- [X] :maple_leaf: [`Thread 操作`](#opr)
- [X] :maple_leaf: [`Java 概念题目`](#face)


----
####  [流的初体验](#top) :star2:<b id="know"></b>  
* **`并发`**:`当有多个线程在操作时,如果系统只有一个CPU,则它根本不可能真正同时进行一个以上的线程，它只能把CPU运行时间划分成若干个时间段,再将时
间 段分配给各个线程执行，在一个时间段的线程代码运行时，其它线程处于挂起状。.这种方式我们称之为并发(Concurrent)。` 

* **`并行`** ：`当系统有一个以上CPU时,则线程的操作有可能非并发。当一个CPU执行一个线程时，另一个CPU可以执行另一个线程，两个线程互不
抢占CPU资源，可以同时进行，这种方式我们称之为并行(Parallel)。`

* **`线程`**:`有时被称为轻量进程(Lightweight Process，LWP），是程序执行流的最小单元。线程是进程中的一个实体，是被系统独立调度和分派的基本单
位，线程自己不拥有系统资源，只拥有一点儿在运行中必不可少的资源，但它可与同属一个进程的其它线程共享进程所拥有的全部资源`

* **`守护线程`**:`守护线程是特殊的线程，一般用于在后台为其他线程提供服务.`

* `每个正在系统上运行的程序都是一个进程。每个进程包含一到多个线程。进程也可能是整个程序或者是部分程序的动态执行。线程是一组指令的集合，或
者是程序的特殊段，它可以在程序里独立执行。也可以把它理解为代码运行的上下文。所以线程基本上是轻量级的进程，它负责在单个程序里执行多任务。
通常由操作系统负责多个线程的调度和执行。`

* `线程是程序中一个单一的顺序控制流程.在单个程序中同时运行多个线程完成不同的工作,称为多线程.`

* `使用线程可以把占据时间长的程序中的任务放到后台去处理`

##### [使用多线程的代价](#top)
`使用多线程有好处,可以同时执行多个任务,防止单线程任务量过大卡死,但是过多的线程也有缺点,使用多线程也具有代价`
* `线程创建代价`:`当程序需要大量使用到多线程的时候 可能需要反复创建 销毁多线程 这也需要时间代价`
* `操作系统上下文切换代价`: `线程属于进程 不占有进程的资源,而只是负责任务的执行,所以每次执行都需要切换上下文分配线程执行所需资源`
* `内存代价`：`更多的线程需要更多的内存空间。`
* `线程的死锁代价`:`多个线程之间可能具有多种关系 需要协调资源共享问题，执行先后问题 所以可能引发死锁`

----
> `推荐对于大量任务 大量计算的时候使用多线程 并发操作`

##### 线程的状态
> `线程也有就绪、阻塞和运行三种基本状态`
* `创建线程`:`当创建一个新的进程时，也创建一个新的线程，进程中的线程可以在同一进程中创建新的线程中创建新的线程。`
* `终止线程`:`可以正常终止自己，也可能某个线程执行错误，由其它线程强行终止。终止线程操作主要负责释放线程占有的寄存器和栈`
* `阻塞线程`:`当线程等待每个事件无法运行时，停止其运行。`
* `唤醒线程`:`当阻塞线程的事件发生时，将被阻塞的线程状态置为就绪态，将其挂到就绪队列。进程仍然具有与执行相关的状态。
例如，所谓进程处于“执行”状态，实际上是指该进程中的某线程正在执行。对进程施加的与进程状态有关的操作，也对其线程起作用
。例如，把某个进程挂起时，该进程中的所有线程也都被挂起，激活也是同样。`


####  [Java多线程](#top) :star2:<b id="thread"></b>  
`Java多线程目前有三种实现方式`
* [`Thread 类`](https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Thread.html)
* [`Runnable 接口`](https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Runnable.html) 
* [`(Callable 接口)`](https://docs.oracle.com/javase/1.5.0/docs/api/java/util/concurrent/Callable.html)

---
`Java的所有的类 运行的时候都有一个 起点 main 方法  Thread类也有自己起点 就是 run方法`

----
`他们的区别呢？`
* `1.Runnable`:`解决单继承定义的局限 定义线程的核心功能`
* `2.Thread类 实现了Runnable 接口 实现操作系统资源分配 调用run方法`
* `3.可以更好用来描述数据共享的概念`
##### :one: Thread 类型
* `支持:JDK1.0开始就支持 Thread 类`
* `继承自`:`java.lang.Object `
* `实现接口`:`Interfaces:Runnable `
* `相关内容请看下面 Thread内介绍`

##### :two: Runnable 接口
`Thread 类是有缺点的 Java是单继承的哟 所以如果有些类需要实现多线程 难道每一个都去基础Thread 吗？ 会不会太麻烦 所有推荐使用 Runnable接口`
```java
//函数式 接口 也就是说支持 
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```
* `发现没有 没有start 方法 那么启动多线程又必须使用 start 方法 怎么办呀 ,借用Thread类的 看到Thread类的构造函数没有  传进去啊`
```java
package com.company;

public class MyRunThread  implements Runnable{
    @Override
    public void run() {
        for(int i =0;i<10;i++){
            System.out.println( Thread.currentThread().getName()+" "+ i);
        }
    }
}
public class Main {
    public static void main(String[] args) {
        MyRunThread run1 = new MyRunThread();
        MyRunThread run2 = new MyRunThread();
        MyRunThread run3 = new MyRunThread();

        new Thread(run1,"Run One").start();
        new Thread(run2,"Run Two").start();
        new Thread(run3,"Run Three").start();
    }
}
```
##### :three: Callable 接口--package java.util.concurrent.Callable<V>;
`源码如何 函数式泛型接口 不知道为何需要设计得那么复杂 了解就好 重点是要 Runnable 配置 Thread 就好了` :hocho:
```java
@FunctionalInterface
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```
* `它的麻烦在于什么呢？ 就是它的多线程调用需要借助其他的东西` 才能被 Thread 执行 并且得到返回值`
    * `Class FutureTask<V> 实现的接口`:`Runnable, Future<V>`
    * `并且具有Get 方法`
```java
import java.util.concurrent.Callable;

public class MyRunThread  implements Callable<String> {

    @Override
    public String call() throws Exception {
        for(int i = 10 ;i > 0;i--){
            System.out.println("卖票 " + i);
        }
        System.out.println( Thread.currentThread().getName() + " 票已经卖光了");
        return "卖票完成";
    }

}

public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        MyRunThread run1 = new MyRunThread();
        MyRunThread run2 = new MyRunThread();

        FutureTask<String> task1 = new FutureTask<>(run1);
        FutureTask<String> task2 = new FutureTask<>(run2);

        new Thread(task1,"call1 ").start();
        new Thread(task2,"call2 ").start();

        String result1 = task1.get();
        String result2 = task2.get();

        System.out.println(result1);
        System.out.println(result2);
    }
}

```
####  [Thread 类](#top) :star2:<b id="use"></b> 
`Thread 类 多线程的一种实现方式 并且整个多线程 都是基于这个Thread 来调用 启动多线程的`

##### run和 start的区别
`分别使用 run 和 start 跑一下 下面的代码`
```Java
public class MyThread  extends  Thread{

    public MyThread(String name){
        super.setName(name);
    }

    @Override
    public void run() {
        for(int i =0;i<10;i++){
            System.out.println( Thread.currentThread().getName()+" "+ i);
        }
    }
}
```

```java
package com.company;

public class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("Thread One");
        MyThread thread2 = new MyThread("Thread Two");
        MyThread thread3 = new MyThread("Thread Three");

        thread1.run();
        thread2.run();
        thread3.run();

        /*
        thread1.start();
        thread2.start();
        thread3.start();
        */
        System.out.println("要不起！");

    }
}
```
* `为什么 Java 多线程 不是通过 run 而是通过 start 方法呢？`
```java
public synchronized void start() {
    /**
     * This method is not invoked for the main method thread or "system"
     * group threads created/set up by the VM. Any new functionality added
     * to this method in the future may have to also be added to the VM.
     *
     * A zero status value corresponds to state "NEW".
     */
    if (threadStatus != 0)
        throw new IllegalThreadStateException(); //运行时异常

    /* Notify the group that this thread is about to be started
     * so that it can be added to the group's list of threads
     * and the group's unstarted count can be decremented. */
    group.add(this);

    boolean started = false;
    try {
        start0();
        // 让操作系统执行这个线程  创建线程 切换上下文 并执行调度和任务
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
            /* do nothing. If start0 threw a Throwable then
              it will be passed up the call stack */
        }
    }
}
private native void start0(); 
//JNI技术 调用操作系统的接口方法 如果现场需要执行需要操作系统来进行资源分配， 此操作严格来说
线程应该由操作系统来执行 并分配资源 切换上下文
```
**`run 方法 `**
```java
//target runnable 接口
public void run() {
    if (target != null) {
        target.run();
    }
}
```
###### 静态属性
* `static class 	Thread.State`:`描述线程状态`
* `static interface Thread.UncaughtExceptionHandler`:`   当线程因未捕获的异常而突然终止时调用的处理程序接口。`
* `static int	MAX_PRIORITY `:` 线程可以拥有的最大优先级。`
* `static int	MIN_PRIORITY`:`线程可以拥有的最低优先级。`
* `static int	NORM_PRIORITY `:`分配给线程的默认优先级。`
###### 构造函数
`有很多的构造方法  推荐几种就可以了`
* `Thread() `:`分配一个新Thread对象。`
* `Thread(String name)` :`分配一个新Thread对象。并给一个线程名称`
* `Thread(Runnable target) `:`还有一个运行对象`
* `Thread(Runnable target, String name)` ：` 运行并且给一个线程名称`
* `Thread(ThreadGroup group, Runnable target, String name, long stackSize) `:`分配一个新Thread对象，使其具有 target作为其运行对
象，具有指定 name的名称，属于所引用的线程组group，并具有指定的堆栈大小。`

###### 方法摘要
* `static int	activeCount() `:`返回当前线程的线程组中活动线程的数量。`
* `static Thread	currentThread() `:`返回对当前正在执行的线程对象的引用。`
* `static int	enumerate(Thread[] tarray) `:`将当前线程的线程组及其子组中的每个活动线程复制到指定的数组中。`
* `static void	dumpStack() `:`打印当前线程的堆栈跟踪。`

###### 线程信息 和配置信息
* `void	checkAccess() `:`确定当前运行的线程是否具有修改此线程的权限。`
* `void	setName(String name) `:`将此线程的名称更改为等于参数 name。`
* `void	setPriority(int newPriority) `:`更改此线程的优先级。`
* `static boolean	interrupted() `:`测试当前线程是否已被中断。`
* `int	getPriority() `:`Returns this thread's priority.`
* `long	getId()`:`返回此Thread的标识符。`
* `String getName()`:`返回此线程的名称`
* `Thread.State	getState() `:`返回此线程的状态。`
* `boolean	isAlive() `:`测试此线程是否存活。`
* `boolean	isDaemon() `:`测试此线程是否为守护程序线程。`
* `void	setDaemon(boolean on) `:`将此线程标记为守护程序线程或用户线程。`
* `boolean	isInterrupted() `:`测试此线程是否已被中断。`
###### 线程操作
* `static void	yield() `:`使当前正在执行的线程对象暂时暂停并允许其他线程执行。`
* `void	run() `:`如果使用单独的Runnable运行对象构造此线程 ，则调用该 Runnable对象的run方法; 否则，此方法不执行任何操作并返回。`
* `void	start()`:`让此线程开始执行; Java虚拟机调用run此线程的方法。 使用它 才是多线程`
* `static void	sleep(long millis) `:`使当前正在执行的线程休眠（暂时停止执行）指定的毫秒数。`
* `static void	sleep(long millis, int nanos) `:`使当前正在执行的线程休眠（停止执行）指定的毫秒数加上指定的纳秒数。`
* `void	join() `:`等待这个线程死亡。`
* `void	join(long millis) `:`等待millis此线程死亡最多毫秒。`
* `void	join(long millis, int nanos) `:`等待此线程死亡时最多millis毫秒加上 nanos纳秒。`
* `void	interrupt() `:`中断此线程。`


####  [Thread 操作](#top) :star2:<b id="opr"></b> 
`所有的线程每一次运行 都可能参数不同的结果,因为每一次线程都会根据自己的情况进行资源抢占`
* `区分线程 需要依靠线程命名，并且建议在线程运行前 给线程命名`
    * `String getName()`:`返回此线程的名称`
    * `void	  setName(String name) `:`设置名称`
    

####  [Java 概念题目](#top) :star2:<b id="face"></b> 
* `每一个JVM进程启动的时候至少需要启动几个线程呢`
   * `回答：两个`
        * `1. main 线程 程序主要执行 以及启动子线程`
        * `2. gc线程 垃圾回收`
























