# AvalonEditor

### 讓編輯器有搜尋功能

```cs
private void BindableAvalonEditor_Initialized(object sender, EventArgs e)
{
    var editor = sender as BindableAvalonEditor;
    SearchPanel.Install(editor);
}
```

### 註冊高亮語法

```cs
using (var stream = new System.IO.MemoryStream(WpfApp15.Properties.Resources.sql))
{
    using (var reader = new System.Xml.XmlTextReader(stream))
    {
        ICSharpCode.AvalonEdit.Highlighting.HighlightingManager.Instance.RegisterHighlighting("SQL", new string[0],
            ICSharpCode.AvalonEdit.Highlighting.Xshd.HighlightingLoader.Load(reader,
                ICSharpCode.AvalonEdit.Highlighting.HighlightingManager.Instance));
    }
}
```
