# ASP.NET MVC

### 產出站台完整的 URL

```cs
string url = $"{Request.Url.Scheme}://{Request.Url.Authority}{Url.Action("Generate", "Home", new { guid = guid })}";
```

### 在 Controller 以外的地方使用 UrlHelper

```cs
UrlHelper Url = new UrlHelper(HttpContext.Current.Request.RequestContext);
```

### 擷取全站台未處理的 Exception

Global.asax.cs

```cs
protected void Application_Error(object sender, EventArgs e)
{
    Exception exception = Server.GetLastError();
    if (exception != null)
    {
        // TODO: write log
    }
}
```

App\_Start\FilterConfig.cs

```cs
public static void RegisterGlobalFilters(GlobalFilterCollection filters)
{
    filters.Add(new MyHandleErrorAttribute());
}
```

MyHandleErrorAttribute.cs

```cs
public class MyHandleErrorAttribute : HandleErrorAttribute
{
    public override void OnException(ExceptionContext filterContext)
    {
        //var url = filterContext.HttpContext.Request.Url.AbsoluteUri;
        var exception = filterContext.Exception;
        // TODO: write log
        //filterContext.ExceptionHandled = true;
        base.OnException(filterContext);
    }
}
```

### 從 HtmlHelper 取得 UrlHelper

```cs
var urlHelper = new UrlHelper(htmlHelper.ViewContext.RequestContext);
```

### 從 Request 解析資料

```cs
if (Request.QueryString.Keys.Cast<string>().Contains(name, StringComparer.OrdinalIgnoreCase))
{
    return Request.QueryString.Get(name);
}
if (Request.Form.HasKeys() && Request.Form.Keys.Cast<string>().Contains(name, StringComparer.OrdinalIgnoreCase))
{
    return Request.Form.Get(name);
}
if (Request.InputStream != null && Request.InputStream.CanSeek)
{
    var inputStream = Request.InputStream;
    inputStream.Seek(0, System.IO.SeekOrigin.Begin);
    string json = new System.IO.StreamReader(inputStream).ReadToEnd();
    if (!string.IsNullOrEmpty(json))
    {
        JObject jObject = JObject.Parse(json);
        return jObject.Property(name, StringComparison.OrdinalIgnoreCase)?.Value.ToString();
    }
}
```

### 資料驗證

```cs
if (!ModelState.IsValid)
{
    string errorMessage = string.Join("\n",
        ModelState.Where(x => x.Value.Errors.Any()).SelectMany(x => x.Value.Errors.Select(y => y.ErrorMessage)));
}
```

### 判斷指定名稱的 View 是否存在

```cs
string customViewName = $"FormList_{listAction}";
if (ViewEngines.Engines.FindView(ControllerContext, customViewName, null).View != null)
{
    return View(customViewName, forms);
}
```

### 取得指定 View 的 Model 資料類型

```cs
Type viewType = BuildManager.GetCompiledType(viewPath);
Type modelType = viewType.GetProperties().First(x => x.Name == "Model" && x.DeclaringType.Name == "WebViewPage`1").PropertyType;
```

### ModelBinder

```cs
public class UserModelBinder : IModelBinder
{
    public object BindModel(ControllerContext controllerContext, ModelBindingContext bindingContext)
    {
        User model;
        if(controllerContext.RequestContext.HttpContext.Request.AcceptTypes.Contains("application/json"))
        {
            var serializer = new JavaScriptSerializer();
            var form = controllerContext.RequestContext.HttpContext.Request.Form.ToString();
            model = serializer.Deserialize<User>(HttpUtility.UrlDecode(form));
        }
        else
        {
            model = (User)ModelBinders.Binders.DefaultBinder.BindModel(controllerContext, bindingContext);
        }
        return model;
    }
}
```

#### 加入清單

```cs
ModelBinders.Binders[typeof(User)] = new UserModelBinder();
```

### 上傳檔案

使用 HttpPostedFileBase 類型接收檔案

```cs
[HttpPost]
public ActionResult UploadFile(HttpPostedFileBase file)
{
    if (file != null && file.ContentLength > 0)
    {
        string uploadPath = ConfigurationManager.AppSettings["UploadPath"];
        file.SaveAs(Path.Combine(uploadPath, file.FileName));
    }
    return View();
}
```

[前端參考](broken-reference)
