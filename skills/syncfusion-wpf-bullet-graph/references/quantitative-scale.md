# Quantitative Scale Configuration

## Table of Contents
- [Overview](#overview)
- [Scale Range Configuration](#scale-range-configuration)
- [Major Ticks](#major-ticks)
- [Minor Ticks](#minor-ticks)
- [Labels](#labels)
- [Scale Length](#scale-length)
- [Complete Examples](#complete-examples)

## Overview

The quantitative scale is the numerical axis of the bullet graph that provides the measurement context for the featured and comparative measures. It consists of major ticks, minor ticks, labels, and the scale range.

The quantitative scale indicators specify numeric values along the scale range, helping users interpret the performance measures in context.

## Scale Range Configuration

The scale range defines the minimum and maximum values displayed on the bullet graph.

### Key Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Minimum` | double | Starting value of the scale | 0 |
| `Maximum` | double | Ending value of the scale | 100 |
| `Interval` | double | Distance between major tick marks | 10 |

### Basic Scale Configuration

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="20"
    FeaturedMeasure="65">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 100;
bulletGraph.Interval = 20;
bulletGraph.FeaturedMeasure = 65;
```

This creates a scale from 0 to 100 with major ticks at 0, 20, 40, 60, 80, and 100.

### Custom Range Examples

**Revenue scale (thousands of dollars):**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="500" 
    Interval="100"
    FeaturedMeasure="325"
    ComparativeMeasure="400">
</syncfusion:SfBulletGraph>
```

**Percentage scale:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="25"
    FeaturedMeasure="75"
    ComparativeMeasure="80">
</syncfusion:SfBulletGraph>
```

**Non-zero starting scale:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="50" 
    Maximum="150" 
    Interval="25"
    FeaturedMeasure="110">
</syncfusion:SfBulletGraph>
```

## Major Ticks

Major ticks are the primary scale indicators placed at intervals along the quantitative scale. They mark significant values and help users read the scale accurately.

### Major Tick Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `MajorTickSize` | double | Length of major tick marks | 13 |
| `MajorTickStroke` | Brush | Color of major tick marks | Gray |
| `MajorTickStrokeThickness` | double | Thickness of major tick lines | 1 |

### Customizing Major Ticks

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    MajorTickSize="15"
    MajorTickStroke="Red"
    MajorTickStrokeThickness="2"
    FeaturedMeasure="6">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.MajorTickSize = 15;
bulletGraph.MajorTickStroke = new SolidColorBrush(Colors.Red);
bulletGraph.MajorTickStrokeThickness = 2;
bulletGraph.FeaturedMeasure = 6;
```

### Major Tick Styling Examples

**Bold major ticks:**
```xml
<syncfusion:SfBulletGraph 
    MajorTickSize="20"
    MajorTickStrokeThickness="3"
    MajorTickStroke="#333333">
</syncfusion:SfBulletGraph>
```

**Subtle major ticks:**
```xml
<syncfusion:SfBulletGraph 
    MajorTickSize="10"
    MajorTickStrokeThickness="1"
    MajorTickStroke="#CCCCCC">
</syncfusion:SfBulletGraph>
```

## Minor Ticks

Minor ticks are secondary scale indicators placed between major ticks, providing finer granularity for reading values.

### Minor Tick Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `MinorTicksPerInterval` | int | Number of minor ticks between major ticks | 0 |
| `MinorTickSize` | double | Length of minor tick marks | 7 |
| `MinorTickStroke` | Brush | Color of minor tick marks | Gray |
| `MinorTickStrokeThickness` | double | Thickness of minor tick lines | 1 |

### Adding Minor Ticks

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    MinorTicksPerInterval="3"
    MinorTickSize="10"
    MinorTickStroke="Green"
    MajorTickSize="15"
    MajorTickStroke="Red"
    FeaturedMeasure="5">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.MinorTicksPerInterval = 3;
bulletGraph.MinorTickSize = 10;
bulletGraph.MinorTickStroke = new SolidColorBrush(Colors.Green);
bulletGraph.MajorTickSize = 15;
bulletGraph.MajorTickStroke = new SolidColorBrush(Colors.Red);
bulletGraph.FeaturedMeasure = 5;
```

### Understanding MinorTicksPerInterval

The `MinorTicksPerInterval` property determines how many minor ticks appear between each pair of major ticks.

**Example with Interval = 10:**
- `MinorTicksPerInterval="1"` → One minor tick at 5 (midpoint)
- `MinorTicksPerInterval="4"` → Four minor ticks at 2, 4, 6, 8
- `MinorTicksPerInterval="9"` → Nine minor ticks at 1, 2, 3, 4, 5, 6, 7, 8, 9

**Recommended for percentage scales:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="25"
    MinorTicksPerInterval="4">
    <!-- Creates ticks at every 5% (25/5 = 5) -->
</syncfusion:SfBulletGraph>
```

### Minor Tick Styling

**Contrasting tick colors:**
```xml
<syncfusion:SfBulletGraph 
    MajorTickStroke="#2C3E50"
    MajorTickSize="14"
    MinorTickStroke="#95A5A6"
    MinorTickSize="8"
    MinorTicksPerInterval="4">
</syncfusion:SfBulletGraph>
```

**Uniform tick appearance:**
```xml
<syncfusion:SfBulletGraph 
    MajorTickStroke="Black"
    MajorTickSize="12"
    MinorTickStroke="Black"
    MinorTickSize="6"
    MinorTicksPerInterval="3">
</syncfusion:SfBulletGraph>
```

## Labels

Labels display the numeric values at major tick positions, helping users interpret the scale.

### Label Properties

| Property | Type | Description |
|----------|------|-------------|
| `LabelStroke` | Brush | Color of the scale labels |
| `LabelFontSize` | double | Font size of labels |
| `LabelOffset` | double | Distance between labels and scale |

### Customizing Labels

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="20"
    LabelStroke="#2C3E50"
    FeaturedMeasure="75">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 100;
bulletGraph.Interval = 20;
bulletGraph.LabelStroke = (Brush)new BrushConverter().ConvertFrom("#2C3E50");
bulletGraph.FeaturedMeasure = 75;
```

### Label Styling Examples

**High contrast labels:**
```xml
<syncfusion:SfBulletGraph 
    LabelStroke="Black"
    MajorTickStroke="Black">
</syncfusion:SfBulletGraph>
```

**Subtle labels:**
```xml
<syncfusion:SfBulletGraph 
    LabelStroke="#999999"
    MajorTickStroke="#CCCCCC">
</syncfusion:SfBulletGraph>
```

## Scale Length

The `QuantitativeScaleLength` property controls the pixel length of the scale axis.

### Setting Scale Length

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    QuantitativeScaleLength="300"
    Minimum="0" 
    Maximum="100"
    FeaturedMeasure="65">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.QuantitativeScaleLength = 300;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 100;
bulletGraph.FeaturedMeasure = 65;
```

**Common Use Cases:**
- **Dashboard panels:** Set to 250-350px for compact displays
- **Full-width displays:** Set to match container width
- **Responsive layouts:** Bind to parent container dimensions

## Complete Examples

### Revenue Analysis Dashboard

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="500" 
    Interval="100"
    MinorTicksPerInterval="4"
    MajorTickSize="14"
    MajorTickStroke="#34495E"
    MinorTickSize="8"
    MinorTickStroke="#95A5A6"
    LabelStroke="#2C3E50"
    QuantitativeScaleLength="400"
    FeaturedMeasure="375"
    ComparativeMeasure="400"
    FeaturedMeasureBarStroke="#27AE60">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="200" RangeStroke="#E74C3C" RangeOpacity="0.3"/>
        <syncfusion:QualitativeRange RangeEnd="350" RangeStroke="#F39C12" RangeOpacity="0.3"/>
        <syncfusion:QualitativeRange RangeEnd="500" RangeStroke="#27AE60" RangeOpacity="0.3"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

### Simple Percentage Tracker

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="25"
    MinorTicksPerInterval="4"
    MajorTickSize="12"
    MinorTickSize="6"
    FeaturedMeasure="72"
    ComparativeMeasure="80">
</syncfusion:SfBulletGraph>
```

### High-Precision Scale with Many Ticks

```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 1;
bulletGraph.MinorTicksPerInterval = 9; // Tick at every 0.1
bulletGraph.MajorTickSize = 15;
bulletGraph.MinorTickSize = 8;
bulletGraph.FeaturedMeasure = 7.3;
bulletGraph.ComparativeMeasure = 8.5;
bulletGraph.QuantitativeScaleLength = 500;
```

## Best Practices

1. **Choose appropriate intervals:** Select intervals that make sense for your data domain (e.g., multiples of 5, 10, 25, 100)

2. **Use minor ticks sparingly:** Too many minor ticks create visual clutter. Typically 1-4 minor ticks per interval is sufficient

3. **Maintain visual hierarchy:** Major ticks should be more prominent than minor ticks (larger size, bolder stroke)

4. **Match label colors to design:** Coordinate label and tick colors with your application's theme

5. **Set appropriate scale length:** Ensure the scale is long enough to be readable but compact enough for dashboard layouts

6. **Consider your data range:** Use non-zero minimum values when appropriate (e.g., 50-150 for temperature in Fahrenheit)

## Troubleshooting

**Issue:** Labels are overlapping  
**Solution:** Increase the `Interval` value or decrease the `QuantitativeScaleLength` to provide more space between labels.

**Issue:** Minor ticks are not visible  
**Solution:** Ensure `MinorTicksPerInterval` is set to a value greater than 0 and `MinorTickSize` is large enough to be visible.

**Issue:** Scale appears too compact  
**Solution:** Increase the `QuantitativeScaleLength` property or adjust the parent container's size.
