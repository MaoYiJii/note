# Newtonsoft.Json

### JsonConverter

#### 將 IEnumerable 轉成 Dictionary 的格式輸出

```cs
public class CollectionDictionaryConverter<T> : JsonConverter
{
    public override bool CanRead => true;

    public override bool CanWrite => true;

    public override bool CanConvert(Type objectType) => typeof(IEnumerable<T>).IsAssignableFrom(objectType);

    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)
    {
        var keyProp = typeof(T).GetProperties().First(x => x.GetCustomAttribute<CollectionDictionaryKeyAttribute>() != null);
        var keyValues = serializer.Deserialize<Dictionary<string, T>>(reader);
        foreach (var keyValue in keyValues)
        {
            keyProp.SetValue(keyValue.Value, keyValue.Key);
        }
        var enumerable = keyValues.Select(x => x.Value).ToList();
        if (objectType.IsAssignableFrom(typeof(List<T>)))
        {
            return enumerable;
        }
        return Activator.CreateInstance(objectType, new object[] { enumerable });
    }

    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer)
    {
        var keyProp = typeof(T).GetProperties().First(x => x.GetCustomAttribute<CollectionDictionaryKeyAttribute>() != null);
        var sources = value as IEnumerable<T>;
        serializer.Serialize(writer, sources.ToDictionary(x => keyProp.GetValue(x) as string, x => x));
    }
}
```

#### 將 enum 轉成 string

```cs
public class EnumStringConverter<TEnum> : JsonConverter<TEnum> where TEnum : struct
{
    public override bool CanRead => true;

    public override bool CanWrite => true;

    public override TEnum ReadJson(JsonReader reader, Type objectType, TEnum existingValue, bool hasExistingValue, JsonSerializer serializer)
    {
        return serializer.Deserialize<string>(reader).ToEnum<TEnum>();
    }

    public override void WriteJson(JsonWriter writer, TEnum value, JsonSerializer serializer)
        => writer.WriteValue(value.ToString());
}
```

#### 從某個屬性的值判斷輸出哪些屬性

```cs
public class SqlConditionsConverter : JsonConverter
{
    public override bool CanRead => true;

    public override bool CanWrite => true;

    public override bool CanConvert(Type objectType) => objectType == typeof(SqlConditions);

    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)
    {
        if (reader.TokenType == JsonToken.Null)
        {
            return null;
        }

        JObject jObject = JObject.Load(reader);
        SqlConditionsType type = Convert.ToString(jObject["Type"]).ToEnum<SqlConditionsType>();
        if (type == SqlConditionsType.Single)
        {
            var condition = JsonConvert.DeserializeObject<SqlCondition>(jObject["Condition"].ToString());
            return new SqlConditions(condition);
        }
        if (type == SqlConditionsType.Multiple)
        {
            var left = JsonConvert.DeserializeObject<SqlConditions>(jObject["Left"].ToString());
            var right = JsonConvert.DeserializeObject<SqlConditions>(jObject["Right"].ToString());
            return new SqlConditions(left, Convert.ToString(jObject["Operator"]).ToEnum<SqlConditionsOperator>(), right);
        }
        throw new NotImplementedException();
    }

    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer)
    {
        SqlConditions conditions = value as SqlConditions;
        if (conditions.Type == SqlConditionsType.Single)
        {
            serializer.Serialize(writer, new
            {
                conditions.Type,
                conditions.Condition
            });
        }
        if (conditions.Type == SqlConditionsType.Multiple)
        {
            serializer.Serialize(writer, new
            {
                conditions.Type,
                conditions.Left,
                conditions.Operator,
                conditions.Right
            });
        }
    }
}
```
