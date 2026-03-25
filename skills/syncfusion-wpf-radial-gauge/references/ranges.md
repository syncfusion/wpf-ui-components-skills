# Ranges

## Table of Contents
- [Ranges Overview](#ranges-overview)
- [Creating Ranges](#creating-ranges)
- [Range Customization](#range-customization)
- [Range Positioning](#range-positioning)
- [Multiple Ranges](#multiple-ranges)
- [Gradient Ranges](#gradient-ranges)
- [Common Patterns](#common-patterns)

## Ranges Overview

**Ranges** are visual elements that mark specific value zones on a circular scale. They appear as colored bands that help users quickly identify different regions (e.g., low/medium/high, safe/warning/danger).

**Key Uses:**
- Visualize zones (safe, warning, critical)
- Show target ranges
- Indicate performance bands
- Create colored backgrounds for pointers

**Basic Structure:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" EndValue="50" 
                             Stroke="Green"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

## Creating Ranges

### Basic Range

A range requires `StartValue` and `EndValue` to define its boundaries.

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" 
                             EndValue="50"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularRange range = new CircularRange
{
    StartValue = 0,
    EndValue = 50
};
scale.Ranges.Add(range);

// Hide default pointer
CircularPointer pointer = new CircularPointer
{
    NeedlePointerVisibility = Visibility.Hidden
};
scale.Pointers.Add(pointer);
```

**Default Appearance:** Light colored band from 0 to 50 on the scale.

### Value Boundaries

Range values must fall within the scale's `StartValue` and `EndValue`:

```xml
<gauge:CircularScale StartValue="0" EndValue="100">
    <gauge:CircularScale.Ranges>
        <!-- Valid: Within scale bounds -->
        <gauge:CircularRange StartValue="20" EndValue="80"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

**Out-of-bounds behavior:** Range is clipped to scale boundaries.

## Range Customization

### Stroke (Color)

Set range color with the `Stroke` property:

**XAML:**
```xml
<gauge:CircularRange StartValue="0" 
                     EndValue="50" 
                     Stroke="Pink"/>
```

**C#:**
```csharp
range.Stroke = new SolidColorBrush(Colors.Pink);
```

**Color Options:**
- Named colors: `"Green"`, `"Red"`, `"Orange"`
- Hex colors: `"#FF5733"`
- RGB: `Color.FromRgb(255, 87, 51)`

### StrokeThickness

Control range band width:

**XAML:**
```xml
<gauge:CircularRange StartValue="0" 
                     EndValue="50" 
                     Stroke="Pink" 
                     StrokeThickness="40"/>
```

**C#:**
```csharp
range.StrokeThickness = 40;
```

**Typical Values:**
- Thin accent: 5-10
- Standard: 15-25
- Wide background: 30-50

### Complete Customization

**XAML:**
```xml
<gauge:CircularScale RangePosition="Custom">
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" 
                             EndValue="50"
                             Offset="0.6"
                             Stroke="Pink"
                             StrokeThickness="40"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.RangePosition = RangePosition.Custom;

CircularRange range = new CircularRange
{
    StartValue = 0,
    EndValue = 50,
    Offset = 0.6,
    Stroke = new SolidColorBrush(Colors.Pink),
    StrokeThickness = 40
};
scale.Ranges.Add(range);
```

## Range Positioning

Control where ranges appear using `RangePosition` and `Offset`.

### RangePosition Property

Set on the **CircularScale** (affects all ranges):

**Options:**
- `Inside` (Default) - Inside the scale rim
- `Outside` - Outside the scale rim
- `Cross` - Overlapping the rim
- `Custom` - Use Offset for precise positioning

**XAML:**
```xml
<gauge:CircularScale RangePosition="Outside">
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" EndValue="50" 
                             Stroke="Green" StrokeThickness="20"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.RangePosition = RangePosition.Outside;
```

### Custom Position with Offset

Use `Offset` for precise placement (0 to 1):

**XAML:**
```xml
<gauge:CircularScale RangePosition="Custom">
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" 
                             EndValue="50"
                             Offset="0.7"
                             Stroke="Green"
                             StrokeThickness="25"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

**Offset Values:**
- `0` - At gauge center
- `0.5` - Midway between center and rim
- `0.7` - 70% from center to rim
- `1.0` - At the rim

**C#:**
```csharp
scale.RangePosition = RangePosition.Custom;
range.Offset = 0.7;
```

### Advanced: Start and End Offsets

For responsive width positioning:

**XAML:**
```xml
<gauge:CircularScale RangePosition="Custom">
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" 
                             EndValue="50"
                             StartOffset="0.5"
                             EndOffset="0.7"
                             Stroke="Green"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

**Properties:**
- `StartOffset` - Inner edge position (0-1)
- `EndOffset` - Outer edge position (0-1)
- Width automatically calculated from offsets

## Multiple Ranges

Create zone-based visualizations with multiple ranges.

### Three-Zone Example

**XAML:**
```xml
<gauge:CircularScale StartValue="0" EndValue="100">
    <gauge:CircularScale.Ranges>
        <!-- Low Zone (Green) -->
        <gauge:CircularRange StartValue="0" 
                             EndValue="50"
                             Stroke="Green"
                             StrokeThickness="10"/>
        
        <!-- Medium Zone (Orange) -->
        <gauge:CircularRange StartValue="50" 
                             EndValue="80"
                             Stroke="Orange"
                             StrokeThickness="10"/>
        
        <!-- High Zone (Red) -->
        <gauge:CircularRange StartValue="80" 
                             EndValue="100"
                             Stroke="Red"
                             StrokeThickness="10"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="65"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
// Low zone
scale.Ranges.Add(new CircularRange
{
    StartValue = 0,
    EndValue = 50,
    Stroke = new SolidColorBrush(Colors.Green),
    StrokeThickness = 10
});

// Medium zone
scale.Ranges.Add(new CircularRange
{
    StartValue = 50,
    EndValue = 80,
    Stroke = new SolidColorBrush(Colors.Orange),
    StrokeThickness = 10
});

// High zone
scale.Ranges.Add(new CircularRange
{
    StartValue = 80,
    EndValue = 100,
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeThickness = 10
});
```

### Temperature Zones

**XAML:**
```xml
<gauge:CircularScale StartValue="-30" EndValue="50">
    <gauge:CircularScale.Ranges>
        <!-- Freezing (Blue) -->
        <gauge:CircularRange StartValue="-30" 
                             EndValue="0"
                             Stroke="Blue"
                             StrokeThickness="15"/>
        
        <!-- Comfortable (Green) -->
        <gauge:CircularRange StartValue="0" 
                             EndValue="25"
                             Stroke="Green"
                             StrokeThickness="15"/>
        
        <!-- Hot (Red) -->
        <gauge:CircularRange StartValue="25" 
                             EndValue="50"
                             Stroke="Red"
                             StrokeThickness="15"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="22"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Performance Bands

**XAML:**
```xml
<gauge:CircularScale StartValue="0" EndValue="100">
    <gauge:CircularScale.Ranges>
        <!-- Poor: 0-40 (Gray) -->
        <gauge:CircularRange StartValue="0" EndValue="40" 
                             Stroke="Gray"/>
        
        <!-- Fair: 40-60 (Yellow) -->
        <gauge:CircularRange StartValue="40" EndValue="60" 
                             Stroke="Yellow"/>
        
        <!-- Good: 60-80 (LightGreen) -->
        <gauge:CircularRange StartValue="60" EndValue="80" 
                             Stroke="LightGreen"/>
        
        <!-- Excellent: 80-100 (Green) -->
        <gauge:CircularRange StartValue="80" EndValue="100" 
                             Stroke="Green"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

## Gradient Ranges

Create smooth color transitions using gradient brushes.

### Linear Gradient

**XAML:**
```xml
<gauge:CircularRange StartValue="0" EndValue="100" 
                     StrokeThickness="20">
    <gauge:CircularRange.Stroke>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="Green" Offset="0"/>
            <GradientStop Color="Yellow" Offset="0.5"/>
            <GradientStop Color="Red" Offset="1"/>
        </LinearGradientBrush>
    </gauge:CircularRange.Stroke>
</gauge:CircularRange>
```

### Radial Gradient

**XAML:**
```xml
<gauge:CircularRange StartValue="0" EndValue="100" 
                     StrokeThickness="30">
    <gauge:CircularRange.Stroke>
        <RadialGradientBrush>
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="DeepSkyBlue" Offset="1"/>
        </RadialGradientBrush>
    </gauge:CircularRange.Stroke>
</gauge:CircularRange>
```

**C#:**
```csharp
LinearGradientBrush gradient = new LinearGradientBrush();
gradient.StartPoint = new Point(0, 0);
gradient.EndPoint = new Point(1, 0);
gradient.GradientStops.Add(new GradientStop(Colors.Green, 0));
gradient.GradientStops.Add(new GradientStop(Colors.Yellow, 0.5));
gradient.GradientStops.Add(new GradientStop(Colors.Red, 1));

range.Stroke = gradient;
```

## Common Patterns

### Pattern 1: Speedometer Zones

```xml
<gauge:CircularScale StartValue="0" EndValue="160">
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="0" EndValue="80" 
                             Stroke="Green" StrokeThickness="8"/>
        <gauge:CircularRange StartValue="80" EndValue="120" 
                             Stroke="Orange" StrokeThickness="8"/>
        <gauge:CircularRange StartValue="120" EndValue="160" 
                             Stroke="Red" StrokeThickness="8"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

### Pattern 2: Target Range

```xml
<gauge:CircularScale StartValue="0" EndValue="100" 
                     RangePosition="Custom">
    <gauge:CircularScale.Ranges>
        <!-- Target zone (70-90) -->
        <gauge:CircularRange StartValue="70" 
                             EndValue="90"
                             Stroke="LightGreen"
                             Offset="0.6"
                             StrokeThickness="30"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

### Pattern 3: Multi-Layer Ranges

```xml
<gauge:CircularScale RangePosition="Custom">
    <gauge:CircularScale.Ranges>
        <!-- Outer range -->
        <gauge:CircularRange StartValue="0" EndValue="50"
                             StartOffset="0.7" EndOffset="0.9"
                             Stroke="Blue"/>
        
        <!-- Inner range -->
        <gauge:CircularRange StartValue="0" EndValue="75"
                             StartOffset="0.4" EndOffset="0.6"
                             Stroke="Green"/>
    </gauge:CircularScale.Ranges>
</gauge:CircularScale>
```

### Pattern 4: Full Background Range

```xml
<gauge:CircularScale StartValue="0" EndValue="100"
                     SweepAngle="360"
                     RimStroke="Transparent">
    <gauge:CircularScale.Ranges>
        <!-- Background circle -->
        <gauge:CircularRange StartValue="0" 
                             EndValue="100"
                             Stroke="LightGray"
                             StrokeThickness="25"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="75"
                               RangePointerStroke="DeepSkyBlue"
                               RangePointerStrokeThickness="25"
                               RangeCap="Both"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

## Best Practices

### Zone Design

**Do:**
- Use intuitive colors (Green=Good, Red=Bad)
- Make zones visually distinct
- Ensure adequate thickness for visibility
- Consider colorblind-friendly palettes

**Avoid:**
- Too many zones (>5 becomes cluttered)
- Similar adjacent colors
- Overlapping ranges (unless intentional)

### Performance

**For many ranges:**
- Consider gradient instead of multiple ranges
- Use reasonable StrokeThickness (not excessive)
- Test rendering performance with complex ranges

## Next Steps

- **Add Pointers:** Display values on ranges
  → Read: [pointers.md](pointers.md)

- **Customize Labels:** Mark range boundaries
  → Read: [labels-and-ticks.md](labels-and-ticks.md)

- **Style Appearance:** Coordinate ranges with gauge theme
  → Read: [rim-and-appearance.md](rim-and-appearance.md)
