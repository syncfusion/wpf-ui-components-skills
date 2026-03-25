# Scale Configuration

Comprehensive guide to configuring and customizing the LinearScale in SfLinearGauge. The scale is the foundation of the gauge, defining the value range, visual appearance, layout, and positioning of all gauge elements.

## Table of Contents
- [Scale Overview](#scale-overview)
- [Setting Minimum and Maximum Values](#setting-minimum-and-maximum-values)
- [Configuring Intervals](#configuring-intervals)
- [Scale Appearance Customization](#scale-appearance-customization)
- [Scale Size Customization](#scale-size-customization)
- [Setting Scale Direction](#setting-scale-direction)
- [Scale Positioning](#scale-positioning)
- [Complete Examples](#complete-examples)

## Scale Overview

The **MainScale** property of SfLinearGauge accepts a **LinearScale** object that integrates all gauge elements:

- **Scale bar** - The horizontal or vertical bar showing the value range
- **Ticks** - Major and minor tick marks indicating intervals
- **Labels** - Numeric labels showing scale values
- **Pointers** - Value markers (bar and symbol pointers)
- **Ranges** - Colored sections highlighting value zones

**Basic scale structure:**

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            ScaleBarStroke="#E0E0E0" 
            MajorTickStroke="Gray" 
            MinorTickStroke="Gray" 
            LabelStroke="#424242"
            ScaleBarSize="10" 
            MinorTicksPerInterval="3" />
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

gauge.MainScale = scale;
```

## Setting Minimum and Maximum Values

Define the scale's value range with the **Minimum** and **Maximum** properties.

### Basic Range Configuration

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="200" 
    ScaleBarStroke="#E0E0E0" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    Minimum = 0,
    Maximum = 200,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224))
};
```

**Default values:**
- Minimum: `0`
- Maximum: `100`

### Custom Range Examples

**Temperature scale (-20°C to 120°C):**

```xml
<gauge:LinearScale 
    Minimum="-20" 
    Maximum="120"
    LabelPostfix="°C" />
```

**Percentage scale (0% to 100%):**

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="100"
    LabelPostfix="%" />
```

**Pressure scale (0 to 500 PSI):**

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="500"
    LabelPostfix=" PSI"
    Interval="50" />
```

## Configuring Intervals

The **Interval** property determines the spacing between major ticks and labels.

### Setting Explicit Intervals

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="500" 
    Interval="100"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242" 
    ScaleBarSize="10" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    Minimum = 0,
    Maximum = 500,
    Interval = 100, // Labels at 0, 100, 200, 300, 400, 500
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10
};
```

**Result:** Major ticks and labels appear at 0, 100, 200, 300, 400, and 500.

### Auto Interval

If you don't set the Interval property, it's calculated automatically:

**Auto interval formula:** Approximately 3 labels per 100 pixels of scale length.

```xml
<!-- Interval is automatically calculated -->
<gauge:LinearScale 
    Minimum="0" 
    Maximum="100"
    ScaleBarLength="300" />
```

**When to use auto interval:**
- Prototyping and quick implementations
- When scale range changes dynamically
- When you want adaptive label density

**When to set explicit interval:**
- You need specific label values (e.g., every 10, 25, 50)
- Creating professional dashboards with precise labeling
- Ensuring consistency across multiple gauges

### Interval Examples

**Every 5 units (0-50 range):**

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="50" 
    Interval="5" />
<!-- Labels: 0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50 -->
```

**Every 25 units (0-100 range):**

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="100" 
    Interval="25" />
<!-- Labels: 0, 25, 50, 75, 100 -->
```

**Decimal intervals (0-1 range):**

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="1" 
    Interval="0.2" />
<!-- Labels: 0, 0.2, 0.4, 0.6, 0.8, 1.0 -->
```

## Scale Appearance Customization

Customize the scale bar's visual properties.

### Color and Stroke Customization

```xml
<gauge:LinearScale 
    ScaleBarBorderBrush="Red"
    ScaleBarStroke="Blue" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242" 
    ScaleBarBorderThickness="3"
    ScaleBarSize="20" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    ScaleBarStroke = new SolidColorBrush(Colors.Blue),
    ScaleBarBorderBrush = new SolidColorBrush(Colors.Red),
    ScaleBarBorderThickness = new Thickness(3),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 20,
    MinorTicksPerInterval = 3
};
```

**Key properties:**

| Property | Type | Description |
|----------|------|-------------|
| **ScaleBarStroke** | Brush | Fill color of the scale bar |
| **ScaleBarBorderBrush** | Brush | Border/outline color of the scale bar |
| **ScaleBarBorderThickness** | Thickness | Border width around the scale bar |
| **MajorTickStroke** | Brush | Color of major tick marks |
| **MinorTickStroke** | Brush | Color of minor tick marks |
| **LabelStroke** | Brush | Color of numeric labels |

### Gradient Scale Bar

Create a gradient effect on the scale bar:

```xml
<gauge:LinearScale ScaleBarSize="15">
    <gauge:LinearScale.ScaleBarStroke>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="LightBlue" Offset="0" />
            <GradientStop Color="Blue" Offset="0.5" />
            <GradientStop Color="DarkBlue" Offset="1" />
        </LinearGradientBrush>
    </gauge:LinearScale.ScaleBarStroke>
</gauge:LinearScale>
```

**C#:**

```csharp
LinearScale scale = new LinearScale { ScaleBarSize = 15 };

LinearGradientBrush gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0)
};
gradient.GradientStops.Add(new GradientStop(Colors.LightBlue, 0));
gradient.GradientStops.Add(new GradientStop(Colors.Blue, 0.5));
gradient.GradientStops.Add(new GradientStop(Colors.DarkBlue, 1));

scale.ScaleBarStroke = gradient;
```

## Scale Size Customization

Control the physical dimensions of the scale.

### ScaleBarSize vs ScaleBarLength

**ScaleBarSize:**
- In **horizontal orientation:** Height of the scale bar
- In **vertical orientation:** Width of the scale bar
- Controls the thickness/cross-dimension

**ScaleBarLength:**
- In **horizontal orientation:** Width of the scale bar
- In **vertical orientation:** Height of the scale bar
- Controls the length/primary dimension

### Size Configuration

```xml
<gauge:SfLinearGauge Background="AliceBlue" 
                     BorderBrush="Black" 
                     BorderThickness="1"
                     Height="300" 
                     Width="500">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            ScaleBarStroke="Blue" 
            ScaleBarSize="50"
            ScaleBarLength="350" />
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**C#:**

```csharp
SfLinearGauge gauge = new SfLinearGauge
{
    Background = new SolidColorBrush(Colors.AliceBlue),
    BorderBrush = new SolidColorBrush(Colors.Black),
    BorderThickness = new Thickness(1),
    Height = 300,
    Width = 500
};

LinearScale scale = new LinearScale
{
    ScaleBarStroke = new SolidColorBrush(Colors.Blue),
    ScaleBarSize = 50,
    ScaleBarLength = 350
};

gauge.MainScale = scale;
```

### Size Examples

**Thin horizontal gauge:**

```xml
<gauge:LinearScale 
    ScaleBarSize="5"
    ScaleBarLength="400" />
```

**Thick horizontal gauge:**

```xml
<gauge:LinearScale 
    ScaleBarSize="40"
    ScaleBarLength="300" />
```

**Compact vertical gauge:**

```xml
<gauge:SfLinearGauge Orientation="Vertical">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            ScaleBarSize="15"
            ScaleBarLength="200" />
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Responsive Sizing

Let the scale fill available space by omitting ScaleBarLength:

```xml
<Grid>
    <gauge:SfLinearGauge HorizontalAlignment="Stretch">
        <gauge:SfLinearGauge.MainScale>
            <gauge:LinearScale ScaleBarSize="10" />
            <!-- ScaleBarLength adapts to container width -->
        </gauge:SfLinearGauge.MainScale>
    </gauge:SfLinearGauge>
</Grid>
```

## Setting Scale Direction

Control whether the scale values increase from left-to-right (or bottom-to-top) or in reverse.

### Forward Direction (Default)

```xml
<gauge:LinearScale 
    ScaleDirection="Forward"
    ScaleBarStroke="#E0E0E0" />
<!-- 0 is on the left, 100 on the right (horizontal) -->
```

### Backward Direction

```xml
<gauge:LinearScale 
    ScaleDirection="Backward"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3" />
<!-- 100 is on the left, 0 on the right (horizontal) -->
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    ScaleDirection = LinearScaleDirection.Backward,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};
```

**Use cases for backward direction:**
- Right-to-left (RTL) language support
- Countdown timers or reverse progress indicators
- Matching specific design requirements
- Thermometer-style vertical gauges where higher values are at the top

## Scale Positioning

Position the scale bar within the gauge's layout space.

### ElementsPositionMode

Controls whether positioning is automatic or custom:

- **Default** - Automatic positioning based on gauge layout
- **Custom** - Manual positioning using factor properties

### Custom Positioning with ScaleBarPositionFactor

**ScaleBarPositionFactor** positions the scale bar as a factor (0.0 to 1.0) of available space:
- **0** - Far left (horizontal) or bottom (vertical)
- **0.5** - Center
- **1** - Far right (horizontal) or top (vertical)

```xml
<gauge:LinearScale 
    ElementsPositionMode="Custom"
    ScaleBarPositionFactor="0.5"
    TickPositionFactor="0.443"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    MinorTickSize="9" 
    MajorTickSize="15"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    ElementsPositionMode = LinearScalePositionModes.Custom,
    ScaleBarPositionFactor = 0.5, // Center the scale bar
    TickPositionFactor = 0.443, // Position ticks slightly above center
    MajorTickSize = 15,
    MinorTickSize = 9,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};
```

**Related positioning properties:**
- **TickPositionFactor** - Position ticks independently from scale bar
- **LabelOffset** - Offset labels from their default position
- **RangeOffset** - Position ranges relative to scale bar

### Positioning Examples

**Scale at top with ticks below:**

```xml
<gauge:LinearScale 
    ElementsPositionMode="Custom"
    ScaleBarPositionFactor="0.2"
    TickPositionFactor="0.3"
    TickPosition="Below" />
```

**Centered scale with cross ticks:**

```xml
<gauge:LinearScale 
    ElementsPositionMode="Custom"
    ScaleBarPositionFactor="0.5"
    TickPosition="Cross" />
```

## Complete Examples

### Professional Dashboard Gauge

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            Interval="10"
            ScaleBarStroke="#ECEFF1"
            ScaleBarSize="12"
            ScaleBarLength="400"
            LabelStroke="#37474F"
            LabelSize="12"
            MajorTickStroke="#78909C"
            MajorTickSize="16"
            MajorTickStrokeThickness="2"
            MinorTickStroke="#B0BEC5"
            MinorTickSize="8"
            MinorTickStrokeThickness="1"
            MinorTicksPerInterval="4">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="68"
                    BarPointerStroke="#1976D2"
                    BarPointerStrokeThickness="12" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Temperature Gauge with Custom Scale

```xml
<gauge:SfLinearGauge Orientation="Vertical" Height="400">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="-20" 
            Maximum="120"
            Interval="20"
            LabelPostfix="°C"
            ScaleDirection="Forward"
            ScaleBarStroke="#B0BEC5"
            ScaleBarSize="18"
            ScaleBarLength="350"
            LabelStroke="#263238"
            LabelSize="14"
            MajorTickStroke="#455A64"
            MajorTickSize="14"
            MinorTickStroke="#90A4AE"
            MinorTickSize="7"
            MinorTicksPerInterval="3">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="37"
                    BarPointerStroke="#D32F2F"
                    BarPointerStrokeThickness="18" />
            </gauge:LinearScale.Pointers>
            
            <gauge:LinearScale.Ranges>
                <!-- Freezing range -->
                <gauge:LinearRange 
                    StartValue="-20" 
                    EndValue="0"
                    RangeStroke="#2196F3"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Normal range -->
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="40"
                    RangeStroke="#4CAF50"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
                
                <!-- Hot range -->
                <gauge:LinearRange 
                    StartValue="40" 
                    EndValue="120"
                    RangeStroke="#FF9800"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.3" />
            </gauge:LinearScale.Ranges>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Performance Meter with Custom Positioning

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            Interval="25"
            ElementsPositionMode="Custom"
            ScaleBarPositionFactor="0.45"
            TickPositionFactor="0.38"
            ScaleBarStroke="#E0E0E0"
            ScaleBarSize="25"
            ScaleBarLength="450"
            ScaleBarBorderBrush="#9E9E9E"
            ScaleBarBorderThickness="1"
            LabelStroke="#212121"
            LabelSize="16"
            LabelOffset="20"
            MajorTickStroke="#616161"
            MajorTickSize="20"
            MajorTickStrokeThickness="2"
            MinorTickStroke="#9E9E9E"
            MinorTickSize="10"
            MinorTicksPerInterval="4"
            TickPosition="Cross">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="72"
                    SymbolPointerHeight="20"
                    SymbolPointerWidth="20"
                    SymbolPointerPosition="Cross"
                    SymbolPointerStroke="#E91E63"
                    EnableAnimation="True"
                    AnimationDuration="800" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Best Practices

### Choosing Scale Range

1. **Set meaningful min/max** - Choose values that encompass expected data with some buffer
2. **Use round numbers** - Min/Max values like 0, 100, 500, 1000 are easier to interpret
3. **Consider data precision** - Use appropriate decimal places for your data type

### Interval Selection

1. **Aim for 5-10 labels** - Too many labels clutter; too few reduce precision
2. **Use multiples of 5 or 10** - Intervals like 5, 10, 25, 50, 100 are intuitive
3. **Match data granularity** - Set intervals based on the precision users need

### Visual Clarity

1. **High contrast** - Ensure labels and ticks are visible against the scale bar
2. **Appropriate sizing** - Scale bar should be prominent but not dominate the layout
3. **Consistent styling** - Use similar colors and sizes across multiple gauges
4. **Whitespace** - Leave adequate space around the gauge for labels and ticks

### Performance

1. **Limit intervals** - Very small intervals (e.g., Interval="1" on 0-1000) create many elements
2. **Use auto interval** - For dynamic ranges, let the control calculate optimal spacing
3. **Fixed size when possible** - Setting explicit ScaleBarLength improves layout performance

## Troubleshooting

### Scale Too Small or Large

**Issue:** Scale bar doesn't fit properly in the layout.

**Solution:** Set explicit `ScaleBarLength` or ensure the parent container has adequate size:

```xml
<gauge:LinearScale ScaleBarLength="400" />
```

### Labels Overlapping

**Issue:** Scale labels overlap each other.

**Solution:** Increase the `Interval` or increase `ScaleBarLength`:

```xml
<gauge:LinearScale 
    Interval="20"
    ScaleBarLength="500" />
```

### Scale Not Visible

**Issue:** The scale bar doesn't appear.

**Solution:** Ensure `ScaleBarStroke` is set to a visible color and `ScaleBarSize` is adequate:

```xml
<gauge:LinearScale 
    ScaleBarStroke="Gray"
    ScaleBarSize="10" />
```

---

**Next:** Explore [Pointers](pointers.md) to add value markers to your configured scale.
