# Razor Pages

## 環境變數 <a href="#h.n7wupyn53m2h" id="h.n7wupyn53m2h"></a>

[https://sites.google.com/view/maoears-coding-notes/net-core/asp-net-core/%E7%92%B0%E5%A2%83%E8%AE%8A%E6%95%B8](https://sites.google.com/view/maoears-coding-notes/net-core/asp-net-core/%E7%92%B0%E5%A2%83%E8%AE%8A%E6%95%B8)

## appsettings.json <a href="#h.1g518gu98g3g" id="h.1g518gu98g3g"></a>

注入 IConfiguration 並透過 GetSection 與 Bind 取得對應的物件

```cs
private readonly DatabaseLoggerConfig config;

public DatabaseLogger(IConfiguration configuration)
{
    this.config = new DatabaseLoggerConfig();
    configuration.GetSection("EntityFrameworkCore").Bind(config);
}
```

### 讓 cshtml 在執行階段編譯 <a href="#h.4st1bfk4ix1b_l" id="h.4st1bfk4ix1b_l"></a>

#### .NET Core 2.2

加入 `<MvcRazorCompileOnPublish>false</MvcRazorCompileOnPublish>`

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
    <MvcRazorCompileOnPublish>false</MvcRazorCompileOnPublish>
    <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
    <TypeScriptToolsVersion>3.1</TypeScriptToolsVersion>
  </PropertyGroup>
```

#### .NET 6 (.NET Core 3.1 以上)

加入 `AddRazorRuntimeCompilation()`

```cs
builder.Services.AddRazorPages()
    .AddRazorRuntimeCompilation();
```

[官方說明](https://learn.microsoft.com/zh-tw/aspnet/core/mvc/views/view-compilation?tabs=visual-studio)

### 取得 HtmlHelper

```cs
var htmlHelper = this.Context.RequestServices.GetRequiredService<IHtmlHelper>();
(htmlHelper as IViewContextAware).Contextualize(ViewContext);
```

### 取得 UrlHelper

```cs
var urlHelperFactory = this.Context.RequestServices.GetRequiredService<IUrlHelperFactory>();
var urlHelper = urlHelperFactory.GetUrlHelper(ViewContext);
```

### 整合 Angular

[https://gist.github.com/VaclavElias/544b8d097981e217ae0dc7c32adba705](https://gist.github.com/VaclavElias/544b8d097981e217ae0dc7c32adba705)

[https://stackoverflow.com/questions/50815456/asp-net-core-2-1-angular-6-0-spa-template-razor-page-cshtml-for-index-instea](https://stackoverflow.com/questions/50815456/asp-net-core-2-1-angular-6-0-spa-template-razor-page-cshtml-for-index-instea)

[https://momonet.io/posts/2020-03-11-adding-angular-app-to-an-existing-razor-page/](https://momonet.io/posts/2020-03-11-adding-angular-app-to-an-existing-razor-page/)

[https://github.com/AlahmadiQ8/RazorPagesAngular](https://github.com/AlahmadiQ8/RazorPagesAngular)
