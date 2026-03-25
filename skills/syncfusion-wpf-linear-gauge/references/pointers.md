# Pointers

Comprehensive guide to implementing and customizing pointers in SfLinearGauge. Pointers mark specific values on the scale and come in two types: bar pointers (for progress indication) and symbol pointers (for value marking).

## Table of Contents
- [Pointer Types Overview](#pointer-types-overview)
- [Bar Pointer](#bar-pointer)
- [Symbol Pointer](#symbol-pointer)
- [Positioning Symbol Pointers](#positioning-symbol-pointers)
- [Custom Symbol Pointer Shapes](#custom-symbol-pointer-shapes)
- [Adding Multiple Pointers](#adding-multiple-pointers)
- [Pointer Animation](#pointer-animation)
- [Data Binding](#data-binding)
- [Complete Examples](#complete-examples)

## Pointer Types Overview

The LinearGauge supports two pointer types through the **PointerType** property:

1. **BarPointer** - Fills the scale from the beginning to the pointer value
2. **SymbolPointer** - Places a shape at the pointer value

**Key characteristics:**

| Pointer Type | Best For | Visual Style |
|--------------|----------|--------------|
| **BarPointer** | Progress, completion, filled values | Solid bar from 0 to value |
| **SymbolPointer** | Specific value markers, targets | Shape at exact value point |

**Common combinations:**
- Bar + Symbol - Show progress bar with current value marker
- Multiple Symbols - Compare multiple values on one scale
- Multiple Bars - Show layered progress or thresholds

## Bar Pointer

Bar pointers fill the scale from the minimum value to the pointer's value, ideal for showing progress or completion.

### Basic Bar Pointer

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            ScaleBarStroke="#E0E0E0" 
            MajorTickStroke="Gray"
            MinorTickStroke="Gray" 
            LabelStroke="#424242" 
            ScaleBarSize="20" 
            MinorTicksPerInterval="3">
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer" 
                    Value="75"
                    BarPointerStroke="#36D1DC"
                    BarPointerStrokeThickness="10" />
            </gauge:LinearScale.Pointers>
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
    ScaleBarSize = 20,
    MinorTicksPerInterval = 3
};

LinearPointer barPointer = new LinearPointer
{
    PointerType = LinearPointerType.BarPointer,
    Value = 75,
    BarPointerStroke = new SolidColorBrush(Color.FromRgb(54, 209, 220)),
    BarPointerStrokeThickness = 10
};

scale.Pointers.Add(barPointer);
gauge.MainScale = scale;
```

### Bar Pointer Customization

Control the appearance using these properties:

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **Value** | double | The value where the bar ends | 0 |
| **BarPointerStroke** | Brush | Color of the bar | Black |
| **BarPointerStrokeThickness** | double | Width of the bar | 5 |

### Custom Bar Pointer Colors

```xml
<gauge:LinearScale.Pointers>
    <!-- Orange bar pointer -->
    <gauge:LinearPointer 
        PointerType="BarPointer" 
        Value="75"
        BarPointerStroke="Orange"
        BarPointerStrokeThickness="10" />
</gauge:LinearScale.Pointers>
```

**C#:**

```csharp
LinearPointer orangeBar = new LinearPointer
{
    PointerType = LinearPointerType.BarPointer,
    Value = 75,
    BarPointerStroke = new SolidColorBrush(Colors.Orange),
    BarPointerStrokeThickness = 10
};
scale.Pointers.Add(orangeBar);
```

### Gradient Bar Pointer

Create a gradient-filled bar:

```xml
<gauge:LinearPointer PointerType="BarPointer" Value="80" BarPointerStrokeThickness="15">
    <gauge:LinearPointer.BarPointerStroke>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#00C9FF" Offset="0" />
            <GradientStop Color="#92FE9D" Offset="1" />
        </LinearGradientBrush>
    </gauge:LinearPointer.BarPointerStroke>
</gauge:LinearPointer>
```

**C#:**

```csharp
LinearPointer gradientBar = new LinearPointer
{
    PointerType = LinearPointerType.BarPointer,
    Value = 80,
    BarPointerStrokeThickness = 15
};

LinearGradientBrush gradient = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 0)
};
gradient.GradientStops.Add(new GradientStop(Color.FromRgb(0, 201, 255), 0));
gradient.GradientStops.Add(new GradientStop(Color.FromRgb(146, 254, 157), 1));

gradientBar.BarPointerStroke = gradient;
scale.Pointers.Add(gradientBar);
```

### Bar Pointer Use Cases

**Progress indicator:**
```xml
<gauge:LinearPointer 
    PointerType="BarPointer"
    Value="{Binding ProgressPercent}"
    BarPointerStroke="#4CAF50"
    BarPointerStrokeThickness="12" />
```

**Download status:**
```xml
<gauge:LinearScale Minimum="0" Maximum="100" LabelPostfix="%">
    <gauge:LinearScale.Pointers>
        <gauge:LinearPointer 
            PointerType="BarPointer"
            Value="67"
            BarPointerStroke="#2196F3"
            BarPointerStrokeThickness="8" />
    </gauge:LinearScale.Pointers>
</gauge:LinearScale>
```

**Temperature fill:**
```xml
<gauge:SfLinearGauge Orientation="Vertical">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale Minimum="-20" Maximum="120">
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="37"
                    BarPointerStroke="#F44336"
                    BarPointerStrokeThickness="15" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Symbol Pointer

Symbol pointers mark a specific value with a shape, ideal for indicating current values, targets, or thresholds.

### Basic Symbol Pointer

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
            
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="SymbolPointer" 
                    Value="60"
                    SymbolPointerHeight="15" 
                    SymbolPointerWidth="15"
                    SymbolPointerPosition="Above" 
                    SymbolPointerStroke="#5B86E5" />
            </gauge:LinearScale.Pointers>
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

LinearPointer symbolPointer = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 60,
    SymbolPointerHeight = 15,
    SymbolPointerWidth = 15,
    SymbolPointerPosition = LinearSymbolPointersPosition.Above,
    SymbolPointerStroke = new SolidColorBrush(Color.FromRgb(91, 134, 229))
};

scale.Pointers.Add(symbolPointer);
gauge.MainScale = scale;
```

### Symbol Pointer Customization

Customize the size and appearance:

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="60"
    SymbolPointerHeight="15" 
    SymbolPointerWidth="20"
    SymbolPointerStroke="DeepSkyBlue" />
```

**C#:**

```csharp
LinearPointer customSymbol = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 60,
    SymbolPointerHeight = 15,
    SymbolPointerWidth = 20,
    SymbolPointerStroke = new SolidColorBrush(Colors.DeepSkyBlue)
};
scale.Pointers.Add(customSymbol);
```

**Key properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **Value** | double | Value position on scale | 0 |
| **SymbolPointerHeight** | double | Height of the pointer symbol | 10 |
| **SymbolPointerWidth** | double | Width of the pointer symbol | 10 |
| **SymbolPointerStroke** | Brush | Color of the pointer | Black |
| **SymbolPointerPosition** | LinearSymbolPointersPosition | Above, Below, or Cross | Below |

## Positioning Symbol Pointers

Symbol pointers can be placed above, below, or crossing the scale bar.

### Position Options

**1. Above** - Pointer appears above the scale bar

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="60"
    SymbolPointerPosition="Above"
    SymbolPointerHeight="10" 
    SymbolPointerWidth="10"
    SymbolPointerStroke="#5B86E5" />
```

**2. Below (Default)** - Pointer appears below the scale bar

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="60"
    SymbolPointerPosition="Below"
    SymbolPointerHeight="10" 
    SymbolPointerWidth="10"
    SymbolPointerStroke="#5B86E5" />
```

**3. Cross** - Pointer crosses/overlaps the scale bar

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="60"
    SymbolPointerPosition="Cross"
    SymbolPointerHeight="10" 
    SymbolPointerWidth="10"
    SymbolPointerStroke="#5B86E5" />
```

**C#:**

```csharp
// Above position
LinearPointer abovePointer = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 60,
    SymbolPointerPosition = LinearSymbolPointersPosition.Above,
    SymbolPointerHeight = 10,
    SymbolPointerWidth = 10,
    SymbolPointerStroke = new SolidColorBrush(Color.FromRgb(91, 134, 229))
};

// Below position
LinearPointer belowPointer = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 40,
    SymbolPointerPosition = LinearSymbolPointersPosition.Below,
    SymbolPointerHeight = 10,
    SymbolPointerWidth = 10,
    SymbolPointerStroke = new SolidColorBrush(Colors.Red)
};

// Cross position
LinearPointer crossPointer = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 80,
    SymbolPointerPosition = LinearSymbolPointersPosition.Cross,
    SymbolPointerHeight = 10,
    SymbolPointerWidth = 10,
    SymbolPointerStroke = new SolidColorBrush(Colors.Green)
};

scale.Pointers.Add(abovePointer);
scale.Pointers.Add(belowPointer);
scale.Pointers.Add(crossPointer);
```

### Position Use Cases

**Above position:**
- Primary value indicators
- Target values
- Maximum thresholds

**Below position:**
- Secondary value indicators
- Minimum thresholds
- Historical values

**Cross position:**
- Emphasis on specific values
- Combined with thick scale bars
- Overlapping visual indicators

## Custom Symbol Pointer Shapes

Customize the pointer shape using templates.

### Using SymbolPointerStyle

Set the **SymbolPointerStyle** property to **Custom** and provide a template:

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="60" 
    SymbolPointerStyle="Custom">
    <gauge:LinearPointer.SymbolPointerTemplate>
        <DataTemplate>
            <Rectangle 
                Width="10" 
                Height="10" 
                Stroke="Red" 
                StrokeThickness="10" />
        </DataTemplate>
    </gauge:LinearPointer.SymbolPointerTemplate>
</gauge:LinearPointer>
```

### Custom Shape Examples

**Triangle pointer:**

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="75" 
    SymbolPointerStyle="Custom">
    <gauge:LinearPointer.SymbolPointerTemplate>
        <DataTemplate>
            <Polygon 
                Points="0,20 10,0 20,20" 
                Fill="#FF5722" />
        </DataTemplate>
    </gauge:LinearPointer.SymbolPointerTemplate>
</gauge:LinearPointer>
```

**Circle pointer:**

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="50" 
    SymbolPointerStyle="Custom">
    <gauge:LinearPointer.SymbolPointerTemplate>
        <DataTemplate>
            <Ellipse 
                Width="16" 
                Height="16" 
                Fill="#4CAF50"
                Stroke="White"
                StrokeThickness="2" />
        </DataTemplate>
    </gauge:LinearPointer.SymbolPointerTemplate>
</gauge:LinearPointer>
```

**Arrow pointer:**

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="85" 
    SymbolPointerStyle="Custom">
    <gauge:LinearPointer.SymbolPointerTemplate>
        <DataTemplate>
            <Path 
                Data="M 0,0 L 10,10 L 0,20 L 5,10 Z" 
                Fill="#2196F3"
                Stroke="#0D47A1"
                StrokeThickness="1" />
        </DataTemplate>
    </gauge:LinearPointer.SymbolPointerTemplate>
</gauge:LinearPointer>
```

**Diamond pointer:**

```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer" 
    Value="65" 
    SymbolPointerStyle="Custom">
    <gauge:LinearPointer.SymbolPointerTemplate>
        <DataTemplate>
            <Polygon 
                Points="10,0 20,10 10,20 0,10" 
                Fill="#9C27B0"
                Stroke="#4A148C"
                StrokeThickness="2" />
        </DataTemplate>
    </gauge:LinearPointer.SymbolPointerTemplate>
</gauge:LinearPointer>
```

## Adding Multiple Pointers

Add multiple pointers to a single scale for comparison or layered visualization.

### Multiple Symbol Pointers

```xml
<gauge:LinearScale.Pointers>
    <!-- Current value (blue) -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="60"
        SymbolPointerHeight="15" 
        SymbolPointerWidth="15"
        SymbolPointerPosition="Above" 
        SymbolPointerStroke="#5B86E5" />
    
    <!-- Target value (green) -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="90"
        SymbolPointerHeight="15" 
        SymbolPointerWidth="15"
        SymbolPointerPosition="Above" 
        SymbolPointerStroke="#4CAF50" />
</gauge:LinearScale.Pointers>
```

### Combining Bar and Symbol Pointers

```xml
<gauge:LinearScale 
    ScaleBarStroke="#E0E0E0" 
    MajorTickStroke="Gray"
    MinorTickStroke="Gray" 
    LabelStroke="#424242"
    ScaleBarSize="20" 
    MinorTicksPerInterval="3">
    
    <gauge:LinearScale.Pointers>
        <!-- Bar pointer showing progress -->
        <gauge:LinearPointer 
            PointerType="BarPointer" 
            Value="75"
            BarPointerStroke="#36D1DC"
            BarPointerStrokeThickness="10" />
        
        <!-- Symbol pointer marking current value -->
        <gauge:LinearPointer 
            PointerType="SymbolPointer" 
            Value="75"
            SymbolPointerHeight="15" 
            SymbolPointerWidth="15"
            SymbolPointerPosition="Above" 
            SymbolPointerStroke="#5B86E5" />
    </gauge:LinearScale.Pointers>
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
    ScaleBarSize = 20,
    MinorTicksPerInterval = 3
};

// Add bar pointer
LinearPointer barPointer = new LinearPointer
{
    PointerType = LinearPointerType.BarPointer,
    Value = 75,
    BarPointerStroke = new SolidColorBrush(Color.FromRgb(54, 209, 220)),
    BarPointerStrokeThickness = 10
};
scale.Pointers.Add(barPointer);

// Add symbol pointer
LinearPointer symbolPointer = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 75,
    SymbolPointerHeight = 15,
    SymbolPointerWidth = 15,
    SymbolPointerPosition = LinearSymbolPointersPosition.Above,
    SymbolPointerStroke = new SolidColorBrush(Color.FromRgb(91, 134, 229))
};
scale.Pointers.Add(symbolPointer);

gauge.MainScale = scale;
```

### Layered Bar Pointers

Create layered progress indicators:

```xml
<gauge:LinearScale.Pointers>
    <!-- Background/total bar (light) -->
    <gauge:LinearPointer 
        PointerType="BarPointer" 
        Value="100"
        BarPointerStroke="#E0E0E0"
        BarPointerStrokeThickness="15" />
    
    <!-- Completed bar (colored) -->
    <gauge:LinearPointer 
        PointerType="BarPointer" 
        Value="65"
        BarPointerStroke="#4CAF50"
        BarPointerStrokeThickness="15" />
</gauge:LinearScale.Pointers>
```

### Multiple Value Comparison

Compare actual vs target vs previous values:

```xml
<gauge:LinearScale.Pointers>
    <!-- Previous value (gray) -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="50"
        SymbolPointerPosition="Below"
        SymbolPointerStroke="#9E9E9E"
        SymbolPointerHeight="12" 
        SymbolPointerWidth="12" />
    
    <!-- Current value (blue) -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="72"
        SymbolPointerPosition="Cross"
        SymbolPointerStroke="#2196F3"
        SymbolPointerHeight="16" 
        SymbolPointerWidth="16" />
    
    <!-- Target value (green) -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="90"
        SymbolPointerPosition="Above"
        SymbolPointerStroke="#4CAF50"
        SymbolPointerHeight="12" 
        SymbolPointerWidth="12" />
</gauge:LinearScale.Pointers>
```

## Pointer Animation

Enable smooth transitions when pointer values change.

### Basic Animation

```xml
<gauge:LinearPointer 
    PointerType="BarPointer" 
    Value="75"
    BarPointerStroke="#36D1DC"
    BarPointerStrokeThickness="10"
    EnableAnimation="True"
    AnimationDuration="1000" />
```

**C#:**

```csharp
LinearPointer animatedPointer = new LinearPointer
{
    PointerType = LinearPointerType.BarPointer,
    Value = 75,
    BarPointerStroke = new SolidColorBrush(Color.FromRgb(54, 209, 220)),
    BarPointerStrokeThickness = 10,
    EnableAnimation = true,
    AnimationDuration = 1000 // milliseconds
};
scale.Pointers.Add(animatedPointer);
```

### Animation Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **EnableAnimation** | bool | Enable/disable pointer animation | false |
| **AnimationDuration** | int | Animation duration in milliseconds | 500 |

### Different Animation Speeds

```xml
<gauge:LinearScale.Pointers>
    <!-- Fast animation (symbol) -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer" 
        Value="60"
        SymbolPointerHeight="15" 
        SymbolPointerWidth="15"
        SymbolPointerPosition="Above" 
        SymbolPointerStroke="#5B86E5"
        EnableAnimation="True" 
        AnimationDuration="500" />
    
    <!-- Slow animation (bar) -->
    <gauge:LinearPointer 
        PointerType="BarPointer" 
        Value="75"
        BarPointerStroke="#36D1DC"
        BarPointerStrokeThickness="10"
        EnableAnimation="True" 
        AnimationDuration="1500" />
</gauge:LinearScale.Pointers>
```

### Animation Use Cases

**Progress updates:**
```xml
<gauge:LinearPointer 
    PointerType="BarPointer"
    Value="{Binding CurrentProgress}"
    EnableAnimation="True"
    AnimationDuration="800"
    BarPointerStroke="#4CAF50"
    BarPointerStrokeThickness="12" />
```

**Real-time data:**
```xml
<gauge:LinearPointer 
    PointerType="SymbolPointer"
    Value="{Binding LiveValue}"
    EnableAnimation="True"
    AnimationDuration="300"
    SymbolPointerStroke="#F44336"
    SymbolPointerHeight="14"
    SymbolPointerWidth="14" />
```

## Data Binding

Bind pointer values to view model properties.

### Basic Binding

**XAML:**

```xml
<gauge:LinearPointer 
    PointerType="BarPointer"
    Value="{Binding ProgressValue}"
    BarPointerStroke="#2196F3"
    BarPointerStrokeThickness="12"
    EnableAnimation="True" />
```

**View Model:**

```csharp
public class GaugeViewModel : INotifyPropertyChanged
{
    private double _progressValue;
    
    public double ProgressValue
    {
        get => _progressValue;
        set
        {
            _progressValue = value;
            OnPropertyChanged(nameof(ProgressValue));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Multiple Bound Pointers

```xml
<gauge:LinearScale.Pointers>
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="{Binding CurrentValue}"
        SymbolPointerStroke="#2196F3"
        EnableAnimation="True" />
    
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="{Binding TargetValue}"
        SymbolPointerStroke="#4CAF50"
        SymbolPointerPosition="Above"
        EnableAnimation="True" />
</gauge:LinearScale.Pointers>
```

## Complete Examples

### Progress Tracker with Animation

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            Interval="10"
            LabelPostfix="%"
            ScaleBarStroke="#ECEFF1"
            ScaleBarSize="15"
            LabelStroke="#37474F"
            MajorTickStroke="#78909C"
            MinorTickStroke="#B0BEC5"
            MinorTicksPerInterval="4">
            
            <gauge:LinearScale.Pointers>
                <!-- Animated progress bar -->
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="{Binding ProgressPercent}"
                    BarPointerStroke="#4CAF50"
                    BarPointerStrokeThickness="15"
                    EnableAnimation="True"
                    AnimationDuration="600" />
                
                <!-- Current value marker -->
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="{Binding ProgressPercent}"
                    SymbolPointerHeight="18"
                    SymbolPointerWidth="18"
                    SymbolPointerPosition="Above"
                    SymbolPointerStroke="#1B5E20"
                    EnableAnimation="True"
                    AnimationDuration="600" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Multi-Metric Comparison

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            ScaleBarStroke="#E0E0E0"
            ScaleBarSize="12"
            LabelStroke="#212121">
            
            <gauge:LinearScale.Pointers>
                <!-- Metric 1: Below scale -->
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="45"
                    SymbolPointerPosition="Below"
                    SymbolPointerStroke="#FF9800"
                    SymbolPointerHeight="14"
                    SymbolPointerWidth="14" />
                
                <!-- Metric 2: On scale -->
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="72"
                    SymbolPointerPosition="Cross"
                    SymbolPointerStroke="#2196F3"
                    SymbolPointerHeight="16"
                    SymbolPointerWidth="16" />
                
                <!-- Metric 3: Above scale -->
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="88"
                    SymbolPointerPosition="Above"
                    SymbolPointerStroke="#4CAF50"
                    SymbolPointerHeight="14"
                    SymbolPointerWidth="14" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Layered Progress Indicator

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="0" 
            Maximum="100"
            ScaleBarStroke="#FAFAFA"
            ScaleBarSize="20">
            
            <gauge:LinearScale.Pointers>
                <!-- Total capacity (gray background) -->
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="100"
                    BarPointerStroke="#E0E0E0"
                    BarPointerStrokeThickness="20" />
                
                <!-- Used capacity (blue bar) -->
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="68"
                    BarPointerStroke="#2196F3"
                    BarPointerStrokeThickness="20"
                    EnableAnimation="True"
                    AnimationDuration="800" />
                
                <!-- Marker at current value -->
                <gauge:LinearPointer 
                    PointerType="SymbolPointer"
                    Value="68"
                    SymbolPointerPosition="Above"
                    SymbolPointerStroke="#0D47A1"
                    SymbolPointerHeight="16"
                    SymbolPointerWidth="16"
                    EnableAnimation="True"
                    AnimationDuration="800" />
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

## Best Practices

### Pointer Selection

1. **Use bar pointers for:**
   - Progress indication
   - Fill/capacity visualization
   - Cumulative values

2. **Use symbol pointers for:**
   - Specific value markers
   - Target indicators
   - Threshold markers
   - Multi-value comparison

3. **Combine both when:**
   - Showing progress with current value marker
   - Displaying completion with target
   - Creating layered visualizations

### Visual Design

1. **Contrast:** Ensure pointers contrast with scale bar and background
2. **Size:** Make pointers large enough to be easily visible
3. **Color:** Use meaningful colors (green for good, red for critical, etc.)
4. **Consistency:** Use same pointer styles across similar gauges

### Animation

1. **Enable for dynamic data** - When values change frequently
2. **Disable for static displays** - When values rarely change
3. **Match duration to update frequency** - Faster animations for rapid updates
4. **Keep it smooth** - 300-800ms is usually optimal

### Performance

1. **Limit pointer count** - 3-5 pointers per gauge maximum
2. **Avoid complex templates** - Keep custom shapes simple
3. **Use appropriate animation duration** - Don't make it too long

## Troubleshooting

### Pointer Not Visible

**Issue:** Pointer doesn't appear on the gauge.

**Solutions:**
1. Verify the `Value` is within the scale's `Minimum` and `Maximum` range
2. Check that pointer color contrasts with the background
3. Ensure pointer size properties are set (height/width or thickness)

### Symbol Pointer Too Small

**Issue:** Symbol pointer is barely visible.

**Solution:** Increase `SymbolPointerHeight` and `SymbolPointerWidth`:

```xml
<gauge:LinearPointer 
    SymbolPointerHeight="20"
    SymbolPointerWidth="20" />
```

### Bar Pointer Not Filling

**Issue:** Bar pointer doesn't extend to the expected value.

**Solutions:**
1. Verify the `Value` property is set correctly
2. Check scale `Minimum` and `Maximum` values
3. Ensure `BarPointerStrokeThickness` is set to a visible value

### Animation Not Working

**Issue:** Pointer doesn't animate when value changes.

**Solutions:**
1. Set `EnableAnimation="True"`
2. Ensure the pointer value is data-bound or programmatically changed
3. Verify `AnimationDuration` is set to a reasonable value (>0)

---

**Next:** Learn about [Ranges](ranges.md) to add color-coded zones to your gauge with pointers.
