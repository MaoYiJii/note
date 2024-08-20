# URL

獲取以 `/` 開頭的本地端 Url
``` cs
ResolveUrl("WebForm1.aspx")  // => /{virtualPath}/{path}/WebForm1.aspx
ResolveUrl("/WebForm1.aspx") // => /{virtualPath}/WebForm1.aspx
```