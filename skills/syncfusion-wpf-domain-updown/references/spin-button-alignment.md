# Spin Button Alignment

Guide to customizing the position of spin buttons (increment/decrement arrows) in the SfDomainUpDown control.

## Overview

The `SpinButtonsAlignment` property controls where the up/down spin buttons appear in the control. This allows you to customize the layout to match your application's design requirements.

**Available Alignments:**
- **Right** (default) - Both buttons on the right side
- **Left** - Both buttons on the left side
- **Both** - Decrement button on left, increment button on right

## SpinButtonsAlignment Property

**Type:** `Syncfusion.Windows.Controls.SpinButtonsAlignment` (enum)

**Values:**
- `SpinButtonsAlignment.Right`
- `SpinButtonsAlignment.Left`
- `SpinButtonsAlignment.Both`

## Right Alignment (Default)

Both spin buttons appear stacked on the right side of the control.

### When to Use
- Standard Windows application layouts
- Right-to-left (RTL) reading interfaces
- When buttons should not interfere with left-aligned content
- Most common use case

### XAML Example

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           Width="200" 
                           Height="35"
                           SpinButtonsAlignment="Right"
                           ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### C# Example

```csharp
SfDomainUpDown domainUpDown = new SfDomainUpDown();
domainUpDown.Height = 35;
domainUpDown.Width = 200;
domainUpDown.SpinButtonsAlignment = Syncfusion.Windows.Controls.SpinButtonsAlignment.Right;
domainUpDown.ItemsSource = employees;
```

### Visual Layout

```
┌─────────────────────┬───┐
│   Current Value     │ ▲ │
│                     │ ▼ │
└─────────────────────┴───┘
```

## Left Alignment

Both spin buttons appear stacked on the left side of the control.

### When to Use
- Left-to-right (LTR) reading interfaces with emphasis on buttons
- Custom UI designs requiring left-side controls
- When content should be right-aligned
- Mirror layouts for special design requirements

### XAML Example

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           Width="200"
                           Height="35"
                           SpinButtonsAlignment="Left"
                           ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### C# Example

```csharp
SfDomainUpDown domainUpDown = new SfDomainUpDown();
domainUpDown.Height = 35;
domainUpDown.Width = 200;
domainUpDown.SpinButtonsAlignment = Syncfusion.Windows.Controls.SpinButtonsAlignment.Left;
domainUpDown.ItemsSource = employees;
```

### Visual Layout

```
┌───┬─────────────────────┐
│ ▲ │   Current Value     │
│ ▼ │                     │
└───┴─────────────────────┘
```

## Both Alignment (Split Buttons)

Buttons are split: decrement button on the left, increment button on the right.

### When to Use
- When you want to emphasize the increment/decrement action
- Slider-like interfaces where left means "less" and right means "more"
- Custom layouts requiring maximum horizontal button separation
- Intuitive left/right navigation paradigms

### XAML Example

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           Width="200"
                           Height="35"
                           SpinButtonsAlignment="Both"
                           ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}" TextAlignment="Center"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### C# Example

```csharp
SfDomainUpDown domainUpDown = new SfDomainUpDown();
domainUpDown.Height = 35;
domainUpDown.Width = 200;
domainUpDown.SpinButtonsAlignment = Syncfusion.Windows.Controls.SpinButtonsAlignment.Both;
domainUpDown.ItemsSource = employees;
```

### Visual Layout

```
┌───┬─────────────────┬───┐
│ ▼ │ Current Value   │ ▲ │
└───┴─────────────────┴───┘
```

**Note:** With "Both" alignment:
- Left button = Decrement (move backward in list)
- Right button = Increment (move forward in list)

## Complete Examples

### Example 1: Day Selector with Right Alignment

```xml
<syncfusion:SfDomainUpDown SpinButtonsAlignment="Right" 
                           Width="150" 
                           Height="35"
                           Value="Monday">
    <syncfusion:SfDomainUpDown.ItemsSource>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>Monday</sys:String>
            <sys:String>Tuesday</sys:String>
            <sys:String>Wednesday</sys:String>
            <sys:String>Thursday</sys:String>
            <sys:String>Friday</sys:String>
        </x:Array>
    </syncfusion:SfDomainUpDown.ItemsSource>
</syncfusion:SfDomainUpDown>
```

### Example 2: Priority Selector with Both Alignment

```xml
<syncfusion:SfDomainUpDown SpinButtonsAlignment="Both" 
                           Width="200" 
                           Height="40"
                           Value="Medium">
    <syncfusion:SfDomainUpDown.ItemsSource>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>Low</sys:String>
            <sys:String>Medium</sys:String>
            <sys:String>High</sys:String>
            <sys:String>Critical</sys:String>
        </x:Array>
    </syncfusion:SfDomainUpDown.ItemsSource>
</syncfusion:SfDomainUpDown>
```

### Example 3: Dynamic Alignment Change

```csharp
// Toggle alignment based on user preference
public void SetAlignment(string alignmentType)
{
    switch (alignmentType)
    {
        case "Right":
            domainUpDown.SpinButtonsAlignment = SpinButtonsAlignment.Right;
            break;
        case "Left":
            domainUpDown.SpinButtonsAlignment = SpinButtonsAlignment.Left;
            break;
        case "Both":
            domainUpDown.SpinButtonsAlignment = SpinButtonsAlignment.Both;
            break;
    }
}
```

## Design Recommendations

### Consistency
- Use the same alignment throughout your application for consistency
- Right alignment is most familiar to users
- Document your choice if using non-standard alignments

### Content Alignment
- With Right/Left button alignment, content is typically left-aligned
- With Both alignment, consider center-aligning content for balance

### Width Considerations
- "Both" alignment requires more horizontal space
- Ensure adequate width (minimum 150px recommended) for split buttons
- Test different screen sizes for responsive layouts

## Combining with Other Features

### With Custom Styling

```xml
<syncfusion:SfDomainUpDown SpinButtonsAlignment="Both"
                           AccentBrush="SteelBlue"
                           Width="250"
                           Height="40">
    <!-- Content and ItemsSource -->
</syncfusion:SfDomainUpDown>
```

### With AutoReverse

```xml
<syncfusion:SfDomainUpDown SpinButtonsAlignment="Right"
                           AutoReverse="True"
                           ItemsSource="{Binding Days}"/>
```

### With Animation Disabled

```xml
<syncfusion:SfDomainUpDown SpinButtonsAlignment="Left"
                           EnableSpinAnimation="False"
                           ItemsSource="{Binding Items}"/>
```

## Best Practices

1. **Stick to Defaults:** Use Right alignment unless you have specific design requirements
2. **Test Usability:** Both alignment can be more intuitive for some users - test with your audience
3. **Consider Localization:** Left/Right button placement may have different meanings in different cultures
4. **Responsive Design:** Ensure adequate space for your chosen alignment at all resolutions
5. **Visual Balance:** Match button alignment with overall form layout

## Common Use Cases

| Use Case | Recommended Alignment | Reason |
|----------|----------------------|---------|
| Standard forms | Right | Familiar, matches most Windows controls |
| Slider-like controls | Both | Intuitive left=less, right=more |
| RTL languages | Left | Better visual flow for right-to-left reading |
| Compact layouts | Right | Maximizes content area |
| Emphasis on control | Both | Makes increment/decrement more prominent |

## Troubleshooting

**Issue:** Buttons appear cut off  
**Solution:** Increase control width to accommodate button width

**Issue:** Both alignment looks unbalanced  
**Solution:** Center-align content text for visual balance

**Issue:** Alignment doesn't change  
**Solution:** Ensure you're setting the property after the control is initialized

**Issue:** Buttons overlap content  
**Solution:** Check control template or increase padding
