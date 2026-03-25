# Axes Configuration in WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Basic Axis Setup](#basic-axis-setup)
- [Axis Properties](#axis-properties)
- [Header Customization](#header-customization)
- [Label Customization](#label-customization)
- [Range Configuration](#range-configuration)
- [Gridlines and Ticks](#gridlines-and-ticks)
- [Styling Axes](#styling-axes)
- [Complete Examples](#complete-examples)

## Overview

SurfaceAxis is used to locate a data point inside the surface area. In a surface chart, three axes are required to locate data points:
- **X-Axis** - Horizontal axis (width)
- **Y-Axis** - Vertical axis (height/value)
- **Z-Axis** - Depth axis

If you do not define axes explicitly, the chart automatically creates default axes with default property values.

## Basic Axis Setup

### XAML Definition

```xaml
<chart:SfSurfaceChart>
    <chart:SfSurfaceChart.XAxis>
        <chart:SurfaceAxis />
    </chart:SfSurfaceChart.XAxis>
    
    <chart:SfSurfaceChart.YAxis>
        <chart:SurfaceAxis />
    </chart:SfSurfaceChart.YAxis>
    
    <chart:SfSurfaceChart.ZAxis>
        <chart:SurfaceAxis />
    </chart:SfSurfaceChart.ZAxis>
</chart:SfSurfaceChart>
```

### Code-Behind Definition

```csharp
SfSurfaceChart surface = new SfSurfaceChart();

surface.XAxis = new SurfaceAxis();
surface.YAxis = new SurfaceAxis();
surface.ZAxis = new SurfaceAxis();
```

**Note:** Axes are optional. If not defined, default axes are created automatically with sensible defaults.

## Axis Properties

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| **Header** | object | Content for the axis header/title |
| **HeaderTemplate** | DataTemplate | Template for customizing axis header |
| **LabelTemplate** | DataTemplate | Template for customizing axis labels |
| **LabelFormat** | string | Format string for axis labels (e.g., "0.0", "N2") |
| **Minimum** | double | Minimum value for the axis range |
| **Maximum** | double | Maximum value for the axis range |
| **RangePadding** | NumericalPadding | Padding applied to axis range |
| **EdgeLabelsDrawingMode** | EdgeLabelsDrawingMode | How to position edge labels |
| **Interval** | double | Interval between axis labels |
| **SmallTicksPerInterval** | int | Number of minor ticks between major ticks |
| **TickLineSize** | double | Size of axis tick lines |
| **ShowGridLines** | bool | Whether to display gridlines |
| **GridLineStroke** | Brush | Brush for gridline color |
| **GridLineThickness** | double | Thickness of gridlines |
| **AxisLineStyle** | Style | Style for the axis line |
| **MajorTickLineStyle** | Style | Style for major tick lines |
| **MinorTickLineStyle** | Style | Style for minor tick lines |

## Header Customization

### Simple Text Header

```xaml
<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis Header="X-Axis" />
</chart:SfSurfaceChart.XAxis>

<chart:SfSurfaceChart.YAxis>
    <chart:SurfaceAxis Header="Y-Axis" />
</chart:SfSurfaceChart.YAxis>

<chart:SfSurfaceChart.ZAxis>
    <chart:SurfaceAxis Header="Z-Axis" />
</chart:SfSurfaceChart.ZAxis>
```

### Custom Header Template

```xaml
<Grid.Resources>
    <DataTemplate x:Key="headerTemplate">
        <TextBlock Text="YAxis" 
                   FontFamily="Comic Sans MS"
                   FontSize="14"
                   FontWeight="Bold"
                   Foreground="DarkBlue"/>
    </DataTemplate>
</Grid.Resources>

<chart:SfSurfaceChart.YAxis>
    <chart:SurfaceAxis HeaderTemplate="{StaticResource headerTemplate}" />
</chart:SfSurfaceChart.YAxis>
```

### Code-Behind Header

```csharp
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.Header = "X-Axis";
chart.XAxis = xAxis;

SurfaceAxis yAxis = new SurfaceAxis();
yAxis.Header = "Y-Axis";
yAxis.HeaderTemplate = grid.Resources["headerTemplate"] as DataTemplate;
chart.YAxis = yAxis;
```

## Label Customization

### Label Format

Control the numeric format of axis labels:

```xaml
<chart:SfSurfaceChart.YAxis>
    <chart:SurfaceAxis Header="Y-Axis" LabelFormat="0.0"/>
</chart:SfSurfaceChart.YAxis>
```

**Common Format Strings:**
- `"0.0"` - One decimal place
- `"0.00"` - Two decimal places
- `"N2"` - Number with two decimals and thousand separators
- `"C"` - Currency format
- `"P"` - Percentage format
- `"E2"` - Scientific notation

### Custom Label Template

```xaml
<Grid.Resources>
    <DataTemplate x:Key="labelTemplate">
        <TextBlock Text="{Binding LabelContent}" 
                   Foreground="Red"
                   FontSize="10"
                   FontWeight="Bold"/>
    </DataTemplate>
</Grid.Resources>

<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis LabelTemplate="{StaticResource labelTemplate}" />
</chart:SfSurfaceChart.XAxis>
```

### Code-Behind Label Formatting

```csharp
SurfaceAxis yAxis = new SurfaceAxis();
yAxis.LabelFormat = "0.00";
yAxis.LabelTemplate = grid.Resources["labelTemplate"] as DataTemplate;
chart.YAxis = yAxis;
```

## Range Configuration

### Setting Minimum and Maximum

Explicitly control the axis range:

```xaml
<chart:SfSurfaceChart.YAxis>
    <chart:SurfaceAxis Header="Y-Axis"
                      Minimum="-10"
                      Maximum="10"/>
</chart:SfSurfaceChart.YAxis>
```

```csharp
SurfaceAxis yAxis = new SurfaceAxis();
yAxis.Header = "Y-Axis";
yAxis.Minimum = -10;
yAxis.Maximum = 10;
chart.YAxis = yAxis;
```

### Interval Configuration

Control the spacing between labels:

```xaml
<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis Header="X-Axis"
                      Interval="2"/>
</chart:SfSurfaceChart.XAxis>
```

**Example:**
- If range is 0-10 and Interval is 2, labels appear at: 0, 2, 4, 6, 8, 10
- If range is 0-100 and Interval is 25, labels appear at: 0, 25, 50, 75, 100

### Range Padding

Add padding to the axis range:

```xaml
<chart:SurfaceAxis RangePadding="Additional" />
```

**RangePadding Options:**
- `None` - No padding
- `Additional` - Adds padding based on interval
- `Round` - Rounds to nearest interval
- `Auto` - Automatic padding (default)
- `AppendInterval` - Appended with an additional interval.
- `Normal` - Actual range calculated from given items source and series types.
- `PrependInterval`	- Prepended with an additional interval.
- `RoundEnd` - Visible range end round to nearest interval value.
- `RoundStart` - Visible range start round to nearest interval value.

### Edge Labels Drawing Mode

Control how edge labels are positioned:

```xaml
<chart:SurfaceAxis EdgeLabelsDrawingMode="Shift" />
```

**EdgeLabelsDrawingMode Options:**
- `Center` - Center edge labels on axis
- `Shift` - Shift edge labels inside
- `Hide` - Hide edge labels
- `Fit` - Fit within area

## Gridlines and Ticks

### Gridlines Configuration

```xaml
<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis Header="X-Axis"
                      ShowGridLines="True"
                      GridLineStroke="Red"
                      GridLineThickness="1"/>
</chart:SfSurfaceChart.XAxis>
```

```csharp
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.Header = "X-Axis";
xAxis.ShowGridLines = true;
xAxis.GridLineStroke = new SolidColorBrush(Colors.Red);
xAxis.GridLineThickness = 1;
chart.XAxis = xAxis;
```

### Major and Minor Ticks

```xaml
<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis Header="X-Axis"
                      TickLineSize="5"
                      SmallTicksPerInterval="1"/>
</chart:SfSurfaceChart.XAxis>
```

**SmallTicksPerInterval:**
- `0` - No minor ticks
- `1` - One minor tick between major ticks
- `2` - Two minor ticks between major ticks
- And so on...

```csharp
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.TickLineSize = 5;
xAxis.SmallTicksPerInterval = 1;
chart.XAxis = xAxis;
```

## Styling Axes

### Axis Line Style

```xaml
<Grid.Resources>
    <Style TargetType="Line" x:Key="axisLineStyle">
        <Setter Property="Stroke" Value="Green"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Grid.Resources>

<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis Header="X-Axis"
                      AxisLineStyle="{StaticResource axisLineStyle}"/>
</chart:SfSurfaceChart.XAxis>
```

### Major Tick Line Style

```xaml
<Grid.Resources>
    <Style TargetType="Line" x:Key="majorTickStyle">
        <Setter Property="Stroke" Value="Blue"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Grid.Resources>

<chart:SfSurfaceAxis MajorTickLineStyle="{StaticResource majorTickStyle}" />
```

### Minor Tick Line Style

```xaml
<Grid.Resources>
    <Style TargetType="Line" x:Key="minorTickStyle">
        <Setter Property="Stroke" Value="Gray"/>
        <Setter Property="StrokeThickness" Value="1"/>
    </Style>
</Grid.Resources>

<chart:SurfaceAxis MinorTickLineStyle="{StaticResource minorTickStyle}" />
```

### Code-Behind Styling

```csharp
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.Header = "X-Axis";
xAxis.AxisLineStyle = grid.Resources["axisLineStyle"] as Style;
xAxis.MajorTickLineStyle = grid.Resources["majorTickStyle"] as Style;
xAxis.MinorTickLineStyle = grid.Resources["minorTickStyle"] as Style;
chart.XAxis = xAxis;
```

## Complete Examples

### Example 1: Fully Customized Axes

```xaml
<Grid.Resources>
    <DataTemplate x:Key="labelTemplate">
        <TextBlock Text="{Binding LabelContent}" Foreground="Red"/>
    </DataTemplate>
    
    <DataTemplate x:Key="headerTemplate">
        <TextBlock Text="Y-Axis" FontFamily="Comic Sans MS"/>
    </DataTemplate>
    
    <Style TargetType="Line" x:Key="lineStyle">
        <Setter Property="Stroke" Value="Green"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
</Grid.Resources>

<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}"
                     Type="Surface">
    
    <chart:SfSurfaceChart.XAxis>
        <chart:SurfaceAxis Header="X-Axis"
                          GridLineStroke="Red"
                          GridLineThickness="1"
                          SmallTicksPerInterval="1"
                          AxisLineStyle="{StaticResource lineStyle}"
                          LabelTemplate="{StaticResource labelTemplate}"/>
    </chart:SfSurfaceChart.XAxis>
    
    <chart:SfSurfaceChart.YAxis>
        <chart:SurfaceAxis HeaderTemplate="{StaticResource headerTemplate}"
                          LabelFormat="0.00"
                          Minimum="-5"
                          Maximum="20"/>
    </chart:SfSurfaceChart.YAxis>
    
    <chart:SfSurfaceChart.ZAxis>
        <chart:SurfaceAxis Header="Z-Axis"
                          Interval="2"/>
    </chart:SfSurfaceChart.ZAxis>
</chart:SfSurfaceChart>
```

### Example 2: Code-Behind Complete Setup

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.Surface;
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";

// X-Axis Configuration
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.Header = "X-Axis";
xAxis.GridLineStroke = new SolidColorBrush(Colors.Red);
xAxis.GridLineThickness = 1;
xAxis.SmallTicksPerInterval = 1;
xAxis.AxisLineStyle = grid.Resources["lineStyle"] as Style;
xAxis.LabelTemplate = grid.Resources["labelTemplate"] as DataTemplate;
chart.XAxis = xAxis;

// Y-Axis Configuration
SurfaceAxis yAxis = new SurfaceAxis();
yAxis.Header = "Y-Axis";
yAxis.HeaderTemplate = grid.Resources["headerTemplate"] as DataTemplate;
yAxis.LabelFormat = "0.00";
yAxis.Minimum = -5;
yAxis.Maximum = 20;
chart.YAxis = yAxis;

// Z-Axis Configuration
SurfaceAxis zAxis = new SurfaceAxis();
zAxis.Header = "Z-Axis";
zAxis.Interval = 2;
chart.ZAxis = zAxis;

grid.Children.Add(chart);
```

### Example 3: Scientific Notation for Large Ranges

```xaml
<chart:SfSurfaceChart.YAxis>
    <chart:SurfaceAxis Header="Y-Axis (Scientific)"
                      LabelFormat="E2"
                      Minimum="0"
                      Maximum="1000000"/>
</chart:SfSurfaceChart.YAxis>
```

This displays values like: 0.00E+00, 2.00E+05, 4.00E+05, 6.00E+05, 8.00E+05, 1.00E+06

### Example 4: Hide Gridlines

```xaml
<chart:SfSurfaceChart.XAxis>
    <chart:SurfaceAxis Header="X-Axis" ShowGridLines="False"/>
</chart:SfSurfaceChart.XAxis>
```

## Best Practices

### 1. Use Default Axes When Possible

```csharp
// Simple scenario - let defaults handle it
SfSurfaceChart chart = new SfSurfaceChart();
// No need to define axes explicitly
```

### 2. Label Format for Readability

```csharp
// Match format to data scale
yAxis.LabelFormat = "0.00";  // For values 0-100
yAxis.LabelFormat = "N0";    // For large integers
yAxis.LabelFormat = "E2";    // For scientific data
```

### 3. Set Appropriate Intervals

```csharp
// Calculate interval based on range
double range = maximum - minimum;
double interval = range / 10;  // ~10 labels
axis.Interval = interval;
```

### 4. Consistent Styling Across Axes

```csharp
// Create reusable styles
Style axisStyle = CreateAxisStyle();
xAxis.AxisLineStyle = axisStyle;
yAxis.AxisLineStyle = axisStyle;
zAxis.AxisLineStyle = axisStyle;
```

### 5. Responsive Axis Configuration

```csharp
// Adjust based on data
if (dataRange > 1000)
{
    axis.LabelFormat = "N0";  // No decimals for large numbers
}
else if (dataRange < 1)
{
    axis.LabelFormat = "0.0000";  // More precision for small numbers
}
```

---
