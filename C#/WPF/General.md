### 選擇資料夾路徑的對話框
編輯專案檔，加入 UseWindowsForms
``` xml
<UseWindowsForms>true</UseWindowsForms>
```
開啟對話框
``` cs
var dialog = new System.Windows.Forms.FolderBrowserDialog();
if (dialog.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    // dialog.SelectedPath
}
```