<a id="top" href="#top"> Java 8 流库 [Stream] :maple_leaf:</a> 
----
`类似于C#的Linq to Objcet 对象查询方法,Java也提供了流操作!以提高代码质量,减少程序员的工作量,这在现代程序开发中是十分重要的`

- [x] :maple_leaf: <a href="#ClassJieOu">`流的初体验`</a>
- [x] :maple_leaf: <a href="#Ninject">`流的创建`</a>
   - <a href="#RandomStream">`创建无限流`</a>
- [x] :maple_leaf: <a href="#MVCNinject">`filter,map,flatMap`</a>
- [x] :maple_leaf: <a href="#NinjectPropertyConstructor">`limit,skip,concat`</a>
- [x] :maple_leaf: <a href="#DistinctStream">`distinct() :保留不同的元素`</a>
- [x] :maple_leaf: <a href="#SortStream">`sorted() /Sorted(Comparator) 排序`</a>
- [x] :maple_leaf: <a href="#peekStream">`peek(action) 产生一个新的流`</a>
- [x] :maple_leaf: <a href="#NinjectScopeFunction">`Optional`</a>

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
* `利用Arrays 的静态方法创建流 截图部分的数组/全部数组`
```java
  Integer[] vals = new Integer[]{1,2,4,3,9,5,6,78,5,6,15};
  Stream<Integer> v = Arrays.stream(vals,1,6);
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
##### 无限流::随机数流
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
##### 无限流::利用函数创建
* `利用iterate 方法！接受参数 种子和一个函数 满足接口 UnaryOperation<T>的lambda表达式 并且会反复将该函数应用到之前的结果上`
```Java
 public static Stream<Integer> getNumberStrea(){
     Stream<Integer> rand = Stream.iterate(1 , num -> num  + 1).limit(30);
     for (Integer val: rand.collect(Collectors.toList())
          ) {
         System.out.println(val);
     }
     return rand;
 }
```
####  :maple_leaf: <a id="MVCNinject" href="#MVCNinject" >`filter,map,flatMap`</a><a href="#top"> :arrow_up: </a>
`流从转换会形成一个新的流,它的元素派生自另一个流中的元素,我们已经看到了filter转换会产生一个流,它的元素与某种条件想匹配.下面，我们将一个字符串
流抓还为包含长单词的流`
* **`Stream<T> filter(Predicate<? super T> predicate)`**:`传入T  返回Boolean 返回由该流的元素组成的流，该元素与给定的谓词匹配。 `
```java
String strs = "JakeLover list name is what you should knows" +
       " anything do you know fuck the word Names WhatWhere";
Stream<String> wordlist =Arrays.asList(strs.split(" "))
       .stream()
       .filter(word -> word.length() > 5);
```
`通常我们需要按照某种方式来转换流中的值,此时可以使用map方法并传递执行该转换的函数,例如，我们可以像下面这样讲所有单词都转换小写！`
* **`<R> Stream<R> map(Function<? super T,? extends R> mapper)`**:`返回一个流，包括将给定函数应用到该流元素的结果。 R -新的流元素类型 `
```java
String strs = "JakeLover list name is what you should knows" +
       " anything do you know fuck the word Names WhatWhere";
List<String> wordlist =Arrays.asList(strs.split(" "))
       .stream()
       .map(word -> word.toLowerCase()).collect(Collectors.toList());
for (String val:wordlist
    ) {
   System.out.println(val);
}
```
* **`<R> Stream<R> flatMap(Function<? super T,? extends Stream<? extends R>> mapper)`**:`返回由将所提供的映射函数应用到每个元素的映射流的内容替换此流的每个元素的结果的结果流。每个映射流 closed后其内容被放置到该流。（如果一个映射的流 null空流使用，而不是`
```java
  Stream<String> flatResult = Arrays.asList(strs.split(" ")).stream().flatMap(w -> getCharStream(w));
  public  static  Stream<String> getCharStream(String val){
     List<String> list_c = new ArrayList<String>();
     for (char c:val.toCharArray()
          ) {
         list_c.add(String.valueOf(c));
     }
     return list_c.stream();
  }
```
#####  :maple_leaf: <a href="NinjectPropertyConstructor" id="NinjectPropertyConstructor">`limit,skip,concat`</a><a href="#top"> :arrow_up: </a>
`抽取子流和连接流的方法`
* `Stream<T> limit(long maxSize) 返回一个包含该流的元素流，截断长度不超过 maxSize。 `
* `Stream<T> skip(long n) 返回一个包含此流的其余部分丢弃的流的第一 n元素后流。 `
* `static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b) 连接两个流`
```java
 public  static  void  main(String[] args){
  Integer[] nums = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30};
  Integer[] nums2 = {31,32,33,34,35,36,37,38,39,40};
  Stream<Integer> stream = Stream.concat(Arrays.asList(nums).stream(),Arrays.asList(nums2).stream());
  List<Integer> vals = stream.skip(10).limit(10).collect(Collectors.toList());
  for (Integer val: vals
       ) {
      System.out.println("Item:"+ val);
  }
 }
```
#####  :maple_leaf: <a href="DistinctStream" id="DistinctStream">distinct() :保留不同的元素</a><a href="#top">:arrow_up: </a> 
`删除重复元素`
```java
 public static  void distinct(){
     Integer[] nums = {1,1,2,5,66,66,77,1,2,3,5,4,4,8,15,16};
     List<Integer> vals = Arrays.asList(nums).stream().distinct().sorted().collect(Collectors.toList());
     for (Integer val: vals
     ) {
         System.out.println("Item:"+ val);
     }
 }
```
#####  :maple_leaf: <a href="SortStream" id="SortStream">`sorted() /Sorted(Comparator) 排序`</a><a href="#top">:arrow_up: </a> 
```java
 public static  void distinct(){
     Integer[] nums = {1,1,2,5,66,66,77,1,2,3,5,4,4,8,15,16};
     List<Integer> vals = Arrays.asList(nums).stream().distinct().sorted((one,two) -> one%8 - two%8 )
                         .collect(Collectors.toList());
     for (Integer val: vals
     ) {
         System.out.println("Item:"+ val);
     }
 }
```   
##### :maple_leaf: <a href="peekStream" id="peekStream">`peek(action) 产生一个新的流`</a> <a href="#top">:arrow_up: </a>
```java
 public static void peek(){
    Integer[] nums = {1,1,2,5,66,66,77,1,2,3,5,4,4,8,15,16};
    List<Integer> vals = Arrays.asList(nums).stream().distinct()
   .peek(one -> System.out.println(one)).collect(Collectors.toList());
 }
```
```java
 public static void  main(String args[]){
     String strs = "JakeLover list name is what you should knows " +
             "anything do you know fuck the word Names WhatWhere";
     List<String> words = Arrays.asList(strs.split(" "));
     Optional<String> word_max = words.stream().max(String::compareToIgnoreCase);
     System.out.println("最大值："+word_max.orElse(" 集合为空"));
 }
```
