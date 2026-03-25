---
name: syncfusion-wpf-digital-gauge
description:  Implement Syncfusion WPF Digital Gauge (SfDigitalGauge) to display alphanumeric characters in digital LED-style segments. Use this when working with seven-segment displays, digital clocks, LED indicators, or digital counters. This skill covers segment display configuration, dot matrix displays, character-based digital visualizations, and creating virtual digital readouts in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Digital Gauge (SfDigitalGauge)

The **SfDigitalGauge** control displays alphanumeric characters in digital LED-style segments, perfect for creating virtual digital displays, clocks, speedometers, and counters in WPF applications.

## When to Use This Skill

Use SfDigitalGauge when you need to:
- **Display numeric values** in a digital LED format (clocks, timers, counters)
- **Create digital displays** for dashboards or instrument panels
- **Show alphanumeric text** in segment-based format (product codes, status messages)
- **Build digital speedometers** or odometers showing speed/distance
- **Create retro-style digital interfaces** with classic LED segment aesthetics
- **Display special characters** in dot matrix format
- **Implement real-time updating displays** (live data feeds, sensor readings)

**Component Type:** Data Visualization Control  
**Platform:** Windows Presentation Foundation (WPF)  
**Package:** Syncfusion.SfGauge.WPF

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and configure SfDigitalGauge in a WPF project
- Add assembly references (NuGet, Toolbox, or Manual)
- Set up namespaces for XAML and C# code
- Initialize the control for first use
- Display basic values
- Apply theme support

### Character Types and Segments
📄 **Read:** [references/character-types.md](references/character-types.md)

When you need to:
- Choose between 4 different segment display types
- Display numbers only (7-segment)
- Display numbers and alphabets (14-segment or 16-segment)
- Display special characters (8×8 dot matrix)
- Understand which CharacterType to use for your use case
- See visual comparisons of segment types

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)

When you need to:
- Adjust character dimensions (height, width)
- Control spacing between characters
- Change character stroke color
- Style dimmed (inactive) segments
- Adjust opacity of background segments
- Create visually appealing displays

### Transformations
📄 **Read:** [references/transformations.md](references/transformations.md)

When you need to:
- Scale characters (increase/decrease size)
- Apply skew transformations
- Create slanted digital displays
- Transform characters along X or Y axis
- Implement perspective effects

### Segment Styling and Advanced Features
📄 **Read:** [references/segment-styling.md](references/segment-styling.md)

When you need to:
- Adjust segment thickness
- Enable Right-to-Left (RTL) text direction
- Fine-tune visual appearance
- Match background colors

---

## Quick Start

### Basic Digital Display

```xaml
<Window xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf">
    <gauge:SfDigitalGauge Value="12345" />
</Window>
```

```csharp
using Syncfusion.UI.Xaml.Gauges;

SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "12345";
this.Content = digitalGauge;
```

### Digital Clock Display

```xaml
<gauge:SfDigitalGauge Value="11:59:50 PM"
                      Height="100"
                      Width="375"
                      CharacterHeight="60"
                      CharacterWidth="25"
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterStroke="#146CED"
                      DimmedBrush="#F2F2F2"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center" />
```

```csharp
SfDigitalGauge clock = new SfDigitalGauge();
clock.Value = "11:59:50 PM";
clock.Height = 100;
clock.Width = 375;
clock.CharacterHeight = 60;
clock.CharacterWidth = 25;
clock.CharacterType = CharacterType.EightCrossEightDotMatrix;
clock.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
clock.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#F2F2F2");
clock.HorizontalAlignment = HorizontalAlignment.Center;
clock.VerticalAlignment = VerticalAlignment.Center;
```

---

## Common Patterns

### Pattern 1: Simple Numeric Counter

Use 7-segment display for numbers only:

```xaml
<gauge:SfDigitalGauge Value="12345" 
                      CharacterType="SegmentSeven"
                      CharacterHeight="40"
                      CharacterWidth="20"
                      CharacterStroke="Red" />
```

**When to use:** Counters, timers, numeric displays where only digits 0-9 are needed.

### Pattern 2: Alphanumeric Display

Use 14-segment or 16-segment for text and numbers:

```xaml
<gauge:SfDigitalGauge Value="SPEED 088" 
                      CharacterType="SegmentFourteen"
                      CharacterHeight="50"
                      CharacterWidth="30"
                      CharacterStroke="Green" />
```

**When to use:** Status messages, product codes, labels with mixed alphanumeric content.

### Pattern 3: Dot Matrix for Special Characters

Use 8×8 dot matrix for full character support:

```xaml
<gauge:SfDigitalGauge Value="SYNC@2024!" 
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterHeight="60"
                      CharacterWidth="35"
                      CharacterStroke="Orange" />
```

**When to use:** Email addresses, URLs, special symbols, complex text displays.

### Pattern 4: Styled Dashboard Display

Apply comprehensive styling for professional appearance:

```xaml
<gauge:SfDigitalGauge Value="TEMP 72°F"
                      CharacterType="SegmentSixteen"
                      CharacterHeight="55"
                      CharacterWidth="32"
                      CharactersSpacing="8"
                      CharacterStroke="#00FF00"
                      DimmedBrush="#1A1A1A"
                      DimmedBrushOpacity="30"
                      SegmentThickness="3" />
```

**When to use:** Dashboard panels, control rooms, monitoring displays requiring clear visibility.

### Pattern 5: Skewed Digital Speedometer

Create dynamic perspective effect:

```xaml
<gauge:SfDigitalGauge Value="125 MPH"
                      CharacterType="SegmentSeven"
                      CharacterHeight="70"
                      CharacterWidth="40"
                      SkewAngleX="15"
                      CharacterStroke="Cyan" />
```

**When to use:** Speedometers, racing dashboards, dynamic displays with motion feel.

---

## Key Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| **Value** | string | The alphanumeric text to display |
| **CharacterType** | Enum | Segment type: SegmentSeven, SegmentFourteen, SegmentSixteen, EightCrossEightDotMatrix |
| **CharacterHeight** | double | Height of each character in pixels |
| **CharacterWidth** | double | Width of each character in pixels |
| **CharactersSpacing** | double | Distance between characters |
| **CharacterStroke** | Brush | Color of active (lit) segments |
| **DimmedBrush** | Brush | Color of inactive (dimmed) segments |
| **DimmedBrushOpacity** | double | Opacity of dimmed segments (0-100) |
| **SegmentThickness** | double | Thickness of segment lines |
| **SkewAngleX** | double | Horizontal skew angle (degrees) |
| **SkewAngleY** | double | Vertical skew angle (degrees) |
| **EnableRTLFormat** | bool | Enable Right-to-Left text direction |

---

## Common Use Cases

### 1. Real-Time Clock Display
Display current time with updating segments:
```csharp
// Update timer every second
DispatcherTimer timer = new DispatcherTimer();
timer.Interval = TimeSpan.FromSeconds(1);
timer.Tick += (s, e) => digitalGauge.Value = DateTime.Now.ToString("hh:mm:ss tt");
timer.Start();
```

### 2. Countdown Timer
Create countdown displays for events:
```csharp
TimeSpan remaining = TimeSpan.FromMinutes(10);
digitalGauge.Value = remaining.ToString(@"mm\:ss");
```

### 3. Live Sensor Reading
Display sensor data with appropriate formatting:
```csharp
digitalGauge.Value = $"TEMP {sensorReading:F1}°C";
digitalGauge.CharacterType = CharacterType.SegmentSixteen;
```

### 4. Digital Odometer
Show distance with leading zeros:
```csharp
double miles = 12345.6;
digitalGauge.Value = miles.ToString("000000.0");
digitalGauge.CharacterType = CharacterType.SegmentSeven;
```

### 5. Status Indicator
Display system status messages:
```csharp
digitalGauge.Value = isOnline ? "ONLINE" : "OFFLINE";
digitalGauge.CharacterStroke = isOnline ? Brushes.Green : Brushes.Red;
digitalGauge.CharacterType = CharacterType.SegmentFourteen;
```

---

## Best Practices

1. **Choose the right CharacterType:**
   - Numbers only → SegmentSeven
   - Numbers + Letters → SegmentFourteen or SegmentSixteen
   - Special characters → EightCrossEightDotMatrix

2. **Ensure readability:**
   - Use appropriate CharacterHeight (40-70px for most cases)
   - Set proper CharactersSpacing (5-15px)
   - Choose contrasting CharacterStroke colors

3. **Optimize dimmed segments:**
   - Set DimmedBrush to match your background
   - Adjust DimmedBrushOpacity (20-40) for subtle effect

4. **Performance considerations:**
   - Avoid rapid Value updates (<50ms intervals)
   - Use string formatting to prevent unnecessary updates

5. **Accessibility:**
   - Provide text alternatives for screen readers
   - Ensure sufficient color contrast
   - Consider users with visual impairments

---

## Related Skills

- [Implementing Radial Gauges](../implementing-radial-gauges/) - For circular gauge displays
- [Implementing Range Selectors](../implementing-range-selectors/) - For range-based data visualization
- [WPF Theming](../../../../theming/) - For applying consistent themes

---

## Additional Resources

- [Syncfusion Digital Gauge Documentation](https://help.syncfusion.com/wpf/digital-gauge/overview)
- [Theme Studio](https://help.syncfusion.com/wpf/themes/theme-studio)
- [GitHub Sample](https://github.com/SyncfusionExamples/WPF-UG-getting-started-samples/tree/master/GettingStartedDigitalGauge)
