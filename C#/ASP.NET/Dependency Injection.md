## MVC5

### 參考組件
Microsoft.Extensions.DependencyInjection.Abstractions

### Startup.cs
Configuration
``` cs
var services = new ServiceCollection();

// TODO: Add Services

// 註冊 Controller
foreach (var type in System.Reflection.Assembly.GetExecutingAssembly().GetExportedTypes().Where(x => !x.IsAbstract && !x.IsGenericTypeDefinition && typeof(IController).IsAssignableFrom(x)))
{
    services.AddTransient(type);
}
// Build ServiceProvider
var serviceProvider = services.BuildServiceProvider();
var resolver = new AppDependencyResolver(serviceProvider);
DependencyResolver.SetResolver(resolver);
```

### AppDependencyResolver.cs
``` cs
public class AppDependencyResolver : System.Web.Mvc.IDependencyResolver
{
    private readonly IServiceProvider _serviceProvider;

    public AppDependencyResolver(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public object GetService(Type serviceType)
    {
        return _serviceProvider.GetService(serviceType);
    }
    public IEnumerable<object> GetServices(Type serviceType)
    {
        return _serviceProvider.GetServices(serviceType);
    }
}
```

## Web API 2

### 參考組件
Microsoft.Extensions.DependencyInjection.Abstractions

### Startup.cs
Configuration
``` cs
var services = new ServiceCollection();

// TODO: Add Services

// 註冊 ApiController
foreach (var type in Assembly.GetExecutingAssembly().GetExportedTypes().Where(x => !x.IsAbstract && !x.IsGenericTypeDefinition && typeof(IHttpController).IsAssignableFrom(x)))
{
    services.AddTransient(type);
}
// Build ServiceProvider
var serviceProvider = services.BuildServiceProvider(true);
var resolver = new AppDependencyResolver(serviceProvider);
GlobalConfiguration.Configuration.DependencyResolver = resolver;
```

### AppDependencyResolver.cs
``` cs
public class AppDependencyResolver : System.Web.Http.Dependencies.IDependencyResolver
{
    private readonly IServiceProvider _serviceProvider;
    private readonly IServiceScope _serviceScope;
    
    public AppDependencyResolver(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }
    public AppDependencyResolver(IServiceScope serviceScope)
    {
        _serviceScope = serviceScope;
        _serviceProvider = serviceScope.ServiceProvider;
    }

    public virtual object GetService(Type serviceType)
    {
        return _serviceProvider.GetService(serviceType);
    }
    public virtual IEnumerable<object> GetServices(Type serviceType)
    {
        return _serviceProvider.GetServices(serviceType);
    }

    public virtual IDependencyScope BeginScope()
    {
        return new AppDependencyResolver(_serviceProvider.CreateScope());
    }
    public virtual void Dispose()
    {
        _serviceScope?.Dispose();
    }
}
```


## MVC5 + Web API 2

### 參考組件
Microsoft.Extensions.DependencyInjection.Abstractions

### Startup.cs
Configuration
``` cs
var services = new ServiceCollection();

// TODO: Add Services

// 註冊 Controller
foreach (var type in System.Reflection.Assembly.GetExecutingAssembly().GetExportedTypes().Where(x => !x.IsAbstract && !x.IsGenericTypeDefinition && typeof(IController).IsAssignableFrom(x)))
{
    services.AddTransient(type);
}
// 註冊 ApiController
foreach (var type in Assembly.GetExecutingAssembly().GetExportedTypes().Where(x => !x.IsAbstract && !x.IsGenericTypeDefinition && typeof(IHttpController).IsAssignableFrom(x)))
{
    services.AddTransient(type);
}
// Build ServiceProvider
var serviceProvider = services.BuildServiceProvider();
var resolver = new AppDependencyResolver(serviceProvider);
DependencyResolver.SetResolver(resolver);
GlobalConfiguration.Configuration.DependencyResolver = resolver;

```

### AppDependencyResolver.cs
``` cs
public class AppDependencyResolver : System.Web.Mvc.IDependencyResolver, System.Web.Http.Dependencies.IDependencyResolver
{
    private readonly IServiceProvider _serviceProvider;
    private readonly IServiceScope _serviceScope;
    
    public AppDependencyResolver(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }
    public AppDependencyResolver(IServiceScope serviceScope)
    {
        _serviceScope = serviceScope;
        _serviceProvider = serviceScope.ServiceProvider;
    }

    public object GetService(Type serviceType)
    {
        return _serviceProvider.GetService(serviceType);
    }
    public IEnumerable<object> GetServices(Type serviceType)
    {
        return _serviceProvider.GetServices(serviceType);
    }

    public IDependencyScope BeginScope()
    {
        return new AppDependencyResolver(_serviceProvider.CreateScope());
    }
    public void Dispose()
    {
        _serviceScope?.Dispose();
    }
}
```