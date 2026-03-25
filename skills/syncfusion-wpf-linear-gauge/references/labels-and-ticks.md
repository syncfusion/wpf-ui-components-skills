# Labels and Ticks

Comprehensive guide to configuring and customizing labels and tick marks in SfLinearGauge. Labels provide numeric values at scale intervals, while ticks visually mark those intervals with major and minor divisions.

## Table of Contents
- [Labels](#labels)
  - [Label Color Customization](#label-color-customization)
  - [Label Font Customization](#label-font-customization)
  - [Label Positioning](#label-positioning)
  - [Label Prefix and Postfix](#label-prefix-and-postfix)
  - [Label Visibility](#label-visibility)
- [Ticks](#ticks)
  - [Tick Customization](#tick-customization)
  - [Tick Positioning](#tick-positioning)
  - [Minor Ticks Per Interval](#minor-ticks-per-interval)
- [Complete Examples](#complete-examples)

## Labels

Labels display numeric values at major tick positions on the scale. They help users interpret the gauge values.

### Label Color Customization

Change label text color using the **LabelStroke** property.

```xml
<gauge:LinearScale 
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray" 
    MinorTickStroke="Gray" 
    LabelStroke="Purple"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Colors.Purple),
    ScaleBarSize = 10,
    ScaleBarLength = 300,
    MinorTicksPerInterval = 3
};
```

**Property:**
- **LabelStroke** (Brush) - Color of label text

### Label Font Customization

Customize label typography and spacing.

```xml
<gauge:LinearScale 
    FontFamily="Monotype Corsiva"
    FontSize="15"
    FontStyle="Italic"
    LabelOffset="15"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray" 
    MinorTickStroke="Gray" 
    LabelStroke="#424242" 
    LabelSize="20"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    FontFamily = new FontFamily("Monotype Corsiva"),
    FontSize = 15,
    FontStyle = FontStyles.Italic,
    LabelOffset = 15,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    LabelSize = 20,
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};
```

**Key properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **LabelSize** | double | Font size of labels | 12 |
| **FontFamily** | FontFamily | Font family for labels | Segoe UI |
| **FontStyle** | FontStyle | Normal, Italic, Oblique | Normal |
| **LabelOffset** | double | Distance from default position | 0 |

**LabelOffset behavior:**
- **Horizontal orientation:** Offsets labels vertically from scale
- **Vertical orientation:** Offsets labels horizontally from scale
- **Positive values:** Move labels away from scale
- **Negative values:** Move labels toward scale

### Label Positioning

Place labels above or below the scale bar.

**Position options:**

1. **Above** - Labels appear above the scale
2. **Below (Default)** - Labels appear below the scale

```xml
<gauge:LinearScale 
    LabelPosition="Above" 
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    LabelPosition = LinearLabelsPosition.Above,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};
```

**Use cases:**
- **Above:** When pointers are below the scale
- **Below:** Standard position, when pointers are above
- **With LabelOffset:** Fine-tune spacing for custom layouts

### Label Prefix and Postfix

Add text before or after numeric label values.

#### Label Postfix

Append text to every label (e.g., units):

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="50"
    LabelPostfix="%"
    Interval="10"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray" 
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="0" />
<!-- Labels: 0%, 10%, 20%, 30%, 40%, 50% -->
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    Minimum = 0,
    Maximum = 50,
    Interval = 10,
    LabelPostfix = "%",
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 0
};
```

**Common postfix examples:**
- `"%"` - Percentages
- `"°C"` or `"°F"` - Temperature
- `" PSI"` - Pressure
- `" km/h"` - Speed
- `" MB"` - Data size

#### Label Prefix

Prepend text to every label (e.g., currency):

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="50"
    LabelPrefix="$"
    Interval="10"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="0" />
<!-- Labels: $0, $10, $20, $30, $40, $50 -->
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    Minimum = 0,
    Maximum = 50,
    Interval = 10,
    LabelPrefix = "$",
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 10,
    MinorTicksPerInterval = 0
};
```

**Common prefix examples:**
- `"$"` - Currency (US Dollar)
- `"€"` - Currency (Euro)
- `"£"` - Currency (British Pound)
- `"#"` - Numbers or rankings

#### Combining Prefix and Postfix

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="100"
    LabelPrefix="$"
    LabelPostfix="K"
    Interval="25" />
<!-- Labels: $0K, $25K, $50K, $75K, $100K -->
```

### Label Visibility

Hide labels while keeping ticks visible.

```xml
<gauge:LinearScale 
    LabelVisibility="Collapsed"
    TickPosition="Cross" 
    MajorTickSize="20" 
    MinorTickSize="9"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Black"
    MinorTickStroke="Black" 
    LabelStroke="#424242"
    ScaleBarSize="40" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    LabelVisibility = Visibility.Collapsed,
    TickPosition = LinearTicksPosition.Cross,
    MajorTickSize = 20,
    MinorTickSize = 9,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Black),
    MinorTickStroke = new SolidColorBrush(Colors.Black),
    ScaleBarSize = 40,
    MinorTicksPerInterval = 3
};
```

**Use cases:**
- Minimalist designs
- Space-constrained layouts
- When labels would clutter the display
- Visual-only indicators without numeric precision

## Ticks

Tick marks visually indicate scale intervals. **Major ticks** appear at labeled intervals, while **minor ticks** subdivide those intervals.

### Tick Customization

Customize appearance with stroke, size, and thickness properties.

```xml
<gauge:LinearScale 
    MajorTickSize="20" 
    MinorTickSize="9"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="SkyBlue"
    MinorTickStroke="Brown" 
    LabelStroke="#424242"
    MinorTickStrokeThickness="1" 
    MajorTickStrokeThickness="2"
    ScaleBarSize="10" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    MajorTickSize = 20,
    MinorTickSize = 9,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.SkyBlue),
    MinorTickStroke = new SolidColorBrush(Colors.Brown),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    MinorTickStrokeThickness = 1,
    MajorTickStrokeThickness = 2,
    ScaleBarSize = 10,
    MinorTicksPerInterval = 3
};
```

**Key properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **MajorTickStroke** | Brush | Color of major ticks | Black |
| **MajorTickSize** | double | Length of major ticks | 10 |
| **MajorTickStrokeThickness** | double | Width of major tick lines | 1 |
| **MinorTickStroke** | Brush | Color of minor ticks | Black |
| **MinorTickSize** | double | Length of minor ticks | 5 |
| **MinorTickStrokeThickness** | double | Width of minor tick lines | 1 |

### Tick Positioning

Position ticks above, below, or crossing the scale bar.

**Position options:**

1. **Above** - Ticks extend upward from the scale
2. **Below (Default)** - Ticks extend downward from the scale
3. **Cross** - Ticks cross through the scale bar

```xml
<gauge:LinearScale 
    TickPosition="Cross" 
    MajorTickSize="20" 
    MinorTickSize="9"
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Black" 
    MinorTickStroke="Black" 
    LabelStroke="#424242"
    ScaleBarSize="40" 
    MinorTicksPerInterval="3" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    TickPosition = LinearTicksPosition.Cross,
    MajorTickSize = 20,
    MinorTickSize = 9,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    MajorTickStroke = new SolidColorBrush(Colors.Black),
    MinorTickStroke = new SolidColorBrush(Colors.Black),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    ScaleBarSize = 40,
    MinorTicksPerInterval = 3
};
```

**Use cases:**
- **Above:** Labels below, ticks above for separation
- **Below:** Standard layout, ticks point down
- **Cross:** Emphasis on scale bar, balanced design

### Minor Ticks Per Interval

Control the number of minor ticks between major ticks using **MinorTicksPerInterval**.

```xml
<gauge:LinearScale 
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="10" 
    MinorTicksPerInterval="4" />
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
    ScaleBarLength = 300,
    MinorTicksPerInterval = 4
};
```

**Calculation:**
- If `Interval = 10` and `MinorTicksPerInterval = 4`:
  - Minor ticks appear at: 2.5, 5, 7.5 (within each 0-10 interval)
  - Total subdivisions: 5 (4 minor + 1 major)

**Common values:**
- **0** - No minor ticks
- **1** - One minor tick (divides interval in half)
- **3** - Three minor ticks (quarters the interval)
- **4** - Four minor ticks (fifths)
- **9** - Nine minor ticks (tenths)

**Examples:**

```xml
<!-- No minor ticks -->
<gauge:LinearScale MinorTicksPerInterval="0" />

<!-- One minor tick (halfway) -->
<gauge:LinearScale 
    Interval="10" 
    MinorTicksPerInterval="1" />
<!-- Ticks at: 0, 5, 10, 15, 20, ... -->

<!-- Three minor ticks (quarters) -->
<gauge:LinearScale 
    Interval="20" 
    MinorTicksPerInterval="3" />
<!-- Ticks at: 0, 5, 10, 15, 20, 25, 30, ... -->
```

### Tick Visibility

Hide specific tick types while keeping others:

```xml
<!-- Hide minor ticks -->
<gauge:LinearScale 
    MinorTicksPerInterval="0"
    MajorTickSize="15"
    MajorTickStroke="Gray" />

<!-- Hide major ticks (unusual, but possible) -->
<gauge:LinearScale 
    MajorTickSize="0"
    MinorTickSize="6"
    MinorTicksPerInterval="9" />
```

## Complete Examples

### Professional Dashboard Style

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            Interval="10"
            LabelPostfix="%"
            LabelPosition="Below"
            LabelOffset="8"
            LabelStroke="#263238"
            LabelSize="14"
            FontFamily="Segoe UI"
            FontStyle="Normal"
            ScaleBarStroke="#ECEFF1"
            ScaleBarSize="14"
            MajorTickStroke="#546E7A"
            MajorTickSize="18"
            MajorTickStrokeThickness="2"
            MinorTickStroke="#90A4AE"
            MinorTickSize="10"
            MinorTickStrokeThickness="1"
            MinorTicksPerInterval="4"
            TickPosition="Above">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="72"
                    BarPointerStroke="#1976D2"
                    BarPointerStrokeThickness="14" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Thermometer with Temperature Labels

```xml
<gauge:SfLinearGauge Orientation="Vertical" Height="450">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="-20" 
            Maximum="120"
            Interval="20"
            LabelPostfix="°C"
            LabelPosition="Below"
            LabelOffset="10"
            LabelStroke="#212121"
            LabelSize="13"
            ScaleBarStroke="#CFD8DC"
            ScaleBarSize="18"
            MajorTickStroke="#37474F"
            MajorTickSize="12"
            MajorTickStrokeThickness="2"
            MinorTickStroke="#78909C"
            MinorTickSize="6"
            MinorTickStrokeThickness="1"
            MinorTicksPerInterval="3"
            TickPosition="Below">
            
            <gauge:LinearScale.Ranges>
                <gauge:LinearRange 
                    StartValue="-20" 
                    EndValue="0"
                    RangeStroke="#2196F3"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.25" />
                
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="40"
                    RangeStroke="#4CAF50"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.25" />
                
                <gauge:LinearRange 
                    StartValue="40" 
                    EndValue="120"
                    RangeStroke="#FF5722"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.25" />
            </gauge:LinearScale.Ranges>
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="23"
                    BarPointerStroke="#E53935"
                    BarPointerStrokeThickness="18" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Currency Gauge with Prefix

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="10000"
            Interval="2000"
            LabelPrefix="$"
            LabelStroke="#1B5E20"
            LabelSize="15"
            FontFamily="Arial"
            ScaleBarStroke="#E8F5E9"
            ScaleBarSize="12"
            MajorTickStroke="#388E3C"
            MajorTickSize="14"
            MinorTickStroke="#81C784"
            MinorTickSize="8"
            MinorTicksPerInterval="3">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="6500"
                    BarPointerStroke="#4CAF50"
                    BarPointerStrokeThickness="12" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Minimalist Gauge (No Labels)

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            LabelVisibility="Collapsed"
            TickPosition="Cross"
            MajorTickSize="22"
            MinorTickSize="12"
            ScaleBarStroke="#ECEFF1"
            MajorTickStroke="#37474F"
            MajorTickStrokeThickness="2"
            MinorTickStroke="#78909C"
            MinorTickStrokeThickness="1"
            ScaleBarSize="30"
            MinorTicksPerInterval="4">
            
            <gauge:LinearScale.Ranges>
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="100"
                    RangeStroke="#1976D2"
                    StartWidth="8"
                    EndWidth="8"
                    RangeOffset="0.4"
                    RangeOpacity="0.6" />
            </gauge:LinearScale.Ranges>
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="68"
                    SymbolPointerPosition="Cross"
                    SymbolPointerHeight="24"
                    SymbolPointerWidth="24"
                    SymbolPointerStroke="#0D47A1" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Color-Coded Labels and Ticks

Bind label and tick colors to ranges:

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            Interval="10"
            BindRangeStrokeToLabels="True"
            BindRangeStrokeToTicks="True"
            LabelSize="14"
            ScaleBarStroke="#FAFAFA"
            ScaleBarSize="15"
            MajorTickSize="16"
            MajorTickStrokeThickness="2"
            MinorTickSize="8"
            MinorTicksPerInterval="4">
            
            <gauge:LinearScale.Ranges>
                <!-- Low (Red): 0-30 -->
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="30"
                    RangeStroke="#E53935"
                    StartWidth="18"
                    EndWidth="18"
                    RangeOffset="0.4" />
                
                <!-- Medium (Orange): 30-60 -->
                <gauge:LinearRange 
                    StartValue="30" 
                    EndValue="60"
                    RangeStroke="#FB8C00"
                    StartWidth="18"
                    EndWidth="18"
                    RangeOffset="0.4" />
                
                <!-- High (Green): 60-100 -->
                <gauge:LinearRange 
                    StartValue="60" 
                    EndValue="100"
                    RangeStroke="#43A047"
                    StartWidth="18"
                    EndWidth="18"
                    RangeOffset="0.4" />
            </gauge:LinearScale.Ranges>
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="75"
                    BarPointerStroke="#1B5E20"
                    BarPointerStrokeThickness="15" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Best Practices

### Label Formatting

1. **Units:** Always add postfix for measurements (%, °C, PSI, etc.)
2. **Currency:** Use appropriate prefix ($, €, £)
3. **Readability:** Ensure sufficient font size (12-16 for most cases)
4. **Contrast:** High contrast between labels and background

### Tick Configuration

1. **Major ticks:** Should align with labeled values
2. **Minor ticks:** 3-4 per interval provides good granularity
3. **Size ratio:** Major ticks typically 2x the size of minor ticks
4. **Visibility:** Ensure ticks don't blend into the scale bar

### Layout Considerations

1. **Label spacing:** Use `LabelOffset` if labels overlap with other elements
2. **Tick position:** Match with label position for visual balance
3. **Consistent styling:** Use same tick/label style across similar gauges
4. **Avoid clutter:** If space is limited, reduce `MinorTicksPerInterval` or hide labels

### Typography

1. **Font choice:** Use clear, readable fonts (Segoe UI, Arial, Roboto)
2. **Font size:** 12-16px for most displays, larger for dashboards
3. **Font style:** Normal for readability, Italic sparingly for emphasis
4. **Consistency:** Same font family across all gauges in an application

## Troubleshooting

### Labels Overlapping

**Issue:** Label text overlaps between adjacent labels.

**Solutions:**
1. Increase the `Interval` to space labels further apart:
   ```xml
   <gauge:LinearScale Interval="20" />
   ```

2. Increase `ScaleBarLength` to give more space:
   ```xml
   <gauge:LinearScale ScaleBarLength="500" />
   ```

3. Reduce font size:
   ```xml
   <gauge:LinearScale LabelSize="12" />
   ```

4. Hide every other label programmatically (advanced)

### Labels Too Far from Scale

**Issue:** Labels are positioned too far from the scale bar.

**Solution:** Adjust `LabelOffset` to bring them closer:

```xml
<gauge:LinearScale LabelOffset="5" />
```

### Ticks Not Visible

**Issue:** Tick marks don't appear or are too small.

**Solutions:**
1. Increase tick size:
   ```xml
   <gauge:LinearScale 
       MajorTickSize="18" 
       MinorTickSize="10" />
   ```

2. Set contrasting color:
   ```xml
   <gauge:LinearScale 
       MajorTickStroke="Black"
       MinorTickStroke="Gray" />
   ```

3. Increase thickness:
   ```xml
   <gauge:LinearScale 
       MajorTickStrokeThickness="2"
       MinorTickStrokeThickness="1" />
   ```

### Minor Ticks Too Dense

**Issue:** Too many minor ticks making the scale cluttered.

**Solution:** Reduce `MinorTicksPerInterval`:

```xml
<gauge:LinearScale MinorTicksPerInterval="1" />
```

Or disable minor ticks entirely:

```xml
<gauge:LinearScale MinorTicksPerInterval="0" />
```

### Labels Not Showing Prefix/Postfix

**Issue:** Prefix or postfix doesn't appear on labels.

**Solutions:**
1. Verify property spelling is correct: `LabelPrefix` and `LabelPostfix`
2. Ensure the value is properly quoted:
   ```xml
   <gauge:LinearScale LabelPostfix="%" />
   ```
3. Check that labels are visible (`LabelVisibility` is not Collapsed)

---

**Next:** Explore [Orientation](orientation.md) to learn about switching between horizontal and vertical gauge layouts.
