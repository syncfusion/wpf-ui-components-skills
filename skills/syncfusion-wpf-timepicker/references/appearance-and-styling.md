# Appearance and Styling in SfTimePicker

## Setting Foreground Color

Change the text color of the time picker and time selector items:

### Control Foreground

```xaml
<syncfusion:SfTimePicker Foreground="Red"
                         Name="timePicker"
                         Width="200"/>
```

### Time Selector Item Colors

```xaml
<syncfusion:SfTimePicker Name="timePicker" Width="200">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <!-- Color of unselected items -->
            <Setter Property="Foreground" Value="Blue"/>
            <!-- Color of selected/highlighted item -->
            <Setter Property="SelectedForeground" Value="Yellow"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### Combined Example

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.Foreground = new SolidColorBrush(Colors.DarkRed);
timePicker.Width = 200;
```

## Setting Background Color

Change the background color of the time picker and time selector:

### Control Background

```xaml
<syncfusion:SfTimePicker Background="Red"
                         Name="timePicker"
                         Width="200"/>
```

### Time Selector Background

```xaml
<syncfusion:SfTimePicker AccentBrush="Green"
                         Name="timePicker"
                         Width="200">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Background" Value="Blue"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### AccentBrush Property

The **AccentBrush** property sets the highlight color for selected time values:

```xaml
<syncfusion:SfTimePicker AccentBrush="Green" 
                         Name="timePicker"/>
```

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.AccentBrush = new SolidColorBrush(Colors.LimeGreen);
```

## Color Customization Example

Complete styling example with custom colors:

```xaml
<syncfusion:SfTimePicker Foreground="White"
                         Background="DarkBlue"
                         AccentBrush="Gold"
                         Name="styledPicker"
                         Width="200">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Foreground" Value="LightGray"/>
            <Setter Property="SelectedForeground" Value="Gold"/>
            <Setter Property="Background" Value="DarkSlateGray"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

## Flow Direction (RTL Support)

Support right-to-left languages by setting the **FlowDirection** property:

### Default (Left-to-Right)

```xaml
<syncfusion:SfTimePicker FlowDirection="LeftToRight" 
                         Name="timePicker"/>
```

### Right-to-Left Layout

```xaml
<syncfusion:SfTimePicker FlowDirection="RightToLeft" 
                         Name="timePicker"/>
```

```csharp
SfTimePicker timePicker = new SfTimePicker();
timePicker.FlowDirection = FlowDirection.RightToLeft;
```

### RTL Example with Arabic Watermark

```xaml
<syncfusion:SfTimePicker FlowDirection="RightToLeft"
                         AllowNull="True"
                         Value="{x:Null}"
                         Watermark="اختر الوقت"
                         Name="arabicTimePicker"/>
```

## Editing Modes

Control how users interact with the time picker:

### Mode 1: Free Editing (Default)

Allow users to directly type time values:

```xaml
<syncfusion:SfTimePicker AllowInlineEditing="True"
                         ShowDropDownButton="True"
                         FormatString="HH:mm"
                         Value="03:00"
                         Name="timePicker"/>
```

**Behavior:**
- Users can type directly into the control
- Validation occurs on Enter or focus loss
- Dropdown button available for selection

### Mode 2: Selection-Based Editing

Restrict editing to dropdown selection only:

```xaml
<syncfusion:SfTimePicker AllowInlineEditing="False"
                         ShowDropDownButton="True"
                         FormatString="HH:mm"
                         Value="03:00"
                         Name="timePicker"/>
```

**Behavior:**
- Users cannot type; must use dropdown
- Hour and minute values selected via picker
- More structured, less error-prone

### Mode 3: Read-Only (Display Only)

Show time but prevent any editing:

```xaml
<syncfusion:SfTimePicker AllowInlineEditing="False"
                         ShowDropDownButton="False"
                         FormatString="HH:mm"
                         Value="03:00"
                         Name="timePicker"/>
```

**Behavior:**
- No dropdown button visible
- No editing possible
- Display-only mode for read-only scenarios

### Editing Mode Truth Table

| AllowInlineEditing | ShowDropDownButton | Behavior |
|--------------------|-------------------|----------|
| true | true | Free typing + dropdown selection |
| true | false | Free typing only |
| false | true | Dropdown selection only |
| false | false | Read-only display |

## Theme Application

SfTimePicker supports built-in themes via **SfSkinManager**:

### Available Themes

- MaterialLight
- MaterialDark
- Office2019Blue
- Office2019Black
- Office2019Colourful
- HighContrastBlack
- HighContrastWhite
- Fluent
- FluentDark

### Applying Theme to Single Control

```xaml
<syncfusion:SfTimePicker syncfusion:SfSkinManager.VisualStyle="MaterialLight"
                         Name="timePicker"/>
```

### Applying Theme to Entire Application

```xaml
<Application x:Class="TimePickerApp.App"
             xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
             syncfusion:SfSkinManager.VisualStyle="MaterialDark">
    <Application.Resources>
    </Application.Resources>
</Application>
```

### Switching Themes Dynamically

```csharp
using Syncfusion.Windows.Shared;

// Change theme at runtime
SfSkinManager.SetVisualStyle(timePicker, VisualStyles.MaterialDark);
```

## Common Styling Patterns

### Pattern 1: Business Professional

```xaml
<syncfusion:SfTimePicker Foreground="Black"
                         Background="White"
                         AccentBrush="#0078D4"
                         AllowInlineEditing="False"
                         ShowDropDownButton="True"
                         syncfusion:SfSkinManager.VisualStyle="Fluent"
                         Width="150"
                         Name="professional"/>
```

### Pattern 2: Dark Modern

```xaml
<syncfusion:SfTimePicker Foreground="White"
                         Background="#2D2D2D"
                         AccentBrush="#00D9FF"
                         syncfusion:SfSkinManager.VisualStyle="FluentDark"
                         Width="150"
                         Name="darkModern">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Foreground" Value="#CCCCCC"/>
            <Setter Property="SelectedForeground" Value="White"/>
            <Setter Property="Background" Value="#1E1E1E"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

### Pattern 3: High Contrast Accessibility

```xaml
<syncfusion:SfTimePicker syncfusion:SfSkinManager.VisualStyle="HighContrastBlack"
                         Foreground="White"
                         Background="Black"
                         Width="150"
                         Name="accessible"/>
```

### Pattern 4: Minimal/Clean

```xaml
<syncfusion:SfTimePicker Background="Transparent"
                         Foreground="DarkGray"
                         AllowInlineEditing="True"
                         ShowDropDownButton="True"
                         BorderThickness="0"
                         Width="150"
                         Name="minimal"/>
```

### Pattern 5: Material Design

```xaml
<syncfusion:SfTimePicker Foreground="#424242"
                         Background="White"
                         AccentBrush="#2196F3"
                         SelectorItemHeight="50"
                         SelectorItemWidth="50"
                         syncfusion:SfSkinManager.VisualStyle="MaterialLight"
                         Width="150"
                         Name="material">
    <syncfusion:SfTimePicker.SelectorStyle>
        <Style TargetType="syncfusion:SfTimeSelector">
            <Setter Property="Foreground" Value="#757575"/>
            <Setter Property="SelectedForeground" Value="#2196F3"/>
        </Style>
    </syncfusion:SfTimePicker.SelectorStyle>
</syncfusion:SfTimePicker>
```

## Custom Theme (Theme Studio)

For advanced theme customization:

1. Open Syncfusion **Theme Studio**
2. Create custom theme colors
3. Apply to application

See [Syncfusion WPF Theme Studio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme) for detailed instructions.

## Troubleshooting

### Issue: Theme Not Applied

**Solution:** Ensure Syncfusion namespace is imported:
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Issue: Foreground Color Invisible

**Solution:** Verify sufficient contrast between Foreground and Background colors.

### Issue: RTL Layout Breaks Layout

**Solution:** Use appropriate text alignment and margin settings for RTL:
```xaml
<syncfusion:SfTimePicker FlowDirection="RightToLeft"
                         TextAlignment="Right"/>
```

## Next Steps

- Explore **[Time Selector Customization](time-selector-customization.md)** for advanced UI customization
- Learn **[Time Formatting](time-formatting.md)** for display format control
