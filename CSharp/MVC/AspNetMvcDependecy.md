<a id="top" href="#top"> 依赖注入容器 :maple_leaf:</a> 
----
`我们将会讨论三类工具,依赖注入(DI)容器,单元测试框架,和模仿工具,Ninject 是一个简单,优雅易用的DI容器,支持最小配置,还有微软提供的Unity DI工具,我们采用NUnit这是流行的.NET单元测试框架,模仿工具包Moq可以用它创建单元测试中所使用的接口实现也可以使用Rhino Mocks`

- [x] :maple_leaf: <a href="#ClassJieOu">`解耦类`</a>
- [x] :maple_leaf: <a href="#Ninject">`Ninject 容器`</a>
- [x] :maple_leaf: <a href="#MVCNinject">`Ninject 容器 MVC 注入`</a>
- [x] :maple_leaf: <a href="#NinjectPropertyConstructor">`Ninject 指定属性和构造器参数`</a>
- [x] :maple_leaf: <a href="#NinjectWhenFunction">`Ninject 条件绑定方法`</a>
- [x] :maple_leaf: <a href="#NinjectScopeFunction">`Ninject 作用域方法`</a>

####  <a id="ClassJieOu" href="#ClassJieOu">解耦类</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`让我们来试着了解如何解耦类吧,以下代码在VSCDOE 中运行`
##### `下面我们看看以前的结构模式`
-------
`简单的设计模式,导致了许多的问题,类与类之间的耦合度过高,紧密连接`
##### 图书类
```C#
  public class Book
  {
      public string ID{get;set;}
      public string Name{get;set;}
      public int PublishYear{get;set;}
      public double Price{get;set;}
      public override string ToString(){
          return $"ID:{ID} Name:{Name} Publish Year:{PublishYear} Price:{Price}";
      }
      public Book(String ID,String Name,int year,double Price){
          this.ID=ID;
          this.Name=Name;
          this.PublishYear= year;
          this.Price=Price;
      }
      public Book(){}
  }
```
##### 计算总的价格类
```C#
  public class CalculatorSum
  {
      //一个计算总价格的方法,因为有打折优惠之类的 使用一个类来管理
      public double SumPrice(IEnumerable<Book> books){
          return books.Sum(B=>B.Price);
      }
  }
```
##### 书店类
```C#
  public class BookStore{
      public IEnumerable<Book> Books{get;set;}
      public double GetPriceSum(){
          CalculatorSum cal=new CalculatorSum();
          return cal.SumPrice(Books);
      }
  }
```
---------
`我们引入抽象层,使用接口给类之间解耦`
##### `引入抽象层`
```C#
    public interface ICalculatorSum
    {
        //一个计算总价格的方法,因为有打折优惠之类的 使用一个类来管理
        double SumPrice(IEnumerable<Book> books);
    }
    // 实现这个接口
    public class CalculatorSum:ICalculatorSum
    {
        //一个计算总价格的方法,因为有打折优惠之类的 使用一个类来管理
        public double SumPrice(IEnumerable<Book> books){
            return books.Sum(B=>B.Price);
        }
    }
```
##### 改变图书馆类
```C#
  public class BookStore{
      private ICalculatorSum cal;
      public IEnumerable<Book> Books{get;set;}
      public double GetPriceSum(){
          cal=new CalculatorSum();
          return cal.SumPrice(Books);
      }
  }
  //主函数
  class Program
  {   
      static void Main(string[] args)
      {   
         Book[] bks=  JsonMapper.ToObject<Book[]>(File.ReadAllText("resource/Books.json"));
         foreach(var val in  bks){
             Console.WriteLine(val);
         }
         CalculatorSum cal=new CalculatorSum();
         BookStore store=new BookStore(cal);
         store.Books=bks;
      }
  }
```
##### `使用抽象接口抽象类`
```C#
  public class BookStore{
      private ICalculatorSum cal;
      public IEnumerable<Book> Books{get;set;}
      public BookStore(ICalculatorSum cal_){
          this.cal=cal_;
      }
      public double GetPriceSum(){
          return cal.SumPrice(Books);
      }
  }
```
##### 如果此时操作打折呢
```C#
  public class CalculatorSumDiscount:ICalculatorSum
  {
      public CalculatorSumDiscount(int i){
          this.Discount=i;
      }
      public CalculatorSumDiscount(){
          this.Discount=10;
      }
      public int Discount;
      //一个计算总价格的方法,因为有打折优惠之类的 使用一个类来管理
      public double SumPrice(IEnumerable<Book> books){
          return books.Sum(B=>B.Price)*(Discount/10.0);
      }
  }
```
##### 主函数
```C#
  class Program
  {   
      static void Main(string[] args)
      {   
         Book[] bks=  JsonMapper.ToObject<Book[]>(File.ReadAllText("resource/Books.json"));
         foreach(var val in  bks){
             Console.WriteLine(val);
         }
         ICalculatorSum cal=new CalculatorSumDiscount(8);
         BookStore store=new BookStore(cal);
         store.Books=bks;
      }
  }
```
`但是你这样做还是太麻烦了,因为对于一个小型项目来说,你可以每次去更改ICalculatorSum 接口的实现者,很容易但是一旦项目过于巨大,每一才对接口的实现更改
都是巨大的工作,如何 ICalculatorSum cal=new CalculatorSumDiscount(8); 使得我们不做这个而让 BookStore里面的属性 ICalculatorSum自动的实现呢,谓词
人们引入了容器DI 来完成依赖注入`
####  <a id="Ninject" href="#Ninject">Ninject 容器 </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`Ninject是一个非常轻量级切简单配置的DI容器`
##### <a href="#InsertNinject">Ninject 安装</a>
* `通过工具中的NuGet管理器的管理解决方案的包添加下载 搜索Ninject`
* `通过NuGet命令行添加`
   * `Install-Package Ninject -version 3.3.4  [2018 年九月 为止的最新版]`
      * `Ninject: 内核包 ,下面的都是Ninject的扩展包 `
   * `Install-Package Ninject.MVC5 -version 3.3.0  [2018 年九月 为止的最新版]`
      * `Ninject.MVC5 将IOC运用到MVC5中`
   * `Install-Package Ninject.Web.Common -version 3.3.1  [2018 年九月 为止的最新版]`
##### 基本使用
`将实现接口的类绑定到接口上面,将代码转换到MVC程序中`
```C#
  //HomeController 中Index 方法
  public  String Index()
  {
      Book[] bks = JsonMapper.ToObject<Book[]>(System.IO.File.R
      eadAllText(Server.MapPath("~/Resource/Books.json")));
      foreach (var val in bks)
      {
          Console.WriteLine(val);
      }
      IKernel ninjectKernal = new StandardKernel();
      ninjectKernal.Bind<ICalculatorSum>().To<CalculatorSumDiscount>();
      ICalculatorSum cal = ninjectKernal.Get<ICalculatorSum>();
      BookStore store = new BookStore(cal);
      store.Books = bks;
      return $"Total Price: {store.GetPriceSum().ToString()}";
  }
```
##### 说明
* `创建一个Ninject内核实例,为此创建一个Ninject的内核实例,该实例是一个对象(内核对象),它负责解析依赖性并创建新的对象(为依赖性创建的对象),当需要一个
对象时,将使用这个内核而不是使用new关键字`
```C#
  IKernel ninjectKernal = new StandardKernel();
  // IKernel: 接口 通过StandardKernel 来标准化和具体实现 
```
* `第二阶段 配置Ninject内核,使其理解用到的每一个接口所希望使用的实现对象,以下是清单中完成这个项工作的语句`
```C#
  ninjectKernal.Bind<ICalculatorSum>().To<CalculatorSumDiscount>();
```
* `接口获得实现类`
`通过 Get泛型方法,并且指明接口类型即可,既可以返回实现类`
```C#
 ICalculatorSum cal = ninjectKernal.Get<ICalculatorSum>(); 
```

####  <a id="MVCNinject" href="#MVCNinject">Ninject 容器 MVC 注入</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`以上代码其实并未进一步完全解耦Home控制器 和CalculatorSumDiscount仍然是耦合的,因为他们并非是自动解析注入的,还是要进一步写在类的方法里面,
为了进一步解耦,我们需要创建解析器`
```C#
    //方法哦 App_Start文件夹中
    public class NinjectDependencyResolver : IDependencyResolver
    {
        private IKernel kernel;
        public NinjectDependencyResolver(IKernel kernelParam)
        {
            kernel = kernelParam;
            AddBindings();
        }
        public object GetService(Type serviceType)
        {
            return kernel.TryGet(serviceType);
        }
        public IEnumerable<object> GetServices(Type serviceType)
        {
            return kernel.GetAll(serviceType);
        }
        private void AddBindings()
        {
            kernel.Bind<ICalculatorSum>().To<CalculatorSumDiscount>().WithPropertyValue("Discount",5);
        }
    }
```
* `NinjectDependecyResolver类实现了IDependencyResolver 接口,它属于System.Mvc命名空间,也由MVC框架用于获取所需的对象
。MVC框架在需要类实例以便对一个传入的请求进行服务时,会调用GetService或GetServices方法,依赖解析器要做的工作便是创建这一实例`
* `这是一项要通过调用Ninject的TryGet和GetAll方法来完成的任务`,
   * `TryGet方法的工作方式类似与前面的Get方法,但当没有合适的绑定的时候就返回Null,而不是抛出一个异常`
   * `GetAll 支持对于单一类型的多个绑定，当有多个不同的服务提供器可用时,可以使用它`
* `以上依赖项解析类也是建立在Ninject绑定的地方,在AddBinding方法中,使用Bind To方法配置接口和实现类之间的关系`
##### 注册依赖性解析器
`仅仅简单的创建一个IDependencyResolver接口的实现是不够的,还必须告诉MVC矿建需要使用它,接下来我们要将其注册到MVC中,让框架自动调用实现控制器中的
接口字段[请注意是字段 不要写个属性在哪里] 通过控制器的构造方法[不要忘了还有控制器方法]` [`注册官方文件模板样例`](https://github.com/ninject/Ninject.Web.Mvc/blob/master/src/SampleApplication/Global.asax.cs)
* `我们需要修改 Global.asax.cs 文件夹里面的类,重写它来注册服务,重写模板就是就是样例`
* `替换类文件 修改方法 OnApplicationStarted()`
* `加上这句话 DependencyResolver.SetResolver(new NinjectDependencyResolver(CreateKernel())); 注意命名空间的应用`
```C#
    using LabExam.App_Start;
    using Ninject;
    using Ninject.Web.Common.WebHost;
    using System.Reflection;
    using System.Web.Mvc;
    using System.Web.Routing;
    //将整个Global.asax.cs 里面的类换成如下 
    public class MvcApplication : NinjectHttpApplication
    {
        /// <summary>
        /// Registers the global filters.
        /// </summary>
        /// <param name="filters">The filters.</param>
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
        }

        /// <summary>
        /// Registers the routes.
        /// </summary>
        /// <param name="routes">The routes.</param>
        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            routes.MapRoute(
                "Default", // Route name
                "{controller}/{action}/{id}", // URL with parameters
                new { controller = "Home", action = "Index", id = UrlParameter.Optional });
        }

        /// <summary>
        /// Creates the kernel that will manage your application.
        /// </summary>
        /// <returns>The created kernel.</returns>
        protected override IKernel CreateKernel()
        {
            var kernel = new StandardKernel();
            kernel.Load(Assembly.GetExecutingAssembly());
            return kernel;
        }

        /// <summary>
        /// Called when the application is started.
        /// </summary>
        protected override void OnApplicationStarted()
        {
            base.OnApplicationStarted();
            
            DependencyResolver.SetResolver(new NinjectDependencyResolver(CreateKernel()));
            
            AreaRegistration.RegisterAllAreas();
            RegisterGlobalFilters(GlobalFilters.Filters);
            RegisterRoutes(RouteTable.Routes);
        }
    }
```
##### 重写控制器Home
```C#
  public class HomeController : Controller
  {
      [Inject]  //表示这个字段是要通过注入实现的
      private ICalculatorSum cal;
      public HomeController(ICalculatorSum calParam)
      {
          this.cal = calParam;
      }
      public String Index()
      {
          Book[] bks = JsonMapper.ToObject<Book[]>(System.IO.File.Read
          AllText(Server.MapPath("~/Resource/Books.json")));
          foreach (var val in bks)
          {
              Console.WriteLine(val);
          }
          if (cal == null)
          {
              throw new Exception($"注入失败 系统错误");
          }
          return $"Total Price: {cal.SumPrice(bks).ToString()}";
      }
  }
```
`启动项目完成注入`
####  <a id="NinjectPropertyConstructor" href="#NinjectPropertyConstructor">Ninject 指定属性和构造器参数</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`当我们的接口实现类中有一些参数的时候我们需要设置,不然的话就只能调用默认的无参数构造函数`
##### 设置属性
```C#
   kernel.Bind<ICalculatorSum>().To<CalculatorSumDiscount>().WithPropertyValue("Discount",5);
```
##### 调用构造函数
```C#
   //实现类
   public class CalculatorSumDiscount:ICalculatorSum
    {
        public CalculatorSumDiscount(int discount)
        {
            this.Discount= discount;
        }
        public CalculatorSumDiscount(){
            this.Discount=10;
        }
        public int Discount { get; set; }
        //一个计算总价格的方法,因为有打折优惠之类的 使用一个类来管理
        public double SumPrice(IEnumerable<Book> books){
            return books.Sum(B=>B.Price)*(Discount/10.0);
        }
    }
     //绑定 参数名字要相对 
     kernel.Bind<ICalculatorSum>().To<CalculatorSumDiscount>().WithConstructorArgument("discount", 5);
     //多个参数就在后面再加 WithConstructorArgument 方法
```
####  <a id="NinjectWhenFunction" href="#NinjectWhenFunction">Ninject 条件绑定方法  </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `When(谓词)`:`当谓词(一个lambda表达式)的结果为true的时候,实施绑定`
* `WhenClassHas<T>`:`当被注入的类以注解属性进行注释,而类型为T时,实施绑定`
* `WhenInjectedInto<T>`:`当要被注入的类是类型T时,实施注入`
####  <a id="NinjectScopeFunction" href="#NinjectScopeFunction">Ninject 作用域方法   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `InTransientScope`:`与未指定作用域效果相同,为每一个被解析的依赖性创建一个新的对象(每依赖性一实例)`
* ...
