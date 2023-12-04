# NPOI

## 匯入

### 讀取 Workbook

`WorkbookFactory.Create` 會根據檔案來源是 `.xls` 或 `.xlsx` 建立 `HSSFWorkbook` 或 `XSSFWorkbook`
``` cs
IWorkbook workbook = WorkbookFactory.Create(stream);
```

### 讀取 Sheet

以名稱取得 sheet
``` cs
var sheet = workbook.GetSheet(name);
```
以索引取得 sheet
``` cs
var sheet = workbook.GetSheetAt(index);
```

### 讀取資料

以 `ToString` 取得資料的字串結果 
``` cs
var row = sheet.GetRow(rowIndex);
var cell = row.GetCell(columnIndex);
var value = cell?.ToString();
```

根據 `CellType` 來取得資料
``` cs
object ResolveCellType(ICell cell, CellType cellType)
{
    switch (cellType)
    {
        case CellType.Numeric:
            if (DateUtil.IsCellDateFormatted(cell))
            {
                return cell.DateCellValue;
            }
            return cell.NumericCellValue;
        case CellType.String:
            return cell.StringCellValue;
        case CellType.Formula:
            return ResolveCellType(cell, cell.CachedFormulaResultType);
        case CellType.Blank:
            return null;
        case CellType.Boolean:
            return cell.BooleanCellValue;
        case CellType.Error:
            return ErrorEval.GetText(cell.ErrorCellValue);
        default:
            return cell?.ToString();
    }
}
```

### 讀取範圍

可以透過 `sheet.LastRowNum` 來得知 Sheet 最後一個列的索引
可以透過 `row.LastCellNum` 來得知 Row 最後一欄的索引
*雖然屬性名稱結尾為 Num，實際是以 0 起始的索引值*

遍歷列
``` cs
for (var rowIndex = 0; rowIndex <= sheet.LastRowNum; rowIndex++)
{
    var row = sheet.GetRow(rowIndex);

}
```
遍歷欄
``` cs
for (var columnIndex = 0; columnIndex <= row.LastCellNum; columnIndex++)
{
    var cell = row.GetCell(columnIndex);
    
}
```


## 匯出

### 建立 Workbook

建立新的 Workbook
``` cs
IWorkbook workbook = new XSSFWorkbook();
```
- HSSFWorkbook: 用於 `.xls`
- XSSFWorkbook: 用於 `.xlsx`
- SXSSFWorkbook: 用於處理大量資料的 `.xlsx` (著重在資料處理，樣式之類與資料無直接關係的部分功能會不支援)

### 建立 Sheet
``` cs
var sheet = workbook.CreateSheet();
```

### 寫入資料
``` cs
var row = sheet.CreateRow(rowIndex);
var cell = sheet.CreateCell(columnIndex);
cell.SetCellValue(value);
```

### 寫入日期資料
```
var dataFormat = workbook.CreateDataFormat();
var cellStyle = workbook.CreateCellStyle();
cellStyle.DataFormat = dataFormat.GetFormat("yyyy/MM/dd");
cell.SetCellValue(dateTime);
cell.CellStyle = cellStyle;
```
*`dataFormat` 可以重複使用*

### 寫入公式
``` cs
cell.SetCellFormula("A1+B1");
```

### 設定文字字型與大小
``` cs
var cellStyle = workbook.CreateCellStyle();
var font = workbook.CreateFont();
font.FontName = "微軟正黑體";
font.FontHeightInPoints = 12;
cellStyle.SetFont(font);
cell.CellStyle = cellStyle;
```

### 設定文字顏色
``` cs
var cellStyle = workbook.CreateCellStyle();
var font = workbook.CreateFont();
font.Color = IndexedColors.Red.Index;
cellStyle.SetFont(font);
cell.CellStyle = cellStyle;
```

### 設定背景色
``` cs
var cellStyle = workbook.CreateCellStyle();
cellStyle.FillPattern = FillPattern.SolidForeground;
cellStyle.FillForegroundColor = IndexedColors.Red.Index;
cell.CellStyle = cellStyle;
```

### 單一欄位套用多個文字顏色
``` cs
// 建立紅色字型
var fontRed = workbook.CreateFont();
fontRed.Color = IndexedColors.Red.Index;
// 建立藍色字型
var fontBlue = workbook.CreateFont();
fontBlue.Color = IndexedColors.Blue.Index;
// 建立 IRichTextString
var creationHelper = workbook.GetCreationHelper();
var richText = creationHelper.CreateRichTextString("FooBar");
// 套用顏色
richText.ApplyFont(0, 3, fontRed);
richText.ApplyFont(3, fontBlue);
```
*`SXSSFWorkbook` 會讓 `IRichTextString` 無作用，可改用 `XSSFWorkbook`*


### 讀取 Workbook
``` cs
IWorkbook workbook = WorkbookFactory.Create(stream);
```
如果要指定 `HSSFWorkbook` `XSSFWorkbook` 或 `SXSSFWorkbook` 可以直接 `new`
``` cs
IWorkbook workbook = new SXSSFWorkbook(stream);
```
用 `SXSSFWorkbook` 會無法取得已經存在的資料列，而無法覆寫已存在的資料
``` cs
// 使用 SXSSFWorkbook 的情況下，如果 rowIndex 這一列已經有資料，得到的 row 會是 null
var row = sheet.GetRow(rowIndex);
```

### 讀取 Sheet

以名稱取得 sheet
``` cs
var sheet = workbook.GetSheet(name);
```
以索引取得 sheet
``` cs
var sheet = workbook.GetSheetAt(index);
```