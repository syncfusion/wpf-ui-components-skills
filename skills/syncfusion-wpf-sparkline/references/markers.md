# Markers

## Table of Contents
- [Overview](#overview)
- [Enabling Markers](#enabling-markers)
- [Marker Customization with Template Selector](#marker-customization-with-template-selector)
- [Custom Marker Templates](#custom-marker-templates)
- [Marker Sizing](#marker-sizing)
- [Customizing Markers for Specific Data Points](#customizing-markers-for-specific-data-points)
- [Best Practices](#best-practices)

## Overview

Markers are visual indicators placed at data points on Line and Area sparklines. They help users identify individual values and highlight special points like first, last, highest, lowest, and negative values.

**Marker Availability:**
- ✓ **Line Sparkline** (SfLineSparkline)
- ✓ **Area Sparkline** (SfAreaSparkline)
- ✗ **Column Sparkline** (not applicable - columns themselves serve as indicators)
- ✗ **WinLoss Sparkline** (not applicable - binary visualization)

Markers can be customized through:
1. **MarkerVisibility** - Show/hide all markers
2. **MarkerTemplateSelector** - Customize special point markers
3. **MarkerTemplate** - Custom marker shape and appearance
4. **MarkerHeight/MarkerWidth** - Control marker size

## Enabling Markers

By default, markers are hidden. Enable them using the `MarkerVisibility` property.

### XAML Implementation

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.Windows;

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    MarkerVisibility = Visibility.Visible
};
```

### Result

All data points will display default circular markers along the sparkline.

## Marker Customization with Template Selector

The `MarkerTemplateSelector` allows you to differentiate special data points with unique colors and sizes. You can customize:
- **FirstPointBrush** - First data point color
- **LastPointBrush** - Last data point color
- **HighPointBrush** - Highest value point color
- **LowPointBrush** - Lowest value point color
- **NegativePointBrush** - Negative value points color
- **MarkerHeight** - Marker height for special points
- **MarkerWidth** - Marker width for special points

### XAML Implementation

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible"
    Padding="20">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            HighPointBrush="Red"
            LowPointBrush="Blue"
            NegativePointBrush="Orange"
            MarkerHeight="15" 
            MarkerWidth="15"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    MarkerVisibility = Visibility.Visible,
    Padding = new Thickness(20)
};

MarkerTemplateSelector selector = new MarkerTemplateSelector()
{
    FirstPointBrush = new SolidColorBrush(Colors.Yellow),
    LastPointBrush = new SolidColorBrush(Colors.Yellow),
    HighPointBrush = new SolidColorBrush(Colors.Red),
    LowPointBrush = new SolidColorBrush(Colors.Blue),
    NegativePointBrush = new SolidColorBrush(Colors.Orange),
    MarkerHeight = 15,
    MarkerWidth = 15
};

sparkline.MarkerTemplateSelector = selector;
```

### Special Point Identification

The MarkerTemplateSelector automatically identifies:
- **First Point:** Very first data point in the series
- **Last Point:** Very last data point in the series
- **High Point:** Data point with maximum Y-value
- **Low Point:** Data point with minimum Y-value
- **Negative Points:** Any data point with Y-value < 0

**Note:** If multiple points share the same high or low value, only the first occurrence is highlighted.

### Common Color Schemes

**Traffic Light Pattern:**
```xml
<syncfusion:MarkerTemplateSelector 
    HighPointBrush="Green"
    LowPointBrush="Red"
    NegativePointBrush="Red"
    FirstPointBrush="Blue"
    LastPointBrush="Blue"
    MarkerHeight="12" 
    MarkerWidth="12"/>
```

**Heatmap Pattern:**
```xml
<syncfusion:MarkerTemplateSelector 
    HighPointBrush="#FF0000"
    LowPointBrush="#0000FF"
    NegativePointBrush="#FFA500"
    MarkerHeight="10" 
    MarkerWidth="10"/>
```

## Custom Marker Templates

For complete control over marker appearance, define a custom `MarkerTemplate` using XAML DataTemplate.

### Creating Custom Marker Shapes

```xml
<syncfusion:SfLineSparkline 
    Interior="#4a4a4a"  
    BorderBrush="DarkGray"
    MarkerVisibility="Visible"   
    BorderThickness="1"
    ItemsSource="{Binding UsersList}"  
    YBindingPath="NoOfUsers">
    
    <syncfusion:SfLineSparkline.Resources>
        <DataTemplate x:Key="markerTemplate">
            <Grid>
                <!-- Outer circle -->
                <Ellipse Height="15" Width="15" 
                         Fill="LightGoldenrodYellow"
                         Stroke="Black" 
                         StrokeDashArray="1,1" 
                         StrokeThickness="1"/>
                
                <!-- Inner circle -->
                <Ellipse Height="12" Width="12" 
                         Fill="Blue" 
                         Stroke="Black"   
                         StrokeDashArray="1,1" 
                         StrokeThickness="1"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfLineSparkline.Resources>
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            MarkerTemplate="{StaticResource markerTemplate}"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    MarkerVisibility = Visibility.Visible,
    Interior = new SolidColorBrush(Color.FromRgb(0x4a, 0x4a, 0x4a)),
    BorderBrush = new SolidColorBrush(Colors.DarkGray),
    BorderThickness = new Thickness(1)
};

// Define marker template in resources
DataTemplate markerTemplate = sparkline.Resources["markerTemplate"] as DataTemplate;

MarkerTemplateSelector selector = new MarkerTemplateSelector()
{
    MarkerTemplate = markerTemplate
};

sparkline.MarkerTemplateSelector = selector;
```

### Custom Marker Shape Examples

**Square Markers:**
```xml
<DataTemplate x:Key="squareMarker">
    <Rectangle Height="10" Width="10" 
               Fill="Red" 
               Stroke="White" 
               StrokeThickness="1"/>
</DataTemplate>
```

**Diamond Markers:**
```xml
<DataTemplate x:Key="diamondMarker">
    <Grid>
        <Rectangle Height="12" Width="12" 
                   Fill="Purple" 
                   RenderTransformOrigin="0.5,0.5">
            <Rectangle.RenderTransform>
                <RotateTransform Angle="45"/>
            </Rectangle.RenderTransform>
        </Rectangle>
    </Grid>
</DataTemplate>
```

**Star Markers:**
```xml
<DataTemplate x:Key="starMarker">
    <Path Data="M 0,5 L 3,9 L 8,9 L 4,12 L 6,16 L 0,13 L -6,16 L -4,12 L -8,9 L -3,9 Z" 
          Fill="Gold" 
          Stroke="Orange" 
          StrokeThickness="1"/>
</DataTemplate>
```

## Marker Sizing

Control marker size independently from special point identification using `MarkerHeight` and `MarkerWidth` properties.

### Uniform Sizing

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            MarkerHeight="8" 
            MarkerWidth="8"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

### Varying Sizes for Special Points

```xml
<syncfusion:MarkerTemplateSelector 
    FirstPointBrush="Yellow" 
    LastPointBrush="Yellow"
    HighPointBrush="Red"
    MarkerHeight="15"
    MarkerWidth="15">
    <!-- Only special points get 15x15 size -->
</syncfusion:MarkerTemplateSelector>
```

**Note:** Standard (non-special) markers use default size. To control all marker sizes, use custom MarkerTemplate.

## Customizing Markers for Specific Data Points

For advanced scenarios where you need different markers for specific data values (not just first/last/high/low), inherit from `MarkerTemplateSelector` and override the `SelectTemplate` method.

### Step 1: Create Custom Selector Class

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.Windows;
using System.Windows.Controls;

public class CustomMarkersTemplateSelector : MarkerTemplateSelector
{
    protected override DataTemplate SelectTemplate(double x, double y)
    {
        // Highlight the maximum Y value with custom template
        if (y == MaximumY)
        {
            DataTemplate markerTemplate = Application.Current.Resources["markerTemplate"] as DataTemplate;
            return markerTemplate;
        }
        else
        {
            // Use default marker for other points
            return base.SelectTemplate(x, y);
        }
    }
}
```

### Step 2: Define Custom Marker Template

```xml
<Application.Resources>
    <DataTemplate x:Key="markerTemplate">
        <Grid>
            <Ellipse Height="20" Width="20" 
                     Fill="Gold" 
                     Stroke="DarkGoldenrod" 
                     StrokeThickness="2"/>
            <TextBlock Text="★" 
                       FontSize="12" 
                       HorizontalAlignment="Center" 
                       VerticalAlignment="Center"
                       Foreground="White"/>
        </Grid>
    </DataTemplate>
</Application.Resources>
```

### Step 3: Apply Custom Selector

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <local:CustomMarkersTemplateSelector 
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

### Advanced Custom Logic Examples

**Highlight values above threshold:**
```csharp
public class ThresholdMarkerSelector : MarkerTemplateSelector
{
    public double Threshold { get; set; } = 5000;
    
    protected override DataTemplate SelectTemplate(double x, double y)
    {
        if (y > Threshold)
        {
            return Application.Current.Resources["aboveThresholdMarker"] as DataTemplate;
        }
        return base.SelectTemplate(x, y);
    }
}
```

**Highlight specific X-values:**
```csharp
public class SpecificPointMarkerSelector : MarkerTemplateSelector
{
    protected override DataTemplate SelectTemplate(double x, double y)
    {
        // Highlight every 5th data point
        if (x % 5 == 0)
        {
            return Application.Current.Resources["specialMarker"] as DataTemplate;
        }
        return base.SelectTemplate(x, y);
    }
}
```

**Conditional coloring based on Y-value ranges:**
```csharp
public class RangeBasedMarkerSelector : MarkerTemplateSelector
{
    protected override DataTemplate SelectTemplate(double x, double y)
    {
        if (y > 8000)
            return Application.Current.Resources["excellentMarker"] as DataTemplate;
        else if (y > 5000)
            return Application.Current.Resources["goodMarker"] as DataTemplate;
        else if (y > 2000)
            return Application.Current.Resources["averageMarker"] as DataTemplate;
        else
            return Application.Current.Resources["poorMarker"] as DataTemplate;
    }
}
```

## Best Practices

### When to Use Markers

**Use markers when:**
- Users need to identify individual data points
- Highlighting exceptional values (first, last, high, low)
- Data point count is low to moderate (< 20 points)
- Interactive inspection is needed (combine with track ball)

**Avoid markers when:**
- Too many data points (> 30) - clutters visualization
- Focus is on overall trend, not individual values
- Using Column or WinLoss sparklines (not supported)

### Marker Size Guidelines

- **Small datasets (5-10 points):** 10-15px markers
- **Medium datasets (10-20 points):** 8-12px markers
- **Large datasets (20-30 points):** 6-8px markers
- **Dense datasets (30+ points):** Consider disabling markers

### Color Selection

**High Contrast:**
- Use contrasting colors against sparkline interior
- Ensure visibility on both light and dark themes

**Semantic Colors:**
- Red/Orange for negative or low values
- Green for positive or high values
- Blue/Yellow for first/last markers

**Accessibility:**
- Don't rely solely on color for differentiation
- Vary marker sizes or shapes for critical distinctions
- Ensure sufficient contrast ratios (WCAG guidelines)

### Performance Optimization

**For dense data:**
- Disable markers and use track ball for inspection
- Or highlight only special points (first, last, high, low)
- Avoid complex custom marker templates with many elements

**Example - Special Points Only:**
```xml
<syncfusion:MarkerTemplateSelector 
    FirstPointBrush="Yellow" 
    LastPointBrush="Yellow"
    HighPointBrush="Red"
    LowPointBrush="Blue"
    MarkerHeight="12" 
    MarkerWidth="12"/>
<!-- Regular points have no markers, only special ones -->
```

## Combining Markers with Other Features

### Markers + Track Ball

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible"
    ShowTrackBall="True">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            HighPointBrush="Red"
            LowPointBrush="Blue"
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Use case:** Permanent markers show high/low, track ball shows all values on hover.

### Markers + Range Bands

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    MarkerVisibility="Visible"
    BandRangeStart="3000"
    BandRangeEnd="6000"
    RangeBandBrush="LightGreen">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Use case:** Range band shows acceptable values, markers highlight start/end points.

## Next Steps

- **[Track Ball](track-ball.md)** - Add interactive data inspection
- **[Segment Customization](segment-customization.md)** - Color-code sparkline segments
- **[Range Band](range-band.md)** - Highlight Y-axis ranges
