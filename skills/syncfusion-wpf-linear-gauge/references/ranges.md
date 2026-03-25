# Ranges

Comprehensive guide to implementing and customizing ranges in SfLinearGauge. Ranges are visual elements that highlight specific value sections on the scale with colors, ideal for creating threshold zones, color-coded regions, and visual indicators.

## Table of Contents
- [Range Overview](#range-overview)
- [Setting Start and End Values](#setting-start-and-end-values)
- [Range Customization](#range-customization)
- [Binding Range Colors to Ticks and Labels](#binding-range-colors-to-ticks-and-labels)
- [Range Positioning](#range-positioning)
- [Adding Multiple Ranges](#adding-multiple-ranges)
- [Complete Examples](#complete-examples)

## Range Overview

**Ranges** are colored sections that span from a start value to an end value on the scale. They provide visual context for value interpretation:

- **Thresholds** - Safe/warning/danger zones
- **Categories** - Performance levels (poor/fair/good/excellent)
- **Zones** - Temperature ranges, pressure zones, etc.
- **Visual guides** - Help users quickly interpret values

**Key characteristics:**
- Defined by `StartValue` and `EndValue`
- Customizable color, width, and opacity
- Can be positioned above, below, or offset from the scale
- Support multiple ranges on a single scale
- Can bind colors to ticks and labels

## Setting Start and End Values

Define the range boundaries with **StartValue** and **EndValue** properties.

### Basic Range

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            ScaleBarStroke="#E0E0E0" 
            MajorTickStroke="Gray"
            MinorTickStroke="Gray" 
            LabelStroke="#424242"
            ScaleBarSize="10" 
            MinorTicksPerInterval="3">
            
            <gauge:LinearScale.Ranges>
                <gauge:LinearRange 
                    StartValue="0"
                    EndValue="60"
                    RangeStroke="#27BEB7"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.4" />
            </gauge:LinearScale.Ranges>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**C#:**

```csharp
SfLinearGauge gauge = new SfLinearGauge();

LinearScale scale = new LinearScale
{
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};

LinearRange range = new LinearRange
{
    StartValue = 0,
    EndValue = 60,
    RangeStroke = new SolidColorBrush(Color.FromRgb(39, 190, 183)),
    StartWidth = 10,
    EndWidth = 10,
    RangeOffset = 0.4
};

scale.Ranges.Add(range);
gauge.MainScale = scale;
```

**Properties:**
- **StartValue** - Beginning of the range on the scale
- **EndValue** - End of the range on the scale
- Range spans from StartValue to EndValue (inclusive)

### Range Value Examples

**Temperature zones:**

```xml
<!-- Cold zone -->
<gauge:LinearRange 
    StartValue="-20" 
    EndValue="10"
    RangeStroke="#2196F3" />

<!-- Normal zone -->
<gauge:LinearRange 
    StartValue="10" 
    EndValue="30"
    RangeStroke="#4CAF50" />

<!-- Hot zone -->
<gauge:LinearRange 
    StartValue="30" 
    EndValue="50"
    RangeStroke="#FF5722" />
```

**Percentage thresholds:**

```xml
<!-- Low (0-25%) -->
<gauge:LinearRange 
    StartValue="0" 
    EndValue="25"
    RangeStroke="Red" />

<!-- Medium (25-75%) -->
<gauge:LinearRange 
    StartValue="25" 
    EndValue="75"
    RangeStroke="Yellow" />

<!-- High (75-100%) -->
<gauge:LinearRange 
    StartValue="75" 
    EndValue="100"
    RangeStroke="Green" />
```

## Range Customization

Customize the visual appearance of ranges.

### Color and Width

```xml
<gauge:LinearRange 
    StartValue="0"
    EndValue="50"
    RangeStroke="#F95C85"
    StartWidth="5"
    EndWidth="20"
    RangeOpacity="1" />
```

**C#:**

```csharp
LinearRange customRange = new LinearRange
{
    StartValue = 0,
    EndValue = 50,
    RangeStroke = new SolidColorBrush(Color.FromRgb(249, 92, 133)),
    StartWidth = 5,
    EndWidth = 20,
    RangeOpacity = 1
};
scale.Ranges.Add(customRange);
```

### Key Customization Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **RangeStroke** | Brush | Color of the range | Black |
| **StartWidth** | double | Width at the start of the range | 10 |
| **EndWidth** | double | Width at the end of the range | 10 |
| **RangeOpacity** | double | Transparency (0.0 to 1.0) | 1.0 |
| **RangeOffset** | double | Distance from scale bar (multiplier) | 0 |

### Tapered Ranges

Create ranges that change width along their length:

```xml
<!-- Widening range -->
<gauge:LinearRange 
    StartValue="0"
    EndValue="50"
    RangeStroke="#4CAF50"
    StartWidth="5"
    EndWidth="25"
    RangeOffset="0.3" />

<!-- Narrowing range -->
<gauge:LinearRange 
    StartValue="50"
    EndValue="100"
    RangeStroke="#FF9800"
    StartWidth="25"
    EndWidth="5"
    RangeOffset="0.3" />
```

**Use cases for tapered ranges:**
- Indicating increasing/decreasing severity
- Creating funnel or convergence effects
- Visual emphasis on specific portions

### Uniform Width Ranges

Most common pattern - consistent width throughout:

```xml
<gauge:LinearRange 
    StartValue="0"
    EndValue="100"
    RangeStroke="#2196F3"
    StartWidth="15"
    EndWidth="15" />
```

### Range Opacity

Control transparency for layered or subtle effects:

```xml
<!-- Semi-transparent range -->
<gauge:LinearRange 
    StartValue="0"
    EndValue="50"
    RangeStroke="Red"
    StartWidth="12"
    EndWidth="12"
    RangeOpacity="0.5" />

<!-- Fully opaque range -->
<gauge:LinearRange 
    StartValue="50"
    EndValue="100"
    RangeStroke="Green"
    StartWidth="12"
    EndWidth="12"
    RangeOpacity="1.0" />
```

### Gradient Ranges

Use gradient brushes for advanced effects:

```xml
<gauge:LinearRange StartValue="0" EndValue="100" StartWidth="15" EndWidth="15">
    <gauge:LinearRange.RangeStroke>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#00C9FF" Offset="0" />
            <GradientStop Color="#92FE9D" Offset="1" />
        </LinearGradientBrush>
    </gauge:LinearRange.RangeStroke>
</gauge:LinearRange>
```

## Binding Range Colors to Ticks and Labels

Automatically color ticks and labels based on the ranges they fall within.

### Binding to Labels

Use **BindRangeStrokeToLabels** to color labels:

```xml
<gauge:LinearScale 
    BindRangeStrokeToLabels="True"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3">
    
    <gauge:LinearScale.Ranges>
        <gauge:LinearRange 
            StartValue="0" 
            EndValue="35"
            StartWidth="15" 
            EndWidth="15"
            RangeOffset="5" 
            RangeStroke="Green" />
        
        <gauge:LinearRange 
            StartValue="35" 
            EndValue="65"
            StartWidth="15" 
            EndWidth="15" 
            RangeOffset="5" 
            RangeStroke="Yellow" />
        
        <gauge:LinearRange 
            StartValue="65" 
            EndValue="100"
            StartWidth="15" 
            EndWidth="15"
            RangeOffset="5" 
            RangeStroke="Red" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

### Binding to Ticks

Use **BindRangeStrokeToTicks** to color tick marks:

```xml
<gauge:LinearScale 
    BindRangeStrokeToTicks="True"
    BindRangeStrokeToLabels="True"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3">
    
    <gauge:LinearScale.Ranges>
        <!-- Green range: 0-35 -->
        <gauge:LinearRange 
            StartValue="0" 
            EndValue="35"
            StartWidth="15" 
            EndWidth="15"
            RangeOffset="5" 
            RangeStroke="Green" />
        
        <!-- Yellow range: 35-65 -->
        <gauge:LinearRange 
            StartValue="35" 
            EndValue="65"
            StartWidth="15" 
            EndWidth="15" 
            RangeOffset="5" 
            RangeStroke="Yellow" />
        
        <!-- Red range: 65-100 -->
        <gauge:LinearRange 
            StartValue="65" 
            EndValue="100"
            StartWidth="15" 
            EndWidth="15"
            RangeOffset="5" 
            RangeStroke="Red" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    BindRangeStrokeToLabels = true,
    BindRangeStrokeToTicks = true,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};

// Green range
scale.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 35,
    StartWidth = 15,
    EndWidth = 15,
    RangeOffset = 5,
    RangeStroke = new SolidColorBrush(Colors.Green)
});

// Yellow range
scale.Ranges.Add(new LinearRange
{
    StartValue = 35,
    EndValue = 65,
    StartWidth = 15,
    EndWidth = 15,
    RangeOffset = 5,
    RangeStroke = new SolidColorBrush(Colors.Yellow)
});

// Red range
scale.Ranges.Add(new LinearRange
{
    StartValue = 65,
    EndValue = 100,
    StartWidth = 15,
    EndWidth = 15,
    RangeOffset = 5,
    RangeStroke = new SolidColorBrush(Colors.Red)
});

gauge.MainScale = scale;
```

**Result:** Labels and ticks are automatically colored to match the range they fall within.

**Use cases:**
- Color-coded dashboards
- Traffic light systems (red/yellow/green)
- Quick visual assessment of zones
- Enhanced readability

## Range Positioning

Position ranges above, below, or at a specific distance from the scale bar.

### Using RangeOffset

**RangeOffset** positions the range as a multiplier of the scale bar size:

```xml
<gauge:LinearScale RangePosition="Below" ScaleBarSize="10">
    <gauge:LinearScale.Ranges>
        <gauge:LinearRange 
            StartValue="0"
            EndValue="60"
            RangeStroke="#27BEB7"
            RangeOffset="-40"
            StartWidth="10"
            EndWidth="10" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    RangePosition = LinearRangesPosition.Below,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};

LinearRange range = new LinearRange
{
    StartValue = 0,
    EndValue = 60,
    RangeOffset = -40,
    RangeStroke = new SolidColorBrush(Color.FromRgb(39, 190, 183)),
    StartWidth = 10,
    EndWidth = 10
};

scale.Ranges.Add(range);
```

**RangeOffset values:**
- **Positive** - Moves range away from scale bar
- **Negative** - Moves range toward scale bar
- **0** - Range touches the scale bar

### Using RangePosition

Set the base position of all ranges:

**1. Above (Default)** - Ranges appear above the scale bar

```xml
<gauge:LinearScale RangePosition="Above">
    <gauge:LinearScale.Ranges>
        <gauge:LinearRange 
            StartValue="0"
            EndValue="60"
            RangeStroke="#27BEB7"
            StartWidth="10"
            EndWidth="10" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

**2. Below** - Ranges appear below the scale bar

```xml
<gauge:LinearScale RangePosition="Below" ScaleBarStroke="#E0E0E0">
    <gauge:LinearScale.Ranges>
        <gauge:LinearRange 
            StartValue="0"
            EndValue="60"
            RangeStroke="#27BEB7"
            StartWidth="10"
            EndWidth="10" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    RangePosition = LinearRangesPosition.Below,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    // ... other properties
};
```

### Combined Positioning

Use both RangePosition and RangeOffset for precise control:

```xml
<gauge:LinearScale RangePosition="Below">
    <gauge:LinearScale.Ranges>
        <!-- Close to scale bar -->
        <gauge:LinearRange 
            StartValue="0"
            EndValue="33"
            RangeStroke="Green"
            StartWidth="12"
            EndWidth="12"
            RangeOffset="0.2" />
        
        <!-- Further from scale bar -->
        <gauge:LinearRange 
            StartValue="67"
            EndValue="100"
            RangeStroke="Red"
            StartWidth="12"
            EndWidth="12"
            RangeOffset="0.8" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

## Adding Multiple Ranges

Create comprehensive zone visualizations with multiple ranges.

### Three-Zone Pattern

Common pattern for status indicators:

```xml
<gauge:LinearScale 
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3"
    ScaleBarLength="300">
    
    <gauge:LinearScale.Ranges>
        <!-- Safe zone (Green): 0-35 -->
        <gauge:LinearRange 
            StartValue="0" 
            EndValue="35"
            StartWidth="25" 
            EndWidth="10"
            RangeOffset="5" 
            RangeOpacity="1" 
            RangeStroke="Green" />
        
        <!-- Danger zone (Red): 65-100 -->
        <gauge:LinearRange 
            StartValue="65" 
            EndValue="100"
            StartWidth="10" 
            EndWidth="25"
            RangeOffset="5" 
            RangeOpacity="1" 
            RangeStroke="Red" />
    </gauge:LinearScale.Ranges>
</gauge:LinearScale>
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};

// Safe zone (Green)
scale.Ranges.Add(new LinearRange
{
    StartValue = 0,
    EndValue = 35,
    StartWidth = 25,
    EndWidth = 10,
    RangeOffset = 5,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Green)
});

// Danger zone (Red)
scale.Ranges.Add(new LinearRange
{
    StartValue = 65,
    EndValue = 100,
    StartWidth = 10,
    EndWidth = 25,
    RangeOffset = 5,
    RangeOpacity = 1,
    RangeStroke = new SolidColorBrush(Colors.Red)
});

gauge.MainScale = scale;
```

### Continuous Multi-Range

No gaps between ranges:

```xml
<gauge:LinearScale.Ranges>
    <!-- Low range -->
    <gauge:LinearRange 
        StartValue="0" 
        EndValue="25"
        RangeStroke="#F44336"
        StartWidth="12"
        EndWidth="12"
        RangeOffset="0.3" />
    
    <!-- Medium-Low range -->
    <gauge:LinearRange 
        StartValue="25" 
        EndValue="50"
        RangeStroke="#FF9800"
        StartWidth="12"
        EndWidth="12"
        RangeOffset="0.3" />
    
    <!-- Medium-High range -->
    <gauge:LinearRange 
        StartValue="50" 
        EndValue="75"
        RangeStroke="#FFC107"
        StartWidth="12"
        EndWidth="12"
        RangeOffset="0.3" />
    
    <!-- High range -->
    <gauge:LinearRange 
        StartValue="75" 
        EndValue="100"
        RangeStroke="#4CAF50"
        StartWidth="12"
        EndWidth="12"
        RangeOffset="0.3" />
</gauge:LinearScale.Ranges>
```

### Overlapping Ranges

Create layered effects:

```xml
<gauge:LinearScale.Ranges>
    <!-- Background range (large, transparent) -->
    <gauge:LinearRange 
        StartValue="0" 
        EndValue="100"
        RangeStroke="#E0E0E0"
        StartWidth="20"
        EndWidth="20"
        RangeOpacity="0.5" />
    
    <!-- Foreground range (smaller, opaque) -->
    <gauge:LinearRange 
        StartValue="30" 
        EndValue="70"
        RangeStroke="#2196F3"
        StartWidth="12"
        EndWidth="12"
        RangeOpacity="1.0" />
</gauge:LinearScale.Ranges>
```

## Complete Examples

### Traffic Light Gauge

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            Interval="10"
            BindRangeStrokeToLabels="True"
            BindRangeStrokeToTicks="True"
            ScaleBarStroke="#FAFAFA"
            ScaleBarSize="12"
            LabelStroke="#212121"
            MajorTickStroke="#757575"
            MinorTickStroke="#BDBDBD"
            MinorTicksPerInterval="4">
            
            <gauge:LinearScale.Ranges>
                <!-- Red zone (0-40): Danger -->
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="40"
                    RangeStroke="#F44336"
                    StartWidth="15"
                    EndWidth="15"
                    RangeOffset="0.4" />
                
                <!-- Yellow zone (40-70): Warning -->
                <gauge:LinearRange 
                    StartValue="40" 
                    EndValue="70"
                    RangeStroke="#FFC107"
                    StartWidth="15"
                    EndWidth="15"
                    RangeOffset="0.4" />
                
                <!-- Green zone (70-100): Safe -->
                <gauge:LinearRange 
                    StartValue="70" 
                    EndValue="100"
                    RangeStroke="#4CAF50"
                    StartWidth="15"
                    EndWidth="15"
                    RangeOffset="0.4" />
            </gauge:LinearScale.Ranges>
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="58"
                    SymbolPointerHeight="18"
                    SymbolPointerWidth="18"
                    SymbolPointerPosition="Cross"
                    SymbolPointerStroke="#212121"
                    EnableAnimation="True"
                    AnimationDuration="600" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Temperature Gauge with Ranges

```xml
<gauge:SfLinearGauge Orientation="Vertical" Height="400">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="-20" 
            Maximum="120"
            Interval="20"
            LabelPostfix="°C"
            ScaleBarStroke="#CFD8DC"
            ScaleBarSize="15"
            LabelStroke="#263238">
            
            <gauge:LinearScale.Ranges>
                <!-- Freezing: -20 to 0 (Blue) -->
                <gauge:LinearRange 
                    StartValue="-20" 
                    EndValue="0"
                    RangeStroke="#2196F3"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Cold: 0 to 15 (Light Blue) -->
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="15"
                    RangeStroke="#03A9F4"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Normal: 15 to 25 (Green) -->
                <gauge:LinearRange 
                    StartValue="15" 
                    EndValue="25"
                    RangeStroke="#4CAF50"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Warm: 25 to 35 (Yellow) -->
                <gauge:LinearRange 
                    StartValue="25" 
                    EndValue="35"
                    RangeStroke="#FFC107"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Hot: 35 to 50 (Orange) -->
                <gauge:LinearRange 
                    StartValue="35" 
                    EndValue="50"
                    RangeStroke="#FF9800"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Very Hot: 50 to 120 (Red) -->
                <gauge:LinearRange 
                    StartValue="50" 
                    EndValue="120"
                    RangeStroke="#F44336"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
            </gauge:LinearScale.Ranges>
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="22"
                    BarPointerStroke="#E53935"
                    BarPointerStrokeThickness="15"
                    EnableAnimation="True" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Performance Rating Gauge

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="10"
            Interval="1"
            ScaleBarStroke="#ECEFF1"
            ScaleBarSize="20"
            ScaleBarLength="500"
            LabelStroke="#37474F"
            MajorTickStroke="#607D8B"
            MinorTicksPerInterval="0">
            
            <gauge:LinearScale.Ranges>
                <!-- Poor (0-3): Red -->
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="3"
                    RangeStroke="#D32F2F"
                    StartWidth="25"
                    EndWidth="18"
                    RangeOffset="0.5"
                    RangeOpacity="0.8" />
                
                <!-- Fair (3-5): Orange -->
                <gauge:LinearRange 
                    StartValue="3" 
                    EndValue="5"
                    RangeStroke="#F57C00"
                    StartWidth="18"
                    EndWidth="15"
                    RangeOffset="0.5"
                    RangeOpacity="0.8" />
                
                <!-- Good (5-7): Yellow -->
                <gauge:LinearRange 
                    StartValue="5" 
                    EndValue="7"
                    RangeStroke="#FBC02D"
                    StartWidth="15"
                    EndWidth="18"
                    RangeOffset="0.5"
                    RangeOpacity="0.8" />
                
                <!-- Excellent (7-10): Green -->
                <gauge:LinearRange 
                    StartValue="7" 
                    EndValue="10"
                    RangeStroke="#388E3C"
                    StartWidth="18"
                    EndWidth="25"
                    RangeOffset="0.5"
                    RangeOpacity="0.8" />
            </gauge:LinearScale.Ranges>
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="7.8"
                    BarPointerStroke="#1B5E20"
                    BarPointerStrokeThickness="20" />
                
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="7.8"
                    SymbolPointerPosition="Above"
                    SymbolPointerHeight="20"
                    SymbolPointerWidth="20"
                    SymbolPointerStroke="#1B5E20" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Best Practices

### Color Selection

1. **Use standard conventions:**
   - Green = Safe/Good
   - Yellow/Orange = Warning/Caution
   - Red = Danger/Critical

2. **Ensure color contrast** with background and scale elements

3. **Consider colorblindness** - Use patterns or varying intensities in addition to colors

4. **Limit color variety** - 3-5 distinct range colors maximum

### Range Boundaries

1. **No gaps** - Ensure ranges cover the entire scale or intentional gaps are meaningful

2. **Clear boundaries** - Use round numbers for range transitions (25, 50, 75 vs. 23.7, 51.3)

3. **Logical groupings** - Ranges should represent meaningful thresholds

### Visual Hierarchy

1. **Width consistency** - Use same width for all ranges unless tapered effect is intentional

2. **Position consistently** - All ranges at same offset for uniform appearance

3. **Opacity for layering** - Use transparency for overlapping or background ranges

### Performance

1. **Limit range count** - 3-6 ranges per gauge is optimal

2. **Simple brushes** - Solid colors perform better than gradients

3. **Avoid micro-ranges** - Very small ranges (< 5% of scale) may not render well

## Troubleshooting

### Range Not Visible

**Issue:** Range doesn't appear on the gauge.

**Solutions:**
1. Verify `StartValue` and `EndValue` are within scale's min/max
2. Check that `RangeStroke` is set to a visible color
3. Ensure `StartWidth` and `EndWidth` are set to reasonable values (> 0)
4. Check `RangeOpacity` is not set to 0

### Ranges Overlapping Pointers

**Issue:** Ranges cover pointers or other elements.

**Solution:** Adjust `RangeOffset` to position ranges away from other elements:

```xml
<gauge:LinearRange RangeOffset="0.5" />
```

### Range Color Binding Not Working

**Issue:** Labels/ticks aren't colored by ranges.

**Solutions:**
1. Set `BindRangeStrokeToLabels="True"` or `BindRangeStrokeToTicks="True"`
2. Ensure ranges have `RangeStroke` set
3. Verify tick/label values fall within range boundaries

---

**Next:** Learn about [Labels and Ticks](labels-and-ticks.md) to enhance your gauge with formatted labels and customized tick marks.
