---
name: syncfusion-wpf-range-selector
description: Comprehensive guide for implementing Syncfusion WPF Range Selector (SfDateTimeRangeNavigator) for time-bound data visualization with interactive scrolling, zooming, and range selection. Use this when working with range selectors, date-time range navigation, or time-bound data visualization. This skill covers interactive data range selection, chart range zooming, and dashboard time navigation features for large time-based datasets in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Range Selectors

Guide for implementing the Syncfusion® WPF Range Selector (SfDateTimeRangeNavigator) control for time-bound data visualization with interactive scrolling and navigation through large periods of time.

## When to Use This Skill

Use this skill when you need to:
- **Navigate large time-based datasets** with scrolling and zooming
- **Select specific date-time ranges** from large data periods
- **Create interactive dashboards** with range-based data filtering
- **Implement chart zooming** with time range selection
- **Display hierarchical time labels** (year/month, quarter/month, etc.)
- **Combine with other visualizations** (charts, grids) for data exploration
- **Enable data range selection** with visual feedback and tooltips

The Range Selector control excels at scenarios where users need to explore and select portions of large time-series data, providing an intuitive scrollbar interface with smart label calculation.

## Component Overview

The SfDateTimeRangeNavigator is a specialized control for navigating time-bound data with these key capabilities:

**Core Features:**
- **Interactive Navigation:** Scrolling and zooming through large datasets
- **Dual-Level Labels:** Hierarchical time display (higher/lower level bars)
- **Smart Labels:** Automatic label calculation when zooming
- **Range Selection:** Resizable scrollbar for precise region selection
- **Content Integration:** Embed charts, sparklines, or any UI element
- **Data Binding:** Standard WPF IEnumerable support with property binding

**Common Scenarios:**
- Financial data exploration (stock prices, trading volumes)
- Historical data analysis with date range filtering
- Dashboard range controls for synchronized views
- Time-series data selection with visual preview

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Understand the visual structure (higher/lower bars, content, scrollbar)
- Add assembly references and install packages
- Initialize the SfDateTimeRangeNavigator control
- Set ItemsSource and bind to date-time data
- Add content inside the navigator (charts, sparklines)
- Create range navigator from code-behind
- Apply themes to the control

This reference covers complete setup from installation to first working implementation.

### Interactivity and Events
📄 **Read:** [references/interactivity.md](references/interactivity.md)

When you need to:
- Implement zooming and panning functionality
- Bind ZoomPosition and ZoomFactor to chart axes
- Retrieve selected data from chosen ranges
- Handle ValueChanged events for range updates
- Customize thumb appearance (left/right sliders)
- Configure SymbolTemplate and LineStyle for thumbs
- Customize higher and lower bar styles
- Enable deferred updates for performance
- Handle LowerBarLabelsCreated/UpperBarLabelsCreated events

This reference covers all interactive features and event handling.

### Label Customization
📄 **Read:** [references/label-customization.md](references/label-customization.md)

When you need to:
- Set interval types (Year, Quarter, Month, Week, Day, Hour)
- Customize label formats with LabelFormatters
- Style selected and unselected labels
- Position labels inside or outside bars
- Control label bar visibility
- Configure RangePadding (None, Round)
- Customize grid lines and tick lines
- Understand smart label calculation behavior

This reference covers complete label and interval customization.

### Tooltip Configuration
📄 **Read:** [references/tooltip-support.md](references/tooltip-support.md)

When you need to:
- Enable tooltips on slider thumbs
- Format tooltip date-time display
- Customize left and right tooltip templates
- Display selected range values with precision

This reference covers tooltip display and customization.

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)

When you encounter:
- Assembly reference problems
- Data binding issues or ItemsSource errors
- Content not displaying properly
- Zoom binding not synchronizing with charts
- Performance issues with large datasets
- Theme application problems
- License-related errors

This reference provides solutions for common issues.

## Quick Start Example

### Basic Range Navigator with Chart

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    <Grid>
        <!-- Main Chart -->
        <syncfusion:SfChart x:Name="Chart" Margin="10">
            <syncfusion:SfChart.PrimaryAxis>
                <syncfusion:DateTimeAxis 
                    ZoomPosition="{Binding ElementName=RangeNavigator, Path=ZoomPosition, Mode=TwoWay}"
                    ZoomFactor="{Binding ElementName=RangeNavigator, Path=ZoomFactor, Mode=TwoWay}"/>
            </syncfusion:SfChart.PrimaryAxis>
            
            <syncfusion:SfChart.SecondaryAxis>
                <syncfusion:NumericalAxis/>
            </syncfusion:SfChart.SecondaryAxis>
            
            <syncfusion:LineSeries ItemsSource="{Binding Data}" 
                                   XBindingPath="Date" 
                                   YBindingPath="Value"/>
        </syncfusion:SfChart>
        
        <!-- Range Navigator -->
        <syncfusion:SfDateTimeRangeNavigator x:Name="RangeNavigator"
                                             ItemsSource="{Binding Data}"
                                             XBindingPath="Date"
                                             VerticalAlignment="Bottom"
                                             Height="100"
                                             Margin="10">
            <syncfusion:SfDateTimeRangeNavigator.Content>
                <syncfusion:SfLineSparkline ItemsSource="{Binding Data}"
                                            YBindingPath="Value"/>
            </syncfusion:SfDateTimeRangeNavigator.Content>
        </syncfusion:SfDateTimeRangeNavigator>
    </Grid>
</Window>
```

### ViewModel

```csharp
public class DataViewModel : INotifyPropertyChanged
{
    public ObservableCollection<DataPoint> Data { get; set; }
    
    public DataViewModel()
    {
        Data = new ObservableCollection<DataPoint>();
        DateTime startDate = DateTime.Today.AddYears(-1);
        
        for (int i = 0; i < 365; i++)
        {
            Data.Add(new DataPoint 
            { 
                Date = startDate.AddDays(i), 
                Value = 50 + (i % 30) 
            });
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

public class DataPoint
{
    public DateTime Date { get; set; }
    public double Value { get; set; }
}
```

## Common Patterns

### Pattern 1: Chart Synchronization with Range Navigator

Bind the navigator's zoom properties to chart axes for synchronized navigation:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:DateTimeAxis 
            ZoomPosition="{Binding ElementName=RangeNavigator, Path=ZoomPosition, Mode=TwoWay}"
            ZoomFactor="{Binding ElementName=RangeNavigator, Path=ZoomFactor, Mode=TwoWay}"/>
    </syncfusion:SfChart.PrimaryAxis>
</syncfusion:SfChart>

<syncfusion:SfDateTimeRangeNavigator x:Name="RangeNavigator"
                                     ItemsSource="{Binding Data}"
                                     XBindingPath="Date"/>
```

### Pattern 2: Selected Data Retrieval

Access the data within the selected range:

```csharp
private void OnRangeChanged(object sender, EventArgs e)
{
    var navigator = sender as SfDateTimeRangeNavigator;
    var selectedData = navigator.SelectedData;
    
    // Process selected data
    ProcessDataInRange(selectedData);
}
```

### Pattern 3: Custom Interval Configuration

Set specific interval types for label bars:

```xml
<syncfusion:SfDateTimeRangeNavigator ItemsSource="{Binding Data}" 
                                     XBindingPath="Date">
    <syncfusion:SfDateTimeRangeNavigator.Intervals>
        <syncfusion:Interval IntervalType="Year"/>
        <syncfusion:Interval IntervalType="Month"/>
    </syncfusion:SfDateTimeRangeNavigator.Intervals>
</syncfusion:SfDateTimeRangeNavigator>
```

### Pattern 4: Deferred Update for Performance

Enable deferred updates for large datasets:

```xml
<syncfusion:SfDateTimeRangeNavigator EnableDeferredUpdate="True"
                                     DeferredUpdateDelay="500"
                                     ItemsSource="{Binding LargeDataset}"
                                     XBindingPath="Date"/>
```

## Key Properties

### Data Binding
- **ItemsSource** - The data collection (must implement IEnumerable)
- **XBindingPath** - Property name for date-time values
- **Content** - UI element to display inside navigator (chart, sparkline, etc.)

### Range and Zoom
- **ZoomPosition** - Current zoom position (0.0 to 1.0)
- **ZoomFactor** - Current zoom level (0.0 to 1.0)
- **Minimum** - Starting range of the navigator
- **Maximum** - Ending range of the navigator
- **ViewRangeStart** - Start value of selected range (read-only)
- **ViewRangeEnd** - End value of selected range (read-only)
- **SelectedData** - Collection of data within selected range (read-only)

### Intervals and Labels
- **Intervals** - Collection of interval configurations for label bars
- **HigherLevelBarStyle** - Style for upper label bar
- **LowerLevelBarStyle** - Style for lower label bar
- **RangePadding** - Padding type (None, Round)

### Interactivity
- **EnableDeferredUpdate** - Delays updates during scrolling/panning
- **DeferredUpdateDelay** - Delay in milliseconds for deferred updates
- **ShowGridLines** - Show/hide gridlines inside content
- **ShowToolTip** - Enable/disable tooltips on thumbs
- **ToolTipLabelFormat** - Date-time format for tooltips

### Customization
- **LeftThumbStyle** - Style for left slider thumb
- **RightThumbStyle** - Style for right slider thumb
- **LeftToolTipTemplate** - Template for left tooltip
- **RightToolTipTemplate** - Template for right tooltip

## Common Use Cases

### Financial Dashboard
Display stock price charts with range selection for detailed analysis of specific time periods. Users can scroll through years of data and zoom into specific trading days.

### System Monitoring
Monitor system metrics over time with the ability to select and analyze specific time ranges when anomalies occurred.

### Sales Analytics
Explore sales data across multiple years, quarters, and months with hierarchical time labels showing patterns at different time scales.

### Weather Data Visualization
Navigate historical weather data with the ability to select specific date ranges for comparison and analysis.

### IoT Sensor Data
Browse sensor readings over extended periods and select specific time windows for detailed investigation.

## Integration with Other Controls

The Range Selector works seamlessly with:
- **SfChart** - Synchronized zooming and panning
- **SfLineSparkline** - Lightweight data preview in content area
- **DataGrid** - Filter grid data based on selected range
- **Custom Visualizations** - Any UI element can be embedded as content

## Performance Considerations

For optimal performance with large datasets:
- Enable **EnableDeferredUpdate** to reduce update frequency
- Use **SfLineSparkline** instead of full charts in content area for lightweight rendering
- Consider data virtualization for extremely large datasets
- Set appropriate **DeferredUpdateDelay** based on data size

## Related Skills

- **[Implementing Charts](../implementing-charts/)** - For detailed chart implementation
- **Parent Library:** [Implementing Syncfusion WPF Components](../../)

---

**Next Steps:** Navigate to the appropriate reference file above based on your current implementation needs.
