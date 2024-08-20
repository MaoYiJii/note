# Button

### `OnClientClick` 與 `OnClick`

在 `<asp:Button>` 這個元件有 `OnClientClick` 與 `OnClick` 兩個屬性
其中 `OnClientClick` 是前端的觸發事件，內容是 javascript，而 `OnClick` 是後端的觸發事件，內容是對應 C# 的方法
執行順序是先執行 `OnClientClick` 再執行 `OnClick`

#### 前端驗證

如果 `OnClientClick` 有 `return` 且返回的值不為 `true`，則 `OnClick` 的事件就不會被執行
``` aspx
<!-- Button1_Click 會被執行 -->
<asp:Button ID="Button1" runat="server" Text="Button" OnClientClick="return true;" OnClick="Button1_Click" />

<!-- Button2_Click 不會被執行 -->
<asp:Button ID="Button2" runat="server" Text="Button" OnClientClick="return false;" OnClick="Button2_Click" />
```

需要注意如果 `OnClientClick` 是呼叫一個 javascript 的 function，在呼叫的地方也需要加上 `return`
``` aspx
<script>
function foo() {
    return false;
}
</script>

<!-- Button1_Click 會被執行 -->
<asp:Button ID="Button1" runat="server" Text="Button" OnClientClick="foo();" OnClick="Button1_Click" />

<!-- Button2_Click 不會被執行 -->
<asp:Button ID="Button1" runat="server" Text="Button" OnClientClick="return foo();" OnClick="Button2_Click" />
```

如果按鈕元件上有設置 `UseSubmitBehavior` 屬性為 `False`，則 `OnClientClick` 只要有 `return` 都不會執行 `OnClick` 的事件
``` aspx
<script>
function bar() {
    // do something
}
</script>

<!-- Button1_Click 不會被執行 -->
<asp:Button ID="Button1" runat="server" Text="Button" OnClientClick="return true;" OnClick="Button1_Click" UseSubmitBehavior="False" />

<!-- Button2_Click 不會被執行 -->
<asp:Button ID="Button2" runat="server" Text="Button" OnClientClick="return false;" OnClick="Button2_Click" UseSubmitBehavior="False" />

<!-- Button3_Click 會被執行 -->
<asp:Button ID="Button3" runat="server" Text="Button" OnClientClick="bar();" OnClick="Button3_Click" UseSubmitBehavior="False" />
```
