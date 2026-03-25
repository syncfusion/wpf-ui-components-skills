# Axes Configuration

## Table of Contents
- [Overview](#overview)
- [Horizontal Axis (Resistance)](#horizontal-axis-resistance)
- [Radial Axis (Reactance)](#radial-axis-reactance)
- [Major Gridlines](#major-gridlines)
- [Minor Gridlines](#minor-gridlines)
- [Axis Lines](#axis-lines)
- [Label Placement](#label-placement)
- [Label Intersect Actions](#label-intersect-actions)
- [Axis Events](#axis-events)
- [Common Patterns](#common-patterns)

## Overview

SmithChart uses two specialized axes to measure and display transmission line parameters:

1. **Horizontal Axis (Resistance)**: Measures normalized resistance values
2. **Radial Axis (Reactance)**: Measures normalized reactance values

Both axes support extensive customization including gridlines, labels, styling, and interactive events.

**Key Features:**
- Major and minor gridline configuration
- Customizable axis line styling
- Flexible label positioning (Inside/Outside)
- Label intersection handling
- Axis-specific events for advanced scenarios

## Horizontal Axis (Resistance)

The horizontal axis represents normalized resistance values and runs horizontally across the chart.

### Basic Configuration

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis FontSize="11" 
                                   FontFamily="Segoe UI"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
</syncfusion:SfSmithChart>
```

```csharp
chart.HorizontalAxis = new HorizontalAxis();
chart.HorizontalAxis.FontSize = 11;
chart.HorizontalAxis.FontFamily = new FontFamily("Segoe UI");
```

### Common Horizontal Axis Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FontSize` | double | - | Label font size |
| `FontFamily` | FontFamily | - | Label font family |
| `ShowMajorGridlines` | bool | true | Display major gridlines |
| `ShowMinorGridlines` | bool | false | Display minor gridlines |
| `MinorGridlinesCount` | int | 8 | Number of minor gridlines per major interval |
| `ShowAxisLine` | bool | false | Display axis line |
| `LabelPlacement` | LabelPlacement | Outside | Label position (Inside/Outside) |
| `LabelIntersectAction` | LabelIntersectActions | Hide | How to handle overlapping labels |

### Styling Horizontal Axis

```csharp
HorizontalAxis hAxis = new HorizontalAxis
{
    FontSize = 12,
    FontFamily = new FontFamily("Arial"),
    FontWeight = FontWeights.Bold,
    Foreground = new SolidColorBrush(Colors.DarkBlue),
    ShowMajorGridlines = true,
    ShowMinorGridlines = true,
    MinorGridlinesCount = 10
};
chart.HorizontalAxis = hAxis;
```

## Radial Axis (Reactance)

The radial axis represents normalized reactance values and forms circular arcs around the chart.

### Basic Configuration

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis FontSize="11" 
                               FontFamily="Segoe UI"/>
    </syncfusion:SfSmithChart.RadialAxis>
</syncfusion:SfSmithChart>
```

```csharp
chart.RadialAxis = new RadialAxis();
chart.RadialAxis.FontSize = 11;
chart.RadialAxis.FontFamily = new FontFamily("Segoe UI");
```

### Common Radial Axis Properties

Radial axis supports the same properties as horizontal axis:

```csharp
RadialAxis rAxis = new RadialAxis
{
    FontSize = 12,
    FontFamily = new FontFamily("Arial"),
    FontWeight = FontWeights.SemiBold,
    Foreground = new SolidColorBrush(Colors.DarkGreen),
    ShowMajorGridlines = true,
    ShowMinorGridlines = false,
    LabelPlacement = LabelPlacement.Outside
};
chart.RadialAxis = rAxis;
```

## Major Gridlines

Major gridlines are the primary grid divisions on both axes. They are visible by default.

### Controlling Major Gridlines Visibility

**Hide major gridlines:**

```xml
<syncfusion:HorizontalAxis ShowMajorGridlines="False"/>
```

```csharp
chart.HorizontalAxis.ShowMajorGridlines = false;
chart.RadialAxis.ShowMajorGridlines = false;
```

### Styling Major Gridlines

Customize major gridline appearance using MajorGridlineStyle:

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <Style TargetType="Path" x:Key="majorLineStyle">
            <Setter Property="Stroke" Value="BurlyWood"/>
            <Setter Property="StrokeDashArray" Value="5,3"/>
            <Setter Property="StrokeThickness" Value="1"/>
        </Style>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis MajorGridlineStyle="{StaticResource majorLineStyle}"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
    
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis MajorGridlineStyle="{StaticResource majorLineStyle}"/>
    </syncfusion:SfSmithChart.RadialAxis>
</syncfusion:SfSmithChart>
```
### Major Gridline Style Options

**Solid Line:**
```xml
<Setter Property="Stroke" Value="Gray"/>
<Setter Property="StrokeThickness" Value="1"/>
```

**Dashed Line:**
```xml
<Setter Property="StrokeDashArray" Value="5,3"/>  <!-- 5px dash, 3px gap -->
```

**Dotted Line:**
```xml
<Setter Property="StrokeDashArray" Value="1,2"/>  <!-- 1px dot, 2px gap -->
```

**Thick Dashed:**
```xml
<Setter Property="StrokeDashArray" Value="10,5"/>
<Setter Property="StrokeThickness" Value="2"/>
```

## Minor Gridlines

Minor gridlines provide finer divisions between major gridlines. They are hidden by default.

### Enabling Minor Gridlines

```xml
<syncfusion:HorizontalAxis ShowMinorGridlines="True"/>
```

```csharp
chart.HorizontalAxis.ShowMinorGridlines = true;
chart.RadialAxis.ShowMinorGridlines = true;
```

### Setting Minor Gridlines Count

Control the number of minor gridlines between major gridlines:

```xml
<syncfusion:HorizontalAxis ShowMinorGridlines="True" 
                           MinorGridlinesCount="12"/>
```

```csharp
chart.HorizontalAxis.ShowMinorGridlines = true;
chart.HorizontalAxis.MinorGridlinesCount = 12;
```

**Default:** 8 minor gridlines per 100 pixels

**Recommended values:**
- **4-6**: Sparse, cleaner look
- **8-10**: Standard detail level
- **12-16**: High precision, dense grid

### Styling Minor Gridlines

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <Style TargetType="Path" x:Key="minorLineStyle">
            <Setter Property="Stroke" Value="LightGray"/>
            <Setter Property="StrokeThickness" Value="0.5"/>
            <Setter Property="StrokeDashArray" Value="2,2"/>
        </Style>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis ShowMinorGridlines="True" 
                                   MinorGridlinesCount="12"
                                   MinorGridlineStyle="{StaticResource minorLineStyle}"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
</syncfusion:SfSmithChart>
```

```csharp
chart.RadialAxis.MinorGridlineStyle = this.Grid1.Resources["lineStyle"] as Style;
```

### Minor Gridline Best Practices

1. **Use lighter colors than major gridlines** to maintain visual hierarchy
2. **Use thinner lines (0.5-0.8)** to avoid overwhelming the chart
3. **Avoid too many minor gridlines** (>16) as it clutters the view
4. **Consider chart size** - fewer gridlines for small charts
5. **Match minor style to major** for consistent appearance

### Combined Gridline Example

```csharp
// Major gridlines: Bold, dashed
Style majorStyle = new Style(typeof(Path));
majorStyle.Setters.Add(new Setter(Path.StrokeProperty, new SolidColorBrush(Colors.Gray)));
majorStyle.Setters.Add(new Setter(Path.StrokeThicknessProperty, 1.0));
majorStyle.Setters.Add(new Setter(Path.StrokeDashArrayProperty, new DoubleCollection { 5, 3 }));

// Minor gridlines: Light, dotted
Style minorStyle = new Style(typeof(Path));
minorStyle.Setters.Add(new Setter(Path.StrokeProperty, new SolidColorBrush(Colors.LightGray)));
minorStyle.Setters.Add(new Setter(Path.StrokeThicknessProperty, 0.5));
minorStyle.Setters.Add(new Setter(Path.StrokeDashArrayProperty, new DoubleCollection { 1, 2 }));

// Apply to both axes
chart.HorizontalAxis.ShowMajorGridlines = true;
chart.HorizontalAxis.MajorGridlineStyle = majorStyle;
chart.HorizontalAxis.ShowMinorGridlines = true;
chart.HorizontalAxis.MinorGridlinesCount = 10;
chart.HorizontalAxis.MinorGridlineStyle = minorStyle;

chart.RadialAxis.ShowMajorGridlines = true;
chart.RadialAxis.MajorGridlineStyle = majorStyle;
chart.RadialAxis.ShowMinorGridlines = true;
chart.RadialAxis.MinorGridlinesCount = 10;
chart.RadialAxis.MinorGridlineStyle = minorStyle;
```

## Axis Lines

Axis lines are the main dividing lines for horizontal and radial axes. They are hidden by default.

### Enabling Axis Lines

```xml
<syncfusion:HorizontalAxis ShowAxisLine="True"/>
```

```csharp
chart.HorizontalAxis.ShowAxisLine = true;
chart.RadialAxis.ShowAxisLine = true;
```

### Styling Axis Lines

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <Style TargetType="Path" x:Key="axisLineStyle">
            <Setter Property="Stroke" Value="Black"/>
            <Setter Property="StrokeThickness" Value="2"/>
            <Setter Property="StrokeDashArray" Value="7,3"/>
        </Style>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis ShowAxisLine="True" 
                                   AxisLineStyle="{StaticResource axisLineStyle}"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
    
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis ShowAxisLine="True" 
                               AxisLineStyle="{StaticResource axisLineStyle}"/>
    </syncfusion:SfSmithChart.RadialAxis>
</syncfusion:SfSmithChart>
```

### When to Use Axis Lines

**Enable axis lines when:**
- Need clear visual separation of quadrants
- Creating formal/publication charts
- Emphasizing axis positions
- Working with minimal gridlines

**Keep axis lines hidden when:**
- Gridlines provide sufficient structure
- Want clean, minimal appearance
- Axis lines create visual clutter

## Label Placement

Control where axis labels appear relative to the chart area.

### LabelPlacement Options

**Outside (Default):** Labels appear outside the chart circle
```xml
<syncfusion:HorizontalAxis LabelPlacement="Outside"/>
```

**Inside:** Labels appear inside the chart circle
```xml
<syncfusion:HorizontalAxis LabelPlacement="Inside"/>
```

### Horizontal Axis Label Placement

```xml
<!-- Labels below the axis line -->
<syncfusion:HorizontalAxis LabelPlacement="Outside"/>

<!-- Labels above the axis line (inside chart) -->
<syncfusion:HorizontalAxis LabelPlacement="Inside"/>
```

```csharp
// Outside placement (default)
chart.HorizontalAxis.LabelPlacement = LabelPlacement.Outside;

// Inside placement
chart.HorizontalAxis.LabelPlacement = LabelPlacement.Inside;
```

### Radial Axis Label Placement

```xml
<!-- Labels outside the circle -->
<syncfusion:RadialAxis LabelPlacement="Outside"/>

<!-- Labels inside the circle -->
<syncfusion:RadialAxis LabelPlacement="Inside"/>
```

```csharp
// Outside placement (default)
chart.RadialAxis.LabelPlacement = LabelPlacement.Outside;

// Inside placement
chart.RadialAxis.LabelPlacement = LabelPlacement.Inside;
```

### Choosing Label Placement

**Use Outside placement when:**
- Standard chart appearance is desired
- Maximum data visualization space needed
- Labels won't interfere with chart background

**Use Inside placement when:**
- Space constrained layouts
- Creating compact charts
- Want labels closer to data
- Matching specific design requirements

### Combined Example

```csharp
// Horizontal labels inside, radial labels outside
chart.HorizontalAxis.LabelPlacement = LabelPlacement.Inside;
chart.RadialAxis.LabelPlacement = LabelPlacement.Outside;
```

## Label Intersect Actions

Control how axis labels behave when they overlap due to space constraints.

### LabelIntersectAction Options

**Hide (Default):** Automatically hides overlapping labels
```xml
<syncfusion:HorizontalAxis LabelIntersectAction="Hide"/>
```

**None:** Shows all labels regardless of overlap
```xml
<syncfusion:HorizontalAxis LabelIntersectAction="None"/>
```

### Hide vs None Comparison

**LabelIntersectAction = Hide:**
- Automatically detects overlapping labels
- Hides some labels to prevent collision
- Maintains readability
- Recommended for most scenarios

**LabelIntersectAction = None:**
- Displays all labels regardless of space
- May result in overlapping text
- Use when label count is manageable
- Good for large charts with few labels

### Example Usage

```xml
<syncfusion:SfSmithChart Height="300" Width="300">
    <!-- Small chart: Use Hide to prevent clutter -->
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis LabelIntersectAction="Hide"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
    
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis LabelIntersectAction="Hide"/>
    </syncfusion:SfSmithChart.RadialAxis>
</syncfusion:SfSmithChart>

<syncfusion:SfSmithChart Height="800" Width="800">
    <!-- Large chart: Can use None safely -->
    <syncfusion:SfSmithChart.HorizontalAxis>
        <syncfusion:HorizontalAxis LabelIntersectAction="None"/>
    </syncfusion:SfSmithChart.HorizontalAxis>
    
    <syncfusion:SfSmithChart.RadialAxis>
        <syncfusion:RadialAxis LabelIntersectAction="None"/>
    </syncfusion:SfSmithChart.RadialAxis>
</syncfusion:SfSmithChart>
```

```csharp
// Set label intersect action
chart.HorizontalAxis.LabelIntersectAction = LabelIntersectActions.None;
chart.RadialAxis.LabelIntersectAction = LabelIntersectActions.Hide;
```

### Best Practices

1. **Use Hide for variable chart sizes** to ensure readability
2. **Use None for large, spacious charts** where overlap is unlikely
3. **Test with different data ranges** to ensure labels remain readable
4. **Consider font size** - smaller fonts reduce overlap issues

## Axis Events

Respond to axis label creation events for advanced customization.

### LabelCreated Event

Fires when each axis label is created, allowing custom label modification.

```csharp
// Subscribe to event
chart.RadialAxis.LabelCreated += RadialAxis_LabelCreated;
chart.HorizontalAxis.LabelCreated += HorizontalAxis_LabelCreated;

// Event handler for Radial Axis
private void RadialAxis_LabelCreated(object sender, EventArgs e)
{
    var axisLabel = e as ChartAxisLabelEventArgs;
    if (axisLabel != null)
    {
        // Customize specific label
        if (axisLabel.Label.Text == "1")
        {
            axisLabel.Label.Text = "One";
            axisLabel.Label.Foreground = new SolidColorBrush(Colors.Red);
        }
        
        // Add prefix/suffix
        axisLabel.Label.Text = axisLabel.Label.Text + " Ω";
        
        // Conditional formatting
        if (double.TryParse(axisLabel.Label.Text, out double value))
        {
            if (value < 0)
            {
                axisLabel.Label.Foreground = new SolidColorBrush(Colors.Red);
            }
            else
            {
                axisLabel.Label.Foreground = new SolidColorBrush(Colors.Green);
            }
        }
    }
}

// Event handler for Horizontal Axis
private void HorizontalAxis_LabelCreated(object sender, EventArgs e)
{
    var axisLabel = e as ChartAxisLabelEventArgs;
    if (axisLabel != null)
    {
        // Format as percentage
        if (double.TryParse(axisLabel.Label.Text, out double value))
        {
            axisLabel.Label.Text = (value * 100).ToString("F0") + "%";
        }
    }
}
```

### Use Cases for LabelCreated

1. **Custom Formatting**: Add units, format numbers, percentages
2. **Conditional Styling**: Color-code labels based on value
3. **Label Translation**: Replace text with localized versions
4. **Special Highlighting**: Emphasize specific values

### Event Properties

**ChartAxisLabelEventArgs Properties:**
- `Label`: The label element being created
- `Label.Text`: The label text (get/set)
- `Label.Foreground`: Label color (get/set)
- Other label properties available for modification

## Common Patterns

### Pattern 1: Professional Grid Configuration

```csharp
chart.HorizontalAxis = new HorizontalAxis
{
    FontSize = 11,
    ShowMajorGridlines = true,
    MajorGridlineStyle = majorStyle,
    ShowMinorGridlines = true,
    MinorGridlinesCount = 8,
    LabelPlacement = LabelPlacement.Outside
};

chart.RadialAxis = new RadialAxis
{
    FontSize = 11,
    ShowMajorGridlines = true,
    MajorGridlineStyle = majorStyle,
    ShowMinorGridlines = true,
    MinorGridlinesCount = 8,
    LabelPlacement = LabelPlacement.Outside
};
```

### Pattern 2: Minimal Clean Axes

```csharp
// Minimal appearance with only major gridlines
chart.HorizontalAxis = new HorizontalAxis
{
    FontSize = 10,
    ShowMajorGridlines = true,
    ShowMinorGridlines = false,
    ShowAxisLine = false,
    LabelPlacement = LabelPlacement.Outside,
    LabelIntersectAction = LabelIntersectActions.Hide
};

chart.RadialAxis = new RadialAxis
{
    FontSize = 10,
    ShowMajorGridlines = true,
    ShowMinorGridlines = false,
    ShowAxisLine = false,
    LabelPlacement = LabelPlacement.Outside,
    LabelIntersectAction = LabelIntersectActions.Hide
};
```

### Pattern 3: Custom Label Formatting

```csharp
// Add units to labels
chart.HorizontalAxis.LabelCreated += (sender, e) =>
{
    var args = e as ChartAxisLabelEventArgs;
    if (args != null && !string.IsNullOrEmpty(args.Label.Text))
    {
        args.Label.Text += " Ω";
    }
};

chart.RadialAxis.LabelCreated += (sender, e) =>
{
    var args = e as ChartAxisLabelEventArgs;
    if (args != null && !string.IsNullOrEmpty(args.Label.Text))
    {
        args.Label.Text += " j";
    }
};
```

## Troubleshooting

### Gridlines Not Showing

**Issue**: Gridlines not visible despite being enabled  
**Solutions**:
- Check if gridline style colors match background
- Verify ShowMajorGridlines/ShowMinorGridlines are true
- Ensure StrokeThickness > 0 in gridline style

### Labels Overlapping

**Issue**: Axis labels overlap and are hard to read  
**Solutions**:
- Set LabelIntersectAction to Hide
- Reduce font size
- Increase chart size
- Use Inside label placement if appropriate

### Minor Gridlines Too Dense

**Issue**: Too many minor gridlines clutter the chart  
**Solutions**:
- Reduce MinorGridlinesCount (try 4-8)
- Use lighter colors for minor gridlines
- Disable minor gridlines entirely
- Increase line transparency

### Custom Styles Not Applying

**Issue**: Gridline or axis styles not working  
**Solutions**:
- Ensure Style TargetType is Path
- Define styles in Resources before using
- Check property names match exactly
- Verify StaticResource key matches

### LabelCreated Event Not Firing

**Issue**: Custom label formatting not working  
**Solutions**:
- Ensure event is subscribed before chart renders
- Check if event handler signature is correct
- Verify ChartAxisLabelEventArgs is not null
- Subscribe after axis object is created

## Summary

Axes configuration in SmithChart provides extensive control over the visual presentation of resistance and reactance measurements. Key points:

- Configure both Horizontal (Resistance) and Radial (Reactance) axes independently
- Use major and minor gridlines for precision and readability
- Style gridlines and axis lines with custom colors, thickness, and dash patterns
- Control label placement (Inside/Outside) based on layout needs
- Handle label intersection to maintain readability
- Use LabelCreated event for advanced label customization
- Balance detail with clarity - too many gridlines can overwhelm the chart