### :triangular_flag_on_post: [小型购物网站搭建](#top)  <b id="top"></b>

-----
`采用Ajax 前后端交互 和JSP EL表达式 JSTL 循环判断 MYSQL数据库 简单分页`

- [x] [`功能展示`](#show)
- [x] [`部署数据库`](#sql)



----
##### :triangular_flag_on_post: [功能展示](#top) <b id="show"></b>


![图片列表](/Image/shopIndex.png)

----

![图片列表](/Image/shopDetail.png)

-----

![图片列表](/Image/shopCart.png)

-----

![图片列表](/Image/ShopPage.png)

----

![图片列表](/Image/shopLogin.png)

##### :triangular_flag_on_post: [部署数据库](#top) <b id="sql"></b>
`那么怎么让它在你的机器上面跑起来呢?`
* `MYSQL 数据库[建议 8.0版本]`
* `IDEA 2018版本 或者以上`

-----
`对于数据库,我们有两种链接方式 ,一个是纯链接方式 在 src/com.zzw.dao 下的BaseDao 中  项目默认是使用的连接池配置 `

##### 创建数据库
`创建数据库的sql 语句`

```sql
Create DataBase If Not Exists Zzw Charset utf8mb4 ; 

use Zzw;
 
Create table productType(
	TypeID int primary key auto_increment,
    TypeName varchar(120) not null,
    TypeStatus int default 30
);
 
Create table Product(
   ProductID int primary key auto_increment,
   TypeID int not null,
   ProductName varchar(300) not null,
   Price float not null,
   MonthlySale int default 0,
   Review int default 0,
   SaveCount int not null comment '库房库存',
   integral int default 0 comment '真钻石积分',
   Picture01 varchar(500),
   Picture2 varchar(500),
   Picture3 varchar(500),
   Picture4 varchar(500),
   Picture5 varchar(500),
   constraint fk_typeID_to_ProductType foreign key(TypeID) references productType(TypeID)
);
  
  
create table Accounts(
 AccountID  varchar(200) primary key  ,
 AccountPasswrod varchar(300) not null,
 AccountName varchar(300) not null,
 AccountEmail varchar(400) not null,
 AccountDescription varchar(400) not null
);

 
insert into zzw.producttype(TypeName,TypeStatus) values('IT 书籍',30);
insert into zzw.producttype(TypeName,TypeStatus) values('生活用品',30);
insert into zzw.producttype(TypeName,TypeStatus) values('儿童服装',30);
insert into zzw.producttype(TypeName,TypeStatus) values('少年玩具',30);
insert into zzw.producttype(TypeName,TypeStatus) values('中考习题',30);
insert into zzw.producttype(TypeName,TypeStatus) values('高考相关',30);
insert into zzw.producttype(TypeName,TypeStatus) values('床单用具',30);
insert into zzw.producttype(TypeName,TypeStatus) values('宿舍用品',30);
insert into zzw.producttype(TypeName,TypeStatus) values('修养茶具',30);

 
  -- 1.
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'Java核心技术卷I基础知识+Java核心技术卷II高级特性（原书第10版）',179.3,473,172,75,15,
 './Product/Java01.jpg','./Product/Java02.jpg','./Product/Java03.jpg','./Product/Java04.jpg','./Product/Java05.jpg');
 
 -- 2.
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'Java编程思想第4版/第四版/thinking in java中文版/java基础入门书',86.4,155,4171,45,5,
 './Product/JavaThinK01.jpg','./Product/JavaThing02.jpg','./Product/JavaThing03	.jpg','./Product/JavaThing04.jpg','./Product/JavaThing05.jpg');
 
 -- 3.
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'正版 JavaEE开发的颠覆者Spring Boot实战 spring boot框架学习开发入门教程',89.00,75,191,90,10,
 './Product/boot1.jpg','./Product/boot1.jpg','./Product/boot1.jpg','./Product/boot1.jpg','./Product/boot1.jpg');
 
-- 4. 
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'正版 自己动手写Docker Docker开发基础入门书籍 Docker容器开发技术',45.80,780,172,62,5,
 './Product/docker01.jpg','./Product/docker02.jpg','./Product/docker03.jpg','./Product/docker04.jpg','./Product/docker05.jpg');
 
-- 5.
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'javascript高级程序设计第3版JS入门html5+css3实战教程精粹',67.50,210,206,20,8,
 './Product/js01.jpg','./Product/js02.jpg','./Product/js03.jpg','./Product/js04.jpg','./Product/js05.jpg');
 
-- 6.
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'Go语言核心编程 Go语言编程入门教程书籍 golang教程实战',56.80,67,206,120,10,
 './Product/go01.jpg','./Product/go02.jpg','./Product/go03.jpg','./Product/go04.jpg','./Product/go05.jpg');
 
 -- 7.
insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(2,'康绮墨丽韩国多效呵护育发洗发乳精华液免洗男女滋养头皮发根套装 ',109.00,75,216,155,25,
 './Product/洗发水.jpg','./Product/洗发水2.jpg','./Product/洗发水3.jpg','./Product/洗发水4.jpg','./Product/洗发水5.jpg');
 
 -- 8
 insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(2,'苹果 2017 Macbook Pro 13英寸(i5 7360U) ',10899.00,15,40,120,900,
 './Product/mac01.jpg','./Product/mac02.jpg','./Product/mac03.jpg','./Product/mac04.jpg','./Product/mac05.jpg');
 
  -- 8
 insert into zzw.product(TypeID,ProductName,Price,MonthlySale,Review,SaveCount,integral,Picture01,Picture2,Picture3,Picture4,Picture5) 
 values(1,'深入浅出Webpack 吴浩麟 webpack入门教程书籍 webpack安装使用指南 ',61.60,8,23,1000,30,
 './Product/webpack01.jpg','./Product/webpack02.jpg','./Product/webpack03.jpg','./Product/webpack04.jpg','./Product/webpack05.jpg');

```

##### 配置连接池来运行MYSQL
* `第一步站到 tomcat目录`：`apache-tomcat-9.0.12\conf\context.xml 里面增加下面的信息`
   * `注意修改 username password `
```xml
<Resource name="jdbc/registration" 
auth="Container" type="javax.sql.DataSource"  maxActive="100"  
maxIdle="30" maxWait="10000"   username="root"   password="Jiangxing627" 
driverClassName="com.mysql.cj.jdbc.Driver"    
url="jdbc:mysql://localhost:3306/Zzw?useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=UTC"/>
```
* `项目目录下的 web.xml 里面增加 配置文件 [已经添加了]`
```xml
  <resource-ref>
      <res-ref-name>jdbc/registration </res-ref-name>
      <res-type> javax.sql.DataSource </res-type>
      <res-auth> Container </res-auth>
  </resource-ref>
```
* `包装 BaseDao 里面的函数 正确 [已经正确]`
```java
public Connection createConnection() {
  Connection conn = null;
  Context ctx = null;
  // 获取连接并捕获异常
  try {
    ctx = new InitialContext();
    DataSource ds = (DataSource) ctx
		.lookup("java:comp/env/jdbc/registration");
    conn = ds.getConnection();
	} catch (NamingException e) {
		e.printStackTrace();
	} catch (SQLException e) {
		e.printStackTrace();
	}
	if(conn == null) {
		System.err.println("无法建立数据库连接。");
	}
	return conn;
}
```

##### 如果你觉得 意思操作太过于麻烦了 那么久这样
`把里面 BaseDao的createConnection 函数代码 注释 然后把 上面被注释的代码 取消注释 改一下链接密码 和用户 就行了`

![链接图片](/Image/shopconnect.png)



----
`然后就可以跑了`












