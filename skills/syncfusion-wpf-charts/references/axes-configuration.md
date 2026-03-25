# Axes Configuration

Chart axes are used to locate data points and define the scale of the chart. This guide covers axis types, configuration, and customization.

## Table of Contents
- [Axis Types](#axis-types)
- [Axis Properties](#axis-properties)
- [Multiple Axes](#multiple-axes)
- [Axis Customization](#axis-customization)

## Axis Types

### Category Axis

Used for non-numeric, categorical data (strings, labels).

```xml
<syncfusion:SfChart.PrimaryAxis>
    <syncfusion:CategoryAxis Header="Products"
                             LabelPlacement="BetweenTicks"/>
</syncfusion:SfChart.PrimaryAxis>
```

**Best for:** Product names, categories, months (as text)

### Numerical Axis

Used for numeric values with linear scale.

```xml
<syncfusion:SfChart.SecondaryAxis>
    <syncfusion:NumericalAxis Header="Sales ($)"
                              Minimum="0"
                              Maximum="100000"
                              Interval="20000"/>
</syncfusion:SfChart.SecondaryAxis>
```

**Best for:** Sales figures, quantities, measurements

### DateTime Axis

Used for date and time values with automatic formatting.

```xml
<syncfusion:SfChart.PrimaryAxis>
    <syncfusion:DateTimeAxis Header="Date"
                             IntervalType="Months"
                             Interval="1"
                             LabelFormat="MMM yyyy"/>
</syncfusion:SfChart.PrimaryAxis>
```

**IntervalType options:** Days, Hours, Minutes, Months, Seconds, Years

**Best for:** Time series data, trends over time

### Logarithmic Axis

Used for exponential data with logarithmic scale.

```xml
<syncfusion:SfChart.SecondaryAxis>
    <syncfusion:LogarithmicAxis Header="Log Scale"
                                LogarithmicBase="10"/>
</syncfusion:SfChart.SecondaryAxis>
```

**Best for:** Scientific data, exponential growth, wide value ranges

## Axis Properties

### Header

Set the axis title:

```xml
<syncfusion:NumericalAxis Header="Revenue ($)"/>
```

### Range (Minimum and Maximum)

Define the visible range:

```xml
<syncfusion:NumericalAxis Minimum="0" Maximum="1000"/>
```

**Auto-range:** If not specified, the axis automatically calculates the range based on data.

### Interval

Set the spacing between axis labels:

```xml
<syncfusion:NumericalAxis Interval="100"/>
```

For DateTimeAxis, combine with IntervalType:

```xml
<syncfusion:DateTimeAxis Interval="2" IntervalType="Months"/>
```

### Label Format

Format axis labels:

```xml
<!-- Currency format -->
<syncfusion:NumericalAxis LabelFormat="C"/>

<!-- Percentage -->
<syncfusion:NumericalAxis LabelFormat="P"/>

<!-- Date format -->
<syncfusion:DateTimeAxis LabelFormat="dd/MM/yyyy"/>

<!-- Custom numeric format -->
<syncfusion:NumericalAxis LabelFormat="0.00"/>
```

### Label Rotation

Rotate axis labels to prevent overlap:

```xml
<syncfusion:CategoryAxis LabelRotationAngle="45"/>
```

### Grid Lines

Control grid line visibility:

```xml
<syncfusion:NumericalAxis ShowGridLines="True">
    <syncfusion:NumericalAxis.MajorGridLineStyle>
        <Style TargetType="Line">
            <Setter Property="Stroke" Value="LightGray"/>
            <Setter Property="StrokeDashArray" Value="2,2"/>
        </Style>
    </syncfusion:NumericalAxis.MajorGridLineStyle>
</syncfusion:NumericalAxis>
```

## Multiple Axes

Add additional axes for different data scales or units.

### Adding Secondary Axes

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis x:Name="axis1" Header="Temperature (°C)"/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- Additional Y-Axis -->
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis x:Name="axis2" 
                                  Header="Humidity (%)"
                                  OpposedPosition="True"/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- First series uses axis1 -->
    <syncfusion:LineSeries ItemsSource="{Binding TempData}"
                           XBindingPath="Time"
                           YBindingPath="Value"
                           YAxis="{Binding ElementName=axis1}"/>
    
    <!-- Second series uses axis2 -->
    <syncfusion:LineSeries ItemsSource="{Binding HumidityData}"
                           XBindingPath="Time"
                           YBindingPath="Value"
                           YAxis="{Binding ElementName=axis2}"/>
</syncfusion:SfChart>
```

### Opposed Position

Place axis on the opposite side:

```xml
<syncfusion:NumericalAxis OpposedPosition="True"/>
```

## Axis Customization

### Axis Line Style

Customize the axis line appearance:

```xml
<syncfusion:NumericalAxis>
    <syncfusion:NumericalAxis.AxisLineStyle>
        <Style TargetType="Line">
            <Setter Property="Stroke" Value="DarkBlue"/>
            <Setter Property="StrokeThickness" Value="2"/>
        </Style>
    </syncfusion:NumericalAxis.AxisLineStyle>
</syncfusion:NumericalAxis>
```

### Axis Label Style

Customize axis label appearance:

```xml
<syncfusion:CategoryAxis>
    <syncfusion:CategoryAxis.LabelStyle>
        <syncfusion:LabelStyle FontSize="14" 
                               Foreground="DarkGreen"
                               FontWeight="Bold"/>
    </syncfusion:CategoryAxis.LabelStyle>
</syncfusion:CategoryAxis>
```

### Header Style

Customize the axis header:

```xml
<syncfusion:NumericalAxis Header="Revenue">
    <syncfusion:NumericalAxis.HeaderStyle>
        <syncfusion:LabelStyle FontFamily="Algerian" FontSize="13" Foreground="Black"> 
        </syncfusion:LabelStyle>
    </syncfusion:NumericalAxis.HeaderStyle>
</syncfusion:NumericalAxis>
```

### Inversing Axis

Reverse the axis direction:

```xml
<syncfusion:NumericalAxis IsInversed="True"/>
```

## Edge Labels Positioning

Control how edge labels are drawn:

```xml
<syncfusion:NumericalAxis EdgeLabelsDrawingMode="Shift"/>
```

**EdgeLabelsDrawingMode options:**
- `Center` - Default, labels centered
- `Fit` - Adjust labels to fit within bounds
- `Hide` - Hide edge labels
- `Shift` - Shift labels inside

## Auto Interval Calculation

Let the axis automatically calculate optimal intervals:

```xml
<syncfusion:NumericalAxis EnableAutoIntervalOnZooming="True"/>
```

## Common Axis Configurations

### Time Series Chart

```xml
<syncfusion:SfChart.PrimaryAxis>
    <syncfusion:DateTimeAxis Header="Date"
                             IntervalType="Days"
                             Interval="7"
                             LabelFormat="MMM dd"/>
</syncfusion:SfChart.PrimaryAxis>

<syncfusion:SfChart.SecondaryAxis>
    <syncfusion:NumericalAxis Header="Price ($)"
                              RangePadding="Round"/>
</syncfusion:SfChart.SecondaryAxis>
```

### Percentage Chart

```xml
<syncfusion:SfChart.SecondaryAxis>
    <syncfusion:NumericalAxis Header="Completion"
                              Minimum="0"
                              Maximum="100"
                              Interval="20"
                              LabelFormat="0'%'"/>
</syncfusion:SfChart.SecondaryAxis>
```

### Large Number Formatting

```xml
<syncfusion:NumericalAxis LabelFormat="0,,M"
                          Header="Revenue (Millions)"/>
```

## Key Properties Summary

| Property | Description |
|----------|-------------|
| `Header` | Axis title |
| `Minimum` | Minimum value |
| `Maximum` | Maximum value |
| `Interval` | Label spacing |
| `LabelFormat` | Format string for labels |
| `LabelRotationAngle` | Rotate labels (degrees) |
| `ShowGridLines` | Show/hide grid lines |
| `OpposedPosition` | Position on opposite side |
| `IsInversed` | Reverse axis direction |
| `EdgeLabelsDrawingMode` | Edge label positioning |
| `RangePadding` | Padding around data range |
