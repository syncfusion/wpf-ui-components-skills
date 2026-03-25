# Tooltip Support in Range Selectors

Complete guide to configuring and customizing tooltips for the Range Selector's slider thumbs.

## Overview

The SfDateTimeRangeNavigator provides tooltip support on both left and right slider thumbs. These tooltips display the exact date-time values at the thumb positions, allowing users to see precise range boundaries while dragging or resizing the selection.

**Key Tooltip Features:**
- **Automatic Display** - Shows on hover and during drag operations
- **Precision Values** - Display exact date-time with millisecond precision
- **Dual Tooltips** - Separate tooltips for left and right thumbs
- **Custom Formatting** - Control date-time display format
- **Template Customization** - Complete visual control with DataTemplates
- **Real-time Updates** - Tooltips update continuously during interaction

## Basic Tooltip Configuration

### Enabling Tooltips

Tooltips are disabled by default. Enable them with the `ShowToolTip` property:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ShowToolTip="True"/>
```

```csharp
rangeNavigator.ShowToolTip = true;
```

### Setting Tooltip Label Format

Control the date-time format displayed in tooltips using `ToolTipLabelFormat`:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ShowToolTip="True"
    ToolTipLabelFormat="MMM/dd/yyyy"/>
```

**Result:** Tooltip displays dates like "Jan/15/2024"

```csharp
rangeNavigator.ShowToolTip = true;
rangeNavigator.ToolTipLabelFormat = "yyyy-MM-dd HH:mm";
```

**Result:** Tooltip displays dates like "2024-01-15 14:30"

### Common Format Patterns

| Format String | Example Output | Use Case |
|--------------|----------------|----------|
| `MMM/dd/yyyy` | Jan/15/2024 | Standard US format |
| `dd/MM/yyyy` | 15/01/2024 | European format |
| `yyyy-MM-dd` | 2024-01-15 | ISO format |
| `MMMM d, yyyy` | January 15, 2024 | Long format |
| `MM/dd/yy` | 01/15/24 | Short format |
| `yyyy-MM-dd HH:mm` | 2024-01-15 14:30 | With time |
| `MMM dd, HH:mm:ss` | Jan 15, 14:30:45 | With seconds |
| `ddd, MMM dd` | Mon, Jan 15 | Day and date |

## Complete Basic Example

```xml
<Window x:Class="RangeNavigatorDemo.TooltipWindow"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="150"/>
        </Grid.RowDefinitions>
        
        <!-- Chart -->
        <syncfusion:SfChart Grid.Row="0">
            <syncfusion:SfChart.PrimaryAxis>
                <syncfusion:DateTimeAxis 
                    ZoomPosition="{Binding ElementName=RangeNavigator, Path=ZoomPosition, Mode=TwoWay}"
                    ZoomFactor="{Binding ElementName=RangeNavigator, Path=ZoomFactor, Mode=TwoWay}"/>
            </syncfusion:SfChart.PrimaryAxis>
            
            <syncfusion:SfChart.SecondaryAxis>
                <syncfusion:NumericalAxis/>
            </syncfusion:SfChart.SecondaryAxis>
            
            <syncfusion:LineSeries 
                ItemsSource="{Binding Data}"
                XBindingPath="Date"
                YBindingPath="Value"/>
        </syncfusion:SfChart>
        
        <!-- Range Navigator with Tooltips -->
        <syncfusion:SfDateTimeRangeNavigator 
            Grid.Row="1"
            x:Name="RangeNavigator"
            ItemsSource="{Binding Data}"
            XBindingPath="Date"
            ShowToolTip="True"
            ToolTipLabelFormat="MMMM dd, yyyy">
            
            <syncfusion:SfDateTimeRangeNavigator.Content>
                <syncfusion:SfLineSparkline 
                    ItemsSource="{Binding Data}"
                    YBindingPath="Value"/>
            </syncfusion:SfDateTimeRangeNavigator.Content>
            
        </syncfusion:SfDateTimeRangeNavigator>
    </Grid>
</Window>
```

## Custom Tooltip Templates

For advanced customization, use DataTemplates to completely control tooltip appearance.

### LeftToolTipTemplate

Customize the tooltip for the left (start) thumb:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ShowToolTip="True">
    
    <syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
        <DataTemplate>
            <Border Background="#2196F3" 
                    BorderBrush="White" 
                    BorderThickness="2"
                    CornerRadius="5"
                    Padding="10,5">
                <StackPanel>
                    <TextBlock Text="Start Date" 
                              FontSize="10" 
                              FontWeight="Bold"
                              Foreground="White"/>
                    <TextBlock Text="{Binding}" 
                              FontSize="12"
                              Foreground="White"
                              Margin="0,2,0,0"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### RightToolTipTemplate

Customize the tooltip for the right (end) thumb:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ShowToolTip="True">
    
    <syncfusion:SfDateTimeRangeNavigator.RightToolTipTemplate>
        <DataTemplate>
            <Border Background="#FF5722" 
                    BorderBrush="White" 
                    BorderThickness="2"
                    CornerRadius="5"
                    Padding="10,5">
                <StackPanel>
                    <TextBlock Text="End Date" 
                              FontSize="10" 
                              FontWeight="Bold"
                              Foreground="White"/>
                    <TextBlock Text="{Binding}" 
                              FontSize="12"
                              Foreground="White"
                              Margin="0,2,0,0"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfDateTimeRangeNavigator.RightToolTipTemplate>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### Complete Custom Template Example

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ShowToolTip="True"
    ToolTipLabelFormat="MMM dd, yyyy">
    
    <!-- Left Tooltip Template -->
    <syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
        <DataTemplate>
            <Grid>
                <!-- Arrow pointing down -->
                <Path Data="M 0,10 L 10,0 L 20,10" 
                      Fill="#4CAF50" 
                      Stretch="Fill"
                      Height="10"
                      VerticalAlignment="Top"
                      Margin="15,0,0,0"/>
                
                <!-- Tooltip content -->
                <Border Background="#4CAF50" 
                        BorderBrush="#388E3C" 
                        BorderThickness="2"
                        CornerRadius="8"
                        Padding="12,8"
                        Margin="0,8,0,0">
                    <StackPanel>
                        <TextBlock Text="FROM" 
                                  FontSize="9" 
                                  FontWeight="Bold"
                                  Foreground="White"
                                  Opacity="0.8"/>
                        <TextBlock Text="{Binding}" 
                                  FontSize="13"
                                  FontWeight="SemiBold"
                                  Foreground="White"
                                  Margin="0,3,0,0"/>
                    </StackPanel>
                </Border>
            </Grid>
        </DataTemplate>
    </syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
    
    <!-- Right Tooltip Template -->
    <syncfusion:SfDateTimeRangeNavigator.RightToolTipTemplate>
        <DataTemplate>
            <Grid>
                <!-- Arrow pointing down -->
                <Path Data="M 0,10 L 10,0 L 20,10" 
                      Fill="#2196F3" 
                      Stretch="Fill"
                      Height="10"
                      VerticalAlignment="Top"
                      Margin="15,0,0,0"/>
                
                <!-- Tooltip content -->
                <Border Background="#2196F3" 
                        BorderBrush="#1976D2" 
                        BorderThickness="2"
                        CornerRadius="8"
                        Padding="12,8"
                        Margin="0,8,0,0">
                    <StackPanel>
                        <TextBlock Text="TO" 
                                  FontSize="9" 
                                  FontWeight="Bold"
                                  Foreground="White"
                                  Opacity="0.8"/>
                        <TextBlock Text="{Binding}" 
                                  FontSize="13"
                                  FontWeight="SemiBold"
                                  Foreground="White"
                                  Margin="0,3,0,0"/>
                    </StackPanel>
                </Border>
            </Grid>
        </DataTemplate>
    </syncfusion:SfDateTimeRangeNavigator.RightToolTipTemplate>
    
</syncfusion:SfDateTimeRangeNavigator>
```

## Advanced Tooltip Scenarios

### Tooltips with Additional Information

Display more than just the date in tooltips:

```xml
<syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
    <DataTemplate>
        <Border Background="#673AB7" 
                BorderBrush="White" 
                BorderThickness="2"
                CornerRadius="5"
                Padding="10">
            <StackPanel>
                <TextBlock Text="Range Start" 
                          FontSize="10" 
                          FontWeight="Bold"
                          Foreground="White"/>
                <TextBlock Text="{Binding}" 
                          FontSize="12"
                          Foreground="White"
                          Margin="0,3,0,0"/>
                <Separator Margin="0,5" Background="White" Opacity="0.3"/>
                <TextBlock Text="{Binding ElementName=RangeNavigator, Path=SelectedData.Count, StringFormat='Data Points: {0}'}" 
                          FontSize="9"
                          Foreground="White"
                          Opacity="0.9"/>
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
```

### Conditionally Styled Tooltips

Use triggers or converters to style tooltips based on data:

```xml
<syncfusion:SfDateTimeRangeNavigator.Resources>
    <local:DateRangeColorConverter x:Key="ColorConverter"/>
</syncfusion:SfDateTimeRangeNavigator.Resources>

<syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
    <DataTemplate>
        <Border Background="{Binding Converter={StaticResource ColorConverter}}" 
                BorderBrush="White" 
                BorderThickness="2"
                CornerRadius="5"
                Padding="10,5">
            <TextBlock Text="{Binding}" 
                      FontSize="12"
                      Foreground="White"/>
        </Border>
    </DataTemplate>
</syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
```

```csharp
public class DateRangeColorConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is DateTime date)
        {
            // Different colors for different quarters
            int quarter = (date.Month - 1) / 3 + 1;
            return quarter switch
            {
                1 => new SolidColorBrush(Colors.Green),
                2 => new SolidColorBrush(Colors.Blue),
                3 => new SolidColorBrush(Colors.Orange),
                4 => new SolidColorBrush(Colors.Red),
                _ => new SolidColorBrush(Colors.Gray)
            };
        }
        return new SolidColorBrush(Colors.Gray);
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

### Tooltips with Icons

```xml
<syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
    <DataTemplate>
        <Border Background="#009688" 
                BorderBrush="White" 
                BorderThickness="2"
                CornerRadius="5"
                Padding="8">
            <StackPanel Orientation="Horizontal">
                <!-- Calendar Icon (using Unicode) -->
                <TextBlock Text="📅" 
                          FontSize="16"
                          VerticalAlignment="Center"
                          Margin="0,0,8,0"/>
                
                <StackPanel>
                    <TextBlock Text="Start" 
                              FontSize="9" 
                              FontWeight="Bold"
                              Foreground="White"/>
                    <TextBlock Text="{Binding}" 
                              FontSize="11"
                              Foreground="White"/>
                </StackPanel>
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:SfDateTimeRangeNavigator.LeftToolTipTemplate>
```

## Tooltip Behavior Tips

### When Tooltips Appear
- On **mouse hover** over thumb symbols
- During **drag operations** on thumbs
- While **resizing** the selected range
- Tooltips follow the thumb position in real-time

### Tooltip Positioning
- Tooltips automatically position above the thumb
- If space is limited at the top, they may flip below
- Custom templates should consider arrow direction

### Performance Considerations
- Tooltips update continuously during drag operations
- Keep templates simple for smooth interaction
- Avoid heavy bindings or complex converters in tooltip templates
- Use `ToolTipLabelFormat` for simple date formatting (more efficient than templates)

## Best Practices

### 1. Choose the Right Approach
- **Simple formatting:** Use `ToolTipLabelFormat` for performance
- **Visual styling:** Use templates with simple borders and colors
- **Additional data:** Use templates with complex layouts

### 2. Maintain Readability
- Use high contrast between background and text
- Keep font sizes readable (11-14pt for main text)
- Limit information to essentials

### 3. Consistent Design
- Match tooltip style with your application theme
- Use same colors for left and right tooltips, or complementary colors
- Align tooltip design with thumb customization

### 4. Formatting Guidelines
- Include enough precision for your data (day, hour, minute)
- Avoid overly verbose formats (keep tooltips compact)
- Consider your audience's locale preferences

## Summary

You've learned how to:
- ✓ Enable tooltips with ShowToolTip property
- ✓ Format tooltip content with ToolTipLabelFormat
- ✓ Create custom tooltip templates for left and right thumbs
- ✓ Display additional information in tooltips
- ✓ Style tooltips with borders, colors, and effects
- ✓ Add icons and visual elements to tooltips
- ✓ Use converters for conditional tooltip styling
- ✓ Apply best practices for tooltip design

Next: Review [troubleshooting.md](troubleshooting.md) for solutions to common Range Selector issues.
