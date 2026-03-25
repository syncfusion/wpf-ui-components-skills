# Scales and Configuration

## Table of Contents
- [Scale Overview](#scale-overview)
- [Setting Start and End Values](#setting-start-and-end-values)
- [Configuring Angles](#configuring-angles)
- [Setting Interval](#setting-interval)
- [Sweep Direction](#sweep-direction)
- [Multiple Scales](#multiple-scales)
- [Scale Sizing](#scale-sizing)
- [Common Patterns](#common-patterns)

## Scale Overview

The **CircularScale** is the core element of a circular gauge. It defines the value range, visual layout, and contains all sub-elements like labels, ticks, ranges, and pointers.

**Key Responsibilities:**
- Define minimum and maximum values
- Control the angular layout (start angle, sweep angle)
- Host pointers, ranges, labels, and ticks
- Manage scale radius and sizing

**Basic Structure:**
```xml
<gauge:SfCircularGauge>
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale>
            <!-- Pointers, Ranges, etc. -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

A gauge can contain multiple scales for complex visualizations.

## Setting Start and End Values

The `StartValue` and `EndValue` properties define the minimum and maximum values displayed on the scale.

### Basic Value Range

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularScale scale = new CircularScale
{
    StartValue = 0,
    EndValue = 100
};

CircularPointer pointer = new CircularPointer
{
    NeedlePointerVisibility = Visibility.Hidden
};
scale.Pointers.Add(pointer);
```

### Negative Value Ranges

For temperature gauges or other metrics with negative values:

**XAML:**
```xml
<gauge:CircularScale StartValue="-30" 
                     EndValue="50">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="15"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularScale scale = new CircularScale
{
    StartValue = -30,
    EndValue = 50
};
```

**Use Cases:**
- Temperature scales (Celsius/Fahrenheit)
- Financial charts (profit/loss)
- Altitude indicators
- Voltage meters (positive/negative)

### Large Value Ranges

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="10000" 
                     Interval="1000"/>
```

**Note:** Adjust `Interval` to control label density for large ranges.

## Configuring Angles

Control the angular layout of your gauge using `StartAngle` and `SweepAngle`.

### StartAngle Property

The `StartAngle` determines where the scale begins (in degrees).

**Angle Reference:**
- `0°` - Right (3 o'clock position)
- `90°` - Bottom (6 o'clock)
- `180°` - Left (9 o'clock)
- `270°` - Top (12 o'clock)

**XAML:**
```xml
<gauge:CircularScale StartAngle="180" 
                     SweepAngle="180">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularScale scale = new CircularScale
{
    StartAngle = 180,
    SweepAngle = 180
};
```

**Result:** Semi-circular gauge starting at left (9 o'clock).

### SweepAngle Property

The `SweepAngle` defines how many degrees the scale spans.

**Common Sweep Angles:**
- `360°` - Full circle
- `270°` - Three-quarter circle
- `180°` - Semi-circle
- `90°` - Quarter circle

**Example: Speedometer Layout (270°)**
```xml
<gauge:CircularScale StartAngle="135" 
                     SweepAngle="270">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="80"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Example: Semi-Circle Gauge (180°)**
```xml
<gauge:CircularScale StartAngle="180" 
                     SweepAngle="180">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="50"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Example: Full Circle Progress (360°)**
```xml
<gauge:CircularScale StartAngle="270" 
                     SweepAngle="360">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" Value="75"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Angle Calculation Tips

**Speedometer (Classic Car Dashboard):**
```xml
StartAngle="130" SweepAngle="280"
```

**Watch-Style (12 o'clock start):**
```xml
StartAngle="270" SweepAngle="360"
```

**Bottom Semi-Circle:**
```xml
StartAngle="180" SweepAngle="180"
```

## Setting Interval

The `Interval` property controls the spacing between major tick marks and labels.

### Basic Interval

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="500" 
                     Interval="100">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularScale scale = new CircularScale
{
    StartValue = 0,
    EndValue = 500,
    Interval = 100
};
```

**Result:** Labels at 0, 100, 200, 300, 400, 500

### Auto Interval

When `Interval` is not specified, the gauge automatically calculates appropriate intervals:

```xml
<gauge:CircularScale StartValue="0" EndValue="100"/>
```

**Auto behavior:**
- Analyzes value range
- Determines optimal label count
- Prevents label overlap

### Decimal Intervals

You can specify intervals up to 5 decimal places:

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="1" 
                     Interval="0.1"/>
```

**Result:** Labels at 0.0, 0.1, 0.2, ..., 1.0

**Use Cases:**
- Percentage displays (0 to 1)
- Precision measurements
- Scientific instruments

### Interval Best Practices

**For Readability:**
- Large ranges (0-1000): Use intervals of 100-200
- Medium ranges (0-100): Use intervals of 10-20
- Small ranges (0-10): Use intervals of 1-2

**Avoid:**
- Too many labels (overcrowding)
- Odd intervals (7, 13, etc.) unless domain-specific

## Sweep Direction

Control whether the scale progresses clockwise or counterclockwise using `SweepDirection`.

### Clockwise Direction (Default)

**XAML:**
```xml
<gauge:CircularScale SweepDirection="Clockwise">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Values increase:** Clockwise from start angle

### Counterclockwise Direction

**XAML:**
```xml
<gauge:CircularScale SweepDirection="Counterclockwise">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularScale scale = new CircularScale
{
    SweepDirection = SweepDirection.Counterclockwise
};
```

**Values increase:** Counterclockwise from start angle

### Use Cases

**Counterclockwise:**
- Countdown timers
- Fuel gauges (traditional layout)
- Specialized instruments

**Clockwise:**
- Standard speedometers
- Progress indicators
- Most common gauges

## Multiple Scales

Add multiple scales to a single gauge for complex visualizations.

### Basic Multi-Scale

**XAML:**
```xml
<gauge:SfCircularGauge SpacingMargin="0.7">
    <gauge:SfCircularGauge.Scales>
        
        <!-- Outer Scale -->
        <gauge:CircularScale Radius="175">
            <gauge:CircularScale.Ranges>
                <gauge:CircularRange StartValue="0" EndValue="60" 
                                     Stroke="#B0B0B0" StrokeThickness="5"/>
                <gauge:CircularRange StartValue="60" EndValue="100" 
                                     Stroke="#C1252C" StrokeThickness="5"/>
            </gauge:CircularScale.Ranges>
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="30" 
                                       PointerType="SymbolPointer" 
                                       Symbol="InvertedArrow"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
        
        <!-- Inner Scale -->
        <gauge:CircularScale Radius="90" LabelStroke="Black">
            <gauge:CircularScale.MajorTickSettings>
                <gauge:MajorTickSetting Stroke="Black"/>
            </gauge:CircularScale.MajorTickSettings>
            <gauge:CircularScale.MinorTickSettings>
                <gauge:MinorTickSetting Stroke="Black"/>
            </gauge:CircularScale.MinorTickSettings>
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="30" 
                                       PointerType="NeedlePointer"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
        
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**C#:**
```csharp
SfCircularGauge gauge = new SfCircularGauge();
gauge.SpacingMargin = 0.7;

// Outer scale
CircularScale outerScale = new CircularScale
{
    Radius = 175
};
// Configure outer scale...

// Inner scale
CircularScale innerScale = new CircularScale
{
    Radius = 90,
    LabelStroke = new SolidColorBrush(Colors.Black)
};
// Configure inner scale...

gauge.Scales.Add(outerScale);
gauge.Scales.Add(innerScale);
```

### SpacingMargin Property

`SpacingMargin` determines the size ratio between scales.

**Range:** 0.1 to 1.0

**XAML:**
```xml
<gauge:SfCircularGauge SpacingMargin="0.7">
    <!-- Multiple scales -->
</gauge:SfCircularGauge>
```

**Values:**
- `1.0` - Scales touch gauge edges (no margin)
- `0.7` - 30% margin around scales
- `0.5` - 50% margin (smaller scales)

### Use Cases for Multiple Scales

**Dual Metric Display:**
- Speed (mph) and Engine RPM
- Temperature (°C and °F)
- Pressure and Volume

**Comparison Gauges:**
- Current vs. Target
- Actual vs. Budget
- Real-time vs. Average

## Scale Sizing

Control the size of scales using radius properties.

### Radius Property

Set absolute radius in pixels:

**XAML:**
```xml
<gauge:CircularScale Radius="150">
    <!-- Scale content -->
</gauge:CircularScale>
```

**C#:**
```csharp
CircularScale scale = new CircularScale
{
    Radius = 150
};
```

### RadiusFactor Property

Set relative radius as a fraction (0-1):

**XAML:**
```xml
<gauge:CircularScale RadiusFactor="0.9">
    <!-- Scale content -->
</gauge:CircularScale>
```

**Values:**
- `1.0` - Full gauge size
- `0.9` - 90% of gauge size
- `0.5` - Half gauge size

**Use Case:** Responsive sizing that adapts to gauge container.

### Gauge Sizing

Control overall gauge dimensions:

**XAML:**
```xml
<gauge:SfCircularGauge Height="400" Width="400">
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale RadiusFactor="0.95">
            <!-- Content -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**Best Practices:**
- Use square dimensions (Height = Width) for circular gauges
- Use RadiusFactor for responsive designs
- Use absolute Radius for precise multi-scale layouts

## Common Patterns

### Pattern 1: Speedometer Layout

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="160" 
                     Interval="20"
                     StartAngle="130" 
                     SweepAngle="280"
                     ShowRim="True"
                     RimStroke="LightGray"
                     RimStrokeThickness="5">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="85"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Pattern 2: Full-Circle Progress

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100" 
                     StartAngle="270" 
                     SweepAngle="360"
                     Interval="25">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="75"
                               RangeCap="Both"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Pattern 3: Temperature Scale

```xml
<gauge:CircularScale StartValue="-40" 
                     EndValue="140" 
                     Interval="20"
                     StartAngle="180" 
                     SweepAngle="180">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="72"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Pattern 4: Responsive Multi-Scale

```xml
<gauge:SfCircularGauge SpacingMargin="0.8">
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale RadiusFactor="0.95" 
                             LabelStroke="Blue">
            <!-- Outer metric -->
        </gauge:CircularScale>
        <gauge:CircularScale RadiusFactor="0.6" 
                             LabelStroke="Red">
            <!-- Inner metric -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

## Next Steps

- **Add Pointers:** Learn about needle, range, and symbol pointers
  → Read: [pointers.md](pointers.md)

- **Configure Ranges:** Add visual zones to your scale
  → Read: [ranges.md](ranges.md)

- **Customize Labels:** Style scale labels and ticks
  → Read: [labels-and-ticks.md](labels-and-ticks.md)
