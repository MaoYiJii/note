# System.Text.Json

序列化

```cs
JsonSerializer.Serialize(obj)
```

反序列化

```cs
JsonSerializer.Deserialize<T>(jsonString);
```

## 從 Newtonsoft.Json

| 功能                   | Newtonsoft.Json                                                | System.Text.Json                                                |
| ---------------------- | -------------------------------------------------------------- | --------------------------------------------------------------- |
| 序列化                 | `JsonConvert.SerializeObject(obj)`                             | `JsonSerializer.Serialize(obj)`                                 |
| 反序列化               | `JsonConvert.DeserializeObject<T>(jsonString)`                 | `JsonSerializer.Deserialize<T>(jsonString)`                     |
| 忽略值為 `null` 的屬性 | `[JsonProperty(NullValueHandling = NullValueHandling.Ignore)]` | `[JsonIgnore(Condition = JsonIgnoreCondition.WhenWritingNull)]` |
| 指定序列化屬性名稱     | `[JsonProperty("foo")]`                                        | `[JsonPropertyName("foo")]`                                     |


## 反序列化動態處理 (不使用強型別)

在 `Newtonsoft.Json` 時會透過 `JToken` `JObject` `JArray` `JValue` 來處理

在 `System.Text.Json` 雖然有對應的 `JsonNode` `JsonObject` `JsonArray` `JsonValue`
但通常會用 `JsonElement` 來處理 (可以參考黑暗執行緒的[這篇](https://blog.darkthread.net/blog/jsonelement/))

### 從 json 取得 JsonElement

``` cs
var doc = JsonDocument.Parse(jsonString);
var element = doc.RootElement;
```

### 判斷是不是陣列
``` cs
if (element.ValueKind == JsonValueKind.Array)
{
}
```

### 判斷是不是物件
``` cs
if (element.ValueKind == JsonValueKind.Object)
{
}
```

### 遍歷物件屬性
``` cs
var props = element.EnumerateObject();
foreach (var prop in props)
{
    // prop.Name: string
    // prop.Value: JsonElement
}
```

### 遍歷陣列
``` cs
var items = element.EnumerateArray();
foreach (var item in items)
{
    // item: JsonElement
}
```


## DataTable

預設 `System.Text.Json` 序列化 `DataTable` 時會將它做為一般物件處理，無法獲取預期的物件陣列結果
可以自行增加 `JsonConverter` 來取得 `DataTable` 預期的序列化結果
``` cs
/// <summary>
/// 讓 <see cref="DataTable"/> 可以序列化為陣列
/// </summary>
public class DataTableJsonConverter : JsonConverter<DataTable>
{
    public override DataTable Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        return JsonDocument.ParseValue(ref reader).RootElement.ToDataTable();
    }

    public override void Write(Utf8JsonWriter writer, DataTable value, JsonSerializerOptions options)
    {
        writer.WriteStartArray();
        foreach (DataRow dr in value.Rows)
        {
            writer.WriteStartObject();
            foreach (DataColumn dc in dr.Table.Columns)
            {
                writer.WritePropertyName(dc.ColumnName);
                JsonSerializer.Serialize(writer, dr[dc], options);
            }
            writer.WriteEndObject();
        }
        writer.WriteEndArray();
    }
}
```

## DataSet

在已經有 `DataTableJsonConverter` 的情況下，再定義一個 `JsonConverter` 來序列化 `DataSet`
``` cs
/// <summary>
/// 讓 <see cref="DataSet"/> 可以序列化為二維陣列
/// </summary>
public class DataSetJsonConverter : JsonConverter<DataSet>
{
    public override DataSet Read(ref Utf8JsonReader reader, Type typeToConvert, JsonSerializerOptions options)
    {
        return JsonDocument.ParseValue(ref reader).RootElement.ToDataSet();
    }

    public override void Write(Utf8JsonWriter writer, DataSet value, JsonSerializerOptions options)
    {
        writer.WriteStartArray();
        foreach (DataTable dt in value.Tables)
        {
            JsonSerializer.Serialize(writer, dt, options);
        }
        writer.WriteEndArray();
    }
}
```

