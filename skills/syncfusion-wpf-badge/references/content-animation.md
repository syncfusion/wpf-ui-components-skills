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
        <DataTemplate>
            <Grid>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center">
                    <TextBlock 
                        Text="{Binding Title}"
                        FontSize="10"
                        FontWeight="Bold"/>
                    <TextBlock 
                        Text="{Binding Count}"
                        FontSize="8"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </notification:SfBadge.ContentTemplate>
</notification:SfBadge>
```

### C# Implementation

```csharp
// Create DataTemplate programmatically
DataTemplate template = new DataTemplate();
FrameworkElementFactory textBlock = new FrameworkElementFactory(typeof(TextBlock));
textBlock.SetValue(TextBlock.ForegroundProperty, Brushes.Yellow);
textBlock.SetBinding(TextBlock.TextProperty, new Binding());
template.VisualTree = textBlock;

badge.ContentTemplate = template;
```

## Animation Types

### Purpose

Animations trigger when badge content changes, providing visual feedback to the user.

### Available Animation Types

| Type | Effect | Use Case |
|------|--------|----------|
| `None` | No animation (default) | Static content |
| `Scale` | Badge scales up/down | Attention-grabbing |
| `Opacity` | Badge fades in/out | Smooth transitions |

### Default Value

- `AnimationType` = **None**

### Scale Animation

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            AnimationType="Scale"
            Content="5"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

**Effect:** When content changes, badge scales up, then back to normal size.

### Opacity Animation

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            AnimationType="Opacity"
            Content="5"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

**Effect:** When content changes, badge fades out, content updates, then fades back in.

### C# Implementation

```csharp
badge.AnimationType = BadgeAnimationType.Scale;
// Animation triggers when content changes
badge.Content = "New Value";
```

### Real-Time Update Example

```xaml
<StackPanel>
    <Button Width="100" Height="50" Content="Inbox">
        <notification:SfBadge.Badge>
            <notification:SfBadge 
                AnimationType="Scale"
                Content="{Binding UnreadCount}"
                x:Name="badge"/>
        </notification:SfBadge.Badge>
    </Button>
    
    <StackPanel Orientation="Horizontal" Margin="0,10,0,0">
        <TextBlock Text="Update count: "/>
        <notification:UpDown 
            MinValue="0"
            MaxValue="100"
            Step="1"
            NumberDecimalDigits="0"
            Value="5"
            x:Name="counter"
            ValueChanged="Counter_ValueChanged"/>
    </StackPanel>
</StackPanel>
```

```csharp
private void Counter_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    // Animation triggers automatically when binding updates
    this.badge.Content = Convert.ToInt32(e.NewValue).ToString();
}
```

## Number Formatting

### Purpose

Format large numbers for display (e.g., 1000 → "1K", 1000000 → "1M").

### Common Formatting Patterns

#### Capping at Threshold

```csharp
private void UpdateBadgeContent(int value)
{
    if (value > 99)
        badge.Content = "99+";
    else
        badge.Content = value;
}
```

#### Abbreviation Formatting

```csharp
private string FormatBadgeNumber(int value)
{
    if (value <= 99)
        return value.ToString();
    else if (value <= 999)
        return "99+";
    else if (value < 99999)
        return (value / 1000).ToString("0.#") + "K";
    else if (value < 999999)
        return (value / 1000).ToString("#,0K");
    else if (value < 9999999)
        return (value / 1000000).ToString("0.#") + "M";
    else
        return (value / 1000000).ToString("#,0M");
}

// Usage
badge.Content = FormatBadgeNumber(125000); // "125K"
```

#### Using Converter

```csharp
public class NumberFormattingConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (int.TryParse(value?.ToString(), out int number))
        {
            if (number <= 99)
                return number.ToString();
            else if (number <= 999)
                return "99+";
            else if (number < 1000000)
                return (number / 1000.0).ToString("0.#K");
            else
                return (number / 1000000.0).ToString("0.#M");
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

### Formatting Examples

```csharp
FormatBadgeNumber(5);      // "5"
FormatBadgeNumber(42);     // "42"
FormatBadgeNumber(100);    // "99+"
FormatBadgeNumber(1200);   // "1.2K"
FormatBadgeNumber(50000);  // "50K"
FormatBadgeNumber(1500000); // "1.5M"
```

## Text Formatting

### Purpose

Control the appearance of badge text content.

### Text Properties

| Property | Default | Description |
|----------|---------|-------------|
| `FontFamily` | Segoe UI | Font face |
| `FontSize` | 14 | Text size in pixels |
| `FontStyle` | Normal | Normal, Italic, Oblique |
| `FontWeight` | Normal | Normal, Bold, etc. |
| `Foreground` | null | Text color (overridden by content) |

### Examples

#### Large Bold Text

```xaml
<notification:SfBadge 
    FontFamily="Segoe UI"
    FontSize="18"
    FontWeight="Bold"
    Content="5"
    x:Name="badge"/>
```

#### Italic Styling

```xaml
<notification:SfBadge 
    FontFamily="Perpetua"
    FontSize="16"
    FontStyle="Italic"
    Content="NEW"
    x:Name="badge"/>
```

#### Custom Font with Color

```xaml
<notification:SfBadge 
    FontFamily="Arial"
    FontSize="12"
    FontWeight="SemiBold"
    Foreground="White"
    Background="DarkBlue"
    Content="ALERT"
    x:Name="badge"/>
```

### C# Implementation

```csharp
badge.FontFamily = new FontFamily("Segoe UI");
badge.FontSize = 18;
badge.FontWeight = FontWeights.Bold;
badge.FontStyle = FontStyles.Normal;
```

### Text Formatting Guidelines

- **Small badges (< 20px height):** Use `FontSize = 10-12`
- **Standard badges (40x30):** Use `FontSize = 12-14`
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
