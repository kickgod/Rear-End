<a id="top" href="#top">	:maple_leaf: EntityFramework ORM框架 [注解版] :blue_heart:</a> 
-----
- [x] :maple_leaf: <a href="#ModelAgreement">约定,映射</a>
  - [x] <a href="TableName">`表名另取 [Table("tableName")]`</a>
  - [x] <a href="Mainkey">`主键 [key] `</a>

##### <a id="Mainkey" href="#Mainkey">主键 `[key]` </a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
```C#
    [Key]
    public int OId { get; set; } //字段上面加上key注解
```
##### <a id="TableName" href="#TableName">表名另取 `[Table("OrderInfo")]`</a> :star2: <a href="#top"> `置顶` :arrow_up:</a>
`默认是以类名称为表名`
```C#
    [Table("OrderInfo")]
    class Order
    {  
        //etc   
    }
```
