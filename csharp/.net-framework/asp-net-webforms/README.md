# ASP.NET WebForms

### ClientScript.RegisterStartupScript

在後端可以透過 `ClientScript.RegisterStartupScript` 來設置前端渲染後立即執行的 javascript
``` cs
this.ClientScript.RegisterStartupScript(this.GetType(), "", "alert('foo')", true);
```

執行的 javascript 也可以用來呼叫寫在 `aspx` 的 function
- aspx
  ``` html
  <script>
    function foo() {
      alert('foo');
    }
  </script>
  ```
- aspx.cs
  ``` cs
  this.ClientScript.RegisterStartupScript(this.GetType(), "", "foo()", true);
  ```
  
但是如果一次 PostBack 執行了兩次以上的 `ClientScript.RegisterStartupScript`
從第二次開始就會無法識別 `aspx` 的 function
``` cs
this.ClientScript.RegisterStartupScript(this.GetType(), "", "foo()", true); // 第一次可呼叫寫在 aspx 的 foo
this.ClientScript.RegisterStartupScript(this.GetType(), "", "foo()", true); // 第一次會在 js 執行階段報錯: 無法識別 foo
```