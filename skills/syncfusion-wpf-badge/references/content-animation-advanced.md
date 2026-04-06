# Content & Animation — Advanced Examples

## Number Formatting — Full Examples

### Capping at Threshold

```csharp
private void UpdateBadgeContent(int value)
{
    if (value > 99)
        badge.Content = "99+";
    else
        badge.Content = value;
}
```

### Abbreviation Formatting

```csharp
private string FormatBadgeNumber(int value)
{
    if (value <= 99)
        return value.ToString();
    if (value <= 999)
        return "99+";
    if (value < 1000000)
        return (value / 1000.0).ToString("0.#") + "K";
    return (value / 1000000.0).ToString("0.#") + "M";
}
```

### Using Converter

```csharp
public class NumberFormattingConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (int.TryParse(value?.ToString(), out int number))
        {
            if (number <= 99)
                return number.ToString();
            if (number <= 999)
                return "99+";
            if (number < 1000000)
                return (number / 1000.0).ToString("0.#") + "K";
            return (number / 1000000.0).ToString("0.#") + "M";
        }
        return "0";
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xaml
<Window.Resources>
    <local:NumberFormattingConverter x:Key="NumberFormatter"/>
</Window.Resources>

<notification:SfBadge 
    Content="{Binding UnreadCount, Converter={StaticResource NumberFormatter}}"
    x:Name="badge"/>
```

## Text Formatting — Examples

```xaml
<notification:SfBadge 
    FontFamily="Segoe UI"
    FontSize="18"
    FontWeight="Bold"
    Content="5"
    x:Name="badge"/>

<notification:SfBadge 
    FontFamily="Perpetua"
    FontSize="16"
    FontStyle="Italic"
    Content="NEW"
    x:Name="badge"/>

<notification:SfBadge 
    FontFamily="Arial"
    FontSize="12"
    FontWeight="SemiBold"
    Foreground="White"
    Background="DarkBlue"
    Content="ALERT"
    x:Name="badge"/>
```

```csharp
badge.FontFamily = new FontFamily("Segoe UI");
badge.FontSize = 18;
badge.FontWeight = FontWeights.Bold;
badge.FontStyle = FontStyles.Normal;
```

## Data Binding — Full Examples

```xaml
<notification:SfBadge 
    Content="{Binding NotificationCount, Converter={StaticResource NumberFormatter}}"
    x:Name="badge"/>

<notification:SfBadge 
    Content="{Binding UnreadCount, Mode=OneWay, UpdateSourceTrigger=PropertyChanged}"
    x:Name="badge"/>
```

```csharp
public class MailViewModel : INotifyPropertyChanged
{
    private int _unreadCount;
    
    public int UnreadCount
    {
        get { return _unreadCount; }
        set
        {
            if (_unreadCount != value)
            {
                _unreadCount = value;
                OnPropertyChanged(nameof(UnreadCount));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
    
    public void IncrementUnreadCount()
    {
        UnreadCount++;
    }
}
```

### Advanced: Null-Aware Binding

```xaml
<!-- Hide badge when count is 0 -->
<notification:SfBadge 
    Content="{Binding UnreadCount}"
    Visibility="{Binding UnreadCount, Converter={StaticResource CountToVisibilityConverter}}"/>
```

```csharp
public class CountToVisibilityConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (int.TryParse(value?.ToString(), out int count) && count > 0)
            return Visibility.Visible;
        return Visibility.Collapsed;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```
