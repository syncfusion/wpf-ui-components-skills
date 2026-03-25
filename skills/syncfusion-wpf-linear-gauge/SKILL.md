---
name: syncfusion-wpf-linear-gauge
description: Implement Syncfusion WPF Linear Gauge (SfLinearGauge) in Windows Presentation Foundation applications. Use this when working with linear meters, thermometer displays, or progress indicators with scales. This skill covers scale configuration, adding pointers (bar or symbol), creating colored ranges, customizing labels and ticks, setting horizontal or vertical orientation, and implementing thermometer-style visualizations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Linear Gauges

A comprehensive guide for implementing the Syncfusion WPF Linear Gauge (SfLinearGauge) control in Windows Presentation Foundation applications. The Linear Gauge displays numerical values graphically along a linear scale, making it ideal for thermometers, progress indicators, sliding meters, and value visualization in horizontal or vertical orientations.

## When to Use This Skill

Use this skill when you need to:
- **Implement SfLinearGauge** in WPF applications
- **Create thermometer-style displays** for temperature or measurement visualization
- **Build progress indicators** with graduated scales and value markers
- **Visualize numerical values linearly** with scales, pointers, and ranges
- **Create sliding meters** for performance monitoring or value tracking
- **Display values with colored zones** to indicate different value ranges
- **Implement horizontal or vertical gauges** with customizable orientation
- **Add animated pointers** that smoothly transition to new values
- **Configure multiple pointers** for value comparison on a single scale
- **Design custom gauge layouts** with flexible positioning and styling

## Component Overview

The **SfLinearGauge** control measures and displays values along a linear scale in either horizontal or vertical orientation. It provides a complete gauge visualization system with:

- **Linear Scale** - The foundation with configurable min/max values, intervals, and direction
- **Pointers** - Bar and symbol pointers to mark specific values on the scale
- **Ranges** - Colored sections to highlight specific value ranges
- **Labels** - Numeric labels with formatting, prefix/postfix, and positioning options
- **Ticks** - Major and minor tick marks to indicate scale intervals
- **Orientation** - Horizontal (landscape) or vertical (portrait) layout support
- **Animation** - Smooth pointer animations with configurable duration
- **Theming** - Built-in theme support for consistent UI styling

**Key capabilities:**
- Multiple pointer types (bar and symbol) on a single scale
- Color-coded ranges for threshold visualization
- Full XAML and code-behind customization
- MVVM data binding support
- Responsive sizing and layout
- Professional gauge aesthetics

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Starting a new Linear Gauge implementation or first-time setup.

**Covers:**
- Installing Syncfusion.SfGauge.WPF NuGet package
- Adding assembly references (three methods)
- Namespace imports in XAML and C#
- Initializing an empty SfLinearGauge control
- Configuring your first scale with basic properties
- Adding a symbol pointer to mark values
- Adding a bar pointer for progress indication
- Creating colored ranges for value zones
- Complete minimal working example
- Applying themes with SfSkinManager

### Scale Configuration

📄 **Read:** [references/scale-configuration.md](references/scale-configuration.md)

**When to read:** Configuring the scale's appearance, range, intervals, size, direction, or position.

**Covers:**
- Understanding the MainScale property and LinearScale
- Setting minimum and maximum values for scale range
- Configuring intervals for label and tick spacing
- Customizing scale appearance (color, borders, stroke)
- Scale size customization (ScaleBarSize, ScaleBarLength)
- Setting scale direction (Forward or Backward)
- Scale positioning with ScaleBarPositionFactor
- ElementsPositionMode for custom positioning
- Auto-interval calculation
- Complete code examples in XAML and C#

### Pointers

📄 **Read:** [references/pointers.md](references/pointers.md)

**When to read:** Adding pointers to mark values, implementing bar or symbol pointers, customizing pointer appearance, enabling animations, or adding multiple pointers.

**Covers:**
- Pointer types overview (BarPointer and SymbolPointer)
- Implementing bar pointers for progress/fill indication
- Bar pointer customization (stroke color, thickness)
- Implementing symbol pointers for value markers
- Symbol pointer customization (size, shape, stroke)
- Symbol pointer positioning (Above, Below, Cross)
- Custom symbol pointer shapes with templates
- Adding multiple pointers to a single scale
- Enabling pointer animations with EnableAnimation
- Controlling animation speed with AnimationDuration
- Binding pointer values to data
- Complete examples for each pointer type

### Ranges

📄 **Read:** [references/ranges.md](references/ranges.md)

**When to read:** Highlighting value ranges with colors, creating threshold zones, adding multiple ranges, or customizing range appearance and position.

**Covers:**
- Range overview and visual purpose
- Setting StartValue and EndValue for ranges
- Range customization (stroke color, width, opacity)
- Variable-width ranges (StartWidth, EndWidth)
- Binding range colors to ticks and labels
- Range positioning with RangeOffset
- RangePosition property (Above, Below)
- Adding multiple ranges for color-coded zones
- Common range patterns (safe/warning/danger zones)
- Complete multi-range examples

### Labels and Ticks

📄 **Read:** [references/labels-and-ticks.md](references/labels-and-ticks.md)

**When to read:** Customizing scale labels, formatting label text, positioning labels, configuring tick marks, or adjusting tick intervals.

**Covers:**
- **Labels:**
  - Label color customization with LabelStroke
  - Font customization (LabelSize, FontFamily, FontStyle)
  - Label positioning (LabelPosition: Above, Below)
  - Label offset for spacing control
  - Adding prefix and postfix to label text
  - Label visibility control
- **Ticks:**
  - Major and minor tick customization (stroke, size, thickness)
  - Tick positioning (TickPosition: Above, Below, Cross)
  - Configuring MinorTicksPerInterval
  - Tick appearance and styling
- Complete examples combining labels and ticks

### Orientation

📄 **Read:** [references/orientation.md](references/orientation.md)

**When to read:** Changing gauge orientation from horizontal to vertical or vice versa.

**Covers:**
- Orientation property overview
- Horizontal orientation (default landscape layout)
- Vertical orientation (portrait layout)
- Orientation use cases and considerations
- Layout implications for pointers and ranges
- Complete examples for both orientations

## Quick Start Example

Here's a minimal working Linear Gauge with a scale, bar pointer, symbol pointer, and colored range:

### XAML

```xml
<Window xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf">
    <Grid>
        <gauge:SfLinearGauge>
            <gauge:SfLinearGauge.MainScale>
                <gauge:LinearScale 
                    Minimum="0" 
                    Maximum="100"
                    Interval="10"
                    ScaleBarStroke="#E0E0E0"
                    LabelStroke="#424242"
                    MajorTickStroke="Gray"
                    MajorTickSize="15"
                    MinorTickStroke="Gray"
                    MinorTickSize="7"
                    MinorTicksPerInterval="3"
                    ScaleBarSize="10"
                    ScaleBarLength="300">
                    
                    <!-- Bar Pointer -->
                    <gauge:LinearScale.Pointers>
                        <gauge:LinearPointer 
                            PointerType="BarPointer"
                            Value="65"
                            BarPointerStroke="#36D1DC"
                            BarPointerStrokeThickness="10"/>
                        
                        <!-- Symbol Pointer -->
                        <gauge:LinearPointer 
                            PointerType="SymbolPointer"
                            Value="65"
                            SymbolPointerHeight="15"
                            SymbolPointerWidth="15"
                            SymbolPointerPosition="Above"
                            SymbolPointerStroke="#5B86E5"/>
                    </gauge:LinearScale.Pointers>
                    
                    <!-- Colored Ranges -->
                    <gauge:LinearScale.Ranges>
                        <gauge:LinearRange 
                            StartValue="0" 
                            EndValue="35"
                            RangeStroke="#27BEB7"
                            StartWidth="10"
                            EndWidth="10"
                            RangeOffset="0.4"/>
                        
                        <gauge:LinearRange 
                            StartValue="35" 
                            EndValue="70"
                            RangeStroke="Orange"
                            StartWidth="10"
                            EndWidth="10"
                            RangeOffset="0.4"/>
                        
                        <gauge:LinearRange 
                            StartValue="70" 
                            EndValue="100"
                            RangeStroke="Red"
                            StartWidth="10"
                            EndWidth="10"
                            RangeOffset="0.4"/>
                    </gauge:LinearScale.Ranges>
                </gauge:LinearScale>
            </gauge:SfLinearGauge.MainScale>
        </gauge:SfLinearGauge>
    </Grid>
</Window>
```

### C# Code-Behind

```csharp
using Syncfusion.UI.Xaml.Gauges;
using System.Windows.Media;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        CreateLinearGauge();
    }
    
    private void CreateLinearGauge()
    {
        // Create gauge
        SfLinearGauge gauge = new SfLinearGauge();
        
        // Create scale
        LinearScale scale = new LinearScale
        {
            Minimum = 0,
            Maximum = 100,
            Interval = 10,
            ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
            LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
            MajorTickStroke = new SolidColorBrush(Colors.Gray),
            MajorTickSize = 15,
            MinorTickStroke = new SolidColorBrush(Colors.Gray),
            MinorTickSize = 7,
            MinorTicksPerInterval = 3,
            ScaleBarSize = 10,
            ScaleBarLength = 300
        };
        
        // Add bar pointer
        scale.Pointers.Add(new LinearPointer
        {
            PointerType = LinearPointerType.BarPointer,
            Value = 65,
            BarPointerStroke = new SolidColorBrush(Color.FromRgb(54, 209, 220)),
            BarPointerStrokeThickness = 10
        });
        
        // Add symbol pointer
        scale.Pointers.Add(new LinearPointer
        {
            PointerType = LinearPointerType.SymbolPointer,
            Value = 65,
            SymbolPointerHeight = 15,
            SymbolPointerWidth = 15,
            SymbolPointerPosition = LinearSymbolPointersPosition.Above,
            SymbolPointerStroke = new SolidColorBrush(Color.FromRgb(91, 134, 229))
        });
        
        // Add ranges
        scale.Ranges.Add(new LinearRange
        {
            StartValue = 0,
            EndValue = 35,
            RangeStroke = new SolidColorBrush(Color.FromRgb(39, 190, 183)),
            StartWidth = 10,
            EndWidth = 10,
            RangeOffset = 0.4
        });
        
        gauge.MainScale = scale;
        this.Content = gauge;
    }
}
```

## Common Patterns

### Thermometer Pattern

Create a thermometer-style gauge with vertical orientation:

```xml
<gauge:SfLinearGauge Orientation="Vertical">
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            Minimum="-20" 
            Maximum="120"
            LabelPostfix="°C"
            ScaleBarSize="15">
            <gauge:LinearScale.Pointers>
                <gauge:LinearPointer 
                    PointerType="BarPointer"
                    Value="37"
                    BarPointerStroke="Red"
                    BarPointerStrokeThickness="15"/>
            </gauge:LinearScale.Pointers>
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

### Progress Indicator Pattern

Show progress with animated pointer:

```xml
<gauge:LinearScale.Pointers>
    <gauge:LinearPointer 
        PointerType="BarPointer"
        Value="{Binding ProgressValue}"
        EnableAnimation="True"
        AnimationDuration="500"
        BarPointerStroke="#36D1DC"
        BarPointerStrokeThickness="12"/>
</gauge:LinearScale.Pointers>
```

### Multi-Zone Value Meter Pattern

Create safe/warning/danger zones:

```xml
<gauge:LinearScale.Ranges>
    <!-- Safe Zone (Green) -->
    <gauge:LinearRange 
        StartValue="0" 
        EndValue="60"
        RangeStroke="Green"
        StartWidth="15"
        EndWidth="15"/>
    
    <!-- Warning Zone (Yellow) -->
    <gauge:LinearRange 
        StartValue="60" 
        EndValue="80"
        RangeStroke="Yellow"
        StartWidth="15"
        EndWidth="15"/>
    
    <!-- Danger Zone (Red) -->
    <gauge:LinearRange 
        StartValue="80" 
        EndValue="100"
        RangeStroke="Red"
        StartWidth="15"
        EndWidth="15"/>
</gauge:LinearScale.Ranges>
```

### Multiple Pointer Comparison Pattern

Compare two values on one scale:

```xml
<gauge:LinearScale.Pointers>
    <!-- Current Value -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="75"
        SymbolPointerStroke="Blue"
        SymbolPointerPosition="Above"/>
    
    <!-- Target Value -->
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="90"
        SymbolPointerStroke="Green"
        SymbolPointerPosition="Below"/>
</gauge:LinearScale.Pointers>
```

## Key Properties

### SfLinearGauge Properties

| Property | Type | Description |
|----------|------|-------------|
| **MainScale** | LinearScale | The primary scale containing all gauge elements |
| **Orientation** | Orientation | Horizontal (default) or Vertical layout |

### LinearScale Properties

| Property | Type | Description |
|----------|------|-------------|
| **Minimum** | double | Minimum value of the scale (default: 0) |
| **Maximum** | double | Maximum value of the scale (default: 100) |
| **Interval** | double | Spacing between major ticks and labels |
| **ScaleBarStroke** | Brush | Scale bar background color |
| **ScaleBarSize** | double | Scale bar thickness (height in horizontal) |
| **ScaleBarLength** | double | Scale bar length (width in horizontal) |
| **ScaleDirection** | LinearScaleDirection | Forward or Backward |
| **Pointers** | ObservableCollection | Collection of LinearPointer objects |
| **Ranges** | ObservableCollection | Collection of LinearRange objects |
| **LabelStroke** | Brush | Label text color |
| **LabelPosition** | LinearLabelsPosition | Above or Below scale |
| **LabelPrefix** | string | Text to prepend to labels |
| **LabelPostfix** | string | Text to append to labels |
| **MajorTickStroke** | Brush | Major tick color |
| **MajorTickSize** | double | Major tick length |
| **MinorTickStroke** | Brush | Minor tick color |
| **MinorTickSize** | double | Minor tick length |
| **MinorTicksPerInterval** | int | Number of minor ticks between major ticks |
| **TickPosition** | LinearTicksPosition | Above, Below, or Cross |

### LinearPointer Properties

| Property | Type | Description |
|----------|------|-------------|
| **PointerType** | LinearPointerType | BarPointer or SymbolPointer |
| **Value** | double | The pointer's value on the scale |
| **BarPointerStroke** | Brush | Bar pointer color |
| **BarPointerStrokeThickness** | double | Bar pointer width |
| **SymbolPointerStroke** | Brush | Symbol pointer color |
| **SymbolPointerHeight** | double | Symbol pointer height |
| **SymbolPointerWidth** | double | Symbol pointer width |
| **SymbolPointerPosition** | LinearSymbolPointersPosition | Above, Below, or Cross |
| **EnableAnimation** | bool | Enable smooth pointer transitions |
| **AnimationDuration** | int | Animation duration in milliseconds |

### LinearRange Properties

| Property | Type | Description |
|----------|------|-------------|
| **StartValue** | double | Range beginning value |
| **EndValue** | double | Range ending value |
| **RangeStroke** | Brush | Range color |
| **StartWidth** | double | Range width at start |
| **EndWidth** | double | Range width at end (for tapered ranges) |
| **RangeOffset** | double | Distance from scale bar |
| **RangeOpacity** | double | Range transparency (0.0 to 1.0) |

## Common Use Cases

### Temperature Monitoring
Display temperature readings with color-coded ranges for normal, warning, and critical temperatures. Use vertical orientation for thermometer-style visualization.

### Performance Metrics
Track KPIs, CPU usage, memory consumption, or other performance indicators with bar pointers showing current values and ranges indicating acceptable thresholds.

### Progress Tracking
Show completion percentage for tasks, downloads, or processes with animated bar pointers that smoothly transition as values update.

### Measurement Displays
Create virtual instruments for pressure gauges, speedometers, voltmeters, or any measurement that benefits from linear scale visualization.

### Value Comparison
Use multiple pointers to compare actual vs. target values, current vs. previous values, or multiple metrics on a single scale.

### Dashboard Widgets
Integrate linear gauges into dashboards as compact, visually appealing widgets for at-a-glance data monitoring.

## API Documentation

**Online API Reference:**
- [SfLinearGauge Class](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.SfLinearGauge.html)
- [LinearScale Class](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.LinearScale.html)
- [LinearPointer Class](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.LinearPointer.html)
- [LinearRange Class](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.LinearRange.html)
- [Syncfusion.UI.Xaml.Gauges Namespace](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Gauges.html)


---

**Next Steps:** Navigate to the reference files above based on your implementation needs. Start with **getting-started.md** for initial setup, then explore specific features as required.
