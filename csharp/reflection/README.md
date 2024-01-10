# Reflection

## 類別

舉例現在有一個類別 `MyClass`
``` cs
public class MyClass
{
}
```

### 建立物件

一般在已知類型的情況下，只需要 `new` 即可
``` cs
var myObject = new MyClass();
```

在 `Reflection` 則需要分為兩個步驟

1. 取得 `類型 (Type)`
   - 方法 1. 使用名稱
     ``` cs
     var t = Type.GetType("Namespace.MyClass, AssemblyName");
     ```
   - 方法 2. 使用類型
     ``` cs
     var t = typeof(MyClass);
     ```
     *這可以搭配泛型來使用，如 `typeof(T)`*
  
2. 建立物件
   ``` cs
   var myObject = Activator.CreateInstance(t);
   ```

## 成員

類別中的成員有好幾種，可以參考[微軟官方說明文件](https://learn.microsoft.com/zh-tw/dotnet/csharp/programming-guide/classes-and-structs/members)
比較常用到的大概只有 `欄位 (field)` `屬性 (property)` `方法 (method)`

``` cs
public class MyClass
{
    // 欄位 (field)
    public string foo;
    
    // 屬性 (property)
    public string Bar { get; set; }
    
    // 方法 (method)
    public string Baz()
    {
    }
}
```

## 欄位存取

不使用 `Reflection`
``` cs
// 建立物件
var myObject = new MyClass();
// 寫入
myObject.foo = "fooValue";
// 讀取
Console.WriteLine(myObject.foo);
```

使用 `Reflection`
*建立物件沒有使用 `Reflection`，如果需要可以查看上面的 `類別` 說明*
``` cs
// 建立物件
var myObject = new MyClass();
// 取得欄位
var fooField = t.GetField("foo");
// 寫入
fooField.SetValue(myObject, "fooValue");
// 讀取
Console.WriteLine(fooField.GetValue(myObject));
```

## 屬性存取
不使用 `Reflection`
``` cs
// 建立物件
var myObject = new MyClass();
// 寫入
myObject.Bar = "BarValue";
// 讀取
Console.WriteLine(myObject.Bar);
```

使用 `Reflection`
``` cs
```



#### 以 Type 建立物件

無參數建構子

```cs
Activator.CreateInstance(type);
```

#### 以 Type 建立泛型物件

以 List\<T> 為例

```cs
Activator.CreateInstance(typeof(List<>).MakeGenericType(resultGenericType)) as IList;
```

#### 呼叫有 out 參數的方法

以 Enum.TryParse 為例

```cs
var enumParse = typeof(Enum).GetMethods()
    .First(x => x.Name == "TryParse" && x.GetParameters().Length == 2)
    .MakeGenericMethod(new[] { type });
object[] parameters = new object[] { source.ToString(), null };
isParsed = (bool)enumParse.Invoke(null, parameters);
result = parameters[1];
```

#### 從 Expression 取得 PropertyName

```cs
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

#### 以 explicit 轉換類型

```cs
// 如果沒有對應的 explicit 會擲回 InvalidOperationException
var explicitMethod = Expression.Convert(Expression.Parameter(value.GetType(), null), type).Method;
return explicitMethod.Invoke(null, new object[] { value });
```
