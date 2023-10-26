# System.Text.Json

序列化

```cs
JsonSerializer.Serialize(obj)
```

反序列化

```cs
JsonSerializer.Deserialize<T>(jsonString);
```

忽略 null 的屬性

```cs
[JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
```

```cs
[JsonIgnore(Condition = JsonIgnoreCondition.WhenWritingNull)]
```

\[JsonProperty("Wind")]

\[JsonPropertyName("Wind")]

https://blog.darkthread.net/blog/jsonelement/

IHostingEnvironment 改為 IWebHostEnvironment

compiled
