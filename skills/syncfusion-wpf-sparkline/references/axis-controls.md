# Axis Controls

## Table of Contents
- [Overview](#overview)
- [Enabling Axis Display](#enabling-axis-display)
- [Axis Positioning](#axis-positioning)
- [Axis Styling](#axis-styling)
- [Custom Axis Line Styles](#custom-axis-line-styles)
- [Axis with Other Features](#axis-with-other-features)
- [Best Practices](#best-practices)

## Overview

Sparklines typically omit axes to maintain their compact, minimalist design. However, Syncfusion WPF Sparklines support an optional horizontal axis line that can be displayed to provide additional context, especially when visualizing data that crosses zero or has a meaningful baseline.

**Axis Availability:**
- ✓ **Line Sparkline** (SfLineSparkline)
- ✓ **Column Sparkline** (SfColumnSparkline)
- ✓ **Area Sparkline** (SfAreaSparkline)
- ✗ **WinLoss Sparkline** (NOT supported)

The axis feature includes:
- **ShowAxis** - Toggle axis visibility
- **AxisOrigin** - Position axis at specific Y-value
- **AxisStyle** - Customize axis line appearance

## Enabling Axis Display

By default, sparklines do not display an axis. Enable it using the `ShowAxis` property.

### Basic Axis Display

### XAML Implementation

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    ShowAxis = true
};
```

### Default Behavior

When `ShowAxis="True"`:
- A horizontal line appears across the sparkline
- Default position is at Y=0 (zero baseline)
- Line uses default stroke color and thickness
- Provides visual reference for positive/negative values

## Axis Positioning

Control where the axis line appears using the `AxisOrigin` property. This is particularly useful for data that doesn't naturally center around zero.

### Setting Axis Origin

The `AxisOrigin` property specifies the Y-value where the axis line is drawn.

### XAML Implementation

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="2"
    Interior="#4a4a4a">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    ShowAxis = true,
    AxisOrigin = 2,
    Interior = new SolidColorBrush(Color.FromRgb(0x4a, 0x4a, 0x4a)),
};
```

### Common Axis Origin Values

**Zero Baseline (Default):**
```xml
<syncfusion:SfLineSparkline ShowAxis="True" AxisOrigin="0"/>
```

**Use case:** Financial data where positive/negative has meaning (profit/loss).

**Mean/Average Baseline:**
```xml
<syncfusion:SfLineSparkline ShowAxis="True" AxisOrigin="5000"/>
```

**Use case:** Position axis at the average value to show above/below average performance.

**Minimum Value:**
```xml
<syncfusion:SfLineSparkline ShowAxis="True" AxisOrigin="0"/>
```

**Use case:** When all values are positive and you want a baseline at the bottom.

**Custom Threshold:**
```xml
<syncfusion:SfLineSparkline ShowAxis="True" AxisOrigin="3500"/>
```

**Use case:** Show a target or threshold line (e.g., sales quota, performance target).

## Axis Styling

Customize the axis line appearance using the `AxisStyle` property. The axis is rendered as a WPF `Line` element, so you can apply line styling properties.

### Basic Axis Styling

Define a Style targeting `Line`:

```xml
<Window.Resources>
    <Style TargetType="Line" x:Key="blueAxisStyle">
        <Setter Property="Stroke" Value="Blue"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Window.Resources>

<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisStyle="{StaticResource blueAxisStyle}">
</syncfusion:SfLineSparkline>
```

### Complete Styled Example

```xml
<Window.Resources>
    <Style TargetType="Line" x:Key="customAxisStyle">
        <Setter Property="Stroke" Value="Blue"/>
        <Setter Property="StrokeThickness" Value="2"/>
        <Setter Property="StrokeDashArray" Value="1,1"/>
    </Style>
</Window.Resources>

<syncfusion:SfLineSparkline 
    Interior="#4a4a4a"   
    ShowAxis="True" 
    AxisStyle="{StaticResource customAxisStyle}"   
    AxisOrigin="1" 
    ItemsSource="{Binding UsersList}"   
    YBindingPath="NoOfUsers">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
Style axisStyle = new Style(typeof(Line));
axisStyle.Setters.Add(new Setter(Line.StrokeProperty, new SolidColorBrush(Colors.Blue)));
axisStyle.Setters.Add(new Setter(Line.StrokeThicknessProperty, 2.0));
axisStyle.Setters.Add(new Setter(Line.StrokeDashArrayProperty, new DoubleCollection { 1, 1 }));

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    ShowAxis = true,
    AxisOrigin = 1,
    AxisStyle = axisStyle,
    Interior = new SolidColorBrush(Color.FromRgb(0x4a, 0x4a, 0x4a)),
};
```

## Custom Axis Line Styles

### Solid Axis Line

```xml
<Style TargetType="Line" x:Key="solidAxis">
    <Setter Property="Stroke" Value="Black"/>
    <Setter Property="StrokeThickness" Value="2"/>
</Style>
```

**Use case:** Strong, clear baseline for emphasis.

### Dashed Axis Line

```xml
<Style TargetType="Line" x:Key="dashedAxis">
    <Setter Property="Stroke" Value="Gray"/>
    <Setter Property="StrokeThickness" Value="1"/>
    <Setter Property="StrokeDashArray" Value="4,2"/>
</Style>
```

**Use case:** Subtle reference line that doesn't dominate the visualization.

### Dotted Axis Line

```xml
<Style TargetType="Line" x:Key="dottedAxis">
    <Setter Property="Stroke" Value="DarkGray"/>
    <Setter Property="StrokeThickness" Value="1"/>
    <Setter Property="StrokeDashArray" Value="1,2"/>
</Style>
```

**Use case:** Minimal, unobtrusive baseline.

### Thick Colored Axis

```xml
<Style TargetType="Line" x:Key="thickAxis">
    <Setter Property="Stroke" Value="Red"/>
    <Setter Property="StrokeThickness" Value="3"/>
</Style>
```

**Use case:** Emphasize a critical threshold or target line.

### Semi-Transparent Axis

```xml
<Style TargetType="Line" x:Key="transparentAxis">
    <Setter Property="Stroke" Value="#80000000"/>
    <Setter Property="StrokeThickness" Value="2"/>
</Style>
```

**Use case:** Visible but subtle reference line.

## Axis with Different Sparkline Types

### Line Sparkline with Axis

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    Interior="Blue">
</syncfusion:SfLineSparkline>
```

**Use case:** Show positive/negative trends around zero baseline.

### Column Sparkline with Axis

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    Interior="SteelBlue">
</syncfusion:SfColumnSparkline>
```

**Use case:** Columns above/below axis clearly show positive/negative values.

### Area Sparkline with Axis

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    Interior="LightBlue">
</syncfusion:SfAreaSparkline>
```

**Use case:** Area above axis is one color, area below could be styled differently (though this requires custom rendering).

### WinLoss Sparkline (NOT Supported)

```xml
<!-- This will NOT display an axis -->
<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding Match}" 
    YBindingPath="Result"
    ShowAxis="True"
    Interior="Green">
</syncfusion:SfWinLossSparkline>
```

**Note:** The `ShowAxis` property has no effect on WinLoss sparklines. The binary nature of WinLoss data makes an axis unnecessary.

## Axis with Other Features

### Axis + Markers

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    MarkerVisibility="Visible">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            HighPointBrush="Green"
            LowPointBrush="Red"
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Use case:** Axis provides baseline context, markers highlight special points.

### Axis + Range Band

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    BandRangeStart="3000"
    BandRangeEnd="1000"
    RangeBandBrush="LightGreen">
</syncfusion:SfLineSparkline>
```

**Use case:** Axis shows zero line, range band highlights target zone.

### Axis + Track Ball

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    ShowTrackBall="True">
</syncfusion:SfLineSparkline>
```

**Use case:** Axis provides static context, track ball enables interactive inspection.

### Axis + Segment Customization

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    ShowAxis="True"
    AxisOrigin="0"
    Interior="Gray">
    
    <syncfusion:SfColumnSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            HighPointBrush="Green"
            LowPointBrush="Red"
            NegativePointBrush="DarkRed"/>
    </syncfusion:SfColumnSparkline.SegmentTemplateSelector>
    
</syncfusion:SfColumnSparkline>
```

**Use case:** Axis shows baseline, segment colors highlight exceptional values.

## Best Practices

### When to Show Axis

**Show axis when:**
- Data crosses zero (positive/negative values)
- A meaningful baseline exists (average, target, threshold)
- Comparing values relative to a reference point
- Context is needed for interpretation
- Financial or performance data where above/below matters

**Hide axis when:**
- All values are positive and similar magnitude
- Focus is purely on trend shape, not absolute values
- Maintaining absolute minimal design
- Using WinLoss sparkline (not supported)
- Space is extremely limited

### Axis Origin Guidelines

**Zero Origin (AxisOrigin="0"):**
- Default and most common
- Natural for profit/loss, gain/loss, above/below
- Financial data, temperature changes

**Average Origin:**
```csharp
double average = data.Average(d => d.Value);
sparkline.AxisOrigin = average;
```

- Shows above/below average performance
- Good for performance metrics

**Custom Threshold:**
```xml
<syncfusion:SfLineSparkline AxisOrigin="5000"/>
```

- Target line (e.g., sales quota)
- Performance threshold
- Regulatory limit

### Axis Styling Guidelines

**Subtle Axis (Recommended):**
```xml
<Style TargetType="Line" x:Key="subtleAxis">
    <Setter Property="Stroke" Value="LightGray"/>
    <Setter Property="StrokeThickness" Value="1"/>
    <Setter Property="StrokeDashArray" Value="2,2"/>
</Style>
```

**Why:** Axis provides context without dominating the visualization.

**Emphasized Axis:**
```xml
<Style TargetType="Line" x:Key="emphasizedAxis">
    <Setter Property="Stroke" Value="Black"/>
    <Setter Property="StrokeThickness" Value="2"/>
</Style>
```

**Why:** When the baseline is critical to understanding (e.g., profitability threshold).

### Color Selection

**Neutral Colors (Recommended):**
- Gray, DarkGray, LightGray
- Black (for strong emphasis)
- Semi-transparent black (`#80000000`)

**Avoid:**
- Bright colors that compete with data
- Colors similar to data interior
- Multiple axis styles in the same dashboard (maintain consistency)

### Combining with Range Bands

When using both axis and range bands, ensure they serve different purposes:

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    ShowAxis="True"
    AxisOrigin="0"
    BandRangeStart="80"
    BandRangeEnd="100">
</syncfusion:SfLineSparkline>
```

- **Axis (Y=0):** Shows positive/negative boundary
- **Range Band (80-100):** Shows target/acceptable range

This combination is effective when zero and target are different values.

### Axis in Dashboards

For multiple sparklines in a dashboard:

```xml
<UniformGrid Columns="2" Rows="2">
    <!-- All sparklines use same axis style for consistency -->
    <syncfusion:SfLineSparkline 
        ShowAxis="True" 
        AxisOrigin="0"
        AxisStyle="{StaticResource sharedAxisStyle}"/>
    
    <syncfusion:SfLineSparkline 
        ShowAxis="True" 
        AxisOrigin="0"
        AxisStyle="{StaticResource sharedAxisStyle}"/>
    
    <syncfusion:SfColumnSparkline 
        ShowAxis="True" 
        AxisOrigin="0"
        AxisStyle="{StaticResource sharedAxisStyle}"/>
    
    <syncfusion:SfAreaSparkline 
        ShowAxis="True" 
        AxisOrigin="0"
        AxisStyle="{StaticResource sharedAxisStyle}"/>
</UniformGrid>
```

**Benefit:** Consistent axis styling creates visual harmony and aids comparison.

### Performance Considerations

Axis display has negligible performance impact. Feel free to enable it when it adds value.

### Accessibility

- Axis provides visual reference but doesn't improve accessibility for screen readers
- Ensure axis color has sufficient contrast with background
- Don't rely solely on axis position to convey critical information

## Dynamic Axis Configuration

Bind axis properties to ViewModel for runtime control:

### ViewModel

```csharp
using System.ComponentModel;

public class AxisViewModel : INotifyPropertyChanged
{
    private bool _showAxis = true;
    private double _axisOrigin = 0;
    
    public bool ShowAxis
    {
        get => _showAxis;
        set
        {
            _showAxis = value;
            OnPropertyChanged(nameof(ShowAxis));
        }
    }
    
    public double AxisOrigin
    {
        get => _axisOrigin;
        set
        {
            _axisOrigin = value;
            OnPropertyChanged(nameof(AxisOrigin));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### XAML Binding

```xml
<Window.DataContext>
    <local:AxisViewModel/>
</Window.DataContext>

<StackPanel>
    <CheckBox Content="Show Axis" IsChecked="{Binding ShowAxis}"/>
    
    <StackPanel Orientation="Horizontal" Margin="0,10">
        <TextBlock Text="Axis Origin:" VerticalAlignment="Center" Margin="5"/>
        <TextBox Text="{Binding AxisOrigin, UpdateSourceTrigger=PropertyChanged}" Width="80"/>
    </StackPanel>
    
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding UsersList}" 
        YBindingPath="NoOfUsers"
        ShowAxis="{Binding ShowAxis}"
        AxisOrigin="{Binding AxisOrigin}">
    </syncfusion:SfLineSparkline>
</StackPanel>
```

**Result:** User can toggle axis visibility and adjust position in real-time.

## Next Steps

- **[Range Band](range-band.md)** - Combine axis with range highlighting
- **[Markers](markers.md)** - Add markers with axis context
- **[Segment Customization](segment-customization.md)** - Color-code segments relative to axis
