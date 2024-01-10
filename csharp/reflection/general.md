# General

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