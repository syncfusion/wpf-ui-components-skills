---
name: syncfusion-wpf-sparkline
description: Comprehensive guide for implementing Syncfusion WPF Sparkline (SfSparkline) controls in Windows Presentation Foundation applications. Use this when working with sparklines, mini charts, or trend visualization. This skill covers sparkline types (line, column, area, WinLoss), markers, track ball, range bands, axis controls, and segment customization for compact data visualization in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF Sparklines

Comprehensive guide for implementing Syncfusion® WPF Sparkline (SfSparkline) controls in Windows Presentation Foundation applications. Sparklines are compact, highly condensed charts that display data trends without axes or coordinates, perfect for dashboards and inline visualizations.

## When to Use This Skill

Use this skill when you need to:
- **Create sparklines** for compact data visualization in WPF applications
- **Display data trends** without full chart complexity
- **Implement mini charts** for dashboards, tables, or KPI displays
- **Choose sparkline types** (Line, Column, Area, WinLoss)
- **Add markers** to highlight specific data points
- **Enable interactive features** like track ball for data inspection
- **Customize segments** with special point highlighting
- **Configure range bands** to highlight Y-axis ranges
- **Control axis visibility** and positioning
- **Bind data** to sparkline controls
- **Apply themes** and visual customization
- **Troubleshoot** sparkline rendering or data binding issues

This skill covers all four sparkline types and their unique features, from basic implementation to advanced customization.

## Component Overview

The Syncfusion WPF Sparkline (SfSparkline) is a very small chart that presents data trends in a simple, highly condensed format. Unlike traditional charts, sparklines omit axes, labels, and legends to focus purely on the data's shape and pattern.

**Key Capabilities:**
- **4 Sparkline Types:** Line, Column, Area, WinLoss
- **Data Binding:** ObservableCollection, IEnumerable support
- **Markers:** Highlight first, last, high, low, negative points
- **Interactive:** Track ball for mouse hover data inspection
- **Range Bands:** Highlight specific Y-axis ranges
- **Segment Customization:** Color-code special data points
- **Axis Controls:** Show/hide axis with positioning options
- **Animation:** Smooth data updates and transitions
- **Empty Point Support:** Handle missing data gracefully

**Common Use Cases:**
- Dashboard KPI indicators
- Inline trend displays in data grids
- Financial data visualization (stock prices, wins/losses)
- Performance metrics (sales, traffic, conversions)
- Compact reporting interfaces

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this first when implementing sparklines for the first time:
- Installing Syncfusion.SfChart.WPF package
- Adding assembly references to your project
- Importing namespaces in XAML and C#
- Creating basic sparkline controls
- Setting up data models with ObservableCollection
- Binding data with ItemsSource and YBindingPath

### Sparkline Types

📄 **Read:** [references/sparkline-types.md](references/sparkline-types.md)

Read this when choosing or implementing specific sparkline types:
- **Line Sparkline** (SfLineSparkline) - Continuous trend lines
- **Column Sparkline** (SfColumnSparkline) - Discrete rectangular bars
- **Area Sparkline** (SfAreaSparkline) - Filled area under trend line
- **WinLoss Sparkline** (SfWinLossSparkline) - Win/loss/draw indicators
- When to use each sparkline type
- Data structure requirements for each type
- Complete XAML and C# implementation examples

### Markers and Customization

📄 **Read:** [references/markers.md](references/markers.md)

Read this when adding or customizing markers on sparklines:
- Enabling marker visibility (MarkerVisibility property)
- Using MarkerTemplateSelector for special points
- Highlighting first, last, high, low, and negative points
- Custom marker templates (MarkerTemplate)
- Sizing markers (MarkerHeight, MarkerWidth)
- Customizing markers for specific data points
- Advanced marker styling techniques

### Interactive Track Ball

📄 **Read:** [references/track-ball.md](references/track-ball.md)

Read this when implementing interactive data inspection:
- Enabling track ball feature (ShowTrackBall property)
- Customizing track ball appearance (TrackBallStyle, LineStyle)
- Adding labels to track ball display
- Handling OnSparklineMouseMove event
- Displaying data values on mouse hover
- Styling track ball markers and lines
- Creating custom track ball interactions

### Range Bands

📄 **Read:** [references/range-band.md](references/range-band.md)

Read this when highlighting specific Y-axis ranges:
- Configuring range bands (BandRangeStart, BandRangeEnd)
- Setting range band brush colors (RangeBandBrush)
- Use cases for range highlighting (thresholds, targets)
- Visual examples with complete code

### Segment Customization

📄 **Read:** [references/segment-customization.md](references/segment-customization.md)

Read this when customizing segment colors and highlighting:
- Enabling segment highlighting on hover (HighlightSegment)
- Using SegmentTemplateSelector for special segments
- Customizing first, last, high, low point brushes
- Negative point highlighting
- Column and WinLoss segment features
- Line and Area segment coloring
- Complete customization examples

### Axis Controls

📄 **Read:** [references/axis-controls.md](references/axis-controls.md)

Read this when configuring axis visibility and appearance:
- Showing/hiding axis lines (ShowAxis property)
- Positioning axis (AxisOrigin property)
- Styling axis appearance (AxisStyle)
- Custom line styles for axes
- Axis applicability (not available for WinLoss type)
- Complete styling examples

## Quick Start

Here's a minimal example to create a line sparkline:

### XAML Implementation

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <Window.DataContext>
        <local:SparklineViewModel/>
    </Window.DataContext>
    
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding UsersList}" 
        YBindingPath="NoOfUsers"
        Interior="#4a4a4a"/>
        
</Window>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;

// Create sparkline
SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparklineViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    Interior = new SolidColorBrush(Color.FromRgb(0x4a, 0x4a, 0x4a)),
};

// Add to container
this.Content = sparkline;
```

### Data Model

```csharp
public class UserProfile
{
    public DateTime TimeStamp { get; set; }
    public double NoOfUsers { get; set; }
}

public class SparklineViewModel
{
    public ObservableCollection<UserProfile> UsersList { get; set; }
    
    public SparklineViewModel()
    {
        UsersList = new ObservableCollection<UserProfile>();
        DateTime date = DateTime.Today;
        
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(1), NoOfUsers = 3000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(2), NoOfUsers = 5000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(3), NoOfUsers = -3000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(4), NoOfUsers = -4000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(5), NoOfUsers = 2000 });
        UsersList.Add(new UserProfile { TimeStamp = date.AddHours(6), NoOfUsers = 3000 });
    }
}
```

## Common Patterns

### Pattern 1: Column Sparkline for Discrete Values

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding SalesData}" 
    YBindingPath="Amount"
    HighlightSegment="True"/>
```

### Pattern 2: WinLoss Sparkline for Binary Outcomes

```xml
<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding MatchResults}" 
    YBindingPath="Result"/>
```

### Pattern 3: Line Sparkline with Markers and Track Ball

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding TrendData}" 
    YBindingPath="Value"
    MarkerVisibility="Visible"
    ShowTrackBall="True">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            HighPointBrush="Red"
            LowPointBrush="Blue"
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

### Pattern 4: Sparkline with Range Band

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding PerformanceData}" 
    YBindingPath="Score"
    BandRangeStart="80"
    BandRangeEnd="100"
    RangeBandBrush="LightGreen"/>
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **ItemsSource** | IEnumerable | Data collection to bind |
| **YBindingPath** | string | Property path for Y-axis values |
| **XBindingPath** | string | Property path for X-axis values (optional) |
| **Interior** | Brush | Fill color for sparkline |
| **BorderBrush** | Brush | Border color |
| **BorderThickness** | Thickness | Border thickness |
| **MarkerVisibility** | Visibility | Show/hide markers |
| **ShowTrackBall** | bool | Enable track ball feature |
| **ShowAxis** | bool | Display axis line |
| **AxisOrigin** | double | Axis position on Y-axis |
| **HighlightSegment** | bool | Highlight segment on hover (Column/WinLoss) |
| **BandRangeStart** | double | Range band start value |
| **BandRangeEnd** | double | Range band end value |
| **RangeBandBrush** | Brush | Range band fill color |

## Common Use Cases

### Use Case 1: Dashboard KPI Trends
Display multiple sparklines showing key metrics over time (sales, traffic, conversions) in a compact dashboard layout.

**Recommended:** Line or Area sparklines with markers highlighting min/max values.

### Use Case 2: Data Grid Integration
Embed sparklines within data grid cells to show trend data alongside tabular information.

**Recommended:** Column sparklines for discrete data, Line sparklines for continuous trends.

### Use Case 3: Financial Win/Loss Indicators
Show trading results, game outcomes, or binary performance metrics.

**Recommended:** WinLoss sparklines with custom segment colors for wins, losses, and draws.

### Use Case 4: Performance Against Targets
Visualize metrics with target ranges highlighted (e.g., "green zone" for acceptable performance).

**Recommended:** Area or Line sparklines with range bands showing target thresholds.

### Use Case 5: Interactive Data Exploration
Allow users to hover over sparklines to inspect exact data values without cluttering the interface.

**Recommended:** Any sparkline type with track ball enabled and custom labels.

## Related Components

For more comprehensive data visualization needs:
- **[Charts](../implementing-charts/)** - Full-featured charting with axes, legends, and multiple series
- Other data visualization components (coming soon)

## Troubleshooting

**Common issues:**
- **Data not displaying:** Verify ItemsSource binding and YBindingPath property name
- **Markers not showing:** Ensure MarkerVisibility is set to Visible
- **Track ball not working:** Confirm ShowTrackBall="True" and sparkline type is Line or Area
- **Axis not visible:** Check ShowAxis property (not supported in WinLoss type)


For detailed troubleshooting, refer to the specific reference documentation for each feature.
