# Labels and Ticks

## Table of Contents
- [Labels Overview](#labels-overview)
- [Label Customization](#label-customization)
- [Label Positioning](#label-positioning)
- [Label Formatting](#label-formatting)
- [Major Ticks](#major-ticks)
- [Minor Ticks](#minor-ticks)
- [Hiding Labels and Ticks](#hiding-labels-and-ticks)
- [Common Patterns](#common-patterns)

## Labels Overview

**Labels** display the numeric values along the circular scale at regular intervals defined by the `Interval` property.

**Key Features:**
- Automatic placement based on Interval
- Customizable fonts and colors
- Flexible positioning (Inside, Outside, Custom)
- Support for prefix/suffix
- Custom formatting

**Default Behavior:** Labels are placed automatically at major tick intervals.

## Label Customization

### Label Color

Change label color with `LabelStroke`:

**XAML:**
```xml
<gauge:CircularScale LabelStroke="DeepPink">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.LabelStroke = new SolidColorBrush(Colors.DeepPink);
```

### Font Customization

**XAML:**
```xml
<gauge:CircularScale FontFamily="Monotype Corsiva" 
                     FontSize="20"
                     FontStyle="Italic"
                     FontWeight="Bold"
                     LabelStroke="Black">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.FontFamily = new FontFamily("Monotype Corsiva");
scale.FontSize = 20;
scale.FontStyle = FontStyles.Italic;
scale.FontWeight = FontWeights.Bold;
scale.LabelStroke = new SolidColorBrush(Colors.Black);
```

**Font Properties:**
- `FontFamily` - Font name (string)
- `FontSize` - Size in points (double)
- `FontStyle` - Normal, Italic, Oblique
- `FontWeight` - Normal, Bold, Light, etc.

### Complete Label Styling

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     Interval="10"
                     FontFamily="Segoe UI"
                     FontSize="14"
                     FontWeight="SemiBold"
                     LabelStroke="#333">
    <!-- Scale content -->
</gauge:CircularScale>
```

## Label Positioning

Control where labels appear relative to the scale.

### LabelPosition Property

**Options:**
- `Default` - Standard position based on scale
- `Inside` - Inside the scale rim
- `Outside` - Outside the scale rim
- `Custom` - Use LabelOffset for precise placement

**XAML:**
```xml
<gauge:CircularScale LabelPosition="Outside">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.LabelPosition = LabelPosition.Outside;
```

### LabelOffset Property

Fine-tune label distance from rim (0 to 1):

**XAML:**
```xml
<gauge:CircularScale LabelPosition="Custom" 
                     LabelOffset="0.15">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.LabelPosition = LabelPosition.Custom;
scale.LabelOffset = 0.15;
```

**Offset Values:**
- `0` - At the rim
- `0.1` - Slightly inward (typical)
- `0.3` - Further from rim
- Negative values move labels outward

### Multi-Scale Label Positioning

Different label positions for multiple scales:

**XAML:**
```xml
<gauge:SfCircularGauge SpacingMargin="0.7">
    <gauge:SfCircularGauge.Scales>
        <!-- Outer scale: Labels outside -->
        <gauge:CircularScale Radius="175" 
                             LabelPosition="Outside"
                             LabelStroke="Blue"/>
        
        <!-- Inner scale: Labels inside -->
        <gauge:CircularScale Radius="90" 
                             LabelPosition="Inside"
                             LabelStroke="Red"/>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

## Label Formatting

### Prefix and Suffix

Add units or symbols to labels:

**XAML:**
```xml
<gauge:CircularScale LabelPrefix="$" 
                     LabelSuffix="K">
    <!-- Labels: $0K, $10K, $20K, ... -->
</gauge:CircularScale>
```

**C#:**
```csharp
scale.LabelPrefix = "$";
scale.LabelSuffix = "K";
```

**Common Use Cases:**
- Temperature: `LabelSuffix="°C"`
- Speed: `LabelSuffix=" mph"`
- Currency: `LabelPrefix="$"`
- Percentage: `LabelSuffix="%"`

### Custom Label Format

Use `LabelFormat` for number formatting:

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="1"
                     Interval="0.1"
                     LabelFormat="N2">
    <!-- Labels: 0.00, 0.10, 0.20, ... -->
</gauge:CircularScale>
```

**Format Strings:**
- `"N0"` - No decimals (100)
- `"N2"` - Two decimals (100.00)
- `"C"` - Currency ($100.00)
- `"P"` - Percentage (100%)
- `"F1"` - Fixed-point, one decimal (100.0)

**C#:**
```csharp
scale.LabelFormat = "N2";
```

### Custom Label Template

For complete control, use `LabelTemplate`:

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.LabelTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" 
                       Foreground="Blue"
                       FontWeight="Bold"
                       FontSize="12"/>
        </DataTemplate>
    </gauge:CircularScale.LabelTemplate>
</gauge:CircularScale>
```

## Major Ticks

**Major ticks** are the primary tick marks at each label position.

### Basic Configuration

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Length="15" 
                                StrokeThickness="2"
                                Stroke="Black"/>
    </gauge:CircularScale.MajorTickSettings>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.MajorTickSettings = new MajorTickSetting
{
    Length = 15,
    StrokeThickness = 2,
    Stroke = new SolidColorBrush(Colors.Black)
};
```

### Major Tick Properties

**Length:**
```xml
<gauge:MajorTickSetting Length="20"/>
```
Default: 10 pixels

**Stroke:**
```xml
<gauge:MajorTickSetting Stroke="Red"/>
```

**StrokeThickness:**
```xml
<gauge:MajorTickSetting StrokeThickness="3"/>
```

**Offset:**
```xml
<gauge:MajorTickSetting Offset="0.9"/>
```
Distance from center (0-1)

### Complete Example

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     Interval="10">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Length="18"
                                StrokeThickness="2"
                                Stroke="#333"
                                Offset="0.95"/>
    </gauge:CircularScale.MajorTickSettings>
</gauge:CircularScale>
```

## Minor Ticks

**Minor ticks** are smaller tick marks between major ticks.

### Basic Configuration

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.MinorTickSettings>
        <gauge:MinorTickSetting Length="8"
                                StrokeThickness="1"
                                Stroke="Gray"/>
    </gauge:CircularScale.MinorTickSettings>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.MinorTickSettings = new MinorTickSetting
{
    Length = 8,
    StrokeThickness = 1,
    Stroke = new SolidColorBrush(Colors.Gray)
};
```

### MinorTicksPerInterval

Control how many minor ticks appear between major ticks:

**XAML:**
```xml
<gauge:CircularScale Interval="10" 
                     MinorTicksPerInterval="4">
    <!-- 4 minor ticks between each major tick -->
    <gauge:CircularScale.MinorTickSettings>
        <gauge:MinorTickSetting Length="6" 
                                StrokeThickness="1"/>
    </gauge:CircularScale.MinorTickSettings>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.MinorTicksPerInterval = 4;
```

**Default:** 1 minor tick between major ticks

### Minor Tick Properties

Same properties as major ticks:
- `Length` - Tick length in pixels
- `Stroke` - Tick color
- `StrokeThickness` - Tick width
- `Offset` - Distance from center (0-1)

### Complete Example

**XAML:**
```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     Interval="20"
                     MinorTicksPerInterval="3">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Length="15"
                                StrokeThickness="2"
                                Stroke="Black"/>
    </gauge:CircularScale.MajorTickSettings>
    <gauge:CircularScale.MinorTickSettings>
        <gauge:MinorTickSetting Length="8"
                                StrokeThickness="1"
                                Stroke="Gray"/>
    </gauge:CircularScale.MinorTickSettings>
</gauge:CircularScale>
```

## Hiding Labels and Ticks

### Hide Labels

**Make labels transparent:**
```xml
<gauge:CircularScale LabelStroke="Transparent">
```

**Or use visibility:**
```xml
<gauge:CircularScale ShowLabels="False">
```

### Hide Ticks

**XAML:**
```xml
<gauge:CircularScale ShowTicks="False">
```

**C#:**
```csharp
scale.ShowTicks = false;
```

### Hide Specific Tick Type

**Hide major ticks only:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Stroke="Transparent"/>
    </gauge:CircularScale.MajorTickSettings>
</gauge:CircularScale>
```

**Hide minor ticks only:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.MinorTickSettings>
        <gauge:MinorTickSetting Stroke="Transparent"/>
    </gauge:CircularScale.MinorTickSettings>
</gauge:CircularScale>
```

### Minimalist Gauge (No Labels/Ticks)

**XAML:**
```xml
<gauge:CircularScale ShowTicks="False"
                     LabelStroke="Transparent"
                     ShowRim="False">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="75"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

## Common Patterns

### Pattern 1: Speedometer Styling

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="160"
                     Interval="20"
                     FontSize="13"
                     FontWeight="SemiBold"
                     LabelStroke="#333"
                     LabelOffset="0.1">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Length="12"
                                StrokeThickness="2"
                                Stroke="#333"/>
    </gauge:CircularScale.MajorTickSettings>
    <gauge:CircularScale.MinorTickSettings>
        <gauge:MinorTickSetting Length="6"
                                StrokeThickness="1"
                                Stroke="#666"/>
    </gauge:CircularScale.MinorTickSettings>
</gauge:CircularScale>
```

### Pattern 2: Temperature Gauge with Units

```xml
<gauge:CircularScale StartValue="-20" 
                     EndValue="50"
                     Interval="10"
                     LabelSuffix="°C"
                     FontSize="12"
                     LabelStroke="Black">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Length="10" 
                                Stroke="Black"/>
    </gauge:CircularScale.MajorTickSettings>
</gauge:CircularScale>
```

### Pattern 3: Minimal Progress Gauge

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     SweepAngle="360"
                     ShowTicks="False"
                     LabelStroke="Transparent">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="75"
                               RangeCap="Both"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Pattern 4: Detailed Scale with Many Minor Ticks

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     Interval="10"
                     MinorTicksPerInterval="9"
                     FontSize="14"
                     LabelOffset="0.15">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Length="20"
                                StrokeThickness="2"
                                Stroke="Black"/>
    </gauge:CircularScale.MajorTickSettings>
    <gauge:CircularScale.MinorTickSettings>
        <gauge:MinorTickSetting Length="8"
                                StrokeThickness="1"
                                Stroke="#999"/>
    </gauge:CircularScale.MinorTickSettings>
</gauge:CircularScale>
```

### Pattern 5: Percentage Gauge

```xml
<gauge:CircularScale StartValue="0" 
                     EndValue="100"
                     Interval="25"
                     LabelSuffix="%"
                     FontSize="16"
                     FontWeight="Bold"
                     LabelStroke="DeepSkyBlue">
    <gauge:CircularScale.MajorTickSettings>
        <gauge:MajorTickSetting Stroke="DeepSkyBlue" 
                                Length="15"/>
    </gauge:CircularScale.MajorTickSettings>
</gauge:CircularScale>
```

## Best Practices

### Readability

**Do:**
- Use adequate font size (12-16pt typically)
- Ensure good contrast (label color vs. background)
- Space labels appropriately with Interval
- Use consistent tick styling

**Avoid:**
- Too many labels (overcrowding)
- Very small fonts (<10pt)
- Low contrast colors

### Tick Guidelines

**Major Ticks:**
- Should be prominent (length 10-20px)
- Thicker than minor ticks
- Match label positions

**Minor Ticks:**
- Smaller than major ticks (50-70% of major length)
- Use subtle colors (gray)
- Don't overuse (3-9 per interval max)

### Performance

**For complex scales:**
- Minimize custom templates
- Use reasonable tick counts
- Cache brushes if creating many gauges

## Next Steps

- **Style the Rim:** Coordinate tick styling with rim
  → Read: [rim-and-appearance.md](rim-and-appearance.md)

- **Add Ranges:** Visual zones aligned with labels
  → Read: [ranges.md](ranges.md)

- **Theming:** Apply consistent styles across gauges
  → Read: [rim-and-appearance.md#theming](rim-and-appearance.md#theming)
