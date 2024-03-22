# AspectCore

## 官方文件
<https://github.com/dotnetcore/AspectCore-Framework/blob/master/docs/1.%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97.md>

## 環境
Web API (.NET 6)

## 前置作業
安裝套件 `AspectCore.Extensions.DependencyInjection` (2.4.0)

## 基礎用法

先在 `Program.cs` 加入以下兩行
``` cs
builder.Services.ConfigureDynamicProxy();

//Replace the default IOC container with the AspectCore one.
builder.Host.UseServiceProviderFactory(new DynamicProxyServiceProviderFactory());
```

建立一個繼承 `AbstractInterceptorAttribute` 的標籤，在 `Invoke` 方法內實作 AOP
``` cs
public class FooInterceptorAttribute : AbstractInterceptorAttribute
{
    public override async Task Invoke(AspectContext context, AspectDelegate next)
    {
        // before invoke method
        await next(context);
        // after invoke method
    }
}
```

在 DI 加入這個標籤 `FooInterceptorAttribute`
``` cs
builder.Services.AddScoped<FooInterceptorAttribute>();
```

找一個要套用這個標籤來做 AOP 的服務
``` cs
builder.Services.AddScoped<IFooService, FooService>();
```

將標籤放在 `interface` 或是 `class` 之上
``` cs
[FooInterceptor]
public interface IFooService
{
    string Test();
}

public class FooService : IFooService
{
    public string Test()
    {
        return "Hello World";
    }
}
```

如果 `FooInterceptorAttribute` 需要從 DI 取得服務
可以在 `FooInterceptorAttribute` 定義一個屬性並放置 `[FromServiceContext]`
``` cs
[FromServiceContext]
public ILogger<FooInterceptorAttribute> Logger { get; set; }
```

## 不放置標籤的用法

除了直接將標籤放置在 `interface` 或是 `class` 之上
也可以調整 `ConfigureDynamicProxy` 來以指定的標籤來對特定的服務做 AOP
``` cs
builder.Services.ConfigureDynamicProxy(config =>
{
    // 代表以 FooInterceptorAttribute 來作用所有服務名稱以 Service 結尾的類別上
    config.Interceptors.AddServiced<FooInterceptorAttribute>(Predicates.ForService("*Service"));
});
```
這邊需要注意 `Predicates.ForService` 比對的是 ServiceType 的名稱而不是 ImplementationType 的名稱

不用自行放置標籤後，就可以將標籤的建構子拿來給 DI 注入用 (不用再透過 `[FromServiceContext]` 從屬性注入)
``` cs
private readonly ILogger<FooInterceptorAttribute> _logger;

public FooInterceptorAttribute(ILogger<FooInterceptorAttribute> logger)
{
    _logger = logger;
}
```

## 什麼情況才能使用 [FromServiceContext]

| 使用情境  | `[FromServiceContext]` 有作用                                 | `[FromServiceContext]` 沒作用                                    |
| --------- | ------------------------------------------------------------- | ---------------------------------------------------------------- |
| Attribute | `[CustomInterceptor]`                                         | `[ServiceInterceptor(typeof(CustomInterceptorAttribute))]`       |
| Config    | `config.Interceptors.AddTyped<CustomInterceptorAttribute>();` | `config.Interceptors.AddServiced<CustomInterceptorAttribute>();` |


## config 設定攔截條件

指定類別名稱結尾
``` cs
Predicates.ForService("*Service")
```

指定命名空間開頭與類別名稱結尾
``` cs
Predicates.ForService("MyProject.Services.*Service")
```

指定方法名稱結尾
``` cs
Predicates.ForMethod("*Test")
```

類別與方法同時過濾
``` cs
Predicates.ForMethod("MyProject.Services.*Service", "*Test")
```

以 MethodInfo 來判斷
``` cs
methodInfo => methodInfo.Name.EndsWith("Test")
```


## 排除攔截器

### Attribute
```
[NonAspect]
public string Test()
```

### Config
``` cs
config.NonAspectPredicates.AddMethod("*Test");
```