### [JAVA8 JDBC 数据库连接技术](#top) <b id="top"></b> :maple_leaf:

-----
`推荐使用MYSQL 数据库 实现数据库连接 并且不再讲述 JDBC-ODBC桥 已经完全被淘汰` `在Java中所有与数据库有关的操作类和接口都在 ` [`Java.sql`](https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html) `包中` `不推荐使用Java 链接 SQL Server 因为什么？ 因为Java的拥有者
Oracle 这个公司的领导人不喜欢微软 导致性能不高`

------

- [x] :maple_leaf: [`基本知识`](#node)
- [x] :maple_leaf: [`加载驱动程序`](#load)
- [x] :maple_leaf: [`连接数据库`](#connect)

-----

##### [基本知识](#top) <b id="node"></b> :maple_leaf:
`JDBC是一种标准每个数据库厂商都按照这个标准编写适合自己的JDBC 所以JDBC可以一次编写 跨多种数据库`
* `使用JDBC链接数据库 需要向容器中加载数据库驱动程序[每个数据库厂商都有不同的实现]`
* `所有JDBC连接数据库的操作流程都是固定的按照如下流程`
   * `加载数据库驱动程序[向容器加载]`
   * `进行数据库连接 通过dirvier 类完成  connection表示连接`
   * `进行数据库操作 PreparedStatement Connection Statement ResultSet`
   * `关闭数据库连接 直接关闭就行了`
`返回数据库连接对象标准`   
```java
public class SqlConnection {
	
  public static String jdbUrl;

  public static String DriverMysqlString="com.mysql.jdbc.Driver";

  public static String JdbcMysqlUrl="jdbc:mysql://127.0.0.1:3306/mydb?&characterEncoding=UTF-8";

  public static Connection getDefalutConnection() throws Exception {
     return getConnection(DriverMysqlString,JdbcMysqlUrl,"root","root");
  }

	
  public static Connection getConnection(String driverClass,String jdbUrl,String user,String password)
   throws Exception {
      Driver driver=(Driver)Class.forName(driverClass).newInstance();
      Properties info=new Properties();
      info.put("user", user);
      info.put("password", password);
      Connection connection;
      connection = (Connection) driver.connect(jdbUrl, info);
      return  connection;
  }
} 
```  
##### [加载驱动程序](#top) <b id="load"></b> :maple_leaf:
`通过CLASSPATH 加载驱动程序` [`MYSQL 连接器`](https://www.mysql.com/products/connector/) `这个文件存放在你的MYSQL 安装文件中最新版的
MYSQL 使用的连接器是8.0版本了` `mysql-connector-java-8.0.13` `需要引入Jar包`

`加载Driver`
```java
public static String DriverMysqlString="com.mysql.jdbc.Driver";

Driver driver=(Driver)Class.forName(DriverMysqlString).newInstance();
```
##### [连接数据库](#top) <b id="connect"></b> :maple_leaf:
```
```


##### [](#top) <b id="top"></b> :maple_leaf:
##### [](#top) <b id="top"></b> :maple_leaf:
##### [](#top) <b id="top"></b> :maple_leaf:
##### [](#top) <b id="top"></b> :maple_leaf:
##### [](#top) <b id="top"></b> :maple_leaf:
##### [](#top) <b id="top"></b> :maple_leaf:
