:sailboat: <a id="top">托管和非托管资源</a> 
----

- [x] <a href="#StrongAndWeakReference">强引用和弱引用</a>

- [x] <a href="#ResourceDealWithSelf">处理非托管的资源</a>

- [x] <a href="#UnsafeCode">不安全的代码</a>

> `理解C# 内部原理的必知知识要点,C#是一门不需要担心具体的内存管理,垃圾回收器会自动处理所有的内存清除工作`
* `1.了解gc[垃圾回收机制]如何工作`
* `2.知道什么是大小对象堆,以及什么数据类型存储在堆栈上市非常有益的`
* `3.垃圾收集器处理托管的资源,那么非托管的资源呢?他需要由开发人员自动释放,例如文件,数据库连接,网络请求....`
* `4.为了尽早释放非托管资源避免资源浪费,最好了解IDisposaable皆苦和using语句`
* `5.理解内存管理,析构的意义`

----
#### 后台内存管理
`程序运行在内存上,Windows采用虚拟寻址系统,由数据类型的不同决定数据分配的内存位置不同`

* 对于值类型 例如 `int,long` 分配在内存的栈中,栈实际上是向下填充,从高地址向低内存地址填充

* 对于引用类型 例如: 对象 分配在托管堆中由垃圾回收器管理,使用new运算符为它分配内存

```C#

   Person one =new Person("JxKicker");
   
   //one 实际上是一个引用,类似于一个指针,他是分配在栈上面,然后指向堆中的对象所在位置
  
```

#### [垃圾回收](https://docs.microsoft.com/zh-cn/dotnet/standard/garbage-collection/index) <a href="#top">置顶 :arrow_up:</a>  
`托管堆的工作方式非常类似于栈,对象会在内存中一个挨一个地放置,这个就很容易使用指向下一个空闲存储单元的堆指针来确定下一个对象的位置.`

* 在垃圾回收器运行时,它会从堆中删除不再引用的所有对象,垃圾回收器,在引用的根表中找到所有的对象,接着在引用的对象树种查找。在完成删除操作后，堆立即把对象分散开  

* 回收内存后进行`压缩操作`,将分散的对象内存空间变成连续的

* 一般垃圾回收器在.net运行时确定需要进行垃圾回收时运行,可以调用System.GC.Collect() 方法强制在某个地方运行,但是不建议这样使用

* 新创建的对象会放到托管堆的一个地方,这个地方被称为第0代,当垃圾回收过程启动一次后,清理之后的对象会被压缩,压缩后第0代移动到下一部分,被称为第1代,此时第
0代为空 以此再次启动垃圾回收过程,移动到第二代,第三代....

* 在.NET中,垃圾回收提高性能的一个方式,大对象放到大对象堆上面,一般大约85000个字为大对象，就会放到这个特殊的堆上面,因为压缩大对象堆的代价是昂贵的

* 垃圾回收的平衡，确定什么时候进行垃圾回收机制,例如一个线程使用过多的内存,导致其他线程也垃圾回收那么可能会浪费性能。垃圾回收会平衡线程,大小堆确定
回收的时机
-----
`为了利用包含大量内存的硬件,垃圾回收过程添加了` [`GCSettings`](https://msdn.microsoft.com/zh-cn/library/system.runtime.gcsettings(v=vs.110).aspx)类  **`GCSetting.LatencyMode`** 属性 `LatencyMode` `是一个枚举值,去确定垃圾回收的运行模式`

|GCLatencyMode 枚举成员|说明|
|:----:|:----|
|Batch |禁止并发设置,把垃圾回收设置为最大吞吐量,这回重写配置设置|
|Interactive|工作站的默认行为,它使用垃圾回收并发设置,平衡吞吐量和响应|
|LowLatency|保守的垃圾回收,只有系统存在内存压力时,才进行完整的回收.只是应用于特定较短时间,执行特顶的操作|
|SustainedLowLatency|[持续低延迟] 只有系统内存存在压力时,才进行完整的内存回收|
|NoGCRegion|C# 4.6.0 新增成员,只读属性，可以在代码中调用[GC.TryStartNoGCRegion()](https://msdn.microsoft.com/zh-cn/magazine/dn906204) 停止垃圾回收,使用EndNoGCRegion开启垃圾回收|

```C#
   System.GC.Collect();  //进行垃圾回收
   System.Runtime.GCSettings.LatencyMode=GCLatencyMode.Batch;   //设置垃圾回收运行模式

   System.GC.TryStartNoGCRegion(1000); //停止垃圾回收器 设置可用的,GC试图访问的内存大小 字节为单位 太少会报错

   for(int i=0;i<1000;i++){
       Son li=new Son("sds"+i);
   }
   System.GC.EndNoGCRegion(); //开启垃圾回收器    

```
#### <a id="StrongAndWeakReference">强引用和弱引用</a> <a href="#top">置顶 :arrow_up:</a>  
`垃圾回收器不能会后任在引用的对象的内存----这是一个强引用`,`在应用程序代码内实例化一个类或结构,只有有代码引用它,就会形成强引用`

```C#
  var myCache =new MyCache();
  myCache.add(myClassVariable);
```
* `这个情况下,如果myCache 没有释放那么 myClassVariable就一直存在,哪怕是myClassVariable 已经被用完了,没有作用了.`
* `所以在用完了myClassVariable 后应该清空 设置 myClassVariable=null;`
* `有时候忘了设置为null垃圾回收器就无法回收内存了,这个时候使用WeakReference 可以避免这种情况`
* `使用事件很容易错过引用的清理，这个时候应该使用弱引用`
------
##### 弱引用
`弱引用因为允许创建和使用对象,但是垃圾回收器碰巧在运行,就会回收对象释放内存,由于潜在的BUG和性能问题,一般不会这样做,但是在特定的情况下
使用弱引用是很合理的.弱引用对小对象也没有意义,因为弱引用有自己的开销,这个开销比小对象更大。`

```C#
  static void Main(string[] args)
  {
      var PersonWeakReference=new WeakReference(new Person("JiangYiXue",54,"123456",true));
      if(PersonWeakReference.IsAlive){
          WriteLine("Person 的弱引用还在");
          Person strongPerson=PersonWeakReference.Target as Person;
          if(strongPerson!=null){
              WriteLine(strongPerson.ToString());
              strongPerson=null;
          }

      }    
  }
```
#### <a id="ResourceDealWithSelf">处理非托管的资源</a> <a href="#top">置顶 :arrow_up:</a>  
`垃圾回收器不管非托管资源的释放问题,所以这需要我们自己来处理,这里有连个机制来是否非托管的资源`

* 声明一个析构函数(或终结器 C#析构函数 在.NET底层就是终结器),作为类的一个成员
* 在类中实现`System.IDisposible` 接口

----
`在垃圾回收前也可以调用析构函数,析构函数并非释放非托管资源,执行一般清理操作的地方,因为啊,有垃圾回收器了,析构函数即使调用,也不会按照调用顺序执行没有析构函数的对象一次就可以删除,但是有析构函数要运行一次垃圾回收 然后再运行一次析构函数两次处理才能销毁。`

##### C#推荐使用`System.IDisposible` 接口代替析构函数。IDisposible接口定义了一种模具,提供用于释放非托管的资源的机制
`这避免产生析构函数固有的与垃圾回收器相关的问题`
* IDisposible 接口声明了一个Dispose() 方法不带参数,返回void 
----
##### try-cathc-finally 释放资源
`使用try-cathc-finally可以防止因为各种情况而不能释放资源的各种问题`
```C#
   var theResource=null;
   try{
      theResource = new Resource();
   }catch(Exception ex){
        //etc
   }
   finally{
      theResource.Dispose();
   }
```
##### using 自动释放资源
```C#

  using(var theResource= new Resource()){
       //etc      
  }
  
```
* using 关键字在C#中有多个用法,using声明用于导入名称空间。using语句处理实现IDisposible接口的对象,并在作用域的末尾调用Dispose方法
* C#中有许多类有Close和Dispose方法.如果常常要关闭资源,就实现这个两个方法.此时Close方法只是调用了Dispose方法。
##### 实现的 IDisposible接口不一定要实现终结器,终结器会带来额外的开销,它的执行顺序无法被保证
##### 实现了终结器,就应该实现IDisposible 接口,这样本机资源可以早些是否,而不仅是在GC找出被占用的资源时才释放资源.

#### <a id="UnsafeCode">不安全的代码</a> <a href="#top">置顶 :arrow_up:</a>  
`C#支持指针,可以用指针访问内存这个用法和C语言一样,但是原则上这是不安全代码 需要加以管理,使用unsafe关键字标记不安全代码`:x:

-----
```C#
    //修饰方法
    unsafe int GetSomeNumber(){
      int *pWidth,pHeight;
      double *pResult;
      //etc
      
      double in=&pResult; 
    }
    //修饰类
    unsafe class Deal{
     
    }
    //修饰代码块
    unsafe{
    
    }
     
```
* 编译器拒绝不安全代码,如果有不安全代码需要配置project.json文件 或者其他项目配置文件,将文件中的allowUnsafe设置为true.:large_blue_circle:
* sizeof() 运算符 确定大小指针返回预先定义的类型大小 比如自定义的类就无法使用
#### 我们不用指针,要用去C语言去用....所以关于指针的一切 不管
