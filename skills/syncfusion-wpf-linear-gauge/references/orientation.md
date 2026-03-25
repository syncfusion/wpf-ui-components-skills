# Orientation

Guide to configuring the orientation of SfLinearGauge. The Linear Gauge supports both horizontal and vertical orientations, allowing you to create landscape-style meters or portrait-style thermometers based on your layout needs.

## Orientation Overview

The **Orientation** property controls the gauge's layout direction:

- **Horizontal (Default)** - Landscape layout, scale runs left-to-right
- **Vertical** - Portrait layout, scale runs bottom-to-top (or top-to-bottom with ScaleDirection)

**Key behavior changes:**
- **ScaleBarSize** - In horizontal: height; In vertical: width
- **ScaleBarLength** - In horizontal: width; In vertical: height
- **Pointers** - Adjust positioning based on orientation
- **Ranges** - Adapt to the new layout direction
- **Labels/Ticks** - Reposition automatically

## Horizontal Orientation (Default)

The default orientation creates a horizontal gauge where the scale extends from left to right.

### Basic Horizontal Gauge

```xml
<gauge:SfLinearGauge Orientation="Horizontal">
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
SfLinearGauge gauge = new SfLinearGauge
{
    Orientation = System.Windows.Controls.Orientation.Horizontal
};

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

### Horizontal Use Cases

**Progress indicators:**
```xml
<gauge:SfLinearGauge Orientation="Horizontal">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            LabelPostfix="%">
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="65"
                    BarPointerStroke="#4CAF50"
                    BarPointerStrokeThickness="12"
                    EnableAnimation="True" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**Dashboard metrics:**
```xml
<gauge:SfLinearGauge Orientation="Horizontal">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="10"
            ScaleBarLength="400"
            ScaleBarSize="15">
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="7.5"
                    SymbolPointerStroke="#2196F3" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**Speed meters:**
```xml
<gauge:SfLinearGauge Orientation="Horizontal">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="200"
            LabelPostfix=" km/h"
            Interval="20">
            <gauge:LinearScale.Ranges>
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="60"
                    RangeStroke="Green" />
                <gauge:LinearRange 
                    StartValue="60" 
                    EndValue="120"
                    RangeStroke="Yellow" />
                <gauge:LinearRange 
                    StartValue="120" 
                    EndValue="200"
                    RangeStroke="Red" />
            </gauge:LinearScale.Ranges>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Vertical Orientation

Vertical orientation creates a gauge where the scale extends from bottom to top (or top to bottom), ideal for thermometer-style displays.

### Basic Vertical Gauge

```xml
<gauge:SfLinearGauge Orientation="Vertical">
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
SfLinearGauge gauge = new SfLinearGauge
{
    Orientation = System.Windows.Controls.Orientation.Vertical
};

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

### Vertical Use Cases

**Thermometer:**
```xml
<gauge:SfLinearGauge Orientation="Vertical" Height="400" Width="150">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="-20" 
            Maximum="120"
            Interval="20"
            LabelPostfix="°C"
            ScaleBarSize="18"
            ScaleBarLength="350">
            
            <gauge:LinearScale.Ranges>
                <gauge:LinearRange 
                    StartValue="-20" 
                    EndValue="0"
                    RangeStroke="#2196F3"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.3" />
                
                <gauge:LinearRange 
                    StartValue="0" 
                    EndValue="40"
                    RangeStroke="#4CAF50"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.3" />
                
                <gauge:LinearRange 
                    StartValue="40" 
                    EndValue="120"
                    RangeStroke="#FF5722"
                    StartWidth="10"
                    EndWidth="10"
                    RangeOffset="0.3" />
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

**Tank level indicator:**
```xml
<gauge:SfLinearGauge Orientation="Vertical" Height="350">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            LabelPostfix="%"
            Interval="10"
            ScaleBarSize="25"
            ScaleBarLength="300">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="68"
                    BarPointerStroke="#03A9F4"
                    BarPointerStrokeThickness="25" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**Vertical progress bar:**
```xml
<gauge:SfLinearGauge Orientation="Vertical" Height="300" Width="80">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            LabelVisibility="Collapsed"
            ScaleBarSize="20"
            ScaleBarLength="280"
            ScaleBarStroke="#E0E0E0">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="75"
                    BarPointerStroke="#8BC34A"
                    BarPointerStrokeThickness="20"
                    EnableAnimation="True"
                    AnimationDuration="800" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Layout Considerations

### Size Properties Behavior

**Horizontal orientation:**
- **ScaleBarSize** = Height of the scale bar
- **ScaleBarLength** = Width of the scale bar
- **Gauge Width** = Influenced by ScaleBarLength
- **Gauge Height** = Influenced by ScaleBarSize + labels + ticks

**Vertical orientation:**
- **ScaleBarSize** = Width of the scale bar
- **ScaleBarLength** = Height of the scale bar
- **Gauge Width** = Influenced by ScaleBarSize + labels + ticks
- **Gauge Height** = Influenced by ScaleBarLength

### Container Sizing

**Horizontal gauge in container:**
```xml
<Grid>
    <gauge:SfLinearGauge 
        Orientation="Horizontal" 
        HorizontalAlignment="Stretch"
        VerticalAlignment="Center"
        Height="100">
        <!-- Gauge content -->
    </gauge:SfLinearGauge>
</Grid>
```

**Vertical gauge in container:**
```xml
<Grid>
    <gauge:SfLinearGauge 
        Orientation="Vertical" 
        HorizontalAlignment="Center"
        VerticalAlignment="Stretch"
        Width="120">
        <!-- Gauge content -->
    </gauge:SfLinearGauge>
</Grid>
```

### Responsive Layouts

**Horizontal - fill width:**
```xml
<gauge:SfLinearGauge 
    Orientation="Horizontal"
    HorizontalAlignment="Stretch">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale ScaleBarSize="12">
            <!-- ScaleBarLength adapts to container width -->
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**Vertical - fill height:**
```xml
<gauge:SfLinearGauge 
    Orientation="Vertical"
    VerticalAlignment="Stretch">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale ScaleBarSize="12">
            <!-- ScaleBarLength adapts to container height -->
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Combining Orientation with Scale Direction

Control value progression direction with **ScaleDirection**.

### Horizontal Forward (Default)

Values increase left-to-right:
```xml
<gauge:SfLinearGauge Orientation="Horizontal">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale ScaleDirection="Forward" />
        <!-- 0 on left, 100 on right -->
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Horizontal Backward

Values increase right-to-left:
```xml
<gauge:SfLinearGauge Orientation="Horizontal">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale ScaleDirection="Backward" />
        <!-- 100 on left, 0 on right -->
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Vertical Forward (Default)

Values increase bottom-to-top:
```xml
<gauge:SfLinearGauge Orientation="Vertical">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale ScaleDirection="Forward" />
        <!-- 0 at bottom, 100 at top -->
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Vertical Backward

Values increase top-to-bottom:
```xml
<gauge:SfLinearGauge Orientation="Vertical">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale ScaleDirection="Backward" />
        <!-- 100 at bottom, 0 at top -->
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Complete Examples

### Horizontal Progress Dashboard

```xml
<StackPanel Spacing="20">
    <TextBlock Text="Task Completion" FontSize="16" FontWeight="Bold" />
    
    <gauge:SfLinearGauge Orientation="Horizontal">
        <gauge:SfLinearGauge.MainScale>
            <gauge:LinearScale 
                Minimum="0" 
                Maximum="100"
                Interval="10"
                LabelPostfix="%"
                ScaleBarStroke="#ECEFF1"
                ScaleBarSize="14"
                ScaleBarLength="450"
                LabelStroke="#263238"
                MajorTickStroke="#546E7A"
                MinorTickStroke="#90A4AE"
                MinorTicksPerInterval="4">
                
                <gauge:LinearScale.Pointers>
                    <gauge:LinearPointer 
                        PointerType="BarPointer"
                        Value="72"
                        BarPointerStroke="#1976D2"
                        BarPointerStrokeThickness="14"
                        EnableAnimation="True"
                        AnimationDuration="600" />
                    
                    <gauge:LinearPointer 
                        PointerType="SymbolPointer"
                        Value="72"
                        SymbolPointerPosition="Above"
                        SymbolPointerHeight="16"
                        SymbolPointerWidth="16"
                        SymbolPointerStroke="#0D47A1"
                        EnableAnimation="True"
                        AnimationDuration="600" />
                </gauge:LinearScale.Pointers>
            </gauge:LinearScale>
        </gauge:SfLinearGauge.MainScale>
    </gauge:SfLinearGauge>
</StackPanel>
```

### Vertical Temperature Monitor

```xml
<Grid Width="180" Height="500">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>
    
    <TextBlock 
        Grid.Column="0"
        Text="Temperature Monitor" 
        FontSize="14" 
        FontWeight="Bold"
        VerticalAlignment="Top"
        Margin="0,0,0,10" />
    
    <gauge:SfLinearGauge 
        Grid.Column="1"
        Orientation="Vertical" 
        Height="450" 
        Width="140"
        VerticalAlignment="Bottom">
        <gauge:SfLinearGauge.MainScale>
            <gauge:LinearScale 
                Minimum="-20" 
                Maximum="120"
                Interval="20"
                LabelPostfix="°C"
                LabelPosition="Below"
                LabelOffset="8"
                LabelStroke="#212121"
                LabelSize="12"
                ScaleBarStroke="#CFD8DC"
                ScaleBarSize="18"
                ScaleBarLength="400"
                MajorTickStroke="#37474F"
                MajorTickSize="12"
                MinorTickStroke="#78909C"
                MinorTickSize="6"
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
                        Value="28"
                        BarPointerStroke="#E53935"
                        BarPointerStrokeThickness="18"
                        EnableAnimation="True"
                        AnimationDuration="800" />
                </gauge:LinearScale.Pointers>
            </gauge:LinearScale>
        </gauge:SfLinearGauge.MainScale>
    </gauge:SfLinearGauge>
</Grid>
```

## Best Practices

### Choosing Orientation

**Use horizontal when:**
- Creating progress bars or completion indicators
- Building dashboard widgets with limited vertical space
- Displaying metrics in wide containers
- Implementing slider-style indicators

**Use vertical when:**
- Creating thermometer-style displays
- Building tank level or fluid indicators
- Displaying metrics in tall, narrow containers
- Implementing vertical progress trackers

### Layout Tips

1. **Set explicit dimensions** - Especially for vertical gauges in flexible layouts
2. **Account for labels/ticks** - Add margin/padding for elements outside scale bar
3. **Test responsiveness** - Verify behavior when container resizes
4. **Consistent sizing** - Use similar dimensions for multiple gauges in a layout

### Visual Design

1. **Orientation consistency** - Use same orientation for related gauges
2. **Appropriate sizing** - Horizontal: wider than tall; Vertical: taller than wide
3. **Clear labeling** - Ensure labels don't overlap with other UI elements
4. **Spacing** - Leave adequate space around gauges for visual clarity

## Troubleshooting

### Gauge Not Displaying Correctly

**Issue:** Gauge doesn't appear or is clipped.

**Solutions:**
1. Set explicit Width/Height on the gauge:
   ```xml
   <gauge:SfLinearGauge Orientation="Vertical" Height="400" Width="120">
   ```

2. Ensure container has adequate space

3. Check ScaleBarLength isn't larger than container

### Labels Cut Off

**Issue:** Labels at the edges are clipped.

**Solution:** Add margin to the gauge:

```xml
<gauge:SfLinearGauge Margin="20,10">
```

### Vertical Gauge Too Wide

**Issue:** Vertical gauge takes up too much horizontal space.

**Solutions:**
1. Reduce ScaleBarSize:
   ```xml
   <gauge:LinearScale ScaleBarSize="12" />
   ```

2. Set gauge Width explicitly:
   ```xml
   <gauge:SfLinearGauge Width="100">
   ```

3. Position labels closer or hide them:
   ```xml
   <gauge:LinearScale LabelVisibility="Collapsed" />
   ```

### Pointer Positioning Issues

**Issue:** Pointers don't align correctly after orientation change.

**Solution:** Verify pointer positioning properties are appropriate for the orientation. Symbol pointer positions (Above/Below/Cross) work in both orientations but have different visual effects.

---

**Congratulations!** You now have comprehensive knowledge of the Syncfusion WPF Linear Gauge. Refer back to the [main SKILL.md](../SKILL.md) for navigation to specific topics or to review the Quick Start example.
