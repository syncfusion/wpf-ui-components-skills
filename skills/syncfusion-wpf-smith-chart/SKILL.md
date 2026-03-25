---
name: syncfusion-wpf-smith-chart
description: Implement and configure Syncfusion WPF SmithChart (SfSmithChart) component for visualizing impedance and admittance in transmission line applications. Use this when working with high-frequency circuit analysis, RF engineering, or telecommunications data. This skill covers series configuration, dual-axis systems, data markers, legends, appearance customization, and interactive features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF SmithCharts

## When to Use This Skill

Use this skill **immediately** when the user needs to:

- Implement a **Syncfusion WPF SmithChart** (SfSmithChart) component
- Visualize **impedance or admittance** data for transmission lines
- Create charts for **high-frequency circuit applications**
- Plot data with **resistance (horizontal) and reactance (radial) axes**
- Add **line series** with data markers and labels
- Configure **dual-axis systems** (resistance and reactance)
- Customize chart **appearance, palettes, and themes**
- Implement **interactive features** like tooltips
- Switch between **Impedance and Admittance** rendering types
- Work with **legends** for multiple series visualization
- Handle **transmission line data visualization**

## Component Overview

**SfSmithChart** is a specialized data visualization component for high-frequency circuit applications. It plots transmission line parameters using two sets of circles representing normalized resistance and reactance values.

**Key Capabilities:**
- **Dual-Axis System**: Horizontal (Resistance) and Radial (Reactance) axes
- **Line Series Visualization**: Plot data with customizable line series
- **Data Markers**: Multiple marker shapes with custom templates
- **Data Labels**: Automatic label positioning with collision detection
- **Interactive Legend**: Toggle series visibility, multiple positioning options
- **Two Rendering Modes**: Impedance (default) and Admittance
- **Rich Customization**: Palettes, colors, gridlines, axis styling
- **Tooltips**: Interactive tooltips with custom templates
- **Animation Support**: Smooth series animations

## Documentation and Navigation Guide

### Getting Started and Installation

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**Read this when the user needs to:**
- Install and set up the SmithChart component
- Create a SmithChart from XAML or code-behind
- Add assembly references (Syncfusion.SfSmithChart.WPF)
- Initialize the chart with basic configuration
- Create data models for transmission line data
- Add header, axes, series, and legend
- Apply themes (SfSkinManager)
- See complete working examples

### Series Configuration

📄 **Read:** [references/series-configuration.md](references/series-configuration.md)

**Read this when the user needs to:**
- Configure LineSeries for data visualization
- Set up data binding (ItemsSource, ResistancePath, ReactancePath)
- Customize series appearance (Interior, StrokeThickness)
- Enable series animation (EnableAnimation, AnimationDuration)
- Control series visibility programmatically
- Handle multiple series in one chart
- Customize data plotting order (ArrangeByIndex)
- Apply series-specific palettes

### Axes Configuration

📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)

**Read this when the user needs to:**
- Configure Horizontal Axis (Resistance) properties
- Configure Radial Axis (Reactance) properties
- Customize major and minor gridlines
- Control gridline visibility and count
- Style axis lines and gridlines
- Position axis labels (Inside/Outside)
- Handle label intersection and overlap
- Respond to axis label events (LabelCreated)

### Data Markers and Labels

📄 **Read:** [references/data-markers-and-labels.md](references/data-markers-and-labels.md)

**Read this when the user needs to:**
- Add markers to data points (ShowMarker, MarkerType)
- Use built-in marker shapes (Circle, Rectangle, Diamond, etc.)
- Customize marker size, color, and stroke
- Create custom marker templates
- Enable data labels (ShowLabel)
- Handle automatic label positioning and collision
- Style data labels (LabelStyle)
- Create custom label templates (LabelTemplate)

### Legend and Appearance

📄 **Read:** [references/legend-and-appearance.md](references/legend-and-appearance.md)

**Read this when the user needs to:**
- Add and configure chart legend
- Position legend (Docked or Floating)
- Customize legend icons and appearance
- Toggle series visibility from legend
- Apply color palettes (Metro, BlueChrome, etc.)
- Customize chart area (Background, BorderBrush)
- Set series-specific palettes
- Control circle radius
- Get chart properties (AreaBounds, CenterPoint, Radius)
- Style legend items (FontSize, colors, spacing)

### Rendering Types and User Interactions

📄 **Read:** [references/rendering-and-interactions.md](references/rendering-and-interactions.md)

**Read this when the user needs to:**
- Understand Impedance vs Admittance rendering
- Switch rendering types (RenderingType property)
- Enable tooltips for data points (ShowToolTip)
- Configure tooltip duration
- Create custom tooltip templates
- Implement interactive features

## Quick Start Example

Here's a minimal example to create a functional SmithChart with transmission line data:

### XAML Approach

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.SmithChart;assembly=Syncfusion.SfSmithChart.WPF">
    <Grid>
        <syncfusion:SfSmithChart Header="Impedance Transmission" Height="400" Width="500">
            <!-- LineSeries with data binding -->
            <syncfusion:LineSeries ResistancePath="Resistance" 
                                   ReactancePath="Reactance" 
                                   ItemsSource="{Binding Data}" 
                                   Label="TransmissionLine"
                                   ShowMarker="True">
            </syncfusion:LineSeries>
            
            <!-- Horizontal (Resistance) Axis -->
            <syncfusion:SfSmithChart.HorizontalAxis>
                <syncfusion:HorizontalAxis FontSize="11"/>
            </syncfusion:SfSmithChart.HorizontalAxis>
            
            <!-- Radial (Reactance) Axis -->
            <syncfusion:SfSmithChart.RadialAxis>
                <syncfusion:RadialAxis FontSize="11"/>
            </syncfusion:SfSmithChart.RadialAxis>
            
            <!-- Legend -->
            <syncfusion:SfSmithChart.Legend>
                <syncfusion:SmithChartLegend/>
            </syncfusion:SfSmithChart.Legend>
        </syncfusion:SfSmithChart>
    </Grid>
</Window>
```

### Code-Behind Approach

```csharp
using Syncfusion.UI.Xaml.SmithChart;

// Create SmithChart
SfSmithChart chart = new SfSmithChart();
chart.Header = "Impedance Transmission";

// Configure axes
chart.HorizontalAxis = new HorizontalAxis { FontSize = 11 };
chart.RadialAxis = new RadialAxis { FontSize = 11 };

// Add series
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "TransmissionLine",
    ShowMarker = true
};
chart.Series.Add(series);

// Add legend
chart.Legend = new SmithChartLegend();

// Add to layout
this.Content = chart;
```

### Data Model

```csharp
public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}

// Sample data
ObservableCollection<TransmissionData> Data = new ObservableCollection<TransmissionData>
{
    new TransmissionData { Resistance = 0, Reactance = 0.05 },
    new TransmissionData { Resistance = 0.3, Reactance = 0.1 },
    new TransmissionData { Resistance = 1.0, Reactance = 0.4 },
    new TransmissionData { Resistance = 2.0, Reactance = 0.5 }
};
```

## Common Patterns

### Pattern 1: Multiple Series Comparison

```csharp
// Add multiple series for comparison
LineSeries series1 = new LineSeries
{
    ItemsSource = Data1,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Transmission-1",
    ShowMarker = true
};

LineSeries series2 = new LineSeries
{
    ItemsSource = Data2,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Transmission-2",
    ShowMarker = true,
    Interior = new SolidColorBrush(Colors.Orange)
};

chart.Series.Add(series1);
chart.Series.Add(series2);
```

### Pattern 2: Enhanced Visual Feedback

```csharp
// Series with markers, labels, and tooltip
LineSeries series = new LineSeries
{
    ShowMarker = true,
    MarkerType = MarkerType.Circle,
    MarkerHeight = 10,
    MarkerWidth = 10,
    ShowToolTip = true,
    ToolTipDuration = TimeSpan.FromSeconds(3),
    StrokeThickness = 2,
    EnableAnimation = true,
    AnimationDuration = TimeSpan.FromSeconds(1)
};

series.DataLabel.ShowLabel = true;
```

### Pattern 3: Custom Appearance

```csharp
// Apply custom palette and styling
chart.ColorModel = new SmithChartColorModel
{
    Palette = ColorPalette.BlueChrome
};

chart.Background = new SolidColorBrush(Colors.WhiteSmoke);
chart.ChartAreaBackground = new SolidColorBrush(Colors.White);
chart.ChartAreaBorderBrush = new SolidColorBrush(Colors.Gray);
chart.ChartAreaBorderThickness = new Thickness(1);
chart.Radius = 0.9; // 90% of plot area
```

### Pattern 4: Admittance Rendering

```csharp
// Switch to Admittance mode for different visualization
chart.RenderingType = RenderingType.Admittance;
// Data is now rendered from left to right
```

## Key Properties

### SfSmithChart Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `Header` | object | Chart title/header text |
| `HorizontalAxis` | HorizontalAxis | Resistance axis configuration |
| `RadialAxis` | RadialAxis | Reactance axis configuration |
| `Series` | ObservableCollection | Collection of chart series |
| `Legend` | SmithChartLegend | Legend configuration |
| `RenderingType` | RenderingType | Impedance or Admittance mode |
| `Radius` | double | Chart circle radius (0.1 to 1) |
| `ColorModel` | SmithChartColorModel | Palette and color settings |
| `ChartAreaBackground` | Brush | Chart plotting area background |

### LineSeries Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | IEnumerable | Data source for the series |
| `ResistancePath` | string | Property path for resistance values |
| `ReactancePath` | string | Property path for reactance values |
| `Label` | string | Series name (shows in legend) |
| `Interior` | Brush | Line color |
| `StrokeThickness` | double | Line thickness |
| `ShowMarker` | bool | Display markers at data points |
| `MarkerType` | MarkerType | Marker shape (Circle, Rectangle, etc.) |
| `ShowToolTip` | bool | Enable tooltips on hover |
| `EnableAnimation` | bool | Animate series on load |
| `IsSeriesVisible` | bool | Show/hide series |

### Axis Common Properties

| Property | Type | Description |
|----------|------|-------------|
| `ShowMajorGridlines` | bool | Display major gridlines |
| `ShowMinorGridlines` | bool | Display minor gridlines |
| `MinorGridlinesCount` | int | Number of minor gridlines (default: 8) |
| `MajorGridlineStyle` | Style | Style for major gridlines |
| `MinorGridlineStyle` | Style | Style for minor gridlines |
| `ShowAxisLine` | bool | Display axis line |
| `AxisLineStyle` | Style | Style for axis line |
| `LabelPlacement` | LabelPlacement | Inside or Outside positioning |
| `LabelIntersectAction` | LabelIntersectActions | Handle overlapping labels (Hide/None) |

## Common Use Cases

### Use Case 1: Basic Transmission Line Visualization
**Goal**: Display impedance data for a transmission line

**Approach**: 
- Create data model with Resistance and Reactance properties
- Set up basic SmithChart with header
- Add LineSeries with data binding
- Configure both axes
- Add legend for reference

**Reference**: See [getting-started.md](references/getting-started.md)

### Use Case 2: Comparing Multiple Transmission Lines
**Goal**: Compare impedance characteristics of different transmission lines

**Approach**:
- Add multiple LineSeries with different ItemsSource
- Assign unique Label to each series
- Use different Interior colors for distinction
- Enable legend for series identification
- Consider using markers for clarity

**Reference**: See [series-configuration.md](references/series-configuration.md) and [legend-and-appearance.md](references/legend-and-appearance.md)

### Use Case 3: Detailed Data Point Analysis
**Goal**: Provide detailed information about each data point

**Approach**:
- Enable ShowMarker for visual data points
- Enable data labels (DataLabel.ShowLabel)
- Configure tooltips (ShowToolTip)
- Customize marker and label appearance
- Consider custom templates for specialized information

**Reference**: See [data-markers-and-labels.md](references/data-markers-and-labels.md) and [rendering-and-interactions.md](references/rendering-and-interactions.md)

### Use Case 4: Admittance Analysis
**Goal**: Visualize admittance instead of impedance

**Approach**:
- Set RenderingType to Admittance
- Data automatically renders left to right
- Axis labels adjust automatically
- All other features remain available

**Reference**: See [rendering-and-interactions.md](references/rendering-and-interactions.md)

### Use Case 5: Professional Report Visualization
**Goal**: Create publication-ready SmithChart with custom branding

**Approach**:
- Apply custom ColorModel palette
- Customize chart area and background colors
- Style axes with custom gridline styles
- Configure legend with custom icons
- Adjust Radius for optimal sizing
- Apply themes using SfSkinManager

**Reference**: See [legend-and-appearance.md](references/legend-and-appearance.md) and [getting-started.md](references/getting-started.md)

## Edge Cases and Troubleshooting

### Assembly Reference Issues
- Ensure Syncfusion.SfSmithChart.WPF assembly is properly referenced
- Check .NET Framework version compatibility
- Verify xmlns namespace declaration in XAML

### Data Not Displaying
- Verify ItemsSource is properly bound
- Check ResistancePath and ReactancePath match property names exactly
- Ensure DataContext is set for data binding
- Verify data collection is not empty

### Overlapping Labels
- Use LabelIntersectAction property to control overlap behavior
- Labels automatically reposition when ShowLabel is true
- Consider reducing data points or adjusting label templates

### Performance with Large Datasets
- Use ArrangeByIndex = false for sorted data (default behavior)
- Enable animation selectively for better initial load
- Consider data sampling for extremely large datasets

## Summary

The Syncfusion WPF SmithChart is a specialized component for high-frequency circuit applications, particularly for visualizing transmission line impedance and admittance data. It features a dual-axis system, rich customization options, and interactive features suitable for engineering and scientific applications. Always start with the getting-started reference for initial setup, then explore specific features as needed for your use case.