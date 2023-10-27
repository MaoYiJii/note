# Blazor

### 頁面載入時執行

**這會在 View 跑過一遍之後才執行，所以有物件在這個方法才取到值的，要判斷是否為 null**

```razor
@code {
    protected override async Task OnInitializedAsync()
    {
        // TODO:
    }
}
```

### 表單及驗證

[https://docs.microsoft.com/zh-tw/aspnet/core/blazor/forms-validation?view=aspnetcore-5.0](https://docs.microsoft.com/zh-tw/aspnet/core/blazor/forms-validation?view=aspnetcore-5.0)

### Components (多個 .razor 組合)

[https://docs.microsoft.com/zh-tw/aspnet/core/blazor/components/?view=aspnetcore-5.0](https://docs.microsoft.com/zh-tw/aspnet/core/blazor/components/?view=aspnetcore-5.0)

[https://docs.microsoft.com/zh-tw/aspnet/core/blazor/advanced-scenarios?view=aspnetcore-5.0](https://docs.microsoft.com/zh-tw/aspnet/core/blazor/advanced-scenarios?view=aspnetcore-5.0)

### 加入 ApiController

在 Startup.cs 找到這段加入 MapControllers

app.UseEndpoints(endpoints => { endpoints.MapControllers(); endpoints.MapBlazorHub(); endpoints.MapFallbackToPage("/\_Host"); });

### Runtime Compile

_實作未成功_

[https://github.com/dotnet/aspnetcore/issues/5456](https://github.com/dotnet/aspnetcore/issues/5456)

[https://stackoverflow.com/questions/40152854/how-to-watch-for-file-changes-dotnet-watch-with-visual-studio-asp-net-core](https://stackoverflow.com/questions/40152854/how-to-watch-for-file-changes-dotnet-watch-with-visual-studio-asp-net-core)

### 很多文件的網站

[https://blazor-university.com/](https://blazor-university.com/)
