---
name: syncfusion-wpf-radial-gauge
description: Comprehensive guide for implementing Syncfusion WPF Radial Gauge (SfCircularGauge) component for data visualization in Windows Presentation Foundation applications. Use this when working with circular gauges, speedometers, KPI displays, or progress meters. This skill covers gauge configuration with needles and pointers, scale customization, ranges and zones, interactive pointer dragging, animations, and annotations for creating rich dashboard visualizations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Circular Gauges (Radial Gauge)

Comprehensive guide for implementing the Syncfusion® WPF **Radial Gauge (SfCircularGauge)** component to visualize numeric values on circular scales. This skill helps you create highly customizable gauges including speedometers, temperature indicators, KPI dashboards, and progress meters with needles, ranges, and interactive features.

## When to Use This Skill

Use this skill when you need to:

- **Create circular gauges** for data visualization (speedometers, KPI meters, temperature gauges)
- **Implement needle pointers** to indicate current values on circular scales
- **Add range pointers** to show progress or completion (colored arcs)
- **Configure symbol pointers** for marker-style value indicators
- **Set up scales** with start/end values, angles, and intervals
- **Add visual ranges** to indicate zones (low, medium, high, danger zones)
- **Customize labels and ticks** on gauge scales
- **Enable pointer dragging** for interactive value selection
- **Animate pointer movements** for smooth transitions
- **Add annotations** to display additional information on gauges
- **Create multi-scale gauges** for comparing multiple metrics
- **Build dashboard visualizations** with multiple gauges

This skill is essential for creating any circular/radial visualization where you need to display numeric values on a circular scale with customizable pointers, ranges, and visual indicators.

## Component Overview

The **SfCircularGauge** is a powerful data visualization component that displays numeric values on a circular scale. It's highly customizable and supports multiple visual elements:

### Key Elements

- **Scales:** Circular scales with configurable start/end values, angles, and sweep directions
- **Pointers:** Three types - Needle (traditional gauge needle), Range (colored arc), Symbol (marker icons)
- **Ranges:** Visual zones that mark different value ranges (e.g., low/medium/high)
- **Labels:** Numeric labels along the scale
- **Ticks:** Major and minor tick marks for precise value reading
- **Rim:** Circular border around the scale
- **Header:** Title or label for the gauge
- **Annotations:** Custom views, text, or images overlaid on the gauge

### Common Use Cases

- **Speedometers** - Vehicle speed indicators with needle pointers
- **KPI Dashboards** - Business metrics with range pointers showing progress
- **Temperature Gauges** - Climate control displays with colored zones
- **Progress Meters** - Completion indicators with percentage displays
- **Performance Indicators** - System metrics (CPU, memory, battery)
- **Medical Instruments** - Health monitoring displays
- **Industrial Controls** - Machine operation indicators

## Documentation and Navigation Guide

This skill provides comprehensive coverage of all circular gauge features organized by functionality.

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Setting up your first circular gauge, installing packages, understanding basic structure.

Topics covered:
- Installing Syncfusion.SfGauge.WPF NuGet package
- Assembly references and namespace imports
- Creating your first SfCircularGauge
- Adding basic scale, pointer, and header
- Complete minimal working example
- License configuration

### Scale Configuration

📄 **Read:** [references/scales-and-configuration.md](references/scales-and-configuration.md)

**When to read:** Configuring the circular scale's values, angles, intervals, and appearance.

Topics covered:
- Setting StartValue and EndValue for scale range
- Configuring StartAngle and SweepAngle (partial or full circles)
- Setting Interval for label frequency
- Sweep direction (Clockwise/Counterclockwise)
- Adding multiple scales to a single gauge
- Scale radius and sizing with SpacingMargin
- RadiusFactor for responsive sizing

### Pointers (Value Indicators)

📄 **Read:** [references/pointers.md](references/pointers.md)

**When to read:** Adding needles, range pointers, or symbol markers to indicate values.

Topics covered:
- Needle Pointer types (Rectangle, Triangle, Tapered, Arrow)
- Needle customization (length, stroke, thickness, knob, tail)
- Range Pointer for progress visualization
- Range pointer positioning (Inside, Outside, Cross, Custom)
- Symbol Pointer types (Arrow, Diamond, Ellipse, Triangle, Pentagon, etc.)
- Custom symbol templates
- Multiple pointers on one scale
- Pointer value binding and updates

### Ranges (Visual Zones)

📄 **Read:** [references/ranges.md](references/ranges.md)

**When to read:** Adding colored zones to indicate different value ranges (low/medium/high).

Topics covered:
- Creating ranges with StartValue and EndValue
- Range stroke and thickness customization
- Range positioning with Offset
- Multiple ranges for zone visualization
- Range start/end offset for custom positioning
- Gradient fill patterns
- Use cases (temperature zones, performance bands, danger zones)

### Labels and Ticks

📄 **Read:** [references/labels-and-ticks.md](references/labels-and-ticks.md)

**When to read:** Customizing scale labels, major ticks, and minor ticks.

Topics covered:
- Label stroke and font customization
- Label position (Inside, Outside, Custom)
- Label offset and formatting
- Label prefix/suffix for units
- MajorTickSettings configuration
- MinorTickSettings and MinorTicksPerInterval
- Tick length, stroke, and thickness
- Hiding labels or ticks

### Rim and Appearance

📄 **Read:** [references/rim-and-appearance.md](references/rim-and-appearance.md)

**When to read:** Styling the gauge rim, background, size, and applying themes.

Topics covered:
- ShowRim property and rim customization
- RimStroke and RimStrokeThickness
- Gauge sizing (Height, Width, Radius)
- Background and visual styling
- Theming with SfSkinManager
- Built-in themes (MaterialLight, FluentDark, etc.)
- Custom styling approaches

### Header

📄 **Read:** [references/header.md](references/header.md)

**When to read:** Adding titles or labels to your gauge.

Topics covered:
- GaugeHeader property for text headers
- Custom header views
- Header alignment options
- GaugeHeaderPosition for custom placement
- Header styling and formatting
- Multiple text elements in headers

### Annotations

📄 **Read:** [references/annotations.md](references/annotations.md)

**When to read:** Overlaying custom views, text, images, or nested gauges on the circular gauge.

Topics covered:
- Adding text annotations
- Image annotations
- Custom view annotations (nested gauges, controls)
- Positioning with Angle property
- Offset property (0 to 1 from center to edge)
- ViewMargin for pixel-precise positioning
- Multiple annotations
- Dynamic annotation updates

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** Implementing animations, pointer dragging, events, and MVVM patterns.

Topics covered:
- Pointer animation (EnableAnimation, AnimationDuration)
- Interactive pointer dragging (EnableDragging)
- Events (ValueChangeStarted, ValueChanging, ValueChanged, ValueChangeCompleted)
- PointerPositionChanged event
- MVVM pattern with data binding
- INotifyPropertyChanged implementation
- Real-time gauge updates
- Performance optimization

## Quick Start Example

Here's a complete example to create a speedometer gauge with needle pointer, range zones, and header:

### XAML Implementation

```xml
<Window xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf">
    
    <gauge:SfCircularGauge Height="300" Width="300"
                           HeaderAlignment="Custom"
                           GaugeHeaderPosition="0.63,0.75">
        
        <!-- Header -->
        <gauge:SfCircularGauge.GaugeHeader>
            <TextBlock Text="Speedometer" 
                       FontSize="16" 
                       FontWeight="SemiBold"
                       Foreground="Black"/>
        </gauge:SfCircularGauge.GaugeHeader>
        
        <gauge:SfCircularGauge.Scales>
            <gauge:CircularScale StartValue="0" 
                                 EndValue="160" 
                                 Interval="20"
                                 StartAngle="130"
                                 SweepAngle="280"
                                 ShowRim="True"
                                 RimStroke="LightGray"
                                 RimStrokeThickness="5">
                
                <!-- Ranges for zones -->
                <gauge:CircularScale.Ranges>
                    <gauge:CircularRange StartValue="0" EndValue="80" 
                                         Stroke="Green" StrokeThickness="5"/>
                    <gauge:CircularRange StartValue="80" EndValue="120" 
                                         Stroke="Orange" StrokeThickness="5"/>
                    <gauge:CircularRange StartValue="120" EndValue="160" 
                                         Stroke="Red" StrokeThickness="5"/>
                </gauge:CircularScale.Ranges>
                
                <!-- Needle Pointer -->
                <gauge:CircularScale.Pointers>
                    <gauge:CircularPointer PointerType="NeedlePointer"
                                           Value="85"
                                           NeedlePointerType="Triangle"
                                           NeedleLengthFactor="0.6"
                                           NeedlePointerStroke="#333"
                                           NeedlePointerStrokeThickness="5"
                                           KnobFill="#333"
                                           KnobStroke="#333"
                                           PointerCapDiameter="15"/>
                </gauge:CircularScale.Pointers>
            </gauge:CircularScale>
        </gauge:SfCircularGauge.Scales>
    </gauge:SfCircularGauge>
    
</Window>
```

### C# Code-Behind

```csharp
using Syncfusion.UI.Xaml.Gauges;
using System.Windows;
using System.Windows.Media;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // License registration (in App.xaml.cs OnStartup)
        // Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    }
}
```

### Programmatic Creation

```csharp
SfCircularGauge gauge = new SfCircularGauge();
gauge.Height = 300;
gauge.Width = 300;

// Header
TextBlock header = new TextBlock
{
    Text = "Speedometer",
    FontSize = 16,
    FontWeight = FontWeights.SemiBold,
    Foreground = new SolidColorBrush(Colors.Black)
};
gauge.GaugeHeader = header;
gauge.HeaderAlignment = HeaderAlignment.Custom;
gauge.GaugeHeaderPosition = new Point(0.63, 0.75);

// Scale
CircularScale scale = new CircularScale
{
    StartValue = 0,
    EndValue = 160,
    Interval = 20,
    StartAngle = 130,
    SweepAngle = 280,
    ShowRim = true,
    RimStroke = new SolidColorBrush(Colors.LightGray),
    RimStrokeThickness = 5
};

// Ranges
scale.Ranges.Add(new CircularRange 
{ 
    StartValue = 0, EndValue = 80, 
    Stroke = new SolidColorBrush(Colors.Green), 
    StrokeThickness = 5 
});
scale.Ranges.Add(new CircularRange 
{ 
    StartValue = 80, EndValue = 120, 
    Stroke = new SolidColorBrush(Colors.Orange), 
    StrokeThickness = 5 
});
scale.Ranges.Add(new CircularRange 
{ 
    StartValue = 120, EndValue = 160, 
    Stroke = new SolidColorBrush(Colors.Red), 
    StrokeThickness = 5 
});

// Needle Pointer
CircularPointer pointer = new CircularPointer
{
    PointerType = PointerType.NeedlePointer,
    Value = 85,
    NeedlePointerType = NeedlePointerType.Triangle,
    NeedleLengthFactor = 0.6,
    NeedlePointerStroke = new SolidColorBrush(Color.FromRgb(51, 51, 51)),
    NeedlePointerStrokeThickness = 5,
    KnobFill = new SolidColorBrush(Color.FromRgb(51, 51, 51)),
    KnobStroke = new SolidColorBrush(Color.FromRgb(51, 51, 51)),
    PointerCapDiameter = 15
};
scale.Pointers.Add(pointer);

gauge.Scales.Add(scale);
this.Content = gauge;
```

## Common Patterns

### Pattern 1: Progress Gauge with Range Pointer

Use range pointers for progress/completion visualization:

```xml
<gauge:SfCircularGauge>
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale StartValue="0" EndValue="100" 
                             SweepAngle="360"
                             RimStroke="LightGray" 
                             RimStrokeThickness="20">
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer PointerType="RangePointer"
                                       Value="75"
                                       RangePointerStroke="DeepSkyBlue"
                                       RangePointerStrokeThickness="20"
                                       RangeCap="Both"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

### Pattern 2: Temperature Gauge with Color Zones

Create temperature displays with multiple ranges:

```xml
<gauge:CircularScale StartValue="-30" EndValue="50">
    <gauge:CircularScale.Ranges>
        <gauge:CircularRange StartValue="-30" EndValue="0" 
                             Stroke="Blue" StrokeThickness="10"/>
        <gauge:CircularRange StartValue="0" EndValue="25" 
                             Stroke="Green" StrokeThickness="10"/>
        <gauge:CircularRange StartValue="25" EndValue="50" 
                             Stroke="Red" StrokeThickness="10"/>
    </gauge:CircularScale.Ranges>
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="NeedlePointer" Value="22"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Pattern 3: Interactive Draggable Pointer

Enable user interaction with pointer dragging:

```xml
<gauge:CircularPointer PointerType="SymbolPointer"
                       Symbol="InvertedTriangle"
                       EnableDragging="True"
                       EnableAnimation="False"
                       Value="50"
                       ValueChanged="Pointer_ValueChanged"/>
```

```csharp
private void Pointer_ValueChanged(object sender, ValueChangedEventArgs e)
{
    // Update UI or data model with new value
    CurrentValue = e.Value;
}
```

### Pattern 4: Multi-Scale Gauge

Display multiple metrics in one gauge:

```xml
<gauge:SfCircularGauge SpacingMargin="0.7">
    <gauge:SfCircularGauge.Scales>
        <!-- Outer Scale -->
        <gauge:CircularScale Radius="180">
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="60"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
        
        <!-- Inner Scale -->
        <gauge:CircularScale Radius="100">
            <gauge:CircularScale.Pointers>
                <gauge:CircularPointer Value="80"/>
            </gauge:CircularScale.Pointers>
        </gauge:CircularScale>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

### Pattern 5: MVVM Data Binding

Bind pointer values to ViewModel properties:

```xml
<gauge:CircularPointer Value="{Binding CurrentSpeed, Mode=TwoWay}"/>
```

```csharp
public class GaugeViewModel : INotifyPropertyChanged
{
    private double _currentSpeed;
    public double CurrentSpeed
    {
        get => _currentSpeed;
        set
        {
            _currentSpeed = value;
            OnPropertyChanged(nameof(CurrentSpeed));
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

## Key Properties Summary

### SfCircularGauge

| Property | Type | Description |
|----------|------|-------------|
| `Scales` | ObservableCollection<CircularScale> | Collection of scales in the gauge |
| `GaugeHeader` | object | Header content for the gauge |
| `HeaderAlignment` | HeaderAlignment | Header position (Top, Bottom, Custom) |
| `GaugeHeaderPosition` | Point | Custom header position (0-1, 0-1) |
| `Annotations` | CircularGaugeAnnotationCollection | Custom annotations overlay |
| `SpacingMargin` | double | Spacing between multiple scales (0.1-1) |

### CircularScale

| Property | Type | Description |
|----------|------|-------------|
| `StartValue` | double | Minimum value of the scale |
| `EndValue` | double | Maximum value of the scale |
| `Interval` | double | Interval between labels |
| `StartAngle` | double | Starting angle (0-360) |
| `SweepAngle` | double | Sweep angle (0-360) |
| `SweepDirection` | SweepDirection | Clockwise or Counterclockwise |
| `Radius` | double | Scale radius |
| `RadiusFactor` | double | Radius factor (0-1) |
| `ShowRim` | bool | Show/hide rim |
| `RimStroke` | Brush | Rim color |
| `RimStrokeThickness` | double | Rim thickness |
| `Pointers` | ObservableCollection<CircularPointer> | Pointers collection |
| `Ranges` | ObservableCollection<CircularRange> | Ranges collection |

### CircularPointer

| Property | Type | Description |
|----------|------|-------------|
| `PointerType` | PointerType | NeedlePointer, RangePointer, SymbolPointer |
| `Value` | double | Current pointer value |
| `EnableAnimation` | bool | Enable smooth animation |
| `AnimationDuration` | int | Animation duration (ms) |
| `EnableDragging` | bool | Enable interactive dragging |
| `NeedlePointerType` | NeedlePointerType | Rectangle, Triangle, Tapered, Arrow |
| `NeedleLengthFactor` | double | Needle length (0-1) |
| `Symbol` | Symbol | Symbol type for SymbolPointer |
| `RangeCap` | RangeCap | Range cap style (Start, End, Both, None) |

### CircularRange

| Property | Type | Description |
|----------|------|-------------|
| `StartValue` | double | Range start value |
| `EndValue` | double | Range end value |
| `Stroke` | Brush | Range color |
| `StrokeThickness` | double | Range thickness |
| `Offset` | double | Position offset (0-1) |

## Common Use Cases

### Business Dashboards
Create KPI dashboards with multiple gauges showing metrics like sales, performance, customer satisfaction with color-coded zones for targets.

### Industrial Monitoring
Build control panels with gauges for pressure, temperature, RPM, voltage with real-time updates and alarm zones.

### Vehicle Instrumentation
Implement speedometers, tachometers, fuel gauges, temperature indicators with realistic needle movements and warning zones.

### Health & Fitness
Display heart rate monitors, step counters, calorie trackers with progress visualization and target zones.

### System Monitoring
Create CPU usage, memory utilization, disk space, network bandwidth indicators with real-time updates.

## API Reference

- **SfCircularGauge API:** https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.SfCircularGauge.html
- **CircularScale API:** https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.CircularScale.html
- **CircularPointer API:** https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.CircularPointer.html
- **CircularRange API:** https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.CircularRange.html
- **Full Documentation:** https://help.syncfusion.com/wpf/radial-gauge/overview

---

**Next Steps:** Explore the reference files above based on your specific needs. Start with Getting Started for setup, then dive into specific features as required.