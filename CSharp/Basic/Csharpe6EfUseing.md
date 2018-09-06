<a id="top" href="#top">EF 框架实践之路  :maple_leaf:</a> 
----
`EF框架采坑的道路,为了避免以后别人继续采坑,我记录下这些坑`

- [x] :maple_leaf: <a href="#CreateTable">`建表问题`</a>
  - <a href="#Basiyuanze">`基本原则`</a>
  - <a href="#ZhujieNotNeedUse">`不管用的注解`</a>
  - <a href="#Enumyingshe">`关于枚举`</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>
- [x] :maple_leaf: <a href="#">``</a>

####  <a id="Basiyuanze" href="#Basiyuanze">基本原则</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `使用注解创建表和关系,其他的通过 表名+Map 类继承 EntityTypeConfiguration<表名> 配置映射关系的方式创建表 千万不要 因为这个各种错误都会
产生`
* `不要在上下文类中的OnModelCreating 设置表关系和表结构 实体多的时候 代码一点也不清晰`
####  <a id="ZhujieNotNeedUse" href="#ZhujieNotNeedUse">不管用的注解</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* `一般对于属性的映射类型使用默认的就好,如果一定需要修改它的类型那么可以使用如下的方式,但是一定不要使用注解 [Column(TypeName="datetime2")]
这个注解完全不管用,还报错 ,sql server中找不到对这个datetime2 的数据支持  varchar 也没有`

  ```C#
      protected override void OnModelCreating(DbModelBuilder modelBuilder)
      {
        modelBuilder.Entity<Profession>().Property(t => t.AddTime).HasColumnType("datetime2");
        base.OnModelCreating(modelBuilder);
      }
  ```
####  <a id="Enumyingshe" href="#Enumyingshe">关于枚举</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
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
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
####  <a id="" href="#"></a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>





