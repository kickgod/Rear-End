### [JAVA8 JDBC 数据库连接技术](#top) <b id="top"></b> :maple_leaf:

-----
`推荐使用MYSQL 数据库 实现数据库连接 并且不再讲述 JDBC-ODBC桥 已经完全被淘汰` `在Java中所有与数据库有关的操作类和接口都在 ` [`Java.sql`](https://docs.oracle.com/javase/8/docs/api/java/sql/package-summary.html) `包中` `不推荐使用Java 链接 SQL Server 因为什么？ 因为Java的拥有者
Oracle 这个公司的领导人不喜欢微软 导致性能不高`

------

- [x] :maple_leaf: [`基本知识`](#node)
- [x] :maple_leaf: [`加载驱动程序`](#load)
- [x] :maple_leaf: [`连接数据库 `](#connect)
- [x] :maple_leaf: [`操作数据库 `](#statement)
- [x] :maple_leaf: [`PreparedStatement`](#ps)
- [x] :maple_leaf: [`批处理和事务操作`](#commit)

-----

##### [基本知识](#top) <b id="node"></b> :maple_leaf:
`JDBC是一种标准每个数据库厂商都按照这个标准编写适合自己的JDBC 所以JDBC可以一次编写 跨多种数据库`
* `使用JDBC链接数据库 需要向容器中加载数据库驱动程序[每个数据库厂商都有不同的实现]`
* `所有JDBC连接数据库的操作流程都是固定的按照如下流程`
   * `加载数据库驱动程序[向容器加载]`
   * `进行数据库连接 通过dirvier 类完成  connection表示连接`
   * `进行数据库操作 PreparedStatement Connection Statement ResultSet`
   * `关闭数据库连接 直接关闭就行了`
   
`mysql-connector-java-5.1.31-bin.jar版本的 数据库连接对象标准`   
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
`mysql-connector-java-8.0.13版本的 数据库连接对象标准`  
* `新版本需要加时区 和替换加载类`
```java
public class SqlConnection {
	
  public static String jdbUrl;

  public static String DriverMysqlString="com.mysql.cj.jdbc.Driver";

  public static String JdbcMysqlUrl = 
     "jdbc:mysql://127.0.0.1:3306/mydb?&characterEncoding=UTF-8&serverTimezone=UTC";

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
MYSQL 使用的连接器是8.0版本了` `mysql-connector-java-8.0.13` `需要引入Jar包` [链接参数](https://blog.csdn.net/zyw23zyw23/article/details/70549853?utm_source=blogxgwz6)

`加载Driver:数据库Jar包版本：mysql-connector-java-5.1.31-bin.jar`
```java
public static String DriverMysqlString="com.mysql.jdbc.Driver";

Driver driver=(Driver)Class.forName(DriverMysqlString).newInstance();
```

`数据库Jar包版本：mysql-connector-java-8.0.13-bin.jar`
```java
public static String DriverMysqlString="com.mysql.cj.jdbc.Driver";

Driver driver= (Driver)Class.forName(DriverMysqlString).newInstance();
```
##### [连接数据库](#top) <b id="connect"></b> :maple_leaf:
`连接数据库需要提供一些信息 （前提:数据库服务要打开）`
 * `数据库连接地址`:`jdbc:mysql://127.0.0.1:3306/mydb?&characterEncoding=UTF-8`
 * `jdbc:数据库类型:IP地址:端口/数据库名称？附加信息`
 * `数据库用户名 密码`
```c#
String JdbcMysqlUrl="jdbc:mysql://127.0.0.1:3306/mydb?&characterEncoding=UTF-8&serverTimezone=UTC";

Driver driver=(Driver)Class.forName(driverClass).newInstance();
//指明账户和密码
Properties info=new Properties();
info.put("user", user);
info.put("password", password);
Connection connection = (Connection) driver.connect(JdbcMysqlUrl, info); //链接成功
System.out.println(connection);//输出就说嘛链接成功
connection.close();//关闭链接
```
[`新版的8.0.13版mysql jdbc包 需要指明时区`](#top)
```java
String sqlConnectionString = "jdbc:mysql://127.0.0.1:3306/sakila?&characterEncoding=UTF-8&serverTimezone=UTC";
```

##### [操作数据库](#top) <b id="statement"></b> :maple_leaf:
`如果要操作数据库要使用 statement 接口对象才可以 要想取得此对象 需要借助于Connection 并且一个Connection可以打开多个 Statement`
```java
Statement stat=(Statement) connection.createStatement();
```
##### 对数据库此操作
`对于数据库的操作大约有两种类型 1.数据更新  2.数据查询 接口提供 了 两个方法来实现它`
* `int executeUpdated(String sql)`:`返回影响的行数`
* `ResultSet executeQuery(String sql)`
###### 更新操作
```c#
Statement state = connection.createStatement();
int result = state.executeUpdate("insert into sakila.actor(first_name,last_name,last_update) 
values('jxkicker','stat','2016/06/15 15:24');");
System.out.println(result);
```
###### 查询操作
```c#
Statement state = connection.createStatement();
ResultSet set = state.executeQuery("select * from sakila.actor order by actor_id");
while(set.next()) {
    int actorId=set.getInt("actor_id");
    String fname = set.getString("first_name");
    String lname = set.getString("last_name");
    Date JAddTime = set.getDate("last_update");
    System.out.println(actorId +" " + fname + " " + lname + " " + ((java.sql.Date) JAddTime).toLocalDate() );
}
```

###### statement 的缺点字符串插值操作会导致sql注入不安全
`所以应该使用什么呢? PreparedStatement包装sql参数的安全性  在实际工作中 永远都不会再使用statement`

##### [PreparedStatement](#top) <b id="ps"></b> :maple_leaf:
`查询操作`
```c#
PreparedStatement ps= 
	(PreparedStatement) 
	con.prepareStatement
	("select JId,JTitile,JAddTime ,JAuther,JFeeling from Jouranl where JAuther= ? and
	JTitile like '%"+key+"%' or JFeeling like '%"+key+"%'");

ps.setString(1, UserID);	
ResultSet set= ps.executeQuery();
while(set.next()) {
   int JId=set.getInt("JId"); 
   String JTitile=set.getString("JTitile");
   String JFeeling=set.getString("JFeeling");
   Date JAddTime=set.getDate("JAddTime");
   String JAuther=set.getString("JAuther");
   Journal nal=new Journal(JId, JTitile, JFeeling, JAddTime, JAuther);
   list.add(nal);
}
```
`查询一列的结果`
```c#
Connection con=SqlConnection.getDefalutConnection();
PreparedStatement ps= (PreparedStatement) con.prepareStatement("select JContent from Jouranl where JId = ?");
ps.setInt(1, Id);
ResultSet set= ps.executeQuery();

String JContent="Jk";
while(set.next()) {
	JContent=set.getString("JContent");
}
con.close();
return JContent;

//读取一位的情况
PreparedStatement ptats =(PreparedStatement) connection.prepareStatement("select count(*) from numberinfo where NAge > ?");

ptats.setInt(1, 20);
ResultSet set =ptats.executeQuery();
if(set.next()) {
	int count=set.getInt(1);
	System.out.println("数量:"+count);

}			


```
`更新操作`
```c#
Connection con=SqlConnection.getDefalutConnection();
PreparedStatement ps= (PreparedStatement) con.prepareStatement("insert
into Jouranl(JTitile,JContent,JFeeling,JAuther) values(?,?,?,?)");
//设置参数
ps.setString(1, title);
ps.setString(2, content);
ps.setString(3, fel);
ps.setString(4, uid);
int n= ps.executeUpdate();
```

##### [日期问题](#top)
`在Java.util.Date类下有三个描述事件的子类都在java.sql包中`
* `Java.sql.Date`:`描述的是日期`
* `Java.sql.Time`:`描述的是时间`
* `Java.sql.TimeStamp`:`描述时间戳`
`为此我们需要将 Java.util.Date 变成 Java.sql.Date`
```c#
Date nows=(Date) sdf.parse("2018/6/27 18:25");
java.sql.Date sd = new java.sql.Date(nows.getTime()));
```

##### [批处理和事务操作](#top) <b id="commit"></b> :maple_leaf:
* `关闭事务自动提交`：`connection.setAutoCommit(false);`
* `Connection 方法 事务回滚`:`rollback`
* `addBatch`:`添加批处理`
* `执行批处理`:`executeBatch()`
`例子：`
```c#
Statement stats =(Statement) connection.createStatement();
			connection.setAutoCommit(false);
try {
    stats.addBatch("insert into numberinfo(NName,NAge) values('ad',10)");
    stats.addBatch("insert into numberinfo(NName,NAge) values('as1',11)");
    stats.addBatch("insert into numberinfo(NName,NAge) values('df2',12)");
    stats.addBatch("insert into numberinfo(NName,NAge) values('qq3',13)"); 
	  stats.addBatch("insert into numberinfo(NName,NAge) values('vv4',14)");
    stats.addBatch("insert into numberinfo(NName,NAge) values('aa5',15)");
    stats.addBatch("insert into numberinfo(NName,NAge) values('vv',17)");
    int [] sta=stats.executeBatch();
    System.out.println(Arrays.toString(sta));
  connection.commit();
	}
	catch(Exception ex) {
	  connection.rollback();
	  System.out.println(ex.getMessage());
  }
	connection.close();
} catch (SQLException e) {
	e.printStackTrace();
}
```

