# SelfHost

#### 監聽

加入套件 Microsoft.AspNet.WebApi.SelfHost

```cs
public HttpSelfHostServer HostServer { get; set; }

/// <summary>
/// 開始監聽
/// </summary>
private void Button_Start_Click(object sender, RoutedEventArgs e)
{
    if (HostServer == null)
    {
        var config = new HttpSelfHostConfiguration("http://localhost:60123");
        config.Routes.MapHttpRoute("API", "{controller}/{action}/{id}", new { id = RouteParameter.Optional });
        HostServer = new HttpSelfHostServer(config);
        HostServer.OpenAsync().Wait();
    }
}

/// <summary>
/// 結束監聽
/// </summary>
private void Button_Stop_Click(object sender, RoutedEventArgs e)
{
    if (HostServer != null)
    {
        HostServer.CloseAsync().Wait();
        HostServer.Dispose();
        HostServer = null;
    }
}
```

#### 建立 Controller

```cs
public class HomeController : ApiController
{
    [HttpGet]
    public IHttpActionResult Test()
    {
        return Ok("Hello World");
    }
}
```
