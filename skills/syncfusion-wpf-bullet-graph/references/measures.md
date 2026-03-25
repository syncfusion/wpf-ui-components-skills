# Performance Measures

## Table of Contents
- [Overview](#overview)
- [Featured Measure](#featured-measure)
- [Comparative Measure](#comparative-measure)
- [Visual Hierarchy and Layering](#visual-hierarchy-and-layering)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Performance measures are the core data visualization elements in a bullet graph. They display two types of values:

1. **Featured Measure:** The primary data value you're measuring (e.g., current revenue, actual performance)
2. **Comparative Measure:** The target or comparison value (e.g., goal, budget, target)

These measures work together to show how the current performance relates to the target within the context of qualitative ranges.

## Featured Measure

The featured measure is the primary data point displayed in the bullet graph. It should be the most visually prominent element, typically rendered as a thick horizontal or vertical bar.

### Purpose

The featured measure represents:
- **Current performance** (e.g., year-to-date revenue)
- **Actual values** (e.g., actual expenses)
- **Current progress** (e.g., completed tasks)
- **Primary metric** being tracked

### Featured Measure Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `FeaturedMeasure` | double | The value to display as the primary measure | 0 |
| `FeaturedMeasureBarStroke` | Brush | Color of the featured measure bar | Gray |
| `FeaturedMeasureBarStrokeThickness` | double | Thickness of the featured measure bar | 6 |

### Basic Featured Measure

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="5">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 5;
```

### Customizing Featured Measure Appearance

**XAML with custom styling:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="5" 
    FeaturedMeasureBarStroke="Red" 
    FeaturedMeasureBarStrokeThickness="10"
    ComparativeMeasure="7"
    MinorTicksPerInterval="3"
    MinorTickSize="10" 
    MajorTickSize="14">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="#EBEBEB"/>
        <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="#D8D8D8"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#7F7F7F"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C# with custom styling:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 7;
bulletGraph.FeaturedMeasureBarStroke = new SolidColorBrush(Colors.Red);
bulletGraph.FeaturedMeasureBarStrokeThickness = 10;
bulletGraph.MinorTicksPerInterval = 3;
bulletGraph.MinorTickSize = 10;
bulletGraph.MajorTickSize = 14;

QualitativeRange range1 = new QualitativeRange();
range1.RangeEnd = 4.5;
range1.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#EBEBEB");

QualitativeRange range2 = new QualitativeRange();
range2.RangeEnd = 7.5;
range2.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#D8D8D8");

QualitativeRange range3 = new QualitativeRange();
range3.RangeEnd = 10;
range3.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#7F7F7F");

bulletGraph.QualitativeRanges.Add(range1);
bulletGraph.QualitativeRanges.Add(range2);
bulletGraph.QualitativeRanges.Add(range3);
```

### Featured Measure Color Patterns

**Success indicator (green):**
```xml
<syncfusion:SfBulletGraph 
    FeaturedMeasure="85"
    FeaturedMeasureBarStroke="#27AE60"
    FeaturedMeasureBarStrokeThickness="8">
</syncfusion:SfBulletGraph>
```

**Warning indicator (orange/yellow):**
```xml
<syncfusion:SfBulletGraph 
    FeaturedMeasure="65"
    FeaturedMeasureBarStroke="#F39C12"
    FeaturedMeasureBarStrokeThickness="8">
</syncfusion:SfBulletGraph>
```

**Danger indicator (red):**
```xml
<syncfusion:SfBulletGraph 
    FeaturedMeasure="35"
    FeaturedMeasureBarStroke="#E74C3C"
    FeaturedMeasureBarStrokeThickness="8">
</syncfusion:SfBulletGraph>
```

**Neutral/professional (dark):**
```xml
<syncfusion:SfBulletGraph 
    FeaturedMeasure="70"
    FeaturedMeasureBarStroke="#2C3E50"
    FeaturedMeasureBarStrokeThickness="8">
</syncfusion:SfBulletGraph>
```

## Comparative Measure

The comparative measure serves as a reference point or target against which the featured measure is compared. It should be less visually dominant than the featured measure and is typically rendered as a short perpendicular line.

### Purpose

The comparative measure represents:
- **Target values** (e.g., revenue target)
- **Budgeted amounts** (e.g., expense budget)
- **Goals** (e.g., performance goal)
- **Previous period values** (e.g., last year's revenue)
- **Benchmarks** (e.g., industry standard)

### Comparative Measure Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `ComparativeMeasure` | double | The value to display as the comparison marker | 0 |
| `ComparativeMeasureSymbolStroke` | Brush | Color of the comparative measure marker | Gray |
| `ComparativeMeasureSymbolStrokeThickness` | double | Thickness of the comparative measure marker | 5 |

### Basic Comparative Measure

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    FeaturedMeasure="5"
    ComparativeMeasure="7">
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 7;
```

### Customizing Comparative Measure Appearance

**XAML with custom styling:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="5" 
    FeaturedMeasureBarStroke="Black" 
    ComparativeMeasure="7"
    ComparativeMeasureSymbolStrokeThickness="6" 
    ComparativeMeasureSymbolStroke="Red" 
    MinorTicksPerInterval="3"
    MinorTickSize="10" 
    MajorTickSize="14">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="#EBEBEB"/>
        <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="#D8D8D8"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#7F7F7F"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C# with custom styling:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 7;
bulletGraph.ComparativeMeasureSymbolStroke = new SolidColorBrush(Colors.Red);
bulletGraph.ComparativeMeasureSymbolStrokeThickness = 6;
bulletGraph.FeaturedMeasureBarStroke = new SolidColorBrush(Colors.Black);
bulletGraph.MinorTicksPerInterval = 3;
bulletGraph.MinorTickSize = 10;
bulletGraph.MajorTickSize = 14;

QualitativeRange range1 = new QualitativeRange();
range1.RangeEnd = 4.5;
range1.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#EBEBEB");

QualitativeRange range2 = new QualitativeRange();
range2.RangeEnd = 7.5;
range2.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#D8D8D8");

QualitativeRange range3 = new QualitativeRange();
range3.RangeEnd = 10;
range3.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#7F7F7F");

bulletGraph.QualitativeRanges.Add(range1);
bulletGraph.QualitativeRanges.Add(range2);
bulletGraph.QualitativeRanges.Add(range3);
```

## Visual Hierarchy and Layering

### Rendering Order

When the featured measure and comparative measure overlap:
1. **Comparative measure appears behind** the featured measure
2. Featured measure is always on top for prominence
3. This ensures the primary data remains most visible

### Visual Dominance

The featured measure should always be more visually prominent:
- **Thicker bar** (typically 6-10px)
- **Solid, strong color**
- **Extends across full value**

The comparative measure should be subtle:
- **Thinner marker** (typically 3-6px)
- **Short perpendicular line**
- **Less saturated color**

### Example: Proper Visual Hierarchy

```xml
<syncfusion:SfBulletGraph 
    FeaturedMeasure="75"
    FeaturedMeasureBarStroke="#2C3E50"
    FeaturedMeasureBarStrokeThickness="8"
    ComparativeMeasure="80"
    ComparativeMeasureSymbolStroke="#95A5A6"
    ComparativeMeasureSymbolStrokeThickness="4">
</syncfusion:SfBulletGraph>
```

Here, the featured measure (current: 75) is bold and dark, while the comparative measure (target: 80) is lighter and thinner.

## Complete Examples

### Revenue Tracking Example

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="500" 
    Interval="100"
    FeaturedMeasure="375"
    FeaturedMeasureBarStroke="#27AE60"
    FeaturedMeasureBarStrokeThickness="10"
    ComparativeMeasure="400"
    ComparativeMeasureSymbolStroke="#E74C3C"
    ComparativeMeasureSymbolStrokeThickness="5"
    MinorTicksPerInterval="4">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="200" RangeStroke="#FFEBEE" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="350" RangeStroke="#FFF9C4" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="500" RangeStroke="#E8F5E9" RangeOpacity="1"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

This shows:
- Current revenue: $375K (green bar)
- Target revenue: $400K (red marker)
- Visual interpretation: Close to target, in "good" range

### Expense Budget Example

```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10000;
bulletGraph.Interval = 2000;
bulletGraph.FeaturedMeasure = 7500; // Actual spending
bulletGraph.ComparativeMeasure = 8000; // Budget limit
bulletGraph.FeaturedMeasureBarStroke = new SolidColorBrush(Colors.OrangeRed);
bulletGraph.FeaturedMeasureBarStrokeThickness = 8;
bulletGraph.ComparativeMeasureSymbolStroke = new SolidColorBrush(Colors.DarkRed);
bulletGraph.ComparativeMeasureSymbolStrokeThickness = 4;

// Add ranges: Under budget (good), Near budget (warning), Over budget (danger)
bulletGraph.QualitativeRanges.Add(new QualitativeRange() 
{ 
    RangeEnd = 6000, 
    RangeStroke = new SolidColorBrush(Color.FromRgb(200, 230, 201)) 
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange() 
{ 
    RangeEnd = 8500, 
    RangeStroke = new SolidColorBrush(Color.FromRgb(255, 245, 157)) 
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange() 
{ 
    RangeEnd = 10000, 
    RangeStroke = new SolidColorBrush(Color.FromRgb(255, 205, 210)) 
});
```

### Performance Score Example

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="100" 
    Interval="25"
    FeaturedMeasure="72"
    FeaturedMeasureBarStroke="#3498DB"
    FeaturedMeasureBarStrokeThickness="9"
    ComparativeMeasure="85"
    ComparativeMeasureSymbolStroke="#2C3E50"
    ComparativeMeasureSymbolStrokeThickness="5"
    MinorTicksPerInterval="4">
</syncfusion:SfBulletGraph>
```

## Best Practices

### Featured Measure

1. **Use bold, saturated colors** to ensure prominence
2. **Set thickness between 6-10px** for optimal visibility
3. **Choose colors based on performance:**
   - Green for positive metrics (revenue, completion)
   - Red for negative metrics (expenses, errors)
   - Neutral colors (blue, dark gray) for general tracking

4. **Ensure the value is within scale range** (between Minimum and Maximum)

### Comparative Measure

1. **Use less saturated colors** than the featured measure
2. **Set thickness between 3-6px** (thinner than featured measure)
3. **Position strategically:**
   - At target value for goal tracking
   - At budget value for expense management
   - At previous period value for trend comparison

4. **Coordinate with featured measure color:**
   - Use contrasting colors if they're close in value
   - Use complementary colors for visual harmony

### Value Selection

1. **Featured measure should be dynamic** - bind to current data
2. **Comparative measure can be static or dynamic:**
   - Static: Fixed target or budget
   - Dynamic: Previous period or calculated benchmark

3. **Consider the relationship:**
   - Featured > Comparative: Exceeding target (good for revenue, bad for expenses)
   - Featured < Comparative: Below target (bad for revenue, good for expenses)
   - Featured ≈ Comparative: On target

## Troubleshooting

**Issue:** Measures not visible  
**Solution:** Ensure measure values are within the Minimum/Maximum range. Check that stroke colors contrast with the background.

**Issue:** Comparative measure obscured by featured measure  
**Solution:** This is expected behavior when values overlap. The featured measure appears on top. Consider using tooltips to display both values clearly.

**Issue:** Featured measure bar appears too thin or thick  
**Solution:** Adjust `FeaturedMeasureBarStrokeThickness`. Recommended range: 6-10 pixels for most dashboards.

**Issue:** Colors don't match application theme  
**Solution:** Use theme-compatible colors or bind strokes to application resources for consistent theming.
