# Umbraco

### Umbraco 後台

* Document Type
  * 頁面資料 (ViewModel)
* Template
  * 版面配置頁 (\_Layout)
  * 部分檢視 (View)
* Content
  * 檢視匹配 (Controller)

### Umbraco Mapping

* Document Type
  * 設定允許配對的 Template
  * 設定 Content 節點允許的子節點 Document Type (Permissions > Allowed child node types)
* Content
  * 設定套用的 Template 以及 Document Type

### Controller 分別

* UmbracoApiController (API)\
  route: `/umbraco/api/{controller}`
* SurfaceController (View & API)\
  route: `/umbraco/surface/{controller}/{action}/{id}`
* RenderMvcController (View)\
  (沒有直接對應的 route，所以無法做 post、ajax，除非自己額外加 route)

### MVC Mapping

#### SurfaceController

建立一個 Controller 繼承 SurfaceController\
在 Controller 的 Action 中以 return PartialView(); 結尾\
在 Template 的 Html 加入 @Html.Action(actionName, controllerName); (將 View 顯示在 Template 中)\
可以當 API 用, 預設的 Route: `/umbraco/Surface/{controllerName}/{actionName}`

#### RenderMvcController

1. 建立一個 Controller 繼承 RenderMvcController
2. controllerName 與對應的 Document Type 名稱相同
3. 加入 Action, actionName 與對應的 Template 名稱相同
4. 建立 Content 選用上述的 Document Type 與 Template 就會對應到 Action
5. 可以在 Action 加上一個 RenderModel 類型的參數來接收 Content 的內容
   * Action 最後 return base.Index(model); 其中 model 的類別必須是 RenderModel
   * Action 最後 return CurrentTemplate(model);
     * 如果 model 的類型是 RenderModel\
       在 Template 加上 `@inherits Umbraco.Web.Mvc.UmbracoTemplatePage`
     * 如果 model 的類型是繼承 RenderModel 的類型\
       在 Template 加上 `@inherits Umbraco.Web.Mvc.UmbracoTemplatePage<T>` 其中 T 是 model 的類型 (TODO: 實作發生錯誤，待驗證)
     * 如果 model 的類型不是繼承 RenderModel 的類型\
       在 Template 加上 `@inherits Umbraco.Web.Mvc.UmbracoViewPage<T>` 其中 T 是 model 的類型

_請不要在開發環境為這個 Action 加上 View，否則結果將不會呈現 Template，而是 View_

#### RenderMvcController (進階)

如果 Document Type 有對應的 controllerName (RenderMvcController) 但沒有 Template 對應的 actionName 則會跑去 Index 這個 Action\
如果沒有特別需求可以不用寫 Index 這個 Action，如果是要讓每個套用 Document Type 的 Content 都先執行一段 Code，則可以覆寫 Index

EX:

```cs
public override ActionResult Index(RenderModel renderModel)
```

最後要 `return base.Index(model);`\
但是要注意如果有 Template 對應的 actionName，那就不會執行 Index

### Developer Coding

#### RenderModel

繼承 RenderModel 的類別必須有一個包含一個參數類型為 IPublishedContent 的建構子

EX:

```cs
public DevelopViewModel(IPublishedContent content) : base(content)
```

#### Controller

Redirect

*   SurfaceController:

    ```cs
    return RedirectToUmbracoPage(id);
    ```
*   RenderMvcController:

    ```cs
    return Redirect(umbraco.library.NiceUrl(id));
    ```

#### Validate

```cs
if (Membership.ValidateUser(name, password))
```

#### Sing in

```cs
FormsAuthentication.SetAuthCookie(name, true);
```

#### Sing out

```cs
FormsAuthentication.SignOut();
```

### Debug (IIS)

將站台架在 IIS 上\
到 Visiual Studio 的偵錯 > 附加至處理序 (顯示所有使用者的處理序) 找到 w3wp.exe 選取後點「附加」開始偵錯

### Debug (Log)

到 `\config\log4net.config` 修改 `log4net/root/priority['value']` (預設為 Info), 改為 Trace (才會寫 Trace 及 Debug 的 Log)

列出訊息的方法

```cs
Logger.Debug(this.GetType(), message);
LogHelper.Debug(this.GetType(), message);
```

Log 檔會放在 `\App_Data\Logs` 底下

### Notes

#### Debug

RenderModel 無法直接轉 json (屬性有 loop), 要查看 Content 的參數可以使用這段抓出參數的 json

```cs
JsonConvert.SerializeObject(renderModel.Content.Properties.Select(x => new { name = x.PropertyTypeAlias, value = x.DataValue }))
```

### Umbraco

#### DictionaryItem

```cs
var value = umbraco.library.GetDictionaryItem(key);
umbraco.cms.businesslogic.Dictionary.DictionaryItem.hasKey(key)
umbraco.cms.businesslogic.Dictionary.DictionaryItem.addKey("CWS", "");
int dictionaryID = umbraco.cms.businesslogic.Dictionary.DictionaryItem.addKey(key, defaultText,"CWS");
var dictionaryItem = new umbraco.cms.businesslogic.Dictionary.DictionaryItem(dictionaryID);
dictionaryItem.setValue(l.id, l.CultureAlias.Substring(0, 2).ToLowerInvariant() == "en" ? defaultText : string.Format("[{0}]", key));
dictionaryItem.Save();
```

#### View

Begin Form

```cshtml
using (Html.BeginUmbracoForm<AuthSurfaceController>("HandleRegister"))
```

Link

```cshtml
@Html.ActionLink("Logout", "Logout", "AuthSurface")
```

Is Login

```cshtml
@Members.IsLoggedIn();
```

Get Login Member

```cshtml
@Members.GetCurrentMemberProfileModel();
```

#### Member

[https://our.umbraco.org/documentation/reference/management/services/memberservice](https://our.umbraco.org/documentation/reference/management/services/memberservice)

```cs
if (Membership.ValidateUser(model.EmailAddress, model.Password))
```

#### Membership

web.config 的 allowManuallyChangingPassword 要設為 true [https://our.umbraco.org/Documentation/Reference/Querying/MemberShipHelper/](https://our.umbraco.org/Documentation/Reference/Querying/MemberShipHelper/)

#### Surface Controller

預設的 Route: `/umbraco/Surface/{controllerName}/{actionName}`

### 設定連接字串

```xml
<add name="umbracoDbDSN" connectionString="server=DESKTOP-RO8TCIK\SQLEXPRESS;database=MyDatabase;user id=sa;password='1234'" providerName="System.Data.SqlClient" />
```
