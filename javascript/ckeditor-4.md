# CKEditor 4

### CKEditor 無法作用在 bootstrap modal 的解法

``` js
$.fn.modal.Constructor.prototype._enforceFocus = function () {
    var modalElement = this;
    $(document).on('focusin.modal', function (e) {
        if (modalElement !== e.target
            && !$(modalElement).has(e.target).length
            && $(e.target.parentNode).hasClass('cke_contents cke_reset')) {
            $(modalElement).focus();
        }
    })
};
```

### 初始化

- 使用 id  
  ``` html
  <textarea id="foo"></textarea>
  <script>
    CKEDITOR.replace("foo");
  </script>
  ```
- 使用 name
  ``` html
  <textarea name="foo"></textarea>
  <script>
    CKEDITOR.replace("foo");
  </script>
  ```
- 使用 HTMLElement
  ``` html
  <textarea id="foo"></textarea>
  <script>
    CKEDITOR.replace(document.getElementById("foo"));
  </script>
  ```
  
### 獲取執行個體

- 在初始化的時候獲得
  ``` html
  <textarea id="foo"></textarea>
  <script>
    var cke = CKEDITOR.replace("foo");
  </script>
  ```
- 透過 id 或 name 獲得
  ``` html
  <textarea id="foo"></textarea>
  <textarea name="bar"></textarea>
  <script>
    var ckeFoo = CKEDITOR.instances.foo;
    var ckeBar = CKEDITOR.instances.bar;
  </script>
  ```
  
### 存取內容

- 讀取內容
  ``` js
  var text = cke.getData();
  ```
- 設置內容
  ``` js
  cke.setData(text);
  ```
  
### 其他設定

- 初始化設定
  ``` js
  CKEDITOR.replace(id, {
    // 高度
    height: "270",
    // 唯讀
    readOnly: true
  });
  ```
- 改變唯讀狀態
  ``` js
  cke.setReadOnly(true);
  ```
  
### 事件

- 初始化完成時
  ``` js
  cke.on("instanceReady", function (e) {
    console.debug("instanceReady");
  });
  ```
- 內容改變時
  ``` js
  cke.on("change", function (e) {
    console.debug("change");
  });
  ```
  > 這個事件沒有想像中的好用，很多情境會沒有觸發 (例如：以原始碼編輯、貼上內容..等等)
  
### 上傳圖片

- 前端
  ``` js
  CKEDITOR.replace(id, {
    filebrowserUploadMethod: "form",
    filebrowserImageUploadUrl: "/CKEditor/ImageUpload?type=Images"
  });
  ```
- 後端 (ASP.NET MVC)
  ``` cs
  [HttpPost]
  public async Task<ContentResult> ImageUpload(string CKEditorFuncNum, IFormFile upload)
  {
      string? imageUrl = null;
      string? message = null;
      if (upload != null && upload.Length > 0)
      {
          byte[] bytes;
          using (MemoryStream memoryStream = new MemoryStream())
          {
              upload.CopyTo(memoryStream);
              bytes = memoryStream.ToArray();
          }
          // TODO: save file by 'upload.FileName' and 'bytes'
          // TODO: set url to 'imageUrl' for view file
      }
      if (imageUrl == null)
      {
          message = "上傳檔案失敗";
      }
      if (string.IsNullOrEmpty(CKEditorFuncNum))
      {
          // 如果 CKEditorFuncNum 沒有值，代表不是經由上傳功能來的
          // 回傳格式參考：https://ckeditor.com/docs/ckeditor4/latest/guide/dev_file_upload.html#response-file-uploaded-successfully
          return Content(JsonSerializer.Serialize(new
          {
              uploaded = 1,
              fileName = upload?.FileName,
              url = imageUrl
          }), "application/json", Encoding.UTF8);
      }
      // 有 CKEditorFuncNum 的回傳方式參考 https://ckeditor.com/docs/ckeditor4/latest/guide/dev_file_browser_api.html#passing-the-url-of-the-selected-file
      return Content($"<html><body><script>window.parent.CKEDITOR.tools.callFunction({CKEditorFuncNum}, {JsonSerializer.Serialize(imageUrl)}, {JsonSerializer.Serialize(message)});</script></body></html>", "text/html", Encoding.UTF8);
  }
  ```
  
### 在 ASP.NET Web Forms 上使用

- 如果對資安要求沒那麼高，可以在 aspx 的 Page 標籤加上 `ValidateRequest="false"`
- 如果不能放 `ValidateRequest="false"`
  可以在 `Page_Load` 逐一對所有 CKEditor 的 `TextBox` 來編碼與解碼
  ``` cs
  protected void Page_Load(object sender, EventArgs e)
  {
      // 讓 CKEditor 的值在 Post 之前做 HtmlEncode
      String csname = "OnSubmitScript";
      Type cstype = this.GetType();
      ClientScriptManager cs = Page.ClientScript;
      if (!cs.IsOnSubmitStatementRegistered(cstype, csname))
      {
          String cstext = string.Format(@"
              var cke1 = CKEDITOR.instances['{0}'];
              cke1.setData($(""<div>"").text(cke1.getData()).html());",
              TextBox1.ClientID);
          cs.RegisterOnSubmitStatement(cstype, csname, cstext);
      }

      // 讓 CKEditor 的值在 Post 之後做 HtmlDecode
      if (Page.IsPostBack)
      {
          TextBox1.Text = HttpUtility.HtmlDecode(HttpUtility.HtmlDecode(HttpUtility.HtmlEncode(TextBox1.Text)));
      }
  }
  ```
