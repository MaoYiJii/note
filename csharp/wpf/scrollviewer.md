# ScrollViewer

### 讓滾輪在 ScrollViewer 上層的元件依然可以操作 ScrollViewer 的捲軸

```cs
private void ScrollViewer_PreviewMouseWheel(object sender, MouseWheelEventArgs e)
{
    ScrollViewer scrollViewer = sender as ScrollViewer;
    scrollViewer.ScrollToVerticalOffset(scrollViewer.VerticalOffset - e.Delta);
    e.Handled = true;
}
```
