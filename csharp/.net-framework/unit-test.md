# Unit Test

#### 加入 App.config

單元測試專案執行時不會抓原專案的 config 所以要重新加

#### 執行原專案的 Startup

```cs
[TestClass]
public class Global
{
    private const string HOST_ADDRESS = "http://localhost:43210";
    private static IDisposable webApp;
    private static HttpClient httpClient;

    [AssemblyInitialize]
    public static void AssemblyInitialize(TestContext context)
    {
        AppDomain.CurrentDomain.Load(typeof(Microsoft.Owin.Host.HttpListener.OwinHttpListener).Assembly.GetName());
        webApp = WebApp.Start<Startup>(HOST_ADDRESS);
        Console.WriteLine("Web API started!");
        httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(HOST_ADDRESS);
        Console.WriteLine("HttpClient started!");
    }

    [AssemblyCleanup]
    public static void AssemblyCleanup()
    {
        webApp.Dispose();
    }
}
```

#### 測試前與測試後的動作設定

Create a unit test in your test project. In the unit test class, create methods with the \[TestInitialize] and \[TestCleanup] attributes. They will be run before/after each test method.

Or, if you want to run before/after all test in that class, create static methods with \[ClassInitialize] and \[ClassCleanup] attributes.

Lastly, to run before/after all tests in the assembly, create static methods with \[AssemblyInitialize] and \[AssemblyCleanup] attributes.

https://stackoverflow.com/questions/2304549/how-to-create-startup-and-cleanup-script-for-visual-studio-test-project

#### 使用 Stopwatch 計算執行時間

```cs
var sw = new Stopwatch();
// test method1 start
sw.Reset();
sw.Start();
for (int i = 0; i < 10000; i++)
{
    // invoke test method1
}
sw.Stop();
Console.WriteLine(sw.Elapsed.TotalMilliseconds.ToString(CultureInfo.InvariantCulture));
// test method1 end
```
