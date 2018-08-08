<a id="top" href="#top">:maple_leaf: C# 线程变成 :bird:</a> 
-----
`C# 多线程编程,线程有同步和异步,多线程,特别是C# 这个三个东西每一个都是一大块,慢慢学把;线程的概念 就某该了`

- [x] :mountain_bicyclist:	<a href="#DelegateThreading">`通过委托使用多线程`</a>

- [x] :mountain_bicyclist:	<a href="#UseThreading">`通过Threading使用多线程 `</a>

- [x] :mountain_bicyclist:	<a href="#ChiziThreading">`线程池`</a>

- [x] :mountain_bicyclist:	<a href="#TaskThreading">`基于任务的多线程`</a>

- [x] :mountain_bicyclist:	<a href="#ResourceThreading">`线程争用问题`</a>

#### &nbsp;&nbsp; 通过委托使用多线程 <a id="DelegateThreading"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

* `C#委托实现多线程 需要借助两个方法 BeginInvoke 和 EndInvoke`
* `IAsyncResult 参数 为线程状态EndInvoke 和回调函数的参数` 

   * `IAsyncResult BeginInvoke(callbackFunction(IAsyncResult ar,Object obj))`
   * `IAsyncResult BeginInvoke(param object[],callbackFunction(),Object obj)`
* `Object EndInvoke(IAsyncResult ar) 方法返回泛型方法执行的结果`

```C#
    static void Main(string[] args)
    {
        Console.WriteLine("Start World!");
        //给委托赋予一个方法
        Func<int,int> a = (val)=>
        {
            Console.WriteLine( $"input value:{val}");
            return val % 5;
        };
        // 开始执行方法
        // 倒数第二个参数 为回调函数 有一个参数 IAsyncResult 为线程状态信号作为参数填充到 IAsyncResult的
        // AsyncState 属性中
        IAsyncResult ar =a.BeginInvoke(93, null, null); 

        //主线程不断检测 委托线程的执行状态是否完成
        //
        while (ar.IsCompleted==false)
        {
            Thread.Sleep(10);
            Console.WriteLine("等待中...");
        }
        //执行完毕后 执行EndInvoke 传递信号量,得到返回值
        var reval = a.EndInvoke(ar);
        Console.WriteLine($"Value： {reval}");
        Console.WriteLine("End World!");
        Console.ReadKey();
    }
```
* `限制子线程的执行时间，超过时间则停止返回false`

```C#
      // 倒数第二个参数 为回调函数 有一个参数 IAsyncResult 为线程状态信号作为参数填充到 IAsyncResult的
      AsyncState 属性中
      IAsyncResult ar =a.BeginInvoke(93, null, null);
      Boolean isFinsih = ar.AsyncWaitHandle.WaitOne(1000);//超时时间,1000毫秒线程结束返回true否则返回false

      //如果已经完成了
      if (isFinsih)
      {   
          var vals = a.EndInvoke(ar);
          Console.WriteLine(vals);
      }
      Console.WriteLine("End World!");
      Console.ReadKey();
```

* `通过回调函数得到返回结果,和线程结束` `方法使用 lambda表达式`

```C#
      static void Main(string[] args)
      {

          //给委托赋予一个方法
          Func<int,int> a = (val)=>
          {
              Console.WriteLine( $"input value:{val}");
              return val % 5;
          };
          // 开始执行方法
          // 倒数第二个参数 为回调函数 有一个参数 IAsyncResult 为线程状态信号作为参数填充到 IAsyncResult的
          // AsyncState 属性中
          a.BeginInvoke(93,(IAsyncResult er)=> {
              string val = er.AsyncState as String;
              var reval = a.EndInvoke(er);
              Console.WriteLine($" {val} Said Return Value:{reval}");

          }, "JiangXing");
          Console.WriteLine("Start World!");
          Console.WriteLine("End World!");
          Console.ReadKey();
      }
```
#### &nbsp;&nbsp; 通过Threading使用多线程 <a id="UseThreading"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
* 简单的一个线程

```C#
      Thread load = new Thread(() =>
      {
          Console.WriteLine("下载文件中...");
          Thread.Sleep(2000);
          Console.WriteLine("下载完毕");
      });

      load.Start();

      Console.WriteLine("Mian Function");
      Console.ReadKey();
```
##### 线程信息
  * 获得当前线程ID： `Thread.CurrentThread.ManagedThreadId`
  * 启动线程 : `Start() 方法`
  * 如果方法有参数:`参数必须是Object 类型 ,然后参数传递给Start方法就好了`
    ```C#
      // load是一个 前台前程main函数执行完后,程序并未停止 在等待load线程执行完
      Thread load = new Thread((finaName) =>
      {
          Console.WriteLine(finaName+"下载文件中...");
          Thread.Sleep(3000);
          Console.WriteLine("线程ID:"+Thread.CurrentThread.ManagedThreadId);
          Console.WriteLine("下载完毕");
      });

      load.Start("XXX.种子");

      Console.WriteLine("Mian Function");
    ```
   * 可以给Thread传递lambda表达式,静态方法,和对象方法、
 
 ##### 前台线程/后台线程
 * `只要有一个前台线程在运行,应用程序的进程就在运行,此线程为前台线程,如果主线程停止了,那么所有后台线程全部停止.`
 * `默认情况下,Thread类创建的线程是前台线程。线程池中的线程总是后台线程,在用Thread类时,可以设置` **`isBackGround属性`** `把它设置为后台线程`
 
 ```C#
      Thread load = new Thread((finaName) =>
      {
          Console.WriteLine(finaName+"下载文件中...");
          Thread.Sleep(3000);
          Console.WriteLine("线程ID:"+Thread.CurrentThread.ManagedThreadId);
          Console.WriteLine("下载完毕");
      });
      load.IsBackground = true; //设置为后台前程
      load.Start("XXX.种子"); //不会输出了

      Console.WriteLine("Mian Function");
 ```
 ##### 优先级
 `CPU同一时间只能做一件事情,当很多线程需要执行的时候,线程调度器,会根据线程的优先级去判断先去执行那一个线程,如果优先级相同就按照顺序逐个执行`
 
 * **`Priority`** ：`设置优先级 是一个 ThreadPriority的 枚举值,Highest AboveNormal BelowNormal  Lowest`

##### 控制线程
* `Thread.CurrentThread.ThreadState`:`获取当前线程的状态  ThreadState是一个枚举值`
```C#
//
// 摘要:
//     启动线程，它不会被阻止，并且有没有挂起 System.Threading.ThreadAbortException。
Running = 0,
//
// 摘要:
//     正在请求线程停止。 这是仅供内部使用。
StopRequested = 1,
//
// 摘要:
//     正在请求线程挂起。
SuspendRequested = 2,
//
// 摘要:
//     该线程将作为后台线程，而不是一个前台线程正在执行。 通过设置控制此状态 System.Threading.Thread.IsBackground 属性。
Background = 4,
//
// 摘要:
//     System.Threading.Thread.Start 不对的线程上调用方法。
Unstarted = 8,
//
// 摘要:
//     该线程已停止。
Stopped = 16,
//
// 摘要:
//   线程将受阻。Thread.Sleep(System.Int32) 或Thread.Join,
//   请求锁的 .Threading.Monitor.Enter(System.Object) 或 .Monitor.Wait(System.Object,System.Int32,System.Boolean)
//   Threading.ManualResetEvent。
WaitSleepJoin = 32,
//
// 摘要:
//     该线程已挂起。
Suspended = 64,
//
// 摘要:
//     System.Threading.Thread.Abort(System.Object) 的线程上调用方法，但该线程尚未收到挂起 System.Threading.ThreadAbortException
//     ，将尝试将其终止。
AbortRequested = 128,
//
// 摘要:
//     线程状态包括 System.Threading.ThreadState.AbortRequested 和线程现在出现故障，但其状态不发生更改到 System.Threading.ThreadState.Stopped。
Aborted = 256

```

* `线程的状态`:`Running / Unstarted` ,`当通过Thread 调用Start方法后 线程还不会进入Running状态,而是Unstared状态,只有当操作系统的线程调度器选择了要运行的线程,这个线程的状态才会修改为Running状态.我们可以使用` **`Sleep`** `方法让线程 进入WaitSleepJoin状态`,`使用` **`Abort`** `方法停止线程`

* `如果需要等待线程结束的话,可以调用Thread对象的Join方法,表示把Thread加入进来,停止当前线程,并把它设置为WaitSleepJoin状态,知道加入线程完成为止`

```C#
        static void Main(string[] args)
        {
            Thread load = new Thread((finaName) =>
            {
                Console.WriteLine(finaName+"下载文件中...");
                Thread.Sleep(3000);
                Console.WriteLine("线程ID:"+Thread.CurrentThread.ManagedThreadId);
                Console.WriteLine("下载完毕");

                Console.WriteLine(Thread.CurrentThread.ThreadState);
            });
            load.Start("XXX.种子");

            load.Join(); //等待load线程执行完成
            Console.WriteLine("Mian Function");
        }        
        /* 输出
          XXX.种子下载文件中...
          线程ID:3
          Running
          Main Function
        */
```
#### &nbsp;&nbsp; 线程池 <a id="ChiziThreading"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`创建线程需要时间,如果有不同的小任务需要完成,就可以事先创建许多线程,在完成这些任务的时候发送请求线程执行,这个线程的数量最好在需要更多线程时增加
,在需要释放资源的时候减少,不需要自己创建创建线程池子,系统以及有一个` **`ThreadPool 静态类`** `类管理线程池。会自动增加减少线程数量,池中最大线程数量可以配置, 工作线程数量和IO线程数量,也可以配置最小线程数量`

```C#
      static void Main(string[] args)
      {
          //QueueUserWorkItem 发起工作线程  传入的方法必须带一个参数
          ThreadPool.QueueUserWorkItem(CarryOut);
          ThreadPool.QueueUserWorkItem(CarryOut);
          ThreadPool.QueueUserWorkItem(CarryOut);
          ThreadPool.QueueUserWorkItem(CarryOut);
          ThreadPool.QueueUserWorkItem(CarryOut);
          ThreadPool.QueueUserWorkItem(CarryOut);
          Console.ReadKey();
      }
      public static void CarryOut(object states){
          Console.WriteLine("线程执行ID：" + Thread.CurrentThread.ManagedThreadId);
          Thread.Sleep(2000);
          Console.WriteLine("线程结束了：" + Thread.CurrentThread.ManagedThreadId);
      }
```
* `线程池都是后台线程`
* `不能设置优先级和名称`
* `只能处理时间短的任务,长时间任务请自动创建线程`

#### &nbsp;&nbsp; 基于任务的多线程 <a id="TaskThreading"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>

```C#
      static void Main(string[] args)
      {
          Task t = new Task(CarryOut);
          t.Start();
          Thread.Sleep(3000);
          Console.WriteLine("Main Function Over");

      }
      public static void CarryOut(){

          Console.WriteLine("线程执行ID：" + Thread.CurrentThread.ManagedThreadId);
          Thread.Sleep(2000);
          Console.WriteLine("线程结束了：" + Thread.CurrentThread.ManagedThreadId);
      }
```
* TaskFactory

```C#
      TaskFactory ta = new TaskFactory();
      ta.StartNew(CarryOut);
```

##### 线程依赖/连续任务
`如果任务T1的执行依赖于任务2的,那么久需要任务T2执行完毕后再开始执行T1这个时候可以使用连续任务`

```C#
        static void Main(string[] args)
        {
            Task t = new Task(DoFirst);            
            Task t2 = t.ContinueWith(DoSecond);
            Console.WriteLine("Task " + t.Id + " Start");
            t.Start();

            Console.WriteLine("Main Function Over");
            Console.ReadKey();
        }
        public static void DoFirst()
        {

            Console.WriteLine("线程执行ID：" + Thread.CurrentThread.ManagedThreadId);
            Thread.Sleep(2000);
            Console.WriteLine("线程结束了：" + Thread.CurrentThread.ManagedThreadId);
        }

        public static void DoSecond(Task brefore)
        {
            Console.WriteLine(" Task："+brefore.Id+" Finish");
            Console.WriteLine(" Task：" +Thread.CurrentThread.ManagedThreadId + " Start");
        }
```
* `线程没有父子之分,但是任务有父子关系,父任务要等子任务执行完成后父任务才算完成`

#### &nbsp;&nbsp; 线程争用问题 <a id="ResourceThreading"></a>  :closed_umbrella: <a href="#top"> `置顶` :arrow_up:</a>
`lock(Object){} 锁定一个对象,不能锁定值类型`
* 一个线程争用问题
```C#
    class Person
    {
        public int i = 5;

        public void ChangeStats()
        {
            i++;
            if (i == 5)
            {
                Console.WriteLine($"i==5 { Thread.CurrentThread.ManagedThreadId }");
            }
            i = 5;
        }
    }
    
    
    static void Main(string[] args)
    {
        Person p = new Person();
        Thread t = new Thread(ChangeStats);
        t.Start(p);
        Thread t1 = new Thread(ChangeStats);
        t1.Start(p);
        Console.ReadKey();
    }

    public static void ChangeStats(object o)
    {
        Person p = o as Person;
        while (true)
        {
            if (p.i == 5)
            {
                p.ChangeStats();
            }
        }
    }
```
* lock解决争用问题
```C# 
      public static void ChangeStats(object o)
      {
          Person p = o as Person;
          while (true)
          {
              lock (o)
              {
                  if (p.i == 5)
                  {
                      p.ChangeStats();
                  }
              }

          }
      }
```
