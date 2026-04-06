# Content and Animation in WPF Badge (SfBadge)

## Table of Contents
- [Content Display](#content-display)
- [Content Template](#content-template)
- [Animation Types](#animation-types)
- [Number Formatting](#number-formatting)
- [Text Formatting](#text-formatting)
- [Data Binding Content](#data-binding-content)

## Content Display

### Purpose

The `Content` property stores the badge's primary data to display.

### Property Details

- **Type:** Object (any WPF data type)
- **Default:** null
- **Behavior:** Badge hides when Content is null

### Supported Content Types

#### String Content

```xaml
<notification:SfBadge 
    Content="NEW"
    x:Name="badge"/>
```

```csharp
badge.Content = "NEW";
```

#### Numeric Content

```xaml
<notification:SfBadge 
    Content="42"
    x:Name="badge"/>
```

```csharp
badge.Content = 42;
// Or convert to string for formatting
badge.Content = "99+";
```

#### Symbol Content

```xaml
<notification:SfBadge 
    Content="●"
    Shape="Ellipse"
    x:Name="badge"/>
```

#### Null Content (Hidden Badge)

```csharp
badge.Content = null; // Badge is hidden
```

### Real-World Example

```csharp
public class NotificationViewModel : INotifyPropertyChanged
{
    private int _unreadCount;
    
    public object BadgeContent
    {
        get
        {
            if (_unreadCount == 0)
                return null; // Hide badge
            if (_unreadCount > 99)
                return "99+"; // Cap display at 99+
            return _unreadCount;
        }
    }
    
    public int UnreadCount
    {
        get { return _unreadCount; }
        set
        {
            if (_unreadCount != value)
            {
                _unreadCount = value;
                OnPropertyChanged(nameof(BadgeContent));
            }
        }
    }
}
```

## Content Template

### Purpose

The `ContentTemplate` property allows custom rendering of badge content.

### Property Details

- **Type:** DataTemplate
- **Default:** null
- **DataContext:** The `Content` property value

### Basic Custom Template

```xaml
<notification:SfBadge Content="99+" x:Name="badge">
    <notification:SfBadge.ContentTemplate>
        <DataTemplate>
            <Grid>
                <TextBlock 
                    Text="{Binding}" 
                    Foreground="Yellow"
                    FontWeight="Bold"/>
            </Grid>
        </DataTemplate>
    </notification:SfBadge.ContentTemplate>
</notification:SfBadge>
```

### Complex Template with Layout

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Content="3"
            Shape="Rectangle"
            x:Name="badge">
            <notification:SfBadge.ContentTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal" Margin="4">
                        <TextBlock 
                            Text="◆" 
                            Foreground="Gold"
                            Margin="0,0,4,0"/>
                        <TextBlock Text="{Binding}"/>
                    </StackPanel>
                </DataTemplate>
            </notification:SfBadge.ContentTemplate>
        </notification:SfBadge>
    </notification:SfBadge.Badge>
</Button>
```

### Template with Binding to ViewModel

```xaml
<notification:SfBadge 
    Content="{Binding NotificationData}"
    x:Name="badge">
    <notification:SfBadge.ContentTemplate>
        ## Number Formatting (Summary)

        Formatting large numbers for display is commonly done by capping small values and abbreviating large ones (K/M). For most applications use a small converter that returns: numbers ≤99 as-is, 100–999 as "99+", thousands as `1.2K` style, and millions as `1.5M` style.

        Advanced formatting examples and converters were moved to `content-animation-advanced.md` (same folder) to keep this core reference concise.
- **Large badges (> 50px height):** Use `FontSize = 16+`
- **Bold weight:** Better readability for short content
- **Contrast:** Ensure text color contrasts with background

## Data Binding Content

### Purpose

Update badge content dynamically from ViewModel properties.

### Basic Binding

```xaml
<notification:SfBadge 
    Content="{Binding UnreadMessageCount}"
    x:Name="badge"/>
```

### With Converter

```xaml
<notification:SfBadge 
    Content="{Binding NotificationCount, Converter={StaticResource NumberFormatter}}"
    x:Name="badge"/>
```

### With Mode and UpdateTrigger

```xaml
<notification:SfBadge 
    Content="{Binding UnreadCount, Mode=OneWay, UpdateSourceTrigger=PropertyChanged}"
    x:Name="badge"/>
```

### ViewModel Implementation

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

### XAML View

```xaml
<Window.DataContext>
    <local:MailViewModel/>
</Window.DataContext>

<StackPanel>
    <Button Width="100" Height="50" Content="Inbox" Click="RefreshButton_Click">
        <notification:SfBadge.Badge>
            <notification:SfBadge 
                AnimationType="Scale"
                Content="{Binding UnreadCount}"
                Fill="Error"
                x:Name="badge"/>
        </notification:SfBadge.Badge>
    </Button>
</StackPanel>
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
