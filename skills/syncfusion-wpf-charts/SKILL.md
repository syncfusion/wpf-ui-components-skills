---
name: syncfusion-wpf-charts
description: Comprehensive guide for implementing Syncfusion WPF Charts (SfChart) in Windows Presentation Foundation applications. Use this when working with WPF charts, data visualization, or interactive charting features. This skill covers chart series types, axes configuration, data binding, appearance customization, financial charts, technical indicators, and interactive features like zooming and tooltips for WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Charts (SfChart)

Comprehensive guide for implementing the Syncfusion® WPF Charts (SfChart) control in Windows Presentation Foundation applications. This skill covers everything from basic chart setup to advanced features like technical indicators, interactive behaviors, and performance optimization.

## When to Use This Skill

Use this skill when you need to:
- **Create data visualizations** with charts in WPF applications
- **Implement any of 38+ series types** (Line, Column, Bar, Area, Pie, Financial, etc.)
- **Configure chart axes** (Category, Numeric, DateTime, Logarithmic, TimeSpan,DateTimeCategory)
- **Bind data to charts** using ItemsSource and binding paths
- **Add interactive features** (zooming, panning, tooltips, crosshair, trackball)
- **Customize chart appearance** (colors, themes, styles, templates)
- **Implement financial charts** with technical indicators
- **Optimize chart performance** for large datasets
- **Export or print charts** from WPF applications
- **Troubleshoot chart-related issues** in WPF

## Component Overview

**SfChart** is a powerful data visualization control that provides:

- **38+ Chart Types:** Line, Column, Bar, Area, Bubble, Scatter, Pie, Doughnut, Funnel, Pyramid, Financial (Candle, OHLC), Radar, Polar, Range, Spline, and more
- **High Performance:** Render large datasets (millions of points) in milliseconds
- **Interactive Features:** Zooming, panning, tooltips, crosshair, trackball, selection, visual data editing
- **Flexible Data Binding:** MVVM support, complex data structures, dynamic updates
- **Multiple Axes:** Stack and span multiple axes for complex visualizations
- **Technical Indicators:** 10+ indicators for financial analysis (SMA, EMA, RSI, MACD, etc.)
- **Rich Customization:** Themes, styles, templates, colors, fonts, and layouts
- **Advanced Features:** Annotations, striplines, legends, data labels, markers
- **Export & Print:** Save charts as images, print, serialize/deserialize state

**Namespace:** `Syncfusion.UI.Xaml.Charts`  
**Assembly:** `Syncfusion.SfChart.WPF`

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for basic chart implementation:
- Adding chart references and NuGet packages
- Basic chart initialization with axes
- Adding your first series
- Simple data binding setup
- Running your first chart application
- Minimal working example

### Series Types and Data Visualization

📄 **Read:** [references/series-types.md](references/series-types.md)

Learn about all 38+ available series types:
- Line, Column, Bar, and Area series
- Bubble and Scatter charts
- Financial charts (Candle, OHLC, HiLo, HiLoOpenClose)
- Pie and Doughnut charts
- Funnel and Pyramid charts
- Radar and Polar charts
- Range series (RangeArea, RangeColumn)
- Spline variations (Spline, SplineArea)
- Stacking series (StackingColumn, StackingArea, etc.)
- When to use each series type
- Combining multiple series

### Axes Configuration

📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)

Configure chart axes for proper data representation:
- Axis types (CategoryAxis, NumericalAxis, DateTimeAxis, LogarithmicAxis)
- Primary and Secondary axes
- Multiple axes support and positioning
- Axis labels, formatting, and rotation
- Axis range, intervals, and auto-range
- Axis line customization
- Axis crossing and inversing
- Strip lines on axes

### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

Bind data to chart series:
- ItemsSource property configuration
- XBindingPath and YBindingPath setup
- Binding to collections and lists
- Complex property binding
- Dynamic data updates
- MVVM pattern implementation
- ObservableCollection support
- Multiple series data binding

### Legends

📄 **Read:** [references/legends.md](references/legends.md)

Configure chart legends:
- Legend positioning and docking
- Legend visibility and toggling
- Legend icons and customization
- Legend item templates
- Custom legend items
- Legend orientation

### Interactive Features

📄 **Read:** [references/interactive-features.md](references/interactive-features.md)

Add user interaction capabilities:
- Tooltip configuration and customization
- Crosshair behavior and styling
- Trackball functionality
- Zooming and panning (pinch, selection, mouse wheel)
- Selection modes (single, multiple, series)
- Visual data editing
- Resizable scrollbar
- Touch support

### Adornments (Data Labels and Markers)

📄 **Read:** [references/adornments.md](references/adornments.md)

Display additional information on data points:
- Data labels configuration
- Label content and formatting
- Smart label positioning
- Data markers (symbols)
- Marker customization
- Label rotation and alignment
- Custom adornment templates

### Appearance and Styling

📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

Customize chart visual appearance:
- Chart area styling (background, border)
- Series colors and palettes
- Chart header customization
- Plot area configuration
- Themes and visual styles
- Font customization
- Watermark support
- Gradient and solid color fills

### Annotations

📄 **Read:** [references/annotations.md](references/annotations.md)

Add custom annotations to charts:
- Text annotations
- Shape annotations (line, rectangle, ellipse)
- Image annotations
- Custom annotation templates
- Positioning and alignment
- Annotation interaction
- Dynamic annotations

### Striplines

📄 **Read:** [references/striplines.md](references/striplines.md)

Highlight axis regions with striplines:
- Adding striplines to axes
- Stripline styling and colors
- Multiple striplines
- Stripline labels
- Recurring striplines
- Stripline positioning

### Technical Indicators

📄 **Read:** [references/technical-indicators.md](references/technical-indicators.md)

Implement financial analysis indicators:
- 10+ technical indicators (SMA, EMA, RSI, MACD, Bollinger Bands, etc.)
- Indicator configuration and parameters
- Combining indicators with financial series
- Multiple indicators on same chart
- Custom indicator styling
- Indicator signals

### Performance Optimization

📄 **Read:** [references/performance-optimization.md](references/performance-optimization.md)

Optimize charts for large datasets:
- Fast series types (FastLine, FastColumn, FastBar, etc.)
- Performance best practices
- Rendering optimization
- Memory management
- Data sampling techniques
- Segment rendering
- Animation performance

### Printing and Exporting

📄 **Read:** [references/printing-exporting.md](references/printing-exporting.md)

Export and print charts:
- Print functionality
- Export to image formats (PNG, JPEG, BMP)
- Export configuration options
- Chart serialization
- Saving and loading chart state
- Export quality settings

## Quick Start Example

Here's a minimal example to create a basic column chart:

```xml
<Window x:Class="ChartDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF"
        Title="Sales Chart" Height="450" Width="800">
    
    <syncfusion:SfChart>
        <!-- Primary Axis (X-Axis) -->
        <syncfusion:SfChart.PrimaryAxis>
            <syncfusion:CategoryAxis Header="Month"/>
        </syncfusion:SfChart.PrimaryAxis>
        
        <!-- Secondary Axis (Y-Axis) -->
        <syncfusion:SfChart.SecondaryAxis>
            <syncfusion:NumericalAxis Header="Sales ($)"/>
        </syncfusion:SfChart.SecondaryAxis>
        
        <!-- Column Series -->
        <syncfusion:ColumnSeries ItemsSource="{Binding SalesData}"
                                 XBindingPath="Month"
                                 YBindingPath="Sales"
                                 Label="Monthly Sales">
            <syncfusion:ColumnSeries.AdornmentsInfo>
                <syncfusion:ChartAdornmentInfo ShowLabel="True"/>
            </syncfusion:ColumnSeries.AdornmentsInfo>
        </syncfusion:ColumnSeries>
    </syncfusion:SfChart>
</Window>
```

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.Collections.ObjectModel;

namespace ChartDemo
{
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
                new SalesData { Month = "May", Sales = 67000 },
                new SalesData { Month = "Jun", Sales = 72000 }
            };
        }
    }
}
```

## Common Patterns

### Pattern 1: Multiple Series Chart

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- First Series -->
    <syncfusion:LineSeries ItemsSource="{Binding Data2023}"
                           XBindingPath="Month"
                           YBindingPath="Value"
                           Label="2023"/>
    
    <!-- Second Series -->
    <syncfusion:LineSeries ItemsSource="{Binding Data2024}"
                           XBindingPath="Month"
                           YBindingPath="Value"
                           Label="2024"/>
</syncfusion:SfChart>
```

### Pattern 2: Chart with Tooltip and Zooming

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:DateTimeAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- Enable Tooltip -->
    <syncfusion:LineSeries ItemsSource="{Binding Data}"
                           XBindingPath="Date"
                           YBindingPath="Value"
                           ShowTooltip="True"/>
    
    <!-- Enable Zooming and Panning -->
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartZoomPanBehavior EnableMouseWheelZooming="True"
                                          EnablePanning="True"
                                          EnableSelectionZooming="True"/>
    </syncfusion:SfChart.Behaviors>
</syncfusion:SfChart>
```

### Pattern 3: Financial Chart with Technical Indicator

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:DateTimeAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis/>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- Candlestick Series -->
    <syncfusion:CandleSeries ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"/>
    
    <!-- Simple Moving Average Indicator -->
    <syncfusion:SimpleAverageIndicator ItemsSource="{Binding StockData}"
                             XBindingPath="Date"
                             High="High"
                             Low="Low"
                             Open="Open"
                             Close="Close"
                             Period="14"/>
</syncfusion:SfChart>
```

## Key Properties and Classes

### SfChart Class
- **PrimaryAxis** - The horizontal (X) axis
- **SecondaryAxis** - The vertical (Y) axis
- **Series** - Collection of chart series
- **Legend** - Chart legend configuration
- **Header** - Chart title
- **Behaviors** - Interactive behaviors collection

### ChartSeries Base Properties
- **ItemsSource** - Data source for the series
- **XBindingPath** - Property path for X values
- **YBindingPath** - Property path for Y values
- **Label** - Series name (shown in legend)
- **ShowTooltip** - Enable/disable tooltips
- **Palette** - Color palette for the series

### Common Axis Properties
- **Header** - Axis title
- **Interval** - Space between axis labels
- **Minimum/Maximum** - Axis range limits
- **LabelFormat** - Format string for axis labels
- **ShowGridLines** - Show/hide gridlines

## Common Use Cases

### Use Case 1: Sales Dashboard
Display monthly sales trends with multiple years comparison, tooltips, and data labels.

**When to use:**
- Business reporting dashboards
- Financial performance tracking
- Trend analysis over time

**Recommended series:** Line, Column, or Area series with legends and tooltips.

### Use Case 2: Stock Market Analysis
Show stock prices with candlestick charts and technical indicators (moving averages, RSI, MACD).

**When to use:**
- Financial applications
- Trading platforms
- Investment analysis tools

**Recommended series:** CandleSeries, HiLoOpenCloseSeries with technical indicators.

### Use Case 3: Data Distribution Visualization
Display data distribution and proportions using pie or doughnut charts.

**When to use:**
- Market share analysis
- Budget allocation display
- Survey result visualization

**Recommended series:** PieSeries, DoughnutSeries with data labels.

### Use Case 4: Scientific Data Plotting
Plot large datasets with high-performance series and zooming capabilities.

**When to use:**
- Scientific applications
- Engineering data visualization
- Real-time monitoring

**Recommended series:** FastLineSeries, ScatterSeries with zooming and panning.

### Use Case 5: Comparison Charts
Compare multiple datasets side-by-side with grouped or stacked columns.

**When to use:**
- Product comparisons
- Period-over-period analysis
- Category comparisons

**Recommended series:** ColumnSeries, StackingColumnSeries, GroupedColumnSeries.

## Tips and Best Practices

1. **Choose the Right Series Type:** Select series types based on your data and visualization goals. Line for trends, Column for comparisons, Pie for proportions, Financial for stock data.

2. **Use Fast Series for Large Data:** When dealing with thousands of data points, use Fast series types (FastLineSeries, FastColumnSeries) for better performance.

3. **Enable Interactive Features:** Add tooltips, zooming, and trackball to enhance user experience and data exploration.

4. **Data Binding with MVVM:** Use ObservableCollection and INotifyPropertyChanged for dynamic data updates.

5. **Customize Appearance:** Apply themes and custom styles to match your application's design language.

6. **Optimize Performance:** Disable animations, use segment rendering, and implement data sampling for very large datasets.

7. **Multiple Axes for Different Scales:** Use multiple axes when displaying data with different value ranges or units.

8. **Add Context with Annotations:** Use annotations and striplines to highlight important values, thresholds, or regions.

## Related Components

- **Range Selector:** [../implementing-range-selectors/](../implementing-range-selectors/) - Select date/value ranges for chart data filtering

## Troubleshooting

**Chart not displaying:**
- Verify Syncfusion.SfChart.WPF assembly reference
- Check data binding and ItemsSource
- Ensure axes are properly configured
- Verify license key for licensed applications

**Performance issues:**
- Switch to Fast series types for large datasets
- Disable animations if not needed
- Use data sampling or aggregation
- Enable segment rendering

**Data not updating:**
- Use ObservableCollection for ItemsSource
- Implement INotifyPropertyChanged in data models
- Check binding paths (XBindingPath, YBindingPath)

## Next Steps

1. **Read Getting Started** to set up your first chart
2. **Explore Series Types** to find the right visualization
3. **Configure Data Binding** to connect your data
4. **Add Interactive Features** for better user experience
5. **Customize Appearance** to match your application design

For detailed information on any topic, navigate to the appropriate reference file listed in the Documentation and Navigation Guide section above.
