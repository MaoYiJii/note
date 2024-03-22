# ASP.NET Core

查看 SDK 安裝的版本清單

```
dotnet --list-sdks
```

取得 Request 完整 Url

```
string.Concat(Request.Scheme, "://", Request.Host.ToUriComponent(), Request.PathBase.ToUriComponent(), Request.Path.ToUriComponent(), Request.QueryString.ToUriComponent())
```

設置多語系

```cs
// Localization
app.UseRequestLocalization(options =>
{
    IList<CultureInfo> cultures;
    using (var scope = serviceProvider.CreateScope())
    {
        var dbContext = scope.ServiceProvider.GetRequiredService<DefaultDbContext>();
        cultures = dbContext.Settings
            .Where(x => x.Available && x.Type == "Culture")
            .OrderBy(x => x.Order)
            .Select(x => new CultureInfo(x.Name))
            .ToList();
    }
    options.SupportedCultures = cultures;
    options.SupportedUICultures = cultures;
    var defaultCulture = builder.Configuration.GetValue<string>("DefaultCulture");
    if (!string.IsNullOrEmpty(defaultCulture))
    {
        options.SetDefaultCulture(defaultCulture);
    }
    options.RequestCultureProviders = new IRequestCultureProvider[]
    {
        new RouteDataRequestCultureProvider()
        {
            RouteDataStringKey = "culture",
            UIRouteDataStringKey = "ui-culture",
            Options = options
        },
        new AcceptLanguageHeaderRequestCultureProvider()
        {
            Options = options
        },
        //new CookieRequestCultureProvider()
        //{
        //    CookieName = ".WebPrototype.Culture",
        //    Options = options
        //},
        //new CookieRequestCultureProvider()
        //{
        //    Options = options
        //},
        //new QueryStringRequestCultureProvider()
        //{
        //    Options = options
        //}
    };
});
```

## 加入 Rest API 的連線服務

使用 VS 2022 加入 服務參考 或 連線服務
選擇 OpenAPI

### 修正 `錯誤 CS0579 'System.CodeDom.Compiler.GeneratedCode' 屬性重複`

是因為 operationId 有底線造成的
打開對應的 .json 檔案
使用正規表達式取代 `("operationId":\s*"[^_]+)_` 成 `$1`

### 使用 Interface

打開專案檔 (*.csproj)
在對應的 `<OpenApiReference>` 節點下加入
``` xml
<Options>/GenerateClientInterfaces:true</Options>
```