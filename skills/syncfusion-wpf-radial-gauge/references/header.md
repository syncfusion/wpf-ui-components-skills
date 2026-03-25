# Header

## Table of Contents
- [Header Overview](#header-overview)
- [Adding Text Headers](#adding-text-headers)
- [Header Alignment](#header-alignment)
- [Custom Header Position](#custom-header-position)
- [Custom Header Views](#custom-header-views)
- [Header Styling](#header-styling)
- [Common Patterns](#common-patterns)

## Header Overview

The **GaugeHeader** provides a title or label for the circular gauge. It can contain text, custom views, or complex UI elements.

**Key Features:**
- Simple text headers
- Custom view headers
- Flexible positioning
- Styling support

## Adding Text Headers

### Basic Text Header

Use a TextBlock as the header:

**XAML:**
```xml
<gauge:SfCircularGauge>
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="Speedometer"/>
    </gauge:SfCircularGauge.GaugeHeader>
    
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale>
            <!-- Scale content -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**C#:**
```csharp
TextBlock header = new TextBlock
{
    Text = "Speedometer"
};
gauge.GaugeHeader = header;
```

### Styled Text Header

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <TextBlock Text="Temperature (°C)"
               FontSize="16"
               FontWeight="SemiBold"
               Foreground="#333"
               HorizontalAlignment="Center"/>
</gauge:SfCircularGauge.GaugeHeader>
```

**C#:**
```csharp
TextBlock header = new TextBlock
{
    Text = "Temperature (°C)",
    FontSize = 16,
    FontWeight = FontWeights.SemiBold,
    Foreground = new SolidColorBrush(Color.FromRgb(51, 51, 51)),
    HorizontalAlignment = HorizontalAlignment.Center
};
gauge.GaugeHeader = header;
```

## Header Alignment

The `HeaderAlignment` property controls the header's position.

### Alignment Options

**Top (Default):**
```xml
<gauge:SfCircularGauge HeaderAlignment="Top">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="Speed"/>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

**Bottom:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Bottom">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="Speed"/>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

**Left:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Left">
```

**Right:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Right">
```

**Custom:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom">
    <!-- Use GaugeHeaderPosition for precise placement -->
</gauge:SfCircularGauge>
```

**C#:**
```csharp
gauge.HeaderAlignment = HeaderAlignment.Bottom;
```

## Custom Header Position

Use `GaugeHeaderPosition` with `HeaderAlignment="Custom"` for precise control.

### Position with Point

**XAML:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.63,0.75">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="Speedometer"
                   FontSize="14"
                   FontWeight="SemiBold"/>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

**C#:**
```csharp
gauge.HeaderAlignment = HeaderAlignment.Custom;
gauge.GaugeHeaderPosition = new Point(0.63, 0.75);
```

**Point Values (0-1 range):**
- `(0.5, 0.5)` - Center of gauge
- `(0.5, 0.2)` - Top center
- `(0.5, 0.8)` - Bottom center
- `(0.63, 0.75)` - Typical speedometer position (below center)

### Common Positions

**Center (for full-circle gauges):**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.5">
```

**Bottom Center (for semi-circle gauges):**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.75">
```

**Top Center:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.25">
```

## Custom Header Views

Headers can contain any WPF element, not just text.

### StackPanel with Multiple Elements

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <StackPanel Orientation="Vertical" 
                HorizontalAlignment="Center">
        <TextBlock Text="Current Speed" 
                   FontSize="12" 
                   Foreground="Gray"/>
        <TextBlock Text="85 mph" 
                   FontSize="20" 
                   FontWeight="Bold"
                   Foreground="Black"/>
    </StackPanel>
</gauge:SfCircularGauge.GaugeHeader>
```

### Grid with Icon and Text

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <Path Grid.Column="0" 
              Data="M12,2L4.5,20.29L5.21,21L12,18L18.79,21L19.5,20.29L12,2Z"
              Fill="Orange" 
              Width="16" Height="16"
              Margin="0,0,5,0"/>
        
        <TextBlock Grid.Column="1" 
                   Text="Temperature"
                   FontSize="14"
                   VerticalAlignment="Center"/>
    </Grid>
</gauge:SfCircularGauge.GaugeHeader>
```

### Image Header

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <Image Source="/Images/logo.png" 
           Width="50" 
           Height="50"/>
</gauge:SfCircularGauge.GaugeHeader>
```

### Data-Bound Header

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <TextBlock Text="{Binding GaugeTitle}" 
               FontSize="16"
               FontWeight="Bold"/>
</gauge:SfCircularGauge.GaugeHeader>
```

**ViewModel:**
```csharp
public string GaugeTitle { get; set; } = "CPU Usage";
```

## Header Styling

### Font Properties

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <TextBlock Text="Performance"
               FontFamily="Segoe UI"
               FontSize="18"
               FontWeight="Bold"
               FontStyle="Normal"
               Foreground="DarkBlue"
               TextAlignment="Center"/>
</gauge:SfCircularGauge.GaugeHeader>
```

### With Effects

**Drop Shadow:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <TextBlock Text="Temperature">
        <TextBlock.Effect>
            <DropShadowEffect Color="Gray" 
                              BlurRadius="3" 
                              ShadowDepth="2"/>
        </TextBlock.Effect>
    </TextBlock>
</gauge:SfCircularGauge.GaugeHeader>
```

### Styled Container

**XAML:**
```xml
<gauge:SfCircularGauge.GaugeHeader>
    <Border Background="LightGray" 
            BorderBrush="Gray"
            BorderThickness="1"
            CornerRadius="5"
            Padding="10,5">
        <TextBlock Text="System Monitor" 
                   FontWeight="SemiBold"/>
    </Border>
</gauge:SfCircularGauge.GaugeHeader>
```

## Common Patterns

### Pattern 1: Simple Title

```xml
<gauge:SfCircularGauge HeaderAlignment="Top">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="Speedometer" 
                   FontSize="16"
                   FontWeight="SemiBold"
                   HorizontalAlignment="Center"/>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

### Pattern 2: Centered Title (Progress Gauge)

```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.5">
    <gauge:SfCircularGauge.GaugeHeader>
        <StackPanel HorizontalAlignment="Center" 
                    VerticalAlignment="Center">
            <TextBlock Text="75%" 
                       FontSize="36" 
                       FontWeight="Bold"
                       Foreground="DeepSkyBlue"/>
            <TextBlock Text="Complete" 
                       FontSize="14" 
                       Foreground="Gray"/>
        </StackPanel>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

### Pattern 3: Bottom Label (Speedometer)

```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.8">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="km/h" 
                   FontSize="14" 
                   Foreground="#666"
                   HorizontalAlignment="Center"/>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

### Pattern 4: Multi-Line Header

```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.7">
    <gauge:SfCircularGauge.GaugeHeader>
        <StackPanel>
            <TextBlock Text="Current Temperature" 
                       FontSize="12" 
                       Foreground="Gray"
                       HorizontalAlignment="Center"/>
            <TextBlock Text="72°F" 
                       FontSize="20" 
                       FontWeight="Bold"
                       HorizontalAlignment="Center"/>
        </StackPanel>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

### Pattern 5: Dynamic Header (Data Bound)

**XAML:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.5,0.5">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock FontSize="24" 
                   FontWeight="Bold">
            <TextBlock.Text>
                <MultiBinding StringFormat="{}{0:N0} {1}">
                    <Binding Path="CurrentValue"/>
                    <Binding Path="Unit"/>
                </MultiBinding>
            </TextBlock.Text>
        </TextBlock>
    </gauge:SfCircularGauge.GaugeHeader>
</gauge:SfCircularGauge>
```

**ViewModel:**
```csharp
public class GaugeViewModel : INotifyPropertyChanged
{
    private double _currentValue = 85;
    public double CurrentValue
    {
        get => _currentValue;
        set { _currentValue = value; OnPropertyChanged(); }
    }
    
    public string Unit { get; set; } = "mph";
}
```

## Best Practices

### Positioning

**Do:**
- Center headers horizontally for symmetry
- Use Custom position for precise placement
- Consider gauge shape (full vs. semi-circle)
- Test visibility against pointers/ranges

**Avoid:**
- Overlapping with pointers
- Placing outside gauge bounds
- Blocking important scale labels

### Content

**Do:**
- Keep text concise (1-3 words)
- Include units when applicable
- Use clear, readable fonts
- Maintain adequate font size (12pt minimum)

**Avoid:**
- Very long text (causes wrapping)
- Excessive styling (keep clean)
- Low contrast colors

### Responsive Design

For varying gauge sizes:
- Use relative font sizes
- Test header at different resolutions
- Consider MinWidth/MinHeight constraints
- Use ViewBox for complex headers

## Next Steps

- **Add Annotations:** Additional text/values on gauge
  → Read: [annotations.md](annotations.md)

- **Combine Elements:** Complete gauge with header, ranges, pointers
  → Read all reference files for complete examples
