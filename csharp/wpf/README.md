# WPF

### Window 第一次顯示的事件

```cs
protected override void OnContentRendered(EventArgs e)
{
    // TODO:
}
```

### 選擇資料夾路徑的對話框

編輯專案檔，加入 UseWindowsForms

```xml
<UseWindowsForms>true</UseWindowsForms>
```

開啟對話框

```cs
var dialog = new System.Windows.Forms.FolderBrowserDialog();
if (dialog.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    // dialog.SelectedPath
}
```

### 取得程式路徑

```csharp
System.AppDomain.CurrentDomain.BaseDirectory
```

### 在 xaml 加入其他組件的命名空間

```xml
xmlns:bootstrap="clr-namespace:WPF.Bootstrap.Controls;assembly=WPF.Bootstrap"
```

### 將自己設為 DataContext

```xml
DataContext="{Binding RelativeSource={RelativeSource Self}}"
```

### Style BaseOn

```xml
<Style TargetType="CheckBox" BasedOn="{StaticResource {x:Type CheckBox}}">
```

### Binding 到外層元件

```xml
Content="{Binding SelectedContent, RelativeSource={RelativeSource Mode=FindAncestor, AncestorType=TabControl}}"
```

### Binding 到 App.xaml.cs 的 Property

```xaml
Background="{Binding Path=Theme.MainBackground, Source={x:Static Application.Current}}"
```

### DataTrigger 比對 enum

```xaml
<DataTrigger Binding="{Binding Path=SelectedPageType}" Value="{x:Static local:MainWindow+PageType.Database}">
    <Setter Property="BorderBrush" Value="Red" />
</DataTrigger>
```

### DataTrigger 改變套用的 Style

```xaml
<DataTemplate>
    <TextBox x:Name="TextBox_ActionName" Text="{Binding ActionName}" TextAlignment="Left" VerticalAlignment="Center"/>
    <DataTemplate.Triggers>
        <DataTrigger Binding="{Binding IsLink}" Value="True">
            <Setter TargetName="TextBox_ActionName" Property="Style" Value="{StaticResource {x:Type TextBox}}"/>
        </DataTrigger>
        <DataTrigger Binding="{Binding IsLink}" Value="False">
            <Setter TargetName="TextBox_ActionName" Property="Style" Value="{StaticResource TextBox_ReadOnly}"/>
        </DataTrigger>
    </DataTemplate.Triggers>
</DataTemplate>
```

### 泛型 (Generic Type) Page (同樣適用於 Window)

.xaml.cs 不能直接使用泛型，但可以繼承一個泛型的類別 .xaml 要對應繼承的泛型基底，並加上 x:TypeArguments 對應泛型的類別

MyGenericPage.cs

```cs
namespace MyNamespace
{
    public class MyGenericPage<T> : Page
    {
        public MyGenericPage(T item)
        {
        }
```

MyItem.cs

```cs
namespace MyNamespace.Models
{
    public class MyItem
    {
```

MyPage.xaml.cs

```cs
namespace MyNamespace.Pages
{
    public class MyPage : MyGenericPage<MyItem>
    {
        public MyPage(MyItem item) : base(item)
        {
        }
```

MyPage.xaml

```xaml
<base:MyGenericPage x:Class="MyNamespace.Pages.MyPage"
                    x:TypeArguments="models:MyItem"
                    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
                    xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
                    xmlns:base="clr-namespace:MyNamespace"
                    xmlns:local="clr-namespace:MyNamespace.Pages"
                    xmlns:models="clr-namespace:MyNamespace.Models"
```

### 將檔案拖曳到元件

在元件設置 AllowDrop="True" Drop="Button\_Drop" 即可 如果設置後拖曳檔案還是禁止標誌，可能是因為以系統管理員身分執行的關係，不要用系統管理員身分執行即可

在事件中取得拖拉的檔案

```cs
if (e.Data.GetDataPresent(DataFormats.FileDrop))
{
    string[] fileNames = ((string[])e.Data.GetData(DataFormats.FileDrop));
}
```
