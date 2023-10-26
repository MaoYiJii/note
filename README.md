# General

### byte\[] to Stream

```cs
var stream = new MemoryStream(bytes)
```

### 解析 CSV 檔

參考組件 Microsoft.VisualBasic 參考命名空間 Microsoft.VisualBasic.FileIO

```cs
using (TextFieldParser parser = new TextFieldParser(new System.IO.StringReader(text)) { HasFieldsEnclosedInQuotes = true })
{
    parser.SetDelimiters(",");
    while (!parser.EndOfData)
    {
        string[] fields = parser.ReadFields();
        foreach (var field in fields)
        {
            // TODO:
        }
    }
}
```

### 讀取 Excel (NPOI)

```cs
IWorkbook workbook;
using (var fileStream = new FileStream(request.filePath, FileMode.Open, FileAccess.ReadWrite))
{
    workbook = WorkbookFactory.Create(fileStream);
}
ISheet sheet = workbook.GetSheet("SheetName");
// i = 1 是從第二行開始讀取
for (var i = 1; i <= sheet.LastRowNum; i++)
{
    var row = sheet.GetRow(i);
    // 知道幾列的話直接寫固定的 index
    string s1 = row.GetCell(0)?.ToString().Trim();
    string s2 = row.GetCell(1)?.ToString().Trim();
    string s3 = row.GetCell(2)?.ToString().Trim();
    // 不知道總共幾列的話，可以用 row.LastCellNum
    for (var j = 0; j <= row.LastCellNum; j++)
    {
        string s = row.GetCell(j)?.ToString().Trim();
    }
}
```

### 把參數化的 SQL 轉成純字串的 SQL

```cs
private string ToConstSql(string sql, Dictionary<string, object> param)
{
    foreach (var p in param)
    {
        sql = sql.Replace($"@{p.Key.TrimStart('@')}", ToConstValue(p.Value));
    }
    return sql;
}

private string ToConstValue(object paramValue)
{
    if (paramValue == null)
    {
        return "NULL";
    }
    if (paramValue is string s)
    {
        return $"'{s.Replace("'", "''")}'";
    }
    if (paramValue is IEnumerable en)
    {
        if (en.GetEnumerator().MoveNext())
        {
            List<string> ens = new List<string>();
            foreach (var e in en)
            {
                ens.Add(ToConstValue(e));
            }
            return $"({string.Join(", ", ens)})";
        }
        return "(NULL)";
    }
    if (paramValue is bool b)
    {
        return b ? "1" : "0";
    }
    if (paramValue is Guid g)
    {
        return $"'{g}'";
    }
    if (paramValue is DateTime dt)
    {
        return $"'{dt:yyyy/MM/dd HH:mm:ss}'";
    }
    if (paramValue is Enum @enum)
    {
        return Convert.ChangeType(@enum, @enum.GetTypeCode()).ToString();
    }
    return paramValue.ToString();
}
```

### 壓縮檔案

```cs
// 儲存檔案至暫存目錄
string guid = Guid.NewGuid().ToString();
string fileDirectory = Path.Combine(Path.GetTempPath(), guid);
string filePath = Path.Combine(fileDirectory, fileName);
File.WriteAllBytes(filePath, bytes);
// 壓縮檔案
byte[] resultBytes;
using (ZipFile zip = new ZipFile())
{
    // 設定密碼
    zip.Password = emp_idn;
    zip.AddFile(filePath, "");
    using (var memoryStream = new MemoryStream())
    {
        zip.Save(memoryStream);
        resultBytes = memoryStream.ToArray();
    }
}
// 刪除檔案避免佔用空間
if (Directory.Exists(fileDirectory))
{
    Directory.Delete(fileDirectory, true);
}
// 重新命名為 *.zip 檔
int extIndex = fileName.LastIndexOf(".");
fileName = $"{fileName.Substring(0, extIndex)}.zip";
// 回傳壓縮檔的 bytes
return resultBytes;
```

### Regular (Regex)

#### 用正規表示法驗證字串

```cs
Regex.IsMatch(input, pattern);
Regex.IsMatch(input, pattern, RegexOptions.IgnoreCase);
```

#### 用正規表示法解析字串

解析

```cs
Regex pattern = new Regex(@"<!--\s*#include\s*file\s*=\s*""(?<path>\S+)""\s*-->");
if (pattern.IsMatch(text))
{
    Match match = pattern.Match(text);
    string path = match.Groups["path"].Value;
}
```

#### 多次匹配

```cs
Regex pattern = new Regex(@"<!--\s*#include\s*file\s*=\s*""(?<path>([^""])+)""\s*-->");
if (pattern.IsMatch(text))
{
    foreach (Match match in pattern.Matches(text))
    {
        string path = match.Groups["path"].Value;
    }
}
```

#### 用正規表示法替換文字

用 Dictionary 替換掉所有 \[Key] 的文字

```cs
Regex.Replace(input, @"\[([^\]]*)\]", match =>
{
    string key = match.Groups[1].Value;
    dictionary.TryGetValue(key, out string value);
    return value;
});
```

### 簡體與繁體互相轉換

```cs
const int LocaleSystemDefault = 0x0800;
const int LcmapSimplifiedChinese = 0x02000000;
const int LcmapTraditionalChinese = 0x04000000;

[DllImport("kernel32", CharSet = CharSet.Auto, SetLastError = true)]
private static extern int LCMapString(int locale, int dwMapFlags, string lpSrcStr, int cchSrc, [Out] string lpDestStr, int cchDest);

public string ToSimplified(string source)
{
    int length = source.Length;
    var target = new string(new char[length]);
    LCMapString(LocaleSystemDefault, LcmapSimplifiedChinese, source, length, target, length);
    return target;
}

public string ToTraditional(string source)
{
    int length = source.Length;
    var target = new string(new char[length]);
    LCMapString(LocaleSystemDefault, LcmapTraditionalChinese, source, length, target, length);
    return target;
}
```

### 在不完整 (非 http 開頭) 的 Url 加上 QueryString

```cs
if (data != null)
{
    List<KeyValuePair<string, string>> nameValues = new List<KeyValuePair<string, string>>();
    if (url.IndexOf('?') >= 0)
    {
        foreach (var nameValue in url.Substring(url.IndexOf('?') + 1)
            .Split('&')
            .Select(x => x.Split('='))
            .Select(x => new KeyValuePair<string, string>(x[0], x.ElementAtOrDefault(1))))
        {
            nameValues.Add(nameValue);
        }
    }
    foreach (var prop in data.GetType().GetProperties())
    {
        var propValue = prop.GetValue(data);
        if (propValue is IEnumerable && !(propValue is string))
        {
            foreach (var propUnit in propValue as IEnumerable)
            {
                nameValues.Add(new KeyValuePair<string, string>(prop.Name, HttpUtility.UrlEncode(propUnit?.ToString())));
            }
        }
        else
        {
            nameValues.Add(new KeyValuePair<string, string>(prop.Name, HttpUtility.UrlEncode(propValue?.ToString())));
        }
    }
    if (url.IndexOf('?') >= 0)
    {
        url = $"{url.Substring(0, url.IndexOf('?'))}?{string.Join("&", nameValues.Select(x => $"{x.Key}={x.Value}"))}";
    }
    else
    {
        url = $"{url}?{string.Join("&", nameValues.Select(x => $"{x.Key}={x.Value}"))}";
    }
}
```

### Struct

#### 實值型別+隱含轉換

```cs
public struct ResourceLinksType
{
    private string Value { get; }

    public static ResourceLinksType InLink => "inLink";
    public static ResourceLinksType OutLink => "outLink";

    private ResourceLinksType(string value)
    {
        Value = value;
    }

    public static implicit operator ResourceLinksType(string value)
    {
        return new ResourceLinksType(value);
    }

    public static implicit operator string(ResourceLinksType type)
    {
        return type.Value;
    }
}
```
