# General

## Terminal

### WPF (.NET 6)


建立 WPF 專案
```
dotnet new wpf  -n DbSchemaExport -f net6.0
```

加入 Nuget 套件
```
dotnet add package MediatR --version 12.1.1
dotnet add package Microsoft.Extensions.DependencyInjection --version 6.0.1
dotnet add package Microsoft.Web.WebView2 --version 1.0.2365.46
```

建立新的 cs 檔案
```
dotnet new class -n RouteConfig
dotnet new class -n Route
dotnet new class -n RouteCollection
dotnet new class -n GetDatabaseList -o Handlers
```

執行 WPF 專案
```
dotnet run
```


### Vue 3

建立 Vue 專案
```
vue create DbSchemaExportUI
npm install axios
npm install bootstrap
npm install -D @types/bootstrap
npm install lodash
npm install -D @types/lodash
```


執行 Vue 專案
```
npm run serve
```


