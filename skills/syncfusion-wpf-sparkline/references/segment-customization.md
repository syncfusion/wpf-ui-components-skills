# Segment Customization

## Table of Contents
- [Overview](#overview)
- [Segment Highlighting](#segment-highlighting)
- [Segment Template Selector](#segment-template-selector)
- [Sparkline Type Differences](#sparkline-type-differences)
- [Special Point Brushes](#special-point-brushes)
- [Advanced Customization Techniques](#advanced-customization-techniques)
- [Best Practices](#best-practices)

## Overview

Segment customization allows you to color-code individual data points or segments within a sparkline, making it easy to distinguish special values like first, last, highest, lowest, and negative points.

**Segment Customization Features:**

| Feature | Line | Column | Area | WinLoss |
|---------|------|--------|------|---------|
| Segment Highlighting (hover) | ✗ | ✓ | ✗ | ✓ |
| Segment Brush Customization | ✓ | ✓ | ✓ | ✓ |

**Two Main Approaches:**
1. **Segment Highlighting** - Interactive hover effect (Column and WinLoss only)
2. **Segment Template Selector** - Color-code special points (all types)

## Segment Highlighting

Segment highlighting enables a visual effect when hovering over Column or WinLoss sparkline segments, providing interactive feedback for discrete data points.

**Availability:**
- ✓ **Column Sparkline** (SfColumnSparkline)
- ✓ **WinLoss Sparkline** (SfWinLossSparkline)
- ✗ Line Sparkline (use track ball instead)
- ✗ Area Sparkline (use track ball instead)

### Enabling Segment Highlighting

Set the `HighlightSegment` property to `True`:

### Column Sparkline with Highlighting

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    HighlightSegment="True"
    Interior="SteelBlue"
    BorderBrush="DarkGray"
    BorderThickness="1">
</syncfusion:SfColumnSparkline>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;

SfColumnSparkline sparkline = new SfColumnSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    HighlightSegment = true,
    Interior = new SolidColorBrush(Colors.SteelBlue),
    BorderBrush = new SolidColorBrush(Colors.DarkGray),
    BorderThickness = new Thickness(1)
};
```

### WinLoss Sparkline with Highlighting

```xml
<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding Match}" 
    YBindingPath="Result"
    HighlightSegment="True"
    Interior="Green">
</syncfusion:SfWinLossSparkline>
```

### Behavior

When enabled:
- Mouse hover over a column/segment brightens or changes its appearance
- Default highlight behavior is automatic (slightly lighter shade)
- Provides visual feedback for interactive exploration
- Returns to normal when mouse leaves

**Note:** Highlight appearance is controlled by the sparkline's theme and Interior color. For custom highlight effects, use themes or segment template selectors.

## Segment Template Selector

The `SegmentTemplateSelector` allows you to assign different brush colors to special data points: first, last, high, low, and negative values.

**Availability:** All sparkline types (Line, Column, Area, WinLoss)

### Column Sparkline Segment Customization

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="SteelBlue">
    
    <syncfusion:SfColumnSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow" 
            HighPointBrush="Red"
            LowPointBrush="Blue"
            NegativePointBrush="Orange"/>
    </syncfusion:SfColumnSparkline.SegmentTemplateSelector>
    
</syncfusion:SfColumnSparkline>
```

### C# Implementation

```csharp
SfColumnSparkline sparkline = new SfColumnSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    Interior = new SolidColorBrush(Colors.SteelBlue)
};

SegmentTemplateSelector selector = new SegmentTemplateSelector()
{
    FirstPointBrush = new SolidColorBrush(Colors.Yellow),
    LastPointBrush = new SolidColorBrush(Colors.Yellow),
    HighPointBrush = new SolidColorBrush(Colors.Red),
    LowPointBrush = new SolidColorBrush(Colors.Blue),
    NegativePointBrush = new SolidColorBrush(Colors.Orange)
};

sparkline.SegmentTemplateSelector = selector;
```

### Available Brush Properties

| Property | Description | Applies To |
|----------|-------------|------------|
| **FirstPointBrush** | Color for first data point | All types |
| **LastPointBrush** | Color for last data point | All types |
| **HighPointBrush** | Color for highest value point | All types |
| **LowPointBrush** | Color for lowest value point | All types |
| **NegativePointBrush** | Color for negative value points | All types |

**Priority Order:**
If a point matches multiple categories (e.g., first point is also the lowest), the brush is applied in this order:
1. FirstPointBrush
2. LastPointBrush
3. HighPointBrush
4. LowPointBrush
5. NegativePointBrush
6. Interior (default)

## Sparkline Type Differences

### Column Sparkline Segments

Each column represents a discrete segment. Segment customization colors individual columns.

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding SalesData}" 
    YBindingPath="Amount"
    Interior="Gray">
    
    <syncfusion:SfColumnSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="LightBlue" 
            LastPointBrush="LightBlue"
            HighPointBrush="Green"
            LowPointBrush="Red"
            NegativePointBrush="DarkRed"/>
    </syncfusion:SfColumnSparkline.SegmentTemplateSelector>
    
</syncfusion:SfColumnSparkline>
```

**Result:** Gray columns with special colors for first, last, high, low, and negative values.

### Line Sparkline Segments

Line sparklines color individual **data points** along the line. This works best when combined with markers.

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding TrendData}" 
    YBindingPath="Value"
    Interior="Blue"
    MarkerVisibility="Visible">
    
    <syncfusion:SfLineSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            HighPointBrush="Red"
            LowPointBrush="Blue"
            NegativePointBrush="Orange"/>
    </syncfusion:SfLineSparkline.SegmentTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Important:** For Line sparklines, segment customization primarily affects **markers**. Use `MarkerVisibility="Visible"` to see the effect clearly.

### Area Sparkline Segments

Similar to Line sparklines, Area sparklines apply segment colors to data points. Most visible with markers enabled.

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding TrendData}" 
    YBindingPath="Value"
    Interior="LightBlue"
    BorderBrush="Blue"
    BorderThickness="2"
    MarkerVisibility="Visible">
    
    <syncfusion:SfAreaSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            HighPointBrush="Green"
            LowPointBrush="Red"
            NegativePointBrush="DarkRed"/>
    </syncfusion:SfAreaSparkline.SegmentTemplateSelector>
    
</syncfusion:SfAreaSparkline>
```

### WinLoss Sparkline Segments

Each segment represents a win, loss, or draw. Segment customization applies colors to individual outcome bars.

```xml
<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding Match}" 
    YBindingPath="Result"
    Interior="Green">
    
    <syncfusion:SfWinLossSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="Gold" 
            LastPointBrush="Gold"
            HighPointBrush="LightGreen"
            LowPointBrush="LightCoral"
            NegativePointBrush="Red"/>
    </syncfusion:SfWinLossSparkline.SegmentTemplateSelector>
    
</syncfusion:SfWinLossSparkline>
```

**Use case:** Gold for first/last games, light green for wins, light coral for losses, red for negative points.

## Special Point Brushes

### First and Last Points

Highlight start and end points to show data range boundaries:

```xml
<syncfusion:SegmentTemplateSelector 
    FirstPointBrush="Yellow" 
    LastPointBrush="Yellow"/>
```

**Use case:** Emphasize where data starts and ends, useful for time-series or sequential data.

### High and Low Points

Highlight extremes in the dataset:

```xml
<syncfusion:SegmentTemplateSelector 
    HighPointBrush="Green"
    LowPointBrush="Red"/>
```

**Use case:** Quickly identify best and worst performance points.

### Negative Points

Highlight all negative values:

```xml
<syncfusion:SegmentTemplateSelector 
    NegativePointBrush="Red"/>
```

**Use case:** Draw attention to losses, deficits, or below-zero values.

### Combining Multiple Special Points

```xml
<syncfusion:SegmentTemplateSelector 
    FirstPointBrush="Blue" 
    LastPointBrush="Blue"
    HighPointBrush="Green"
    LowPointBrush="Red"
    NegativePointBrush="Orange"/>
```

**Result:** A comprehensive visualization with:
- Blue markers for start/end
- Green for highest value
- Red for lowest value
- Orange for all negative values

## Advanced Customization Techniques

### Semantic Color Schemes

**Financial Data:**
```xml
<syncfusion:SegmentTemplateSelector 
    HighPointBrush="Green"
    LowPointBrush="Red"
    NegativePointBrush="DarkRed"
    FirstPointBrush="Gray"
    LastPointBrush="Blue"/>
```

**Traffic Light Pattern:**
```xml
<syncfusion:SegmentTemplateSelector 
    HighPointBrush="#00FF00"
    LowPointBrush="#FF0000"
    FirstPointBrush="#FFA500"
    LastPointBrush="#FFA500"/>
```

**Heatmap Pattern:**
```xml
<syncfusion:SegmentTemplateSelector 
    HighPointBrush="#FF0000"
    LowPointBrush="#0000FF"
    NegativePointBrush="#FFA500"/>
```

### Gradient Brushes for Segments

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value">
    
    <syncfusion:SfColumnSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector>
            <syncfusion:SegmentTemplateSelector.HighPointBrush>
                <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                    <GradientStop Color="LightGreen" Offset="0"/>
                    <GradientStop Color="Green" Offset="1"/>
                </LinearGradientBrush>
            </syncfusion:SegmentTemplateSelector.HighPointBrush>
            
            <syncfusion:SegmentTemplateSelector.LowPointBrush>
                <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
                    <GradientStop Color="LightCoral" Offset="0"/>
                    <GradientStop Color="Red" Offset="1"/>
                </LinearGradientBrush>
            </syncfusion:SegmentTemplateSelector.LowPointBrush>
        </syncfusion:SegmentTemplateSelector>
    </syncfusion:SfColumnSparkline.SegmentTemplateSelector>
    
</syncfusion:SfColumnSparkline>
```

### Dynamic Brush Binding

Bind brushes to ViewModel properties for runtime customization:

```csharp
public class CustomizationViewModel : INotifyPropertyChanged
{
    private Brush _highPointBrush = new SolidColorBrush(Colors.Green);
    
    public Brush HighPointBrush
    {
        get => _highPointBrush;
        set
        {
            _highPointBrush = value;
            OnPropertyChanged(nameof(HighPointBrush));
        }
    }
    
    // Implement INotifyPropertyChanged...
}
```

```xml
<syncfusion:SegmentTemplateSelector 
    HighPointBrush="{Binding HighPointBrush}"
    LowPointBrush="{Binding LowPointBrush}"/>
```

### Conditional Segment Coloring

While not directly supported, you can preprocess data to add category flags and bind multiple sparklines:

```csharp
public class DataPoint
{
    public double Value { get; set; }
    public string Category { get; set; }  // "High", "Low", "Normal"
}
```

Then use multiple sparklines or custom styling based on categories.

## Combining Features

### Segment Customization + Highlighting

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    HighlightSegment="True"
    Interior="Gray">
    
    <syncfusion:SfColumnSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            HighPointBrush="Green"
            LowPointBrush="Red"
            NegativePointBrush="Orange"/>
    </syncfusion:SfColumnSparkline.SegmentTemplateSelector>
    
</syncfusion:SfColumnSparkline>
```

**Result:** Colored special points with hover highlighting on all segments.

### Segment Customization + Range Bands

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="SteelBlue"
    BandRangeStart="5000"
    BandRangeEnd="2000"
    RangeBandBrush="LightGreen">
    
    <syncfusion:SfColumnSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            HighPointBrush="Green"
            LowPointBrush="Red"
            NegativePointBrush="DarkRed"/>
    </syncfusion:SfColumnSparkline.SegmentTemplateSelector>
    
</syncfusion:SfColumnSparkline>
```

**Result:** Range band shows target zone, segment colors highlight extremes and negatives.

### Segment Customization + Markers (Line/Area)

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible"
    Interior="Blue">
    
    <syncfusion:SfLineSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            HighPointBrush="Red"
            LowPointBrush="Blue"
            MarkerHeight="12" 
            MarkerWidth="12"/>
    </syncfusion:SfLineSparkline.SegmentTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Note:** For Line/Area sparklines, use `MarkerTemplateSelector` instead of `SegmentTemplateSelector` for better marker control. See [markers.md](markers.md) for details.

## Best Practices

### When to Use Segment Customization

**Use segment customization when:**
- Highlighting exceptional values (first, last, high, low)
- Drawing attention to negative values or losses
- Color-coding data by significance
- Comparing relative performance across segments
- Creating visual emphasis on key data points

**Avoid segment customization when:**
- Too many special points (loses emphasis)
- Colors don't align with user expectations (e.g., red for "good")
- All segments would be colored (no contrast)
- Using Line/Area sparklines without markers (effect is minimal)

### Color Selection Guidelines

**Contrast:**
- Ensure special point colors contrast with Interior brush
- Use high-contrast colors for critical points (high/low)
- Test visibility on both light and dark themes

**Semantic Meaning:**
- **Red** - Negative, low, danger, loss
- **Green** - Positive, high, success, win
- **Yellow/Orange** - Caution, neutral, first/last
- **Blue** - Information, neutral, reference

**Accessibility:**
- Don't rely solely on color; consider shapes (markers) or patterns
- Ensure sufficient contrast ratios (WCAG 2.1 AA: 4.5:1 minimum)
- Provide alternative indicators (labels, tooltips) for color-blind users

### Performance Considerations

Segment customization has minimal performance impact. For best results:
- Use solid colors instead of gradients when possible
- Avoid overly complex brush definitions
- Limit the number of special point types enabled

### Sparkline Type Recommendations

**Column Sparkline:**
- Best suited for segment customization
- Each column is a distinct visual element
- Combine with `HighlightSegment` for interactivity

**WinLoss Sparkline:**
- Excellent for binary outcome coloring
- Use NegativePointBrush to distinguish losses
- Combine FirstPointBrush/LastPointBrush to show start/end of series

**Line/Area Sparkline:**
- Enable `MarkerVisibility` for segment colors to be visible
- Consider using `MarkerTemplateSelector` instead for finer control
- Segment colors primarily affect markers, not the line itself

## Next Steps

- **[Markers](markers.md)** - Use MarkerTemplateSelector for Line/Area sparklines
- **[Range Band](range-band.md)** - Combine segment colors with range highlighting
- **[Axis Controls](axis-controls.md)** - Add axis lines to provide context
