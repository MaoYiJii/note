# Regular (Regex)

### Atomic Group

以 `(?>foo|bar)` 表示 當在這個 Group 匹配了一個結果，不管整段規則最後是否有匹配，都不會再用這個 Group 的下一個項目進行匹配

舉例以 `(abc|ab)c` 匹配 `abcd` 可以匹配成功 若以 `(?>abc|ab)c` 匹配 `abcd` 則不會匹配成功

原因是 Atomic Group 的第一個 abc 已經匹配成功，所以不會再嘗試後面的 ab 所以 `(?>abc|ab)c` 無法匹配 `abcd` 但可以匹配 `abcc`

### Positive Lookahead

以 `(?=foo)` 表示 匹配以這個表達式結尾的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

### Positive Lookbehind

以 `(?<=foo)` 表示 匹配以這個表達式起始的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

### Negative Lookahead

以 `(?!foo)` 表示 匹配排除以這個表達式結尾的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

### Negative Lookbehind

以 `(?<!foo)` 表示 匹配排除以這個表達式起始的字串，但匹配成功後不會將這個表達式的字段當作 Group 的值

### 找左右括號匹配

```regex
\(([^()]|(?R))*\)
```

### 不錯的範例

[https://5xruby.tw/posts/15min-regular-expression](https://5xruby.tw/posts/15min-regular-expression)

* `?=`: 斷言
* `?:`: 不捕獲

### 重複出現

以 `\1` `\2` 來代表對應的 group 重複出現

index
```
<(\w+)><\/\1>
```
    
named (PHP only?)
```
<(?<tag>\w+)><\/\g<tag>>
```

### 排除 \1 \2

一般使用排除會用 `[^]` 把需要排除的字元放在 `^` 之後\
但是 `\1` `\2` 放在裡面會變成排除常數 `1` 或 `2`，因此想要排除 `\1` 可以將 `[^\1]` 調整為 `((?!\1).)`

[https://stackoverflow.com/questions/18242119/general-approach-for-equivalent-of-backreferences-within-character-class](https://stackoverflow.com/questions/18242119/general-approach-for-equivalent-of-backreferences-within-character-class)

### replace
- C#
  <https://docs.microsoft.com/en-us/dotnet/standard/base-types/substitutions-in-regular-expressions>
- php
  <https://www.jetbrains.com/help/phpstorm/tutorial-finding-and-replacing-text-using-regular-expressions.html>
- js
  <https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/String/replace#%E6%8C%87%E5%AE%9A%E4%B8%80%E5%80%8B%E5%AD%97%E4%B8%B2%E7%82%BA%E5%8F%83%E6%95%B8>

輸出
- index: `$1` `$2`
- index(old): `\1` `\2` (有些程式語言不支援)
- named: `${a}` `${b}` (有些程式語言不支援)

## 實際範例

### 檢查 C# 程式碼可能的 SQL Injection

- 普通情境
  ```
  (?<=(SELECT|INSERT|UPDATE|DELETE|FROM|WHERE|EXEC)[^"]+)'[^"'\r\n]*("\s*\+|{)
  ```
- 廣泛情境 (誤判會比較多)
  ``` regex
  ((?<=[\WN])'([^"'\r\n]*("\s*\+|{)|\s*"(;|\)))|'\\'')
  ```
  可匹配的對象
  ``` cs
  "[name] = '" + name + "'"
  "[name] = N'" + name + "'"
  "[name] LIKE '%" + name + "%'"
  "[name] LIKE N'%" + name + "%'"
  "[name] LIKE 'A" + name + "'"
  "[name] LIKE '" + name + "Z'"
  string.Format("[name] = '{0}'", name)
  string.Format("[name] LIKE '%{0}%'", name)
  $"[name] = '{name}'"
  $"[name] LIKE '%{name}%'"
  stringBuilder.Append("'").Append(name).Append("'");
  stringBuilder.Append('\'').Append(name).Append('\'');
  // 誤判
  XmlNode xmlNode = xmlElement.SelectSingleNode("div[id='" + id + "']");
  ```