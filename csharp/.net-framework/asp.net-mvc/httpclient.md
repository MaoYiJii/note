# HttpClient

#### HttpContext.Current.Request.ServerVariables\["HTTP\_USER\_AGENT"] 為 null

在 Server 端會取這個值去判斷客戶端是電腦或手機，如果沒判斷 null 就要在 HttpClient 給他一個值

```cs
httpClient.DefaultRequestHeaders.UserAgent.TryParseAdd("MyApp");
```
