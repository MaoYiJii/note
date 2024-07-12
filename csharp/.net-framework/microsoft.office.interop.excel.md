# Microsoft.Office.Interop.Excel

### 使用 Microsoft.Office.Interop.Excel

從 Nuget 安裝 `Microsoft.Office.Interop.Excel` 套件
套件版本為 `15` 開頭時，電腦必須安裝 `Office 2013` 以上的版本

### 系統找不到指定的檔案

如果電腦有安裝 `Office 2013` 以上的版本
但在執行階段會出現 `系統找不到指定的檔案` 的錯誤訊息
可以將 `C:\Windows\assembly\GAC_MSIL\office\15.0.0.0__71e9bce111e9429c\OFFICE.DLL` 加入至 `啟動專案` 的參考
> 注意是要加在 `啟動專案` 而不是參考 `Microsoft.Office.Interop.Excel` 套件的專案


### 開啟 Workbook 與 Worksheet
``` cs
using Excel = Microsoft.Office.Interop.Excel;

Excel.Application application = new Excel.Application();
Excel.Workbook workbook = application.Workbooks.Open(Options.ImportFilePath);
try
{
    // sheet 可以是名稱也可以是從 1 開始的數字
    Excel.Worksheet worksheet = (Excel.Worksheet)workbook.Worksheets[sheet];
    // TODO: 讀取 worksheet 的內容
    
}
finally
{
    workbook.Close();
    application.Quit();
}
```

### 建立 Workbook 與 Worksheet
``` cs
Excel.Application application = new Excel.Application();
Excel.Workbook workbook = application.Workbooks.Add();
try
{
    // 把新增的 Excel 刪除 Sheet 直到剩一個 (Windows 限制最少要剩一個)
    while (workbook.Worksheets.Count > 1)
    {
        ((Excel.Worksheet)workbook.Worksheets[2]).Delete();
    }

    // 新增一個 Worksheet 放到最後面
    Excel.Worksheet worksheet = (Excel.Worksheet)workbook.Worksheets.Add(After: workbook.Sheets[workbook.Sheets.Count]);
    // TODO: 寫入內容到 worksheet
    // 需要第二個以上的 Worksheet 重複一次即可
    // worksheet = (Excel.Worksheet)workbook.Worksheets.Add(After: workbook.Sheets[workbook.Sheets.Count]);

    // 把原先僅剩的 Sheet 移除
    ((Excel.Worksheet)workbook.Worksheets[1]).Delete();
    // 讓 Excel 打開顯示第一個 Sheet
    ((Excel.Worksheet)workbook.Worksheets[1]).Activate();
    // 儲存成檔案
    workbook.SaveAs(Options.ExportFilePath);
}
finally
{
    workbook.Close();
    application.Quit();
}
```

### 獲取儲存格

單一欄位
``` cs
// rowNumber 與 colNumber 都是從 1 開始的數字
Excel.Range range = (Excel.Range)worksheet.Cells[rowNumber, colNumber];
```
選取範圍
``` cs
Excel.Range range = (Excel.Range)worksheet.Range["A1", "B2"];
```
所有有內容的範圍
``` cs
Excel.Range range = worksheet.UsedRange;
```

### 選取範圍的欄位資訊

``` cs
// 選取範圍第一列的編號
range.Row
// 選取範圍第一欄的編號
range.Column
// 選取範圍總共幾列
range.Rows.Count
// 選取範圍總共幾欄
range.Columns.Count
```

### 儲存格內容

獲取內容
``` cs
var s = Convert.ToString(range.Value);
```
設置內容
``` cs
range.Value = value;
```

### 儲存格顏色

設置文字顏色
``` cs
range.Font.Color = RGB(155, 0, 128);
```
設置背景顏色
``` cs
range.Interior.Color = RGB(204, 255, 204);
```
RBG 轉顏色代碼
``` cs
private int RGB(int red, int green, int blue)
{
    return ColorTranslator.ToOle(Color.FromArgb(red, green, blue));
}
```
``` cs
private int RGB(int red, int green, int blue)
{
    return red + green * 256 + blue * 256 * 256;
}
```

### 字型與大小
``` cs
// 字型
range.Font.Name = "微軟正黑體";
// 大小
range.Font.Size = 10;
// 粗體
range.Font.Bold = true;
```

### 對齊

水平置中
``` cs
range.HorizontalAlignment = Excel.XlHAlign.xlHAlignCenter;
```
垂直置中
``` cs
range.VerticalAlignment = Excel.XlVAlign.xlVAlignCenter;
```

### 合併儲存格

合併儲存格
``` cs
range.Merge();
```
取消合併儲存格
``` cs
range.UnMerge();
```
取得合併儲存格的大小
``` cs
// 取得總共幾列
range.MergeArea.Rows.Count
// 取得總共幾欄
range.MergeArea.Columns.Count
```

### 框線

``` cs
range.Cells.Borders.LineStyle = Excel.XlLineStyle.xlContinuous;
```

### 超連結

``` cs
hyperlink = (Excel.Hyperlink)worksheet.Hyperlinks.Add(
    // 超連結套用的位置
    worksheet.Cells[1, 1],
    // 超連結目標在同一個檔案內不用給值
    "",
    // 超連結位置
    $"{sheetName}!A1",
    // 超連結文字
    TextToDisplay: "超連結文字");
// 套用超連結，儲存格的設定都要以超連結的 Range 來設定
range = hyperlink.Range;
```