<a id="top" href="#top"> Java 8 流库 [Stream] :maple_leaf:</a> 
----
`类似于C#的Linq to Objcet 对象查询方法,Java也提供了流操作!以提高代码质量,减少程序员的工作量,这在现代程序开发中是十分重要的`

- [x] :maple_leaf: <a href="#ClassJieOu">`流的初体验`</a>
- [x] :maple_leaf: <a href="#Ninject">`流的创建`</a>
   - <a href="#RandomStream">创建无限流</a>
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
`Collection 接口的stream 方法可以把任何接口集合转换为一个流,如果你有一个数组,那么可以使用静态的Stream.of(Array)方法` **`集合继承关系`**

![集合接口继承](/Image/Collection.png)

`如何创建并行流呢？` `default Stream<E> parallelStream() ` `调用这个方法就行了` ``
* `Collection 集合来创建并行流的方法：`  `Collection.parallelStream()`
* `将顺序流变成并行流 Stream.parallel() Stream.isParallel() 判断是否是并行流`

```java
 public static  void StreamOf(){
     Stream<Integer> ins = Stream.of(1,2,3,4,8,9,121,56,78,32,15,23).parallel();//变成并行流
     Stream<Integer> vals =  ins.filter(val -> val >12);
     List<Integer>  ins_list = vals.collect(Collectors.toList());
     for (Integer val : ins_list) {
         System.out.println("Item :"+val);
     }
 }

 public static void StreamOfArray(){
    String strs = "JakeLover list name is what you should knows" +
            " anything do you know fuck the word Names WhatWhere";
    List<String> words = Arrays.asList(strs.split(" "));
    Stream<String> wordsStream =words.parallelStream();
    //并行流
    try
     {
         Integer[] vals = new Integer[]{10,56,23,75,94,12};
         Stream<Integer> vals_stream = Stream.of(Arrays.copyOfRange(vals, 2, 7)); //从一个数组中截图部分值组成流
         Stream<Integer> val_para = vals_stream.parallel(); //变成并行流
     }catch (Exception ex ){
         System.out.println(ex.getMessage());
     }
  }
```
#####  <a id="RandomStream" href="#RandomStream">创建无限流</a> :star2: <a href="#top"> :arrow_up: </a>
`Stream有两个创建无限流的静态方法,genetare() 接受一个参数函数 Suppiler 可以传递一个lambda 表达式 传入怎么样的方法  返回怎么样的流`<br/>
`Suppiler 所属包:import java.util.function.Supplier`
```java
//Suppiler 接口源码
public interface Supplier<T> {
    /**
     * Gets a result.
     *
     * @return a result
     */
    T get();
}
```
##### 无限流::随机流
* `传入一个随机函数 就可以创建一个随机流, 要有limit 方法规定数量....不然可能会内存耗尽`
```java
  public static Stream<Double> getRandomStrea(){
      Stream<Double> rand = Stream.generate(Math::random).limit(10);//引用随机函数
      List<Double> dbs = rand.collect(Collectors.toList());
      for (int i=0;i<10;i++){
          System.out.println("Random item:"+dbs.get(i));
      }
      return rand;
  }
```
