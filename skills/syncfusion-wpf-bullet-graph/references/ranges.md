# Qualitative Ranges

## Table of Contents
- [Overview](#overview)
- [Creating Ranges](#creating-ranges)
- [Range Properties and Customization](#range-properties-and-customization)
- [Binding Range Colors to Ticks and Labels](#binding-range-colors-to-ticks-and-labels)
- [Common Range Patterns](#common-range-patterns)
- [Best Practices](#best-practices)

## Overview

Qualitative ranges are background color bands that provide contextual meaning to the bullet graph values. They represent qualitative performance levels such as "poor," "satisfactory," and "good," helping users quickly interpret whether values are in acceptable ranges.

### Purpose

Qualitative ranges:
- **Provide visual context** for interpreting numeric values
- **Indicate performance zones** (e.g., below target, on target, exceeds target)
- **Replace complex explanations** with intuitive color coding
- **Support quick decision-making** in dashboard scenarios

### How Ranges Work

- Each range starts where the previous range ended
- The first range starts at the scale `Minimum`
- Each range is defined by its `RangeEnd` value
- Ranges are automatically arranged in ascending order by `RangeEnd`

## Creating Ranges

Ranges are added to the `QualitativeRanges` collection property of the bullet graph.

### Basic Range Configuration

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="5"
    ComparativeMeasure="7"
    QualitativeRangesSize="30">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="Red" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="Yellow" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="1"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

This creates three ranges:
- **Red range:** 0 to 4.5 (poor performance)
- **Yellow range:** 4.5 to 7.5 (satisfactory performance)
- **Green range:** 7.5 to 10 (good performance)

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 7;
bulletGraph.MinorTickSize = 8;
bulletGraph.MinorTicksPerInterval = 3;
bulletGraph.QualitativeRangesSize = 30;

bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 4.5,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Red)
});

bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 7.5,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Yellow)
});

bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 10,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Green)
});
```

### Understanding RangeEnd

The `RangeEnd` property specifies where a range ends (and the next range begins).

**Example with Minimum = 0, Maximum = 100:**
```xml
<syncfusion:QualitativeRange RangeEnd="30" RangeStroke="Red"/>      <!-- Range: 0 to 30 -->
<syncfusion:QualitativeRange RangeEnd="70" RangeStroke="Yellow"/>   <!-- Range: 30 to 70 -->
<syncfusion:QualitativeRange RangeEnd="100" RangeStroke="Green"/>   <!-- Range: 70 to 100 -->
```

### Using RangeStart (Optional)

While ranges typically start where the previous range ended, you can explicitly set `RangeStart`:

```xml
<syncfusion:QualitativeRange RangeStart="0" RangeEnd="40" RangeStroke="Red"/>
<syncfusion:QualitativeRange RangeStart="40" RangeEnd="75" RangeStroke="Yellow"/>
<syncfusion:QualitativeRange RangeStart="75" RangeEnd="100" RangeStroke="Green"/>
```

**Note:** Explicitly setting `RangeStart` is optional and typically unnecessary unless you need gaps or overlaps between ranges.

## Range Properties and Customization

### Range Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `RangeStart` | double | Starting value of the range | Previous range's RangeEnd (or Minimum) |
| `RangeEnd` | double | Ending value of the range | Required |
| `RangeStroke` | Brush | Color of the range | Gray |
| `RangeOpacity` | double | Opacity of the range (0.0 to 1.0) | 1.0 |
| `RangeCaption` | string | Optional text label for the range | null |

### Bullet Graph Range Container Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `QualitativeRanges` | Collection | Collection of QualitativeRange objects | Empty |
| `QualitativeRangesSize` | double | Height (horizontal) or width (vertical) of range bands | 32 |

### Customizing Range Size

The `QualitativeRangesSize` property controls the thickness of the range bands:

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    QualitativeRangesSize="50"
    Minimum="0" 
    Maximum="100">
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="50" RangeStroke="#FFEBEE"/>
        <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#E8F5E9"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**Common sizes:**
- Small/compact: 20-25 pixels
- Standard: 30-35 pixels
- Large/prominent: 40-50 pixels

### Customizing Range Opacity

Control the transparency of ranges for subtle backgrounds:

**XAML:**
```xml
<syncfusion:SfBulletGraph QualitativeRangesSize="30">
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="30" RangeStroke="Red" RangeOpacity="0.3"/>
        <syncfusion:QualitativeRange RangeEnd="70" RangeStroke="Yellow" RangeOpacity="0.3"/>
        <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="Green" RangeOpacity="0.3"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 30,
    RangeStroke = new SolidColorBrush(Colors.Red),
    RangeOpacity = 0.3
});
```

**Opacity guidelines:**
- 0.2-0.3: Very subtle, non-distracting background
- 0.5-0.7: Moderate visibility, balanced
- 0.8-1.0: Bold, prominent ranges

### Using Hex Colors for Precise Control

**XAML:**
```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="35" RangeStroke="#EBEBEB"/>
    <syncfusion:QualitativeRange RangeEnd="70" RangeStroke="#D8D8D8"/>
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#7F7F7F"/>
</syncfusion:SfBulletGraph.QualitativeRanges>
```

**C#:**
```csharp
QualitativeRange range1 = new QualitativeRange();
range1.RangeEnd = 35;
range1.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#EBEBEB");

QualitativeRange range2 = new QualitativeRange();
range2.RangeEnd = 70;
range2.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#D8D8D8");

bulletGraph.QualitativeRanges.Add(range1);
bulletGraph.QualitativeRanges.Add(range2);
```

## Binding Range Colors to Ticks and Labels

You can synchronize the colors of tick marks and labels with the colors of the qualitative ranges, creating a cohesive color-coded scale.

### BindRangeStrokeToLabels

When set to `true`, labels are colored to match the range they fall within.

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    BindRangeStrokeToLabels="True"
    QualitativeRangesSize="30"
    FeaturedMeasure="5"
    ComparativeMeasure="7">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="Red" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="Yellow" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="1"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
bulletGraph.BindRangeStrokeToLabels = true;
```

Labels at 0, 2, and 4 will be red; labels at 6 will be yellow; labels at 8 and 10 will be green.

### BindRangeStrokeToTicks

When set to `true`, tick marks are colored to match the range they fall within.

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    BindRangeStrokeToTicks="True"
    BindRangeStrokeToLabels="True"
    QualitativeRangesSize="30"
    MinorTicksPerInterval="3">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="Red" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="Yellow" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="1"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
bulletGraph.BindRangeStrokeToTicks = true;
bulletGraph.BindRangeStrokeToLabels = true;
```

Both major and minor ticks will be colored according to the range they're positioned in.

### Combined Example

```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 7;
bulletGraph.Interval = 2;
bulletGraph.MinorTickSize = 8;
bulletGraph.MinorTicksPerInterval = 3;
bulletGraph.QualitativeRangesSize = 30;
bulletGraph.BindRangeStrokeToLabels = true;
bulletGraph.BindRangeStrokeToTicks = true;

bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 4.5,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Red)
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 7.5,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Yellow)
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 10,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Green)
});
```

## Common Range Patterns

### Three-Level Performance (Bad/Satisfactory/Good)

**Traditional colors:**
```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="40" RangeStroke="Red" RangeOpacity="0.7"/>
    <syncfusion:QualitativeRange RangeEnd="75" RangeStroke="Yellow" RangeOpacity="0.7"/>
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="Green" RangeOpacity="0.7"/>
</syncfusion:SfBulletGraph.QualitativeRanges>
```

**Modern colors:**
```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="40" RangeStroke="#E74C3C"/>
    <syncfusion:QualitativeRange RangeEnd="75" RangeStroke="#F39C12"/>
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#27AE60"/>
</syncfusion:SfBulletGraph.QualitativeRanges>
```

**Pastel/subtle colors:**
```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="40" RangeStroke="#FFEBEE"/>
    <syncfusion:QualitativeRange RangeEnd="75" RangeStroke="#FFF9C4"/>
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#E8F5E9"/>
</syncfusion:SfBulletGraph.QualitativeRanges>
```

### Five-Level Detailed Performance

```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="20" RangeStroke="#C62828"/>     <!-- Critical -->
    <syncfusion:QualitativeRange RangeEnd="40" RangeStroke="#F57C00"/>     <!-- Poor -->
    <syncfusion:QualitativeRange RangeEnd="60" RangeStroke="#FBC02D"/>     <!-- Fair -->
    <syncfusion:QualitativeRange RangeEnd="80" RangeStroke="#689F38"/>     <!-- Good -->
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#2E7D32"/>    <!-- Excellent -->
</syncfusion:SfBulletGraph.QualitativeRanges>
```

### Two-Level Binary Status (Below/Above Target)

```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="75" RangeStroke="#FFCDD2"/>     <!-- Below target -->
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#C8E6C9"/>    <!-- Above target -->
</syncfusion:SfBulletGraph.QualitativeRanges>
```

### Grayscale/Monochrome Ranges

```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="35" RangeStroke="#EBEBEB"/>
    <syncfusion:QualitativeRange RangeEnd="70" RangeStroke="#BDBDBD"/>
    <syncfusion:QualitativeRange RangeEnd="100" RangeStroke="#757575"/>
</syncfusion:SfBulletGraph.QualitativeRanges>
```

### Budget Ranges (Revenue)

```xml
<syncfusion:SfBulletGraph 
    Minimum="0" 
    Maximum="500" 
    Interval="100"
    FeaturedMeasure="375"
    ComparativeMeasure="400">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="200" RangeStroke="#FFEBEE" RangeOpacity="1"/>  <!-- Far below -->
        <syncfusion:QualitativeRange RangeEnd="350" RangeStroke="#FFF9C4" RangeOpacity="1"/>  <!-- Below -->
        <syncfusion:QualitativeRange RangeEnd="500" RangeStroke="#E8F5E9" RangeOpacity="1"/>  <!-- On/above -->
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

### Expense Ranges (Inverted Logic)

For expenses, lower is better:

```xml
<syncfusion:SfBulletGraph.QualitativeRanges>
    <syncfusion:QualitativeRange RangeEnd="5000" RangeStroke="#E8F5E9"/>   <!-- Well under budget -->
    <syncfusion:QualitativeRange RangeEnd="8000" RangeStroke="#FFF9C4"/>   <!-- Near budget -->
    <syncfusion:QualitativeRange RangeEnd="10000" RangeStroke="#FFEBEE"/>  <!-- Over budget -->
</syncfusion:SfBulletGraph.QualitativeRanges>
```

## Best Practices

### Color Selection

1. **Use intuitive color associations:**
   - Red/warm colors: Poor, danger, over budget
   - Yellow/amber: Caution, satisfactory, near target
   - Green/cool colors: Good, safe, under budget

2. **Ensure sufficient contrast** between adjacent ranges

3. **Consider accessibility:**
   - Don't rely solely on color (use tooltips, labels)
   - Test with colorblind-friendly palettes
   - Provide adequate contrast ratios

4. **Match your application theme:**
   - Use brand colors where appropriate
   - Coordinate with overall dashboard design
   - Consider light vs. dark themes

### Range Division

1. **Use 2-5 ranges typically:**
   - 2 ranges: Binary (pass/fail, above/below)
   - 3 ranges: Standard (poor/satisfactory/good)
   - 4-5 ranges: Detailed granularity

2. **Divide ranges logically:**
   - Equal divisions (0-33, 33-66, 66-100)
   - Weighted by importance (0-20, 20-80, 80-100)
   - Based on business rules (e.g., 0-70% = red, 70-90% = yellow, 90-100% = green)

3. **Align ranges with business goals:**
   - Set thresholds based on actual targets
   - Consider industry standards or benchmarks
   - Review and adjust based on data trends

### Size and Opacity

1. **Standard size:** 30-35 pixels for most dashboards
2. **Subtle backgrounds:** Use 0.2-0.4 opacity for non-intrusive ranges
3. **Prominent zones:** Use 0.7-1.0 opacity when ranges are primary indicators

### Performance

1. **Limit to 5-7 ranges maximum** to avoid visual clutter
2. **Ranges are lightweight** - performance impact is minimal
3. **Use consistent range definitions** across related bullet graphs

## Troubleshooting

**Issue:** Ranges not visible  
**Solution:** Check that `QualitativeRangesSize` is set to a reasonable value (30-50). Verify range colors contrast with the background.

**Issue:** Ranges appear in wrong order  
**Solution:** Ensure `RangeEnd` values are in ascending order. The control automatically arranges ranges, but verify your values are correct.

**Issue:** Gap between ranges  
**Solution:** Verify each range's `RangeEnd` matches the next range's implicit start. Check for missing ranges in the collection.

**Issue:** Range extends beyond scale  
**Solution:** The last range's `RangeEnd` should equal or be less than the `Maximum` property of the bullet graph.

**Issue:** Colors too intense/distracting  
**Solution:** Reduce `RangeOpacity` to 0.3-0.5 for subtler backgrounds, or use pastel/lighter color variants.
