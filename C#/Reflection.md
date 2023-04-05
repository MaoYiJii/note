### 以 Type 建立物件
無參數建構子
``` cs
Activator.CreateInstance(type);
```

### 以 Type 建立泛型物件
以 List\<T\> 為例
``` cs
Activator.CreateInstance(typeof(List<>).MakeGenericType(resultGenericType)) as IList;
```

### 呼叫有 out 參數的方法
以 Enum.TryParse 為例
``` cs
var enumParse = typeof(Enum).GetMethods()
    .First(x => x.Name == "TryParse" && x.GetParameters().Length == 2)
    .MakeGenericMethod(new[] { type });
object[] parameters = new object[] { source.ToString(), null };
isParsed = (bool)enumParse.Invoke(null, parameters);
result = parameters[1];
```

### 從 Expression 取得 PropertyName
``` cs
public string GetPropertyName<T>(Expression<Func<T, object>> expression)
{
    if (expression.Body is MemberExpression memberExpression)
    {
        return memberExpression.Member.Name;
    }
    if (expression.Body is UnaryExpression unaryExpression
        && unaryExpression.Operand is MemberExpression unaryMemberExpression)
    {
        return unaryMemberExpression.Member.Name;
    }
    return null;
}
```