# Entity Framework Core

### 3.0 以後的變更

[https://docs.microsoft.com/zh-tw/ef/core/what-is-new/ef-core-3.x/breaking-changes](https://docs.microsoft.com/zh-tw/ef/core/what-is-new/ef-core-3.x/breaking-changes)

### AsNoTracking

要以物件傳回的 Model 必須加上 .AsNoTracking()，否則在 Query 時可能會抓到記憶體中的未儲存資料造成非預期的錯誤

### 在 Migration 裡面用 DbContext

[https://stackoverflow.com/questions/42644253/can-i-inject-dependency-into-migration-using-ef-core-code-first-migrations](https://stackoverflow.com/questions/42644253/can-i-inject-dependency-into-migration-using-ef-core-code-first-migrations)

### 在 PowerShell 指定套用環境變數

ex. 套用 Release 環境執行 Update-Database

```
$env:ASPNETCORE_ENVIRONMENT='Production'
Update-Database
```

## 基本指令 (Visual Studio)

建立第一個 migration
```
Add-Migration InitialCreate -OutputDir Data\Migrations
```

建立第二個之後的 migration
```
Add-Migration 001
```

## 基本指令 (CLI)

安裝 dotnet-ef
```
dotnet tool install --global dotnet-ef
```

建立第一個 migration
```
dotnet ef migrations add InitialCreate --output-dir "Data\Migrations"
```

建立第二個之後的 migration
```
dotnet ef migrations add 001
```

將 migration 更新到資料庫
```
dotnet ef database update
```