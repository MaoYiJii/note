# Web API 2

### ActionFilterAttribute

```cs
/// <summary>
/// HttpGet 無法用 class 的替代方案
/// 用 Json 字串傳遞參數，並透過這個 Filter 自動反序列化回物件
/// </summary>
public class JsonParameterFilterAttribute : ActionFilterAttribute
{
    public string[] Names { get; }
    public JsonParameterFilterAttribute(params string[] names)
    {
        Names = names;
    }

    public override void OnActionExecuting(HttpActionContext filterContext)
    {
        var queryString = filterContext.Request.RequestUri.Query;
        if (queryString.Contains('?'))
        {
            var actionParameters = filterContext.ActionDescriptor.GetParameters();
            var queryValues = queryString.Substring(queryString.IndexOf('?') + 1)
                .Split('&')
                .Select(x => x.Split('='))
                .Where(x => x.Length > 1)
                .Select(x => new KeyValuePair<string, string>(x[0], HttpUtility.UrlDecode(x[1])))
                .ToList();
            foreach (var name in Names)
            {
                var parameterType = actionParameters.FirstOrDefault(x => x.ParameterName.Equals(name, StringComparison.OrdinalIgnoreCase))?.ParameterType;
                var queryValue = queryValues.FirstOrDefault(x => x.Key.Equals(name, StringComparison.OrdinalIgnoreCase)).Value;
                if (parameterType != null && !string.IsNullOrEmpty(queryValue))
                {
                    filterContext.ActionArguments[name] = JsonConvert.DeserializeObject(queryValue, parameterType);
                }
            }
        }
    }
}
```

### 新增 ModelBinder

WebApiConfig.cs

```cs
var provider = new System.Web.Http.ModelBinding.Binders.SimpleModelBinderProvider(
    typeof(System.Collections.Specialized.NameValueCollection), new NameValueModelBinder());
config.Services.Insert(typeof(System.Web.Http.ModelBinding.ModelBinderProvider), 0, provider);
```

NameValueModelBinder.cs

```cs
public class NameValueModelBinder : System.Web.Http.ModelBinding.IModelBinder
{
    public bool BindModel(System.Web.Http.Controllers.HttpActionContext actionContext, System.Web.Http.ModelBinding.ModelBindingContext bindingContext)
    {
        if (bindingContext.ModelType == typeof(System.Collections.Specialized.NameValueCollection))
        {
            // TODO: nameValues
            bindingContext.Model = nameValues;
            return true;
        }
        return false;
    }
}
```
