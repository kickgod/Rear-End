
<a id="top" href="#top"> 依赖注入容器 :maple_leaf:</a> 
----
`我们将会讨论三类工具,依赖注入(DI)容器,单元测试框架,和模仿工具,Ninject 是一个简单,优雅易用的DI容器,支持最小配置,还有微软提供的Unity DI工具,我们采用NUnit这是流行的.NET单元测试框架,模仿工具包Moq可以用它创建单元测试中所使用的接口实现也可以使用Rhino Mocks`

- [x] :maple_leaf: <a href="#ClassJieOu">`解耦类`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="ClassJieOu" href="#ClassJieOu">解耦类</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`让我们来试着了解如何解耦类吧`
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
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="  " href="#  ">   </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
