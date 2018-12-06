### [JSP 开发模式](#top) <b id="top"></b> 

----
`Java Web[指的是JSP 不是Spring] 开发模式从2000 年 到最近经历了很多的变化 从最初从` `纯JSP模式` `JSP JavaBean[Model1]`
`MVC[Model2]`  `MVC 三层架构模式` 

- [x] [`纯JSP 模式`](#jsponly) 
- [x] [`JSP JavaBean Model1 模式`](#bean) 
- [x] [`JavaBean`](#java) 
- [x] [`MVC Model2`](#mvc) 
- [x] [`三层架构`](#three) 

------

##### [纯JSP 模式](#top)  <b id="jsponly"></b>
`纯jsp 模式 就是所有的数据展示 业务处理 数据库访问 逻辑判断 前端代码 全部写到JSP 可想而知 页面及其混乱 难以进行二次开发 只注重功能实现 根本不管
设计模式 维护性 可读取 主要流向与2000 左右 几年后这种模式就被淘汰了`

##### [JSP JavaBean Model1 模式](#top)  <b id="bean"></b>
`JSP 将一部分业务逻辑  数据库访问 交给JavaBean 也就是说Java类来承担 但是还是乱整 页面可维护性太差[可读性太差了] 但是还是需要完成业务处理 页面跳转
等等`
##### [JSP JavaBean Model1 模式](#top)  <b id="java"></b>
* `广义的JavaBean 就是一般意义的Java类,按照功能也可以分为 业务处理 JavaBean  数据承载 JavaBean[例如存储 JSP 的FORM 数据] `
* `狭义的JavaBean`:`覆盖SUM 公司定义的规则`
  * `该类必须是公共的`
  * `该类必须实现Serializable 接口`
  * `必须具有无参数构造器 默认和 定义的都可以`
  * `该类如果有成员变量 那么必须是私有的 且必须具有公共的getter setter 属性`

`实际项目中 数据承载JavaBean[实体类] 按照狭义声明书写 `

##### [MVC Model2](#top)  <b id="mvc"></b>
`MVC 模式 M:JavaBean V:JSP C:Servlet  也就是这样的 JavaBean 负责数据库连接`
* `JSP 只能向Servlet 提交`
* `Servlet 处理业务请求`
* `JavaBean 处理业务逻辑 数据访问`

##### [三层架构](#top)  <b id="three"></b>
`三层架构 指的是 视图层 服务层 持久层他们分别完成不同的功能`
* `视图层[view]`    -->    `服务层[Service]`      --> `持久层[DAO]` --> `MYSQL/ORACLE/SQL...`
* `Servlet`  -->  `Interface/Impls`     -->  `Interface/Impls`

`Interface: 接口 Impls：接口的实现类 目的:降低耦合度 面向抽象编码 上层对于下层的使用通过接口 下层对于上层的服务是通过实现接口完成的`

----- 
* `视图层`:`由Servlet 承担 表现层 视图层 也称为 web层,用于接收用户的请求代码在这里书写` 
* `服务层`:`业务逻辑 主要在这里完成 业务层是最复杂的`
* `DAO层`:`数据访问层 书写操作数据库的对象`
