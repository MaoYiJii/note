# Assembly

### 載入其他專案輸出的 dll

```cs
const string projectPath = @"D:\Workspace\ordNTU\ordNTU";
const string projectOutput = "WorkV3.dll";
const string frameworkPath = @"C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\";
const string frameworkVersion = "v4.6.1";

public Assembly WorkV3Assembly { get; set; }
public Type[] WorkV3Types { get; set; }

private void ReflectionOnlyLoad()
{
    AppDomain.CurrentDomain.ReflectionOnlyAssemblyResolve += ReflectionOnlyAssemblyResolve;
    WorkV3Assembly = Assembly.ReflectionOnlyLoadFrom(Path.Combine(projectPath, "bin", projectOutput));
    WorkV3Types = WorkV3Assembly.ExportedTypes.ToArray();
    AppDomain.CurrentDomain.ReflectionOnlyAssemblyResolve -= ReflectionOnlyAssemblyResolve;
}

private Assembly ReflectionOnlyAssemblyResolve(object sender, ResolveEventArgs e)
{
    string assemblyName = $"{new AssemblyName(e.Name)?.Name}.dll";
    var assemblyPath = Path.Combine(projectPath, "bin", assemblyName);
    if (File.Exists(assemblyPath))
    {
        return Assembly.ReflectionOnlyLoadFrom(assemblyPath);
    }
    assemblyPath = Path.Combine(frameworkPath, frameworkVersion, assemblyName);
    if (File.Exists(assemblyPath))
    {
        return Assembly.ReflectionOnlyLoadFrom(assemblyPath);
    }
    return Assembly.ReflectionOnlyLoad(assemblyName);
}
```

### 載入動態編譯的 dll

```cs
public CodeDomCompilerResult()
{
    if (CodeDomCompilerResult.AppCompilerResults.Count == 0)
    {
        AppDomain.CurrentDomain.AssemblyResolve += AssemblyResolve;
    }
    CodeDomCompilerResult.AppCompilerResults.Add(this);
}

/// <summary>
/// 程式找不到組件時，讓它從編譯的組件去找
/// </summary>
private System.Reflection.Assembly AssemblyResolve(object sender, ResolveEventArgs e)
{
    string assemblyName = $"{new System.Reflection.AssemblyName(e.Name)?.Name}.dll";
    var assemblyPath = CodeDomCompilerResult.AppCompilerResults.SelectMany(x => x.Assemblies).FirstOrDefault(x => x.EndsWith(assemblyName, StringComparison.OrdinalIgnoreCase));
    if (!string.IsNullOrEmpty(assemblyPath))
    {
        return System.Reflection.Assembly.LoadFrom(assemblyPath);
    }
    return null;
}

public void Dispose()
{
    CodeDomCompilerResult.AppCompilerResults.Remove(this);
    if (CodeDomCompilerResult.AppCompilerResults.Count == 0)
    {
        AppDomain.CurrentDomain.AssemblyResolve -= AssemblyResolve;
    }
}
```
