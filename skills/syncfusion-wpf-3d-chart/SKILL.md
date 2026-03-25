---
name: syncfusion-wpf-3d-chart
description: Comprehensive guide for implementing Syncfusion WPF 3D Charts (SfChart3D) control for 3D data visualization in Windows Presentation Foundation applications. Use this when working with 3D charts, 3D visualization, or depth axis configurations. This skill covers 3D series types (column, pie, bar), Manhattan charts, 3D rotation, X/Y/Z coordinate plotting, and depth axis configuration for WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF 3D Charts

Comprehensive guide for implementing the Syncfusion® WPF 3D Charts (SfChart3D) control to create interactive 3D data visualizations in Windows Presentation Foundation applications. This skill covers setup, configuration, series types, axes, adornments, appearance customization, and interactive features for 3D charts.

## When to Use This Skill

Use this skill when you need to:
- **Create 3D visualizations** with Column, Bar, Line, Area, Scatter, or Circular series
- **Plot data in 3D space** using X, Y, and Z coordinates with Depth Axis
- **Implement interactive 3D charts** with rotation, tilt, and perspective controls
- **Configure 3D series** with 11+ chart types (Column, Bar, Line, Scatter, Area, Stacking variants, Pie, Doughnut)
- **Set up multiple axes** (Numerical, Category, DateTime, TimeSpan, Logarithmic)
- **Build Manhattan charts** with multiple series on depth axis
- **Add data adornments** with 3D labels, markers, and positioning
- **Customize appearance** with palettes, colors, and themes
- **Enable user interaction** with dynamic rotation, selection, and exploding segments
- **Optimize performance** for large 3D datasets

## Component Overview

The **SfChart3D** control provides comprehensive 3D charting capabilities for WPF applications, allowing you to visualize two-dimensional data in a three-dimensional view with full rotation support.

**Key Capabilities:**
- **11+ Series Types:** Column, Bar, Line, Scatter, Area, Stacking variants, Pie, Doughnut
- **3D Visualization:** Rotation, tilt, perspective angle for optimal data viewing
- **Depth Axis (Z-Axis):** True 3D plotting with X, Y, Z coordinates
- **Manhattan Charts:** Multiple series visualization on depth axis
- **Multiple Axis Types:** Numerical, Category, DateTime, TimeSpan, Logarithmic
- **Interactive Features:** Dynamic rotation, selection, exploding segments
- **Data Adornments:** 3D labels, markers, positioning
- **Rich Customization:** Palettes, colors, themes, styling
- **Data Binding:** Full MVVM support with ObservableCollection
- **Animations:** Series loading and data update animations

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references (Syncfusion.SfChart.WPF)
- Adding SfChart3D control to WPF projects
- XAML namespace configuration
- Setting up PrimaryAxis and SecondaryAxis
- Basic data binding with ItemsSource, XBindingPath, YBindingPath
- Creating your first 3D chart with complete example
- Adding titles, legends, tooltips, and data labels
- ViewModel setup with ObservableCollection

### Series Types
📄 **Read:** [references/series-types.md](references/series-types.md)
- Overview of 11+ series types and common properties
- Column charts (ColumnSeries3D) with Spacing and SegmentSpacing
- Bar charts (BarSeries3D)
- Line charts (LineSeries3D)
- Scatter charts (ScatterSeries3D) with size configuration
- Area charts (AreaSeries3D)
- Stacking charts (StackingColumnSeries3D, StackingColumn100Series3D, StackingBarSeries3D, StackingBar100Series3D)
- Circular charts (PieSeries3D, DoughnutSeries3D)
- Circular chart features (coefficients, semi-circular, dynamic explode)
- Series selection and Interior property
- When to use each series type

### Axes Configuration
📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)
- Axis overview (PrimaryAxis, SecondaryAxis, DepthAxis)
- NumericalAxis3D for numeric data
- CategoryAxis3D for categorical data
- DateTimeAxis3D for time-series data
- TimeSpanAxis3D for time duration data
- LogarithmicAxis3D for exponential data
- Depth Axis (Z-Axis) setup with ZBindingPath
- True 3D plotting with X, Y, Z coordinates
- Manhattan Charts with multiple series on depth axis
- Axis customization (headers, intervals, ranges)
- Troubleshooting axis configuration issues

### Data Adornments
📄 **Read:** [references/adornments.md](references/adornments.md)
- ChartAdornmentInfo3D configuration
- Enabling data labels with ShowLabel property
- Data markers (types, shapes, customization)
- Label positioning options
- Label formatting and templates
- Adornment styling and appearance
- Performance considerations for large datasets

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)
- Predefined color palettes (Metro, AutumnBrights, FloraHues, Pineapple, TomotoSpectrum, RedChrome, PurpleChrome, BlueChrome, GreenChrome, Elite, LightCandy, SandyBeach)
- Applying palettes to series vs segments
- Custom palettes with ColorModel and CustomBrushes
- SegmentColorPath for data-driven coloring
- Series Interior property
- Theme integration
- Styling best practices

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)
- 3D rotation and navigation (EnableRotation, Rotation, Tilt, PerspectiveAngle)
- Dynamic rotation with mouse and touch devices
- Selection support (segment and series selection)
- Circular series explode (ExplodeOnMouseClick, ExplodeIndex, ExplodeAll)
- Tooltip configuration (ShowTooltip property)
- Animation support for series loading
- User interaction patterns and best practices

## Quick Start Example

Here's a minimal example to create a 3D Column chart:

**XAML:**
```xml
<Window xmlns:chart="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <chart:SfChart3D Header="Sales Data 3D" Width="500" Height="500">
        
        <!-- Primary Axis (X-Axis) -->
        <chart:SfChart3D.PrimaryAxis>
            <chart:CategoryAxis3D Header="Month"/>
        </chart:SfChart3D.PrimaryAxis>
        
        <!-- Secondary Axis (Y-Axis) -->
        <chart:SfChart3D.SecondaryAxis>
            <chart:NumericalAxis3D Header="Sales"/>
        </chart:SfChart3D.SecondaryAxis>
        
        <!-- Legend -->
        <chart:SfChart3D.Legend>
            <chart:ChartLegend/>
        </chart:SfChart3D.Legend>
        
        <!-- 3D Column Series -->
        <chart:ColumnSeries3D 
            Label="Revenue" 
            ItemsSource="{Binding SalesData}" 
            XBindingPath="Month" 
            YBindingPath="Sales"
            ShowTooltip="True">
            
            <!-- Data Labels -->
            <chart:ColumnSeries3D.AdornmentsInfo>
                <chart:ChartAdornmentInfo3D ShowLabel="True"/>
            </chart:ColumnSeries3D.AdornmentsInfo>
        </chart:ColumnSeries3D>
        
    </chart:SfChart3D>
    
</Window>
```

**C# ViewModel:**
```csharp
using System.Collections.ObjectModel;

public class SalesData
{
    public string Month { get; set; }
    public double Sales { get; set; }
}

public class ViewModel
{
    public ObservableCollection<SalesData> SalesData { get; set; }
    
    public ViewModel()
    {
        SalesData = new ObservableCollection<SalesData>
        {
            new SalesData { Month = "Jan", Sales = 35000 },
            new SalesData { Month = "Feb", Sales = 48000 },
            new SalesData { Month = "Mar", Sales = 42000 },
            new SalesData { Month = "Apr", Sales = 56000 },
            new SalesData { Month = "May", Sales = 63000 },
            new SalesData { Month = "Jun", Sales = 71000 }
        };
    }
}
```

## Common Patterns

### Pattern 1: 3D Chart with Rotation
Enable interactive rotation for better data exploration:

```xml
<chart:SfChart3D EnableRotation="True" Rotation="45" Tilt="-30" PerspectiveAngle="90">
    <!-- Axes and series configuration -->
</chart:SfChart3D>
```

### Pattern 2: Manhattan Chart (Multiple Series on Depth Axis)
Visualize multiple series across depth axis:

```xml
<chart:SfChart3D Rotation="43">
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Depth Axis (Z-Axis) -->
    <chart:SfChart3D.DepthAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.DepthAxis>
    
    <chart:LineSeries3D Label="Product A" ItemsSource="{Binding Data1}" 
                        XBindingPath="X" YBindingPath="Y"/>
    <chart:LineSeries3D Label="Product B" ItemsSource="{Binding Data2}" 
                        XBindingPath="X" YBindingPath="Y"/>
</chart:SfChart3D>
```

### Pattern 3: True 3D Plotting with X, Y, Z Coordinates
Plot data using all three dimensions:

```xml
<chart:SfChart3D>
    <chart:SfChart3D.PrimaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Depth Axis for Z coordinates -->
    <chart:SfChart3D.DepthAxis>
        <chart:NumericalAxis3D Interval="1"/>
    </chart:SfChart3D.DepthAxis>
    
    <chart:ColumnSeries3D 
        ItemsSource="{Binding Data3D}" 
        XBindingPath="XValue" 
        YBindingPath="YValue" 
        ZBindingPath="ZValue"/>
</chart:SfChart3D>
```

### Pattern 4: 3D Pie Chart with Explode Effect
Create interactive circular charts:

```xml
<chart:PieSeries3D 
    ItemsSource="{Binding CategoryData}" 
    XBindingPath="Category" 
    YBindingPath="Value"
    ExplodeOnMouseClick="True"
    CircleCoefficient="0.8">
</chart:PieSeries3D>
```

### Pattern 5: Custom Color Palette
Apply custom colors to your 3D chart:

```xml
<chart:SfChart3D Palette="Custom">
    <chart:SfChart3D.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="#6366F1"/>
                <SolidColorBrush Color="#8B5CF6"/>
                <SolidColorBrush Color="#EC4899"/>
                <SolidColorBrush Color="#F59E0B"/>
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:SfChart3D.ColorModel>
    
    <!-- Series configuration -->
</chart:SfChart3D>
```

## Key Properties Reference

### SfChart3D Control
- **EnableRotation** - Enable mouse/touch rotation
- **Rotation** - Initial rotation angle (0-360)
- **Tilt** - Vertical tilt angle
- **PerspectiveAngle** - 3D perspective (0-180)
- **Depth** - Chart depth for 3D effect
- **PrimaryAxis** - X-axis configuration
- **SecondaryAxis** - Y-axis configuration
- **DepthAxis** - Z-axis configuration (optional)
- **Header** - Chart title
- **Legend** - Legend configuration
- **Palette** - Color palette selection

### Series Common Properties
- **ItemsSource** - Data collection binding
- **XBindingPath** - Property for X values
- **YBindingPath** - Property for Y values
- **ZBindingPath** - Property for Z values (depth axis)
- **Label** - Series name for legend
- **ShowTooltip** - Enable tooltips
- **Interior** - Series fill color
- **AdornmentsInfo** - Data label configuration

### Spacing Properties (Column/Bar)
- **Spacing** - Space between segments (0-1)
- **SegmentSpacing** - Space between series (0-1)

### Circular Series Properties
- **CircleCoefficient** - Inner circle size (Pie)
- **DoughnutCoefficient** - Inner circle size (Doughnut)
- **StartAngle** - Starting angle for semi-circular
- **EndAngle** - Ending angle for semi-circular
- **ExplodeOnMouseClick** - Enable click-to-explode
- **ExplodeIndex** - Index of segment to explode
- **ExplodeAll** - Explode all segments

## Common Use Cases

### Use Case 1: Financial Data Visualization
Visualize quarterly revenue across multiple products in 3D:
- Use **ColumnSeries3D** with **DepthAxis** for product comparison
- Apply **CategoryAxis3D** for quarters and products
- Enable **Rotation** for different viewing angles
- Add **data labels** to show exact values

### Use Case 2: Scientific Data Analysis
Plot scientific measurements with X, Y, Z coordinates:
- Use **ScatterSeries3D** with **ZBindingPath** for 3D points
- Apply **NumericalAxis3D** for all three axes
- Enable **dynamic rotation** for data exploration
- Use **custom markers** for data point visualization

### Use Case 3: Market Share Dashboard
Display market share distribution in 3D:
- Use **PieSeries3D** or **DoughnutSeries3D** for proportional data
- Apply **custom palettes** for brand colors
- Enable **ExplodeOnMouseClick** for interactivity
- Add **tooltips** for detailed information

### Use Case 4: Time-Series Comparison
Compare multiple datasets over time in 3D:
- Use **LineSeries3D** with **Manhattan chart** pattern
- Apply **DateTimeAxis3D** for time-based X-axis
- Use **Label** property for series identification
- Enable **Legend** for series navigation

### Use Case 5: Performance Metrics Dashboard
Visualize stacked performance metrics:
- Use **StackingColumnSeries3D** for cumulative data
- Apply **custom colors** via **ColorModel**
- Add **data labels** for metric values
- Enable **tooltips** for drill-down details

## Troubleshooting Tips

**Issue:** Chart not displaying
- Verify assembly reference: `Syncfusion.SfChart.WPF`
- Check XAML namespace declaration
- Ensure ItemsSource has data
- Verify XBindingPath and YBindingPath match property names

**Issue:** 3D effect not visible
- Set **Depth** property (default: 100)
- Configure **Rotation** and **Tilt** angles
- Adjust **PerspectiveAngle** for depth perception

**Issue:** Rotation not working
- Set **EnableRotation="True"**
- Ensure chart has sufficient space (Width/Height)
- Check for mouse event conflicts

**Issue:** Depth axis not showing
- Add **DepthAxis** configuration to SfChart3D
- Set **ZBindingPath** on series
- Verify series type supports depth axis (Column, Bar, Line, Scatter, StackingColumn, StackingBar)

For detailed troubleshooting and advanced scenarios, refer to the specific reference documentation above.
