<a id="top" href="#top">模型[Model] :maple_leaf:</a> 
----
`Model一般配合数据访问技术Entity Framework来定义和使用这些模型的类,有时候也会定一些其他数据模板,`
#### 

- [x] :maple_leaf: <a href="#UseModel">`使用模型`</a>
- [x] :maple_leaf: <a href="#HttpPost">`HttpPost请求`</a>

####  <a id="UseAddMdoel" href="#UseAddMdoel">使用模型</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`使用EF框架 添加一个模型类`
##### Movie类 
`在Models 文件夹里面添加一个模型类`
```C#
  namespace NetMvc.Models
  {
      [Table("Movies")]
      public class Movie
      {
          [Key]
          public int MovieID { get; set; }
          public string Title { get; set; }
          public DateTime ReleaseDate { get; set; }
          public string Genre { get; set; }
          public decimal Price { get; set; }

      }
  }
```
##### 数据库上下文
```C#
  namespace NetMvc.Models
  {
      public class StoreContent: DbContext
      {
          public StoreContent() : base("name=ConnectionString")
          {
              Database.SetInitializer<StoreContent>(new DropCreateDatabaseIfModelChanges<StoreContent>());
          }
          public DbSet<Movie> Movies { get; set; }
      }
  }
```
##### 添加控制器 
* 选择这个选项: `...with views，using Entity Framwork`
* 需要在根目录下面的Web.config 添加链接字符串 
* 添加后基架会为我们添加许多的控制器方法和视图



####  <a id="HttpPost" href="#HttpPost">HttpPost请求</a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
* 控制器方面
```C#
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<ActionResult> Edit([Bind(Include = "MovieID,Title,ReleaseDate,Genre,Price")] Movie movie)
    {
        if (ModelState.IsValid)
        {
            db.Entry(movie).State = EntityState.Modified;
            await db.SaveChangesAsync();
            return RedirectToAction("Index");
        }
        return View(movie);
    }
```
* 视图 自我定义的视图
```html
  <form action="/Movies/Edit/1" method="post">
  <input name="__RequestVerificationToken" type="hidden" 
  value="il62DKsRUz3d8cH5F4aOYEWnzk3qssk1qEkECwkpDF4ykN7l26q8RXLHJgo8UChlVpSYAEZ6nhWWtmcyXbRZX01EAKVHN2yIpaD8RAw7Bx81" />   
  <div class="form-horizontal">
          <h4>Movie</h4>
          <hr />

          <input data-val="true" data-val-number="字段 MovieID 必须是一个数字。" data-val-required="MovieID 字段是必需的。" id="MovieID" name="MovieID" type="hidden" value="1" />

          <div class="form-group">
              <label class="control-label col-md-2" for="Title">Title</label>
              <div class="col-md-10">
                  <input class="form-control text-box single-line" id="Title" name="Title" type="text" value="我的小可爱啊" />
                  <span class="field-validation-valid text-danger" data-valmsg-for="Title" data-valmsg-replace="true"></span>
              </div>
          </div>

          <div class="form-group">
              <label class="control-label col-md-2" for="ReleaseDate">ReleaseDate</label>
              <div class="col-md-10">
                  <input class="form-control text-box single-line" data-val="true" data-val-date="字段 ReleaseDate 必须是日期。" data-val-required="ReleaseDate 字段是必需的。" id="ReleaseDate" name="ReleaseDate" type="datetime" value="2016/5/26 0:00:00" />
                  <span class="field-validation-valid text-danger" data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
              </div>
          </div>

          <div class="form-group">
              <label class="control-label col-md-2" for="Genre">Genre</label>
              <div class="col-md-10">
                  <input class="form-control text-box single-line" id="Genre" name="Genre" type="text" value="天下无敌" />
                  <span class="field-validation-valid text-danger" data-valmsg-for="Genre" data-valmsg-replace="true"></span>
              </div>
          </div>

          <div class="form-group">
              <label class="control-label col-md-2" for="Price">Price</label>
              <div class="col-md-10">
                  <input class="form-control text-box single-line" data-val="true" data-val-number="字段 Price 必须是一个数字。" data-val-required="Price 字段是必需的。" id="Price" name="Price" type="text" value="59.63" />
                  <span class="field-validation-valid text-danger" data-valmsg-for="Price" data-valmsg-replace="true"></span>
              </div>
          </div>

          <div class="form-group">
              <div class="col-md-offset-2 col-md-10">
                  <input type="submit" value="Save" class="btn btn-default" />
              </div>
          </div>
      </div>
  </form>
```
* 生成视图
```html
@using (Html.BeginForm())
{
    @Html.AntiForgeryToken()
    
    <div class="form-horizontal">
        <h4>Movie</h4>
        <hr />
        @Html.ValidationSummary(true, "", new { @class = "text-danger" })
        @Html.HiddenFor(model => model.MovieID)

        <div class="form-group">
            @Html.LabelFor(model => model.Title, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Title, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Title, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.ReleaseDate, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.ReleaseDate, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.ReleaseDate, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Genre, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Genre, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Genre, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            @Html.LabelFor(model => model.Price, htmlAttributes: new { @class = "control-label col-md-2" })
            <div class="col-md-10">
                @Html.EditorFor(model => model.Price, new { htmlAttributes = new { @class = "form-control" } })
                @Html.ValidationMessageFor(model => model.Price, "", new { @class = "text-danger" })
            </div>
        </div>

        <div class="form-group">
            <div class="col-md-offset-2 col-md-10">
                <input type="submit" value="Save" class="btn btn-default" />
            </div>
        </div>
    </div>
```
       
####  <a id="HttpPost" href="#HttpPost">HttpPost请求 </a>  :star2: <a href="#top"> :arrow_up:  :arrow_up:</a>
`使用模型绑定,默认是隐式的`

```C#
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Edit([Bind(Include = "MovieID,Title,ReleaseDate,Genre,Price")] Movie movie)
        {
            if (ModelState.IsValid)
            {
                db.Entry(movie).State = EntityState.Modified;
                await db.SaveChangesAsync();
                return RedirectToAction("Index");
            }
            return View(movie);
        }
        
        
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create([Bind(Include = "MovieID,Title,ReleaseDate,Genre,Price")] Movie movie)
        {
            if (ModelState.IsValid)
            {
                db.Movies.Add(movie);
                await db.SaveChangesAsync();
                return RedirectToAction("Index");
            }

            return View(movie);
        }
```
##### Bind 注解是防止重复提交攻击

#### 显示绑定
`显示模型绑定,通过控制器的UpdateModel TryUpdateModel方法`

```C#
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<ActionResult> Create()
    {
        var movie = new Movie();
        TryUpdateModel(movie);
        if (ModelState.IsValid)
        {
            db.Movies.Add(movie);
            await db.SaveChangesAsync();
            return RedirectToAction("Index");
        }
        return View(movie);
    }
```

##### 显示指定绑定一部分属性值
```C#
  UpdateModel(movie,new[] {"MovieID","Title","Genre"})
```


























