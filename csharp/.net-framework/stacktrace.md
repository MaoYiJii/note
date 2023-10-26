# StackTrace

#### 取得呼叫堆疊

```cs
/// <summary>
/// 取得執行時的呼叫位置
/// </summary>
private string GetStackFrameInfo()
{
    Stack<string> callStack = new Stack<string>();
    StackTrace stackTrace = new StackTrace(true);
    foreach (var stackFrame in stackTrace.GetFrames())
    {
        var fileName = stackFrame.GetFileName();
        if (fileName != null)
        {
            var method = stackFrame.GetMethod();
            if (method.DeclaringType == typeof(FlowFormApplicationServiceBase))
            {
                // 不包含在 FlowFormRepositoryBase 的資訊
                continue;
            }
            else
            {
                int lineNumber = stackFrame.GetFileLineNumber();
                callStack.Push($"{fileName}: 行 {lineNumber}");
            }
        }
    }
    return string.Join("\r\n", callStack);
}
```

#### 取得呼叫的類別與方法

```cs
string className = null;
string methodName = null;
StackTrace stackTrace = new StackTrace(true);
foreach (var stackFrame in stackTrace.GetFrames())
{
    var callerMethod = stackFrame.GetMethod();
    if (callerMethod != null
        && callerMethod.DeclaringType != typeof(DefaultRepository))
    {
        className = callerMethod.DeclaringType.Name;
        methodName = callerMethod.Name;
        break;
    }
}
```
