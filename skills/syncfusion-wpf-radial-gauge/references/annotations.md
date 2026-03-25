# Annotations

## Table of Contents
- [Annotations Overview](#annotations-overview)
- [Creating Annotations](#creating-annotations)
- [Positioning Annotations](#positioning-annotations)
- [Text Annotations](#text-annotations)
- [Image Annotations](#image-annotations)
- [Custom View Annotations](#custom-view-annotations)
- [Multiple Annotations](#multiple-annotations)
- [Common Patterns](#common-patterns)

## Annotations Overview

**Annotations** allow you to overlay custom views, text, or images on the circular gauge. They're positioned using angle and offset coordinates.

**Key Features:**
- Position views at any angle and distance
- Add text labels, images, or complex UI elements
- Multiple annotations per gauge
- Pixel-precise positioning with ViewMargin

**Use Cases:**
- Display current value as text
- Add icons or indicators
- Create nested gauges
- Show additional metrics
- Custom overlays

## Creating Annotations

Annotations are added to the `Annotations` collection of `SfCircularGauge`.

### Basic Text Annotation

**XAML:**
```xml
<gauge:SfCircularGauge>
    <gauge:SfCircularGauge.Annotations>
        <gauge:GaugeAnnotation>
            <TextBlock Text="75%" 
                       FontSize="24" 
                       FontWeight="Bold"/>
        </gauge:GaugeAnnotation>
    </gauge:SfCircularGauge.Annotations>
    
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale>
            <!-- Scale content -->
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**C#:**
```csharp
TextBlock annotationText = new TextBlock
{
    Text = "75%",
    FontSize = 24,
    FontWeight = FontWeights.Bold
};

GaugeAnnotation annotation = new GaugeAnnotation
{
    Content = annotationText
};

CircularGaugeAnnotationCollection annotations = new CircularGaugeAnnotationCollection();
annotations.Add(annotation);
gauge.Annotations = annotations;
```

## Positioning Annotations

Annotations are positioned using three properties:

1. **Angle** - Angular position (0-360 degrees)
2. **Offset** - Distance from center (0-1)
3. **ViewMargin** - Pixel-level adjustment (X, Y)

### Angle Property

Determines the angular position around the gauge.

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270">
    <TextBlock Text="Top" FontSize="16"/>
</gauge:GaugeAnnotation>
```

**Angle Reference:**
- `0°` - Right (3 o'clock)
- `90°` - Bottom (6 o'clock)
- `180°` - Left (9 o'clock)
- `270°` - Top (12 o'clock)

**C#:**
```csharp
annotation.Angle = 270;
```

### Offset Property

Distance from center to edge (0 to 1).

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <TextBlock Text="Center" FontSize="20"/>
</gauge:GaugeAnnotation>
```

**Offset Values:**
- `0` - At gauge center
- `0.5` - Midway between center and edge
- `0.8` - Near the rim
- `1.0` - At the rim

**C#:**
```csharp
annotation.Offset = 0;  // Center
```

### ViewMargin Property

Fine-tune position with pixel offsets.

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" 
                       Offset="0"
                       ViewMargin="0,-10,0,0">
    <TextBlock Text="Adjusted" FontSize="16"/>
</gauge:GaugeAnnotation>
```

**ViewMargin format:** `Left,Top,Right,Bottom`

**C#:**
```csharp
annotation.ViewMargin = new Thickness(0, -10, 0, 0);
```

**Use Case:** Precise alignment when Angle/Offset aren't exact enough.

## Text Annotations

### Simple Text

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <TextBlock Text="75%" 
               FontSize="28"
               FontWeight="Bold"
               Foreground="DeepSkyBlue"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
</gauge:GaugeAnnotation>
```

### Multi-Line Text

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0.3">
    <StackPanel HorizontalAlignment="Center">
        <TextBlock Text="Current" 
                   FontSize="12" 
                   Foreground="Gray"
                   HorizontalAlignment="Center"/>
        <TextBlock Text="85 mph" 
                   FontSize="24" 
                   FontWeight="Bold"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</gauge:GaugeAnnotation>
```

### Data-Bound Text

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <TextBlock Text="{Binding CurrentValue, StringFormat='{}{0:N0}%'}" 
               FontSize="32"
               FontWeight="Bold"/>
</gauge:GaugeAnnotation>
```

**ViewModel:**
```csharp
public class GaugeViewModel : INotifyPropertyChanged
{
    private double _currentValue = 75;
    public double CurrentValue
    {
        get => _currentValue;
        set
        {
            _currentValue = value;
            OnPropertyChanged(nameof(CurrentValue));
        }
    }
}
```

## Image Annotations

### Static Image

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0.2">
    <Image Source="/Images/icon.png" 
           Width="40" 
           Height="40"/>
</gauge:GaugeAnnotation>
```

### Icon with Text

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="90" Offset="0.4">
    <StackPanel Orientation="Horizontal">
        <Image Source="/Images/warning.png" 
               Width="24" Height="24"
               Margin="0,0,5,0"/>
        <TextBlock Text="Alert" 
                   FontSize="14"
                   VerticalAlignment="Center"/>
    </StackPanel>
</gauge:GaugeAnnotation>
```

### SVG Path as Icon

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0.6">
    <Path Data="M12,2L4.5,20.29L5.21,21L12,18L18.79,21L19.5,20.29L12,2Z"
          Fill="Orange"
          Width="24"
          Height="24"/>
</gauge:GaugeAnnotation>
```

## Custom View Annotations

### Nested Gauge

**XAML:**
```xml
<gauge:SfCircularGauge Height="350" Width="350">
    <gauge:SfCircularGauge.Annotations>
        <gauge:GaugeAnnotation Angle="270" Offset="0">
            <!-- Nested smaller gauge -->
            <gauge:SfCircularGauge Height="150" Width="150">
                <gauge:SfCircularGauge.Scales>
                    <gauge:CircularScale StartValue="0" EndValue="100">
                        <gauge:CircularScale.Pointers>
                            <gauge:CircularPointer Value="60"/>
                        </gauge:CircularScale.Pointers>
                    </gauge:CircularScale>
                </gauge:SfCircularGauge.Scales>
            </gauge:SfCircularGauge>
        </gauge:GaugeAnnotation>
    </gauge:SfCircularGauge.Annotations>
    
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale>
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="80"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

### Button Annotation

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="90" Offset="0.8">
    <Button Content="Reset" 
            Width="60" 
            Height="30"
            Click="ResetButton_Click"/>
</gauge:GaugeAnnotation>
```

### Progress Ring Annotation

**XAML:**
```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <Grid Width="100" Height="100">
        <Ellipse Stroke="LightGray" 
                 StrokeThickness="8"/>
        <TextBlock Text="Loading..." 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"/>
    </Grid>
</gauge:GaugeAnnotation>
```

## Multiple Annotations

### Multiple Text Labels

**XAML:**
```xml
<gauge:SfCircularGauge.Annotations>
    <!-- Center value -->
    <gauge:GaugeAnnotation Angle="270" Offset="0">
        <TextBlock Text="75%" 
                   FontSize="36" 
                   FontWeight="Bold"
                   Foreground="DeepSkyBlue"/>
    </gauge:GaugeAnnotation>
    
    <!-- Bottom label -->
    <gauge:GaugeAnnotation Angle="270" Offset="0.4">
        <TextBlock Text="Progress" 
                   FontSize="14" 
                   Foreground="Gray"/>
    </gauge:GaugeAnnotation>
</gauge:SfCircularGauge.Annotations>
```

### Value Markers at Specific Angles

**XAML:**
```xml
<gauge:SfCircularGauge.Annotations>
    <!-- Min marker -->
    <gauge:GaugeAnnotation Angle="180" Offset="0.7">
        <TextBlock Text="Min" 
                   FontSize="10" 
                   Foreground="Gray"/>
    </gauge:GaugeAnnotation>
    
    <!-- Max marker -->
    <gauge:GaugeAnnotation Angle="0" Offset="0.7">
        <TextBlock Text="Max" 
                   FontSize="10" 
                   Foreground="Gray"/>
    </gauge:GaugeAnnotation>
</gauge:SfCircularGauge.Annotations>
```

### Programmatic Multiple Annotations

**C#:**
```csharp
CircularGaugeAnnotationCollection annotations = new CircularGaugeAnnotationCollection();

// Center annotation
GaugeAnnotation centerAnnotation = new GaugeAnnotation
{
    Angle = 270,
    Offset = 0,
    Content = new TextBlock 
    { 
        Text = "75%", 
        FontSize = 32 
    }
};
annotations.Add(centerAnnotation);

// Bottom annotation
GaugeAnnotation bottomAnnotation = new GaugeAnnotation
{
    Angle = 270,
    Offset = 0.4,
    Content = new TextBlock 
    { 
        Text = "Complete", 
        FontSize = 14 
    }
};
annotations.Add(bottomAnnotation);

gauge.Annotations = annotations;
```

## Common Patterns

### Pattern 1: Center Value Display

```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <TextBlock Text="75%" 
               FontSize="40" 
               FontWeight="Bold"
               Foreground="DeepSkyBlue"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
</gauge:GaugeAnnotation>
```

### Pattern 2: Multi-Line Center Display

```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <StackPanel HorizontalAlignment="Center">
        <TextBlock Text="75%" 
                   FontSize="36" 
                   FontWeight="Bold"
                   Foreground="DeepSkyBlue"
                   HorizontalAlignment="Center"/>
        <TextBlock Text="Complete" 
                   FontSize="14" 
                   Foreground="Gray"
                   HorizontalAlignment="Center"
                   Margin="0,5,0,0"/>
    </StackPanel>
</gauge:GaugeAnnotation>
```

### Pattern 3: Bottom Label (Speedometer Style)

```xml
<gauge:GaugeAnnotation Angle="270" Offset="0.65">
    <TextBlock Text="km/h" 
               FontSize="14" 
               Foreground="#666"
               HorizontalAlignment="Center"/>
</gauge:GaugeAnnotation>
```

### Pattern 4: Corner Status Indicator

```xml
<gauge:GaugeAnnotation Angle="45" Offset="0.85">
    <Border Background="LimeGreen" 
            CornerRadius="10"
            Padding="8,4">
        <TextBlock Text="OK" 
                   Foreground="White"
                   FontSize="12"
                   FontWeight="Bold"/>
    </Border>
</gauge:GaugeAnnotation>
```

### Pattern 5: Data-Bound Value with Unit

```xml
<gauge:GaugeAnnotation Angle="270" Offset="0">
    <StackPanel HorizontalAlignment="Center">
        <TextBlock FontSize="32" FontWeight="Bold"
                   HorizontalAlignment="Center">
            <Run Text="{Binding CurrentValue, StringFormat='{}{0:N0}'}"/>
        </TextBlock>
        <TextBlock Text="{Binding Unit}" 
                   FontSize="14" 
                   Foreground="Gray"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</gauge:GaugeAnnotation>
```

## Best Practices

### Positioning

**Do:**
- Use Offset=0 for center annotations
- Test visibility at different gauge sizes
- Use ViewMargin for fine adjustments
- Consider pointer/range overlap

**Avoid:**
- Overlapping multiple annotations
- Placing outside gauge bounds
- Blocking important scale elements

### Content

**Do:**
- Keep annotations concise
- Use readable font sizes (16pt+ for values)
- Ensure good contrast
- Update dynamically with data binding

**Avoid:**
- Too much text (clutters gauge)
- Very small fonts (<10pt)
- Low contrast on gauge background

### Performance

**For dynamic updates:**
- Use data binding instead of manually updating
- Avoid complex views if updating frequently
- Consider virtualization for many annotations

## Next Steps

- **Add Interactivity:** Combine with draggable pointers
  → Read: [advanced-features.md](advanced-features.md#pointer-dragging)

- **Animations:** Animate annotation content
  → Read: [advanced-features.md](advanced-features.md#animations)

- **Complete Example:** Build full dashboard gauges
  → See SKILL.md for complete patterns
