<a id="top" href="#top">ASP.NET MVC 数据注解和验证  :maple_leaf:</a> 
----
`MVC框架提供了许多的有用的注解`
- [x] :maple_leaf: <a href="#MVCAttribute">`框架提供注解`</a>
   - <a href="#Required">`Required`</a>
   - <a href="#StringLength">`StringLength`</a>
   - <a href="#RegularExpression">`RegularExpression`</a>
   - <a href="#Range">`Range`</a>
   - <a href="#Compare">`Compare`</a>
   - <a href="#Remote">`Remote`</a>
- [x] :maple_leaf: <a href="#DefineByUserself">`自定义验证逻辑注解`</a>
   - <a href="#DisplayName">`DisplayName`</a>
- [x] :maple_leaf: <a href="#ShowAndEditAttribute">`显示和编辑注解`</a>


####  <a id="MVCAttribute" href="#MVCAttribute">框架提供注解</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`注解的验证会在执行模型绑定的时候，模型绑定器一旦使用新值完成对模型属性的更新,就会利用当前的模型元数据获得模型的所有验证器,这个模型验证器会
找所有的验证特性并执行他们包含的验证逻辑,模型绑定器捕获所有的失败并把它们放到模型状态中`
```C#

```
##### <a href="#top" id="Required" >Required :arrow_up:</a>
* `如果模型类的字段是必须填写的,可以加上Required 注解到属性上面`
* `当这属性的值是null或为空的时候,Required特性会引发一个验证错误,验证错误提示信息如果要自定义可以 在注解中设置属性`
```C#
  [Required(ErrorMessage ="请输入书籍编号")]
  public string ID{get;set;}
```
* `如果直接使用那么会使用系统默认的提示信息`
```C#
  [Required]
  public string ID{get;set;}
```
##### <a href="#top" id="StringLength" >StringLength :arrow_up:</a>
`可以指定字符串的最大长度,和最小长度,第二个参数不写就是最小长度为1,最大长度为200 不能超过`
```C#
 [Required(ErrorMessage ="请输入书籍的名称")]
 [StringLength(200,MinimumLength =1)]
 [DisplayName("书名称")]
 public string Name{get;set;}
```
`指定错误信息`
```C#
 [StringLength(200,MinimumLength =1,ErrorMessage ="请输入正确的书名")]
 [DisplayName("书名称")]
 public string Name{get;set;}
```
##### <a href="#top" id="RegularExpression" >RegularExpression :arrow_up:</a>
`使得字段复合正则表达式,添加正则表达式`
```C#
 [MaxLength(250)]
 [RegularExpression(@"^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$")]
 public String UserEmail { get; set; }
```
##### <a href="#top" id="Range" >Range :arrow_up:</a>
`确定整数的范围`
```C#
 [Range(14,120)]
 public String Age { get; set; }
```
`确定小数的范围`
```C#
 [Range(typeof(double),"0.00","199.99")]
 public double Price{get;set;}
  
 [Range(typeof(decimal),"0.00","199.99")]
 public double Price{get;set;}
```
##### <a href="#top" id="Compare" >Compare :arrow_up:</a>
`当要求两个字段要相等的时候,两个属性拥有相同的值`
```C#
  public String Password { get; set; }
  [Compare("Password")]
  public String PasswordConfirm { get; set; }
```


#####  <a id="Remote" href="#Remote">Remote</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`将会将这个字段的值发给一个控制器方法进行验证返回一个Json数据 Json里面只有一个值true/false`
```C#
  [Remote("CheckIsOk","Book")]
  public string ID{get;set;}
```
`控制器验证方法`
```C#
  public JsonResult CheckIsOk(String BooKid)
  {
      List<Book> bks = JsonMapper.ToObject<List<Book>>(System.IO.File.ReadAllText
                       (Server.MapPath("~/Resource/Books.json")));
      var result = bks.Find(b => b.ID == BooKid) == null;
      return Json(result, JsonRequestBehavior.AllowGet); //允许来自客户端的请求
  }
```
####  <a id="DefineByUserself" href="#DefineByUserself">自定义验证逻辑注解</a>  :star2: <a href="#top"> :arrow_up:</a>
`自定义验证注解,所有的验证注解最终都派生自基类 ValidationAttribute 它是抽象类 所有我们的注解验证类也应该派生自 ValidationAttribute`

##### 自定义一个验证类
```C#
  public class MaxWordsAttribute:ValidationAttribute
  {

      private int maxWords;

      public MaxWordsAttribute(int maxWords)
      {
          this.maxWords = maxWords;
      }

      protected override ValidationResult IsValid(object value, ValidationContext validationContext)
      {
          if (value != null)
          {
              var valInt = value.ToString().Split(' ').Count();
              if(valInt > maxWords){
                  return new ValidationResult("Too Many Words");
              }
          }
          return ValidationResult.Success;
      }
  }
```
`使用注解`
```C#
  [MaxWords(200)]
  public string Name{get;set;}
```
####  <a id="ShowAndEditAttribute" href="#ShowAndEditAttribute">显示和编辑注解</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>

##### - <a href="#DisplayName">`DisplayName`</a>
`使用基架的时候显示的就不是ID 而是编号了`
```C#
    [DisplayName("编号")]
    [Required(ErrorMessage ="请输入书籍编号")]
    public string ID{get;set;}
```
