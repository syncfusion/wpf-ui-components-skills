# ToolTip in WPF Rating (SfRating)

Tooltips provide additional information about the rating value, displaying numeric feedback to users. This guide covers tooltip configuration, precision settings, and styling options.

## Table of Contents
- [Overview](#overview)
- [Enabling and Disabling Tooltips](#enabling-and-disabling-tooltips)
- [Tooltip Precision](#tooltip-precision)
- [Tooltip Foreground Color](#tooltip-foreground-color)
- [Tooltip Behavior](#tooltip-behavior)
- [Complete Configuration Examples](#complete-configuration-examples)
- [Tooltip in Data Binding Scenarios](#tooltip-in-data-binding-scenarios)
- [Best Practices](#best-practices)
- [Edge Cases and Considerations](#edge-cases-and-considerations)
- [Troubleshooting](#troubleshooting)

## Overview

The SfRating control's tooltip displays the current value when users hover over rating items. Tooltips are particularly useful for:
- Showing exact numeric ratings
- Providing feedback before selection
- Displaying decimal values with Exact precision
- Enhancing user understanding of rating values

The tooltip appears on mouse hover and disappears when the mouse moves away.

## Enabling and Disabling Tooltips

The `ShowToolTip` property controls tooltip visibility. Default value is `true`.

### Enable Tooltips

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     ShowToolTip="True"
                     Value="3"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    ShowToolTip = true,
    Value = 3
};
this.Content = rating;
```

### Disable Tooltips

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     ShowToolTip="False"
                     Value="3"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    ShowToolTip = false,
    Value = 3
};
```

**When to disable tooltips:**
- Minimalist UI designs
- When numeric values are displayed elsewhere
- Mobile/touch interfaces where hover doesn't apply
- Performance-critical scenarios (rare)
- Read-only ratings where value is already shown

## Tooltip Precision

The `AutoToolTipPrecision` property sets the number of decimal places displayed in the tooltip. Default value is `1`.

### Setting Tooltip Precision

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Precision="Exact" 
                     AutoToolTipPrecision="6"
                     Value="3.141592"
                     ShowToolTip="True"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Precision = Syncfusion.Windows.Primitives.Precision.Exact,
    AutoToolTipPrecision = 6,
    Value = 3.141592,
    ShowToolTip = true
};
this.Content = rating;
```

### Precision Values and Use Cases

| AutoToolTipPrecision | Display Example | Use Case |
|---------------------|-----------------|----------|
| 0 | 3 | Whole numbers only |
| 1 (default) | 3.5 | Standard rating display |
| 2 | 3.45 | Average ratings, analytics |
| 3+ | 3.456 | Scientific, high-precision data |

**Important:** To display decimal places in tooltips, set `Precision="Exact"`. Standard and Half precision modes round the value before displaying.

## Tooltip Foreground Color

The `ToolTipForeground` property customizes the text color of the tooltip.

### Basic Color Setting

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="3"
                     ShowToolTip="True"
                     ToolTipForeground="Red"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3,
    ShowToolTip = true,
    ToolTipForeground = new SolidColorBrush(Colors.Red)
};
this.Content = rating;
```

### Advanced Color Customization

**Using hex colors:**
```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4"
                     ShowToolTip="True"
                     ToolTipForeground="#2196F3"/>
```

**Using system colors:**
```csharp
rating.ToolTipForeground = new SolidColorBrush(SystemColors.HighlightTextColor);
```

**Color recommendations:**
- **White/Light colors** - Use with dark tooltip backgrounds
- **Dark colors** - Use with light tooltip backgrounds  
- **Brand colors** - Match your application's theme
- **High contrast** - Ensure readability against tooltip background

## Tooltip Behavior

### Display Timing

The tooltip appears when:
- Mouse hovers over any rating item
- Mouse moves between rating items (tooltip updates)
- User is previewing a rating selection

The tooltip disappears when:
- Mouse leaves the SfRating control area
- User clicks to select a rating (after brief delay)
- Control loses focus

### Preview Value Display

Tooltips show the `PreviewValue` during hover, which represents the potential rating if the user clicks:

```csharp
// The tooltip will display the PreviewValue while hovering
// Before clicking, tooltip shows preview (e.g., 4.0)
// After clicking, Value is set and tooltip shows selected value
```

### Tooltip with Different Precision Modes

#### Standard Precision
```xml
<syncfusion:SfRating ItemsCount="5"
                     Precision="Standard"
                     ShowToolTip="True"
                     AutoToolTipPrecision="1"/>
<!-- Tooltip shows: 1.0, 2.0, 3.0, 4.0, 5.0 -->
```

#### Half Precision
```xml
<syncfusion:SfRating ItemsCount="5"
                     Precision="Half"
                     ShowToolTip="True"
                     AutoToolTipPrecision="1"/>
<!-- Tooltip shows: 1.0, 1.5, 2.0, 2.5, 3.0, etc. -->
```

#### Exact Precision
```xml
<syncfusion:SfRating ItemsCount="5"
                     Precision="Exact"
                     ShowToolTip="True"
                     AutoToolTipPrecision="2"/>
<!-- Tooltip shows: 1.25, 2.37, 3.89, etc. (any decimal value) -->
```

## Complete Configuration Examples

### Example 1: E-Commerce Product Rating

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4.5"
                     Precision="Half"
                     ShowToolTip="True"
                     AutoToolTipPrecision="1"
                     ToolTipForeground="White"
                     Width="150"/>
```

Displays ratings like "4.5" with white text, perfect for showing product averages.

### Example 2: High-Precision Analytics Display

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="3.847"
                     Precision="Exact"
                     ShowToolTip="True"
                     AutoToolTipPrecision="3"
                     ToolTipForeground="DarkBlue"
                     IsReadOnly="True"
                     Width="150"/>
```

Shows precise calculated values with 3 decimal places, useful for dashboards and reports.

### Example 3: Simple User Feedback

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="0"
                     Precision="Standard"
                     ShowToolTip="True"
                     AutoToolTipPrecision="0"
                     ToolTipForeground="Black"
                     Width="150"/>
```

Shows whole numbers (1, 2, 3, 4, 5) for straightforward user ratings.

### Example 4: Custom Styled Tooltip

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="3"
                     ShowToolTip="True"
                     AutoToolTipPrecision="1"
                     ToolTipForeground="#FF6B35"
                     ItemSize="30"
                     Width="180">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="#FFD700"/>
            <Setter Property="UnRatedFill" Value="#E8E8E8"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

Combines tooltip customization with rating appearance styling.

## Tooltip in Data Binding Scenarios

### Displaying Bound Values

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="{Binding AverageRating}"
                     Precision="Exact"
                     ShowToolTip="True"
                     AutoToolTipPrecision="2"
                     IsReadOnly="True"/>
```

```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private double _averageRating = 4.37;
    
    public double AverageRating
    {
        get => _averageRating;
        set
        {
            _averageRating = value;
            OnPropertyChanged(nameof(AverageRating));
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

The tooltip automatically updates when the bound value changes, showing the current average with 2 decimal places.

### Dynamic Tooltip Text

While you can't directly customize the tooltip content, you can use PreviewValue for custom displays:

```xml
<StackPanel>
    <syncfusion:SfRating x:Name="productRating"
                         ItemsCount="5"
                         Value="3"
                         ShowToolTip="True"
                         AutoToolTipPrecision="1"/>
    
    <TextBlock Text="{Binding ElementName=productRating, Path=PreviewValue, 
                              StringFormat='Rating: {0:F1} stars'}"
               Margin="0,10,0,0"/>
</StackPanel>
```

## Best Practices

### When to Use Tooltips

**Enable tooltips (ShowToolTip="True") when:**
- Using Exact precision mode (shows exact decimal values)
- Displaying calculated averages or aggregated data
- Users need numeric confirmation of visual rating
- Rating scale is not immediately obvious
- Providing additional feedback during interaction

**Disable tooltips (ShowToolTip="False") when:**
- The UI already displays the numeric value prominently
- Creating a minimalist design
- Building for touch-only devices
- Numeric precision is not important to users
- Performance is critical (very rare cases)

### Precision Recommendations

| Scenario | Recommended AutoToolTipPrecision |
|----------|----------------------------------|
| User input ratings | 0 or 1 |
| Product averages | 1 |
| Detailed analytics | 2 |
| Scientific/technical data | 2-4 |
| Whole number ratings | 0 |
| Half-star ratings | 1 |

### Color Accessibility

Ensure tooltip text is readable:
- Use sufficient contrast between foreground and background
- Test with different Windows themes (light/dark)
- Consider color-blind users
- Default tooltip styling usually provides good contrast

### Combining Features

**Tooltip + Exact Precision:**
```xml
<!-- Best for displaying precise calculated values -->
<syncfusion:SfRating Precision="Exact" 
                     ShowToolTip="True" 
                     AutoToolTipPrecision="2"/>
```

**Tooltip + Read-Only:**
```xml
<!-- Best for displaying non-editable ratings with numeric feedback -->
<syncfusion:SfRating IsReadOnly="True" 
                     ShowToolTip="True" 
                     AutoToolTipPrecision="1"/>
```

**Tooltip + Half Precision:**
```xml
<!-- Best for e-commerce and review systems -->
<syncfusion:SfRating Precision="Half" 
                     ShowToolTip="True" 
                     AutoToolTipPrecision="1"/>
```

## Edge Cases and Considerations

### Tooltip with Zero Value

When Value is 0, the tooltip still appears on hover and shows "0" (or "0.0" based on precision):

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="0"
                     ShowToolTip="True"
                     AutoToolTipPrecision="1"/>
<!-- Hovering shows "0.0" -->
```

### Tooltip Position

The tooltip position is automatically calculated by the control to appear above or beside the rating items. The WPF tooltip system handles positioning to ensure visibility.

### Tooltip with Very Large Values

If you programmatically set a value exceeding ItemsCount, the tooltip still displays the clamped value:

```csharp
rating.ItemsCount = 5;
rating.Value = 10;  // Clamped to 5.0
// Tooltip shows "5.0"
```

### Performance Considerations

Tooltips have negligible performance impact. The overhead of showing/hiding tooltips is minimal even with frequent updates during mouse movement.

## Troubleshooting

### Tooltip Not Appearing

**Problem:** Tooltip doesn't show on hover.

**Solutions:**
- Verify `ShowToolTip="True"` is set
- Check that mouse events are not blocked by another control
- Ensure the control is not IsReadOnly (tooltips work with read-only, but verify)
- Rebuild the application

### Wrong Decimal Places

**Problem:** Tooltip shows wrong number of decimal places.

**Solutions:**
- Check `AutoToolTipPrecision` value
- For decimal display, ensure `Precision="Exact"` (Standard/Half may round)
- Verify the Value property has decimal places

### Tooltip Text Color Not Visible

**Problem:** Tooltip text is hard to read or invisible.

**Solutions:**
- Adjust `ToolTipForeground` for better contrast
- Test with different Windows themes
- Use system colors for automatic theme adaptation
- Check for transparency issues in custom styles

### Tooltip Shows Different Value Than Selected

**Problem:** Tooltip value doesn't match the visual rating.

**Solutions:**
- This is expected during hover (shows PreviewValue)
- After clicking, tooltip should match Value property
- Check that Value is properly bound or set
- Verify no conflicting value updates in code
