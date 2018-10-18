<a id="top" href="#top">EF 框架实践之路  :maple_leaf:</a> 
----
`EF框架采坑的道路,为了避免以后别人继续采坑,我记录下这些坑`

- [x] :maple_leaf: <a href="#CreateTable">`建表问题`</a>
  - <a href="#Basiyuanze">`基本原则`</a>
  - <a href="#ZhujieNotNeedUse">`不管用的注解`</a>
  - <a href="#Enumyingshe">`关于枚举`</a>
- [x] :maple_leaf: <a href="#DataSave">`数据修改`</a>


#####  <a id="Basiyuanze" href="#Basiyuanze">基本原则</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `使用注解创建表和关系,其他的通过 表名+Map 类继承 EntityTypeConfiguration<表名> 配置映射关系的方式创建表 千万不要 因为这个各种错误都会
产生`
* `不要在上下文类中的OnModelCreating 设置表关系和表结构 实体多的时候 代码一点也不清晰`
#####  <a id="ZhujieNotNeedUse" href="#ZhujieNotNeedUse">不管用的注解</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `一般对于属性的映射类型使用默认的就好,如果一定需要修改它的类型那么可以使用如下的方式,但是一定不要使用注解 [Column(TypeName="datetime2")]
这个注解完全不管用,还报错 ,sql server中找不到对这个datetime2 的数据支持  varchar 也没有`

  ```C#
      protected override void OnModelCreating(DbModelBuilder modelBuilder)
      {
        modelBuilder.Entity<Profession>().Property(t => t.AddTime).HasColumnType("datetime2");
        base.OnModelCreating(modelBuilder);
      }
  ```
#####  <a id="Enumyingshe" href="#Enumyingshe">关于枚举</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`在EF框架中枚举会被映射为整形,所以放心大胆的用吧`
```C#
    [Table("BUser")]
    public class BUser
    {
        [Key]
        public int BUserID { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }

        public BlogType type { get; set; }

        [DefaultValue(true)]
        public bool Sex { get; set; }
        public String Password { get; set; }
    }
    public enum BlogType
    {
        Link=10,
        Val=20,
        Ttkciker=30,
        WangZhe=40
    }
    
    数据库信息
    BUser{
      BUserID PK int not null
      Name nvarchar(max) not null
      Age int not null
      type int not null
      Sex bit not null
      Password nvarchar(max) null
    }
```
#####  <a id="DataSave" href="#DataSave">修改数据</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`通过EF 框架修改数据是一件并不容易的事情，它有许多的需要注意的地方，仅仅将Model的状态设置为Modified 是不够的！它有需要要注意的地方`
```C#
  //假设是传入的对象
  public Customer GetCustomer(){
    Customer cus = new Customer();
    cus.ID = 3;
    cus.CreateTime = DateTime.Now;
    cus.Name = "JxKicker";
    cus.Age = 18;
    return cus;
  }
  Customer cus = GetCustomer();
  using(EFDbContent ctx = new EFDbContent())
  {
      //查询是否存在这样的对象 可以被修改
      var dataCustomer = ctx.Customers.FirstOrDefault(d=>d.id == cus.ID);
      if(dataCustomer != null){
         ctx.Customer.Attach(cus);
         //如果附加的对象存在于数据库中但是未被上下文所追踪,那么应用Attach方法使其被Customer所追踪。
         ctx.Entry(cus).State = EntityState.Modified;
         if(ctx.SaveChanges() > 0){
             Console.WriteLine("更新成功！");
         }else{
             Console.WriteLine("更新失败！");
         }
      }
  }
```
* `上述的问题： 上面存在两个Customer 对象 dataCustomer/cus 并且如果数据库中确实存在主键ID 为3的对象的话，那么此时存在两个主键相同的Customer
对象,如果此时你将cus对象的状态设置为Modified  让上下文去追踪,但是该主键已经存在内存中了,所以会抛出异常,不能追踪相同键的多个对象,这个时候`
**`解决方法:关闭变更追踪,使得上下文中只存在主键为1的Customer一个对象`**
```C#
  //假设是传入的对象
  public Customer GetCustomer(){
    Customer cus = new Customer();
    cus.ID = 3;
    cus.CreateTime = DateTime.Now;
    cus.Name = "JxKicker";
    cus.Age = 18;
    return cus;
  }
  Customer cus = GetCustomer();
  using(EFDbContent ctx = new EFDbContent())
  {
      //查询是否存在这样的对象 可以被修改
      var dataCustomer = ctx.Customers
          .AsNoTracking() //关闭变更追踪
          .FirstOrDefault(d=>d.id == cus.ID);
      if(dataCustomer != null){0
         //如果附加的对象存在于数据库中但是未被上下文所追踪,那么应用Attach方法使其被Customer所追踪。
         ctx.Entry(cus).State = EntityState.Modified;
         if(ctx.SaveChanges() > 0){
             Console.WriteLine("更新成功！");
         }else{
             Console.WriteLine("更新失败！");
         }
      }
  }
```
**`下一个问题  你会发现如果Customer 还有一个属性为 ModifiedDate 的DateTime 属性,但是你并未在cus对象中设置其值`** <br/>
`此时你会发现数据库中你的ID 为3的哪一行 ModifiedDate的值变为了0001-01-01 原因就是因为ModifiedDate 属性未赋值` `所以上述操作适合更新实体的全部属性`
##### 更新指定属性
`如果要更新制定属性,那么实体状态修改为Modified 就行不通了`
* `修改 查询到的实体的方法`
  * `这个时候就可以 将cus 修改的属性的值复制给查询出的实体对象,然后让它被上下文追踪,所以需要去除AsNoTracking 方法`
```C#
  //假设是传入的对象
  public Customer GetCustomer(){
    Customer cus = new Customer();
    cus.ID = 3;
    cus.CreateTime = DateTime.Now;
    cus.Name = "JxKicker";
    cus.Age = 18;
    return cus;
  }
  Customer cus = GetCustomer();
  using(EFDbContent ctx = new EFDbContent())
  {
      //查询是否存在这样的对象 可以被修改
      var dataCustomer = ctx.Customers.FirstOrDefault(d=>d.id == cus.ID);
      // dataCustomer 自动被上下文追踪
      if(dataCustomer != null){
         dataCustomer.CreateTime = cus.CreateTime;
         dataCustomer.name = cus.Name;
         dataCustomer.Age =cus.Age;
         if(ctx.SaveChanges() > 0){
             Console.WriteLine("更新成功！");
         }else{
             Console.WriteLine("更新失败！");
         }
      }
  }
```
* `修改实体的方法`
  * `对传入的实体,偷懒的办法,但是要确保传归来需要编辑的数据在数据中一定没有被篡改,可以直接将cus对象附加到上下文,然后手动指定要更新的属性`
```C#
  //假设是传入的对象
  public Customer GetCustomer(){
    Customer cus = new Customer();
    cus.ID = 3;
    cus.CreateTime = DateTime.Now;
    cus.Name = "JxKicker";
    cus.Age = 18;
    return cus;
  }
  
  using(EFDbContent ctx = new EFDbContent())
  {
     Customer cus = GetCustomer();
     ctx.Customer.Attach(cus);
     ctx.Entry(cus).Property(p=>p.CreateTime).IsModified = true;
     ctx.Entry(cus).Property(p=>p.Name).IsModified = true;
     ctx.Entry(cus).Property(p=>p.Age).IsModified = true;
     if(ctx.SaveChanges() > 0){
         Console.WriteLine("更新成功！");
     }else{
         Console.WriteLine("更新失败！");
     }
  }
```
##### 解决并发赋值的方式
`Entry(obj).CurrentValues(..) 解决并发赋值问题`
```C#
  Customer cus = GetCustomer();
  using(EFDbContent ctx = new EFDbContent())
  {
      //查询是否存在这样的对象 可以被修改
      var dataCustomer = ctx.Customers.FirstOrDefault(d=>d.id == cus.ID);
      // dataCustomer 自动被上下文追踪
      if(dataCustomer != null){
         ctx.Entry(dataCustomer).CurrentValues.SetValues(cus);
         if(ctx.SaveChanges() > 0){
             Console.WriteLine("更新成功！");
         }else{
             Console.WriteLine("更新失败！");
         }
      }
  }
```
##### 另一种特殊情况
`如果你的修改本质是没有修改  例如 你修改 属性 Year = 2017 为2017 那么就相当于并没修改`
* `那么你需要判断一下,你做了那么多修改 到底修改了没有 是不是改成了原值,不然 SaveChanges() 返回为0 你胆信 ChangeTracker.HasChanges() 用这个判断`
```C#
//如果你本质上没有修改任何东西
 if(!ctx.ChangeTracker.HasChanges()){
  Console.WriteLine("更新成功！");
  
 }else if(ctx.SaveChanges() > 0){
     Console.WriteLine("更新成功！");
 }else{
     Console.WriteLine("更新失败！");
 }
```
* `如果需要更新实体全部属性 那么直接将实体状态修改为Modified`
* `若场景是更新实体指定属性,则可以手动赋值或者利用Entry(obj).CurrentValues(..)  解决问题`
* `在此之前一定要用ChangeTracker.HasChanges()  判断实体是否发生了变化`





