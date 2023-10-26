# Razor Pages

#### 取得 HtmlHelper

```cs
var htmlHelper = this.Context.RequestServices.GetRequiredService<IHtmlHelper>();
(htmlHelper as IViewContextAware).Contextualize(ViewContext);
```

#### 取得 UrlHelper

```cs
var urlHelperFactory = this.Context.RequestServices.GetRequiredService<IUrlHelperFactory>();
var urlHelper = urlHelperFactory.GetUrlHelper(ViewContext);
```
