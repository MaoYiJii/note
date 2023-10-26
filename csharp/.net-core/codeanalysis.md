# CodeAnalysis

#### 載入程式碼

```cs
SyntaxTree tree = CSharpSyntaxTree.ParseText(programText);
CompilationUnitSyntax root = tree.GetRoot() as CompilationUnitSyntax;
```

#### 取得所有 Class

```cs
root.DescendantNodes().OfType<ClassDeclarationSyntax>()
```
