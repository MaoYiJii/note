# MultiValueConverter

### EqualsMultiValueConverter

```cs
/// <summary>
/// 所有的值都相等時回傳 true
/// </summary>
public class EqualsMultiValueConverter : IMultiValueConverter
{
    public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
    {
        if (values != null)
        {
            return values.All(x => x == null) || values.All(x => x != null && x.Equals(values[0]));
        }
        return false;
    }

    public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```
