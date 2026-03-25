# Pointers

## Table of Contents
- [Pointers Overview](#pointers-overview)
- [Needle Pointer](#needle-pointer)
- [Range Pointer](#range-pointer)
- [Symbol Pointer](#symbol-pointer)
- [Multiple Pointers](#multiple-pointers)
- [Pointer Value](#pointer-value)
- [Common Patterns](#common-patterns)

## Pointers Overview

**Pointers** are visual indicators that display the current value on a circular scale. They are the primary elements users look at to read gauge values.

### Three Pointer Types

1. **Needle Pointer** - Traditional gauge needle (like speedometer needle)
2. **Range Pointer** - Colored arc from scale start to current value
3. **Symbol Pointer** - Icon/marker placed at the current value

**Basic Structure:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="NeedlePointer" Value="60"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Key Property:** `PointerType` - Determines which type of pointer to display

## Needle Pointer

The needle pointer displays a traditional gauge needle that points to the current value.

### Basic Needle Pointer

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="NeedlePointer" 
                               Value="60"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularPointer pointer = new CircularPointer
{
    PointerType = PointerType.NeedlePointer,
    Value = 60
};
scale.Pointers.Add(pointer);
```

### Needle Types

The `NeedlePointerType` property offers four shapes:

**1. Rectangle (Default)**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" 
                       NeedlePointerType="Rectangle"
                       Value="60"/>
```

**2. Triangle**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" 
                       NeedlePointerType="Triangle"
                       Value="60"/>
```

**3. Tapered**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" 
                       NeedlePointerType="Tapered"
                       Value="60"/>
```

**4. Arrow**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" 
                       NeedlePointerType="Arrow"
                       Value="60"/>
```

**C#:**
```csharp
pointer.NeedlePointerType = NeedlePointerType.Triangle;
```

### Needle Customization

#### Length

Control needle length with `NeedleLengthFactor` (0 to 1):

**XAML:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" 
                       NeedleLengthFactor="0.5"
                       Value="60"/>
```

**Values:**
- `1.0` - Full scale radius
- `0.7` - 70% of scale radius (typical)
- `0.5` - Half scale radius

#### Stroke and Thickness

**XAML:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" 
                       NeedlePointerType="Triangle"
                       NeedlePointerStroke="DeepSkyBlue"
                       NeedlePointerStrokeThickness="10"
                       Value="60"/>
```

**C#:**
```csharp
pointer.NeedlePointerStroke = new SolidColorBrush(Colors.DeepSkyBlue);
pointer.NeedlePointerStrokeThickness = 10;
```

#### Knob Customization

The knob is the circular cap at the needle's center point.

**Size:**
```xml
<gauge:CircularPointer PointerCapDiameter="20"/>
```

**Styling:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       PointerCapDiameter="20"
                       KnobFill="DeepSkyBlue"
                       KnobStroke="DeepSkyBlue"
                       KnobStrokeThickness="2"
                       Value="60"/>
```

**C#:**
```csharp
pointer.PointerCapDiameter = 20;
pointer.KnobFill = new SolidColorBrush(Colors.DeepSkyBlue);
pointer.KnobStroke = new SolidColorBrush(Colors.DeepSkyBlue);
pointer.KnobStrokeThickness = 2;
```

**Advanced Knob Customization:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       KnobRadiusFactor="0.2"
                       KnobStrokeThickness="5"
                       KnobStroke="DeepSkyBlue"
                       KnobFill="White"
                       Value="60"/>
```

**KnobRadiusFactor:** Size as fraction of scale radius (0-1)

### Needle Tail

Add a tail extending behind the needle's pivot point.

**XAML:**
```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       Value="30"
                       NeedlePointerType="Tapered"
                       TailLengthFactor="0.3"
                       TailStroke="#ed7d31"
                       TailFill="#ed7d31"
                       TailStrokeThickness="3"
                       NeedlePointerStroke="#ed7d31"
                       NeedlePointerStrokeThickness="3"
                       KnobFill="White"/>
```

**C#:**
```csharp
pointer.TailLengthFactor = 0.3;
pointer.TailStroke = new SolidColorBrush(Color.FromRgb(237, 125, 49));
pointer.TailFill = new SolidColorBrush(Color.FromRgb(237, 125, 49));
pointer.TailStrokeThickness = 3;
```

**Properties:**
- `TailLengthFactor` - Tail length as fraction (0-1)
- `TailStroke` - Tail outline color
- `TailFill` - Tail fill color
- `TailStrokeThickness` - Tail thickness

### Visibility Control

Hide the needle pointer while keeping other pointers visible:

**XAML:**
```xml
<gauge:CircularPointer NeedlePointerVisibility="Hidden"/>
```

**C#:**
```csharp
pointer.NeedlePointerVisibility = Visibility.Hidden;
```

**Use Case:** When using only range or symbol pointers.

### Complete Needle Example

```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       Value="85"
                       NeedlePointerType="Triangle"
                       NeedleLengthFactor="0.6"
                       NeedlePointerStroke="#333"
                       NeedlePointerStrokeThickness="7"
                       PointerCapDiameter="15"
                       KnobFill="#333"
                       KnobStroke="#333"
                       TailLengthFactor="0.2"
                       TailStroke="#333"
                       TailFill="#333"/>
```

## Range Pointer

Range pointers display a colored arc from the scale's start value to the current value, ideal for progress visualization.

### Basic Range Pointer

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="50"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularPointer pointer = new CircularPointer
{
    PointerType = PointerType.RangePointer,
    Value = 50
};
```

### Range Pointer Customization

**Stroke and Thickness:**
```xml
<gauge:CircularPointer PointerType="RangePointer" 
                       Value="50"
                       RangePointerStroke="DarkCyan"
                       RangePointerStrokeThickness="20"/>
```

**C#:**
```csharp
pointer.RangePointerStroke = new SolidColorBrush(Colors.DarkCyan);
pointer.RangePointerStrokeThickness = 20;
```

### Range Pointer Positioning

Control where the range pointer appears relative to the scale.

#### Built-in Positions

**RangePointerPosition Options:**
- `Inside` (Default) - Inside the scale rim
- `Outside` - Outside the scale rim
- `Cross` - Overlapping the scale rim
- `Custom` - Custom position with Offset

**XAML:**
```xml
<gauge:CircularScale RangePointerPosition="Outside">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Value="60"
                               RangePointerStroke="HotPink"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
scale.RangePointerPosition = RangePointerPosition.Outside;
```

#### Custom Position with Offset

Use `Custom` position with `Offset` property for precise placement:

**XAML:**
```xml
<gauge:CircularScale RangePointerPosition="Custom">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               Offset="0.5"
                               RangePointerStrokeThickness="20"
                               RangePointerStroke="LightGray"
                               Value="60"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Offset Values:**
- `0` - Center of gauge
- `0.5` - Midpoint between center and rim
- `1.0` - At the rim

**C#:**
```csharp
scale.RangePointerPosition = RangePointerPosition.Custom;
pointer.Offset = 0.5;
pointer.RangePointerStrokeThickness = 20;
```

#### Advanced: Start and End Offsets

For responsive width and position:

**XAML:**
```xml
<gauge:CircularScale RangePointerPosition="Custom">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               RangeStartOffset="0.5"
                               RangeEndOffset="0.7"
                               RangePointerStroke="LightGray"
                               Value="90"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Properties:**
- `RangeStartOffset` - Inner edge position (0-1)
- `RangeEndOffset` - Outer edge position (0-1)
- Width = EndOffset - StartOffset

**C#:**
```csharp
pointer.RangeStartOffset = 0.5;
pointer.RangeEndOffset = 0.7;
```

### Range Start Customization

Change where the range pointer begins (default is scale StartValue):

**XAML:**
```xml
<gauge:CircularPointer PointerType="RangePointer" 
                       RangeStart="20"
                       Value="90"
                       RangeStartOffset="0.5"
                       RangeEndOffset="0.7"
                       RangePointerStroke="LightGray"/>
```

**Use Case:** Show range between two values (e.g., target range).

### Range Cap

Add rounded caps to range pointer ends:

**XAML:**
```xml
<gauge:CircularScale SweepAngle="360" 
                     RimStrokeThickness="30">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer" 
                               RangePointerStrokeThickness="30"
                               RangeCap="Both"
                               RangePointerStroke="LightSkyBlue"
                               Value="75"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**RangeCap Options:**
- `None` (Default) - No caps
- `Start` - Rounded cap at start
- `End` - Rounded cap at end
- `Both` - Rounded caps at both ends

**C#:**
```csharp
pointer.RangeCap = RangeCap.Both;
```

**Best Practice:** Use `Both` with full-circle (360°) progress indicators.

### Visibility Control

**XAML:**
```xml
<gauge:CircularPointer RangePointerVisibility="Hidden"/>
```

**C#:**
```csharp
pointer.RangePointerVisibility = Visibility.Hidden;
```

### Complete Range Pointer Example

```xml
<gauge:CircularScale StartValue="0" EndValue="100"
                     SweepAngle="360"
                     RangePointerPosition="Custom"
                     RimStroke="LightGray"
                     RimStrokeThickness="30">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="RangePointer"
                               Value="75"
                               RangeStartOffset="0.7"
                               RangeEndOffset="1.0"
                               RangeCap="Both"
                               RangePointerStroke="DeepSkyBlue"
                               RangePointerStrokeThickness="30"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

## Symbol Pointer

Symbol pointers use icons/markers to indicate the current value position.

### Basic Symbol Pointer

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="SymbolPointer" 
                               Value="60"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularPointer pointer = new CircularPointer
{
    PointerType = PointerType.SymbolPointer,
    Value = 60
};
```

### Symbol Types

The `Symbol` property offers 12 built-in shapes:

**Available Symbols:**
1. `Rectangle`
2. `Ellipse` (Default)
3. `Triangle`
4. `InvertedTriangle`
5. `Diamond`
6. `Pentagon`
7. `Hexagon`
8. `Arrow`
9. `InvertedArrow`
10. `Cross`
11. `RoundedRectangle`
12. `Custom`

**XAML:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer" 
                       Symbol="Pentagon"
                       Value="60"/>
```

**C#:**
```csharp
pointer.Symbol = Symbol.Pentagon;
```

### Symbol Customization

**Size:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer" 
                       SymbolPointerHeight="20"
                       SymbolPointerWidth="20"
                       Value="60"/>
```

**Stroke:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer" 
                       Symbol="InvertedTriangle"
                       SymbolPointerStroke="Red"
                       SymbolPointerHeight="20"
                       SymbolPointerWidth="20"
                       Value="60"/>
```

**C#:**
```csharp
pointer.SymbolPointerStroke = new SolidColorBrush(Colors.Red);
pointer.SymbolPointerHeight = 20;
pointer.SymbolPointerWidth = 20;
```

### Symbol Offset

Position the symbol anywhere between center and rim:

**XAML:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer" 
                       Offset="0.5"
                       Symbol="Triangle"
                       Value="60"/>
```

**Offset Values:**
- `0` - At center
- `0.5` - Halfway to rim
- `1.0` - At rim (default)

### Custom Symbol Template

Create completely custom symbol shapes:

**XAML:**
```xml
<gauge:CircularPointer PointerType="SymbolPointer"
                       Symbol="Custom"
                       Value="60">
    <gauge:CircularPointer.SymbolPointerTemplate>
        <DataTemplate>
            <Grid>
                <Grid Width="24" Height="24">
                    <Ellipse Fill="White"/>
                </Grid>
                <Path Stretch="Uniform" 
                      Fill="Black" 
                      Width="24" Height="24"
                      Data="M23.93,10.62L20.76,11.22 18.09,13.03 16.28,15.7 15.68,18.87 16.28,22.04 18.09,24.73 20.76,26.55 23.93,27.16 27.10,26.55 29.78,24.73 31.60,22.04 32.20,18.87 31.60,15.7 29.78,13.03 27.10,11.22 23.93,10.62z M25.27,7.36L26.70,9.87 29.37,8.65 29.77,11.48 32.74,11.27 32.02,14.18 34.92,14.98 33.19,17.41 35.58,19.15 33.14,20.79 34.64,23.37 31.80,23.98 32.32,26.98 29.44,26.55 28.92,29.48 26.25,27.96 24.75,30.47 22.80,28.22 20.51,30.02 19.57,27.16 16.64,28.03 16.88,25.10 13.88,24.77 15.14,22.15 12.42,20.74 14.55,18.68 12.49,16.52 15.26,15.30 14.20,12.54 17.13,12.40 17.13,9.42 19.92,10.41 21.05,7.62 23.30,9.49 25.27,7.36z"/>
            </Grid>
        </DataTemplate>
    </gauge:CircularPointer.SymbolPointerTemplate>
</gauge:CircularPointer>
```

**Steps:**
1. Set `Symbol="Custom"`
2. Define `SymbolPointerTemplate`
3. Create custom Path/Shape in DataTemplate

### Visibility Control

**XAML:**
```xml
<gauge:CircularPointer SymbolPointerVisibility="Hidden"/>
```

**C#:**
```csharp
pointer.SymbolPointerVisibility = Visibility.Hidden;
```

### Complete Symbol Pointer Example

```xml
<gauge:CircularPointer PointerType="SymbolPointer"
                       Symbol="InvertedTriangle"
                       Value="80"
                       Offset="0.9"
                       SymbolPointerHeight="18"
                       SymbolPointerWidth="18"
                       SymbolPointerStroke="DarkBlue"/>
```

## Multiple Pointers

A single scale can have multiple pointers to display multiple values or combine pointer types.

### Basic Multiple Pointers

**XAML:**
```xml
<gauge:CircularScale>
    <gauge:CircularScale.Pointers>
        
        <gauge:CircularPointer PointerType="NeedlePointer"
                               Value="60"
                               NeedleLengthFactor="0.4"/>
        
        <gauge:CircularPointer PointerType="RangePointer"
                               Value="100"/>
        
        <gauge:CircularPointer PointerType="SymbolPointer"
                               Value="50"
                               Symbol="Pentagon"
                               SymbolPointerHeight="20"
                               SymbolPointerWidth="20"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
// Needle
CircularPointer needle = new CircularPointer
{
    PointerType = PointerType.NeedlePointer,
    Value = 60,
    NeedleLengthFactor = 0.4
};

// Range
CircularPointer range = new CircularPointer
{
    PointerType = PointerType.RangePointer,
    Value = 100
};

// Symbol
CircularPointer symbol = new CircularPointer
{
    PointerType = PointerType.SymbolPointer,
    Value = 50,
    Symbol = Symbol.Pentagon,
    SymbolPointerHeight = 20,
    SymbolPointerWidth = 20
};

scale.Pointers.Add(needle);
scale.Pointers.Add(range);
scale.Pointers.Add(symbol);
```

### Use Cases for Multiple Pointers

**Current and Target:**
```xml
<gauge:CircularScale.Pointers>
    <!-- Current value -->
    <gauge:CircularPointer PointerType="NeedlePointer" Value="72"/>
    
    <!-- Target value -->
    <gauge:CircularPointer PointerType="SymbolPointer" 
                           Value="85" 
                           Symbol="InvertedTriangle"/>
</gauge:CircularScale.Pointers>
```

**Progress with Needle:**
```xml
<gauge:CircularScale.Pointers>
    <!-- Progress background -->
    <gauge:CircularPointer PointerType="RangePointer" 
                           Value="65"
                           RangePointerStroke="LightBlue"
                           RangePointerStrokeThickness="15"/>
    
    <!-- Precise value needle -->
    <gauge:CircularPointer PointerType="NeedlePointer" Value="65"/>
</gauge:CircularScale.Pointers>
```

## Pointer Value

The `Value` property determines where the pointer points on the scale.

### Setting Value

**XAML:**
```xml
<gauge:CircularPointer Value="75"/>
```

**C#:**
```csharp
pointer.Value = 75;
```

### Value Range

The value must be within the scale's `StartValue` and `EndValue`:

```xml
<gauge:CircularScale StartValue="0" EndValue="100">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="50"/> <!-- Valid -->
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**Out of range behavior:** Pointer stays at min/max boundary.

### Dynamic Value Updates

Update pointer value programmatically:

**C#:**
```csharp
// Update value
myPointer.Value = newValue;

// With bounds checking
myPointer.Value = Math.Clamp(sensorValue, 0, 100);
```

### Data Binding

Bind pointer value to ViewModel:

**XAML:**
```xml
<gauge:CircularPointer Value="{Binding CurrentTemperature}"/>
```

**ViewModel:**
```csharp
public class GaugeViewModel : INotifyPropertyChanged
{
    private double _currentTemperature;
    public double CurrentTemperature
    {
        get => _currentTemperature;
        set
        {
            _currentTemperature = value;
            OnPropertyChanged();
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

## Common Patterns

### Pattern 1: Speedometer with Needle

```xml
<gauge:CircularPointer PointerType="NeedlePointer"
                       Value="85"
                       NeedlePointerType="Triangle"
                       NeedleLengthFactor="0.65"
                       NeedlePointerStroke="#333"
                       NeedlePointerStrokeThickness="6"
                       PointerCapDiameter="16"
                       KnobFill="#333"
                       KnobStroke="#333"/>
```

### Pattern 2: Progress Circle

```xml
<gauge:CircularPointer PointerType="RangePointer"
                       Value="75"
                       RangePointerStroke="DeepSkyBlue"
                       RangePointerStrokeThickness="25"
                       RangeCap="Both"/>
```

### Pattern 3: Target Marker

```xml
<gauge:CircularPointer PointerType="SymbolPointer"
                       Symbol="InvertedTriangle"
                       Value="targetValue"
                       SymbolPointerStroke="Red"
                       SymbolPointerHeight="16"
                       SymbolPointerWidth="16"
                       Offset="0.85"/>
```

### Pattern 4: Multi-Value Display

```xml
<gauge:CircularScale.Pointers>
    <!-- Min value -->
    <gauge:CircularPointer PointerType="SymbolPointer" 
                           Symbol="Triangle" Value="20"/>
    
    <!-- Current value -->
    <gauge:CircularPointer PointerType="NeedlePointer" 
                           Value="65"/>
    
    <!-- Max value -->
    <gauge:CircularPointer PointerType="SymbolPointer" 
                           Symbol="InvertedTriangle" Value="90"/>
</gauge:CircularScale.Pointers>
```

## Next Steps

- **Enable Animation:** Smooth pointer movements
  → Read: [advanced-features.md](advanced-features.md#pointer-animation)

- **Interactive Dragging:** Let users change values by dragging pointers
  → Read: [advanced-features.md](advanced-features.md#pointer-dragging)

- **Add Ranges:** Visual zones behind pointers
  → Read: [ranges.md](ranges.md)
