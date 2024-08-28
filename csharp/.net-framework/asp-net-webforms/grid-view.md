# GridView

### 將渲染結果匯出至 Excel

以 `RenderControl` 的方式將 `runat="server"` 的元件渲染結果匯出
``` cs
string fileName = "foo.xls";

HttpContext.Current.Response.ContentType = "application/vnd.ms-excel";
HttpContext.Current.Response.AppendHeader("Content-Disposition", "attachment; filename=\"" + HttpUtility.UrlEncode(fileName) + "\"");
HttpContext.Current.Response.Write("<meta http-equiv=Content-Type content=text/html;charset=utf-8>");
using (StringWriter stringWriter = new StringWriter())
using (HtmlTextWriter textWriter = new HtmlTextWriter(stringWriter))
{
    GridView1.RenderControl(textWriter);
    HttpContext.Current.Response.Write(stringWriter.ToString());
}
HttpContext.Current.Response.End();
```

如果出現錯誤訊息 `必須置於有 runat=server 的表單標記之中`
可以覆寫 `VerifyRenderingInServerForm` 把造成異常的控制項排除即可
``` cs
public override void VerifyRenderingInServerForm(Control control)
{
    if (control.ID == "GridView1")
    {
        return;
    }
    base.VerifyRenderingInServerForm(control);
}
```