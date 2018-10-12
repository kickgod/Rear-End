<a id="top" href="#top"> Java 8 流库 [Stream] :maple_leaf:</a> 
----
`类似于C#的Linq to Objcet 对象查询方法,Java也提供了流操作!以提高代码质量,减少程序员的工作量,这在现代程序开发中是十分重要的`

- [x] :maple_leaf: <a href="#ClassJieOu">`流的初体验`</a>
- [x] :maple_leaf: <a href="#Ninject">`流的创建`</a>
- [x] :maple_leaf: <a href="#MVCNinject">`Ninject 容器 MVC 注入`</a>
- [x] :maple_leaf: <a href="#NinjectPropertyConstructor">`Ninject 指定属性和构造器参数`</a>
- [x] :maple_leaf: <a href="#NinjectWhenFunction">`Ninject 条件绑定方法`</a>
- [x] :maple_leaf: <a href="#NinjectScopeFunction">`Ninject 作用域方法`</a>

####  <a id="ClassJieOu" href="#ClassJieOu">流的初体验</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
```java
 public static void main(String args[]){
     String strs = "JakeLover list name is what you should knows" +
             " anything do you know fuck the word Names WhatWhere";
     long count = Arrays.asList(strs.split(" "))
             .stream()
             .filter(word -> word.length() >= 5)
             .count();
     System.out.println("数量："+count);
 }
```
* `流的原则`：`定义做什么而非怎么做`
* `流的只是一系列操作而不是存储数据 它定义转换获取 使用数据的方式`
* `流所定义的操作的执行可以并行也可以顺序,所以会有顺序流和并行流两种类型,他们的创建方式不同其他的操作方式大致一样`
####  <a id="Ninject" href="#Ninject">流的创建</a> :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`Collection 接口的stream 方法可以把任何接口集合转换为一个流,如果你有一个数组,那么可以使用静态的Stream.of(Array)方法`
[]('/Image/Collection.png')
