# Sparkline Types

## Table of Contents
- [Overview](#overview)
- [Line Sparkline](#line-sparkline)
- [Column Sparkline](#column-sparkline)
- [Area Sparkline](#area-sparkline)
- [WinLoss Sparkline](#winloss-sparkline)
- [Choosing the Right Type](#choosing-the-right-type)
- [Type-Specific Features](#type-specific-features)

## Overview

Syncfusion WPF Sparklines support four distinct types, each optimized for different data visualization scenarios. Understanding when to use each type ensures your sparklines effectively communicate data trends.

**Available Types:**
1. **Line Sparkline** (SfLineSparkline) - Continuous trend visualization
2. **Column Sparkline** (SfColumnSparkline) - Discrete value comparison
3. **Area Sparkline** (SfAreaSparkline) - Magnitude visualization with filled area
4. **WinLoss Sparkline** (SfWinLossSparkline) - Binary outcome representation

All sparkline types share common properties like ItemsSource, YBindingPath, Interior, and BorderBrush, but each has unique rendering and specialized features.

## Line Sparkline

The Line Sparkline (SfLineSparkline) renders data as a continuous polyline, ideal for showing trends, patterns, and continuous data flow.

### When to Use

- **Continuous data trends** (temperature, stock prices, website traffic)
- **Time-series data** where smooth transitions matter
- **Pattern recognition** (cycles, seasonality, anomalies)
- **Multiple data points** where discrete columns would clutter

### XAML Implementation

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <Window.DataContext>
        <local:UsersViewModel/>
    </Window.DataContext>
    
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding UsersList}" 
        YBindingPath="NoOfUsers"
        Interior="#4a4a4a">
    </syncfusion:SfLineSparkline>
    
</Window>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    Interior = new SolidColorBrush(Color.FromRgb(0x4a, 0x4a, 0x4a)),
    BorderBrush = new SolidColorBrush(Colors.DarkGray),
    BorderThickness = new Thickness(1)
};
```

### Line Sparkline Features

Line sparklines support:
- **Markers** - Highlight specific data points
- **Track Ball** - Interactive data inspection
- **Range Bands** - Highlight Y-axis ranges
- **Axis Controls** - Show/hide axis line
- **Segment Customization** - Color specific points

### Styling Options

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="Blue"
    LineStyle="{StaticResource CustomLineStyle}">
</syncfusion:SfLineSparkline>
```

## Column Sparkline

The Column Sparkline (SfColumnSparkline) visualizes data as discrete rectangular bars, perfect for comparing individual values or showing discrete measurements.

### When to Use

- **Discrete value comparison** (monthly sales, daily counts)
- **Bar chart alternatives** in compact spaces
- **Categorical data** with distinct data points
- **Highlighting individual values** rather than overall trend

### XAML Implementation

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="SteelBlue"
    BorderBrush="DarkGray"
    BorderThickness="1">
</syncfusion:SfColumnSparkline>
```

### C# Implementation

```csharp
SfColumnSparkline sparkline = new SfColumnSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    Interior = new SolidColorBrush(Colors.SteelBlue),
};
```

### Column Sparkline Features

Column sparklines support:
- **Segment Highlighting** - Highlight on mouse hover
- **Segment Customization** - Color first, last, high, low, negative points
- **Range Bands** - Highlight Y-axis ranges
- **Axis Controls** - Show/hide axis line

**Note:** Column sparklines do NOT support markers or track ball features.

### Column Spacing

Columns automatically calculate spacing based on available width and data point count. For custom spacing, use container margins and padding.

## Area Sparkline

The Area Sparkline (SfAreaSparkline) displays data as a filled area under a trend line, emphasizing magnitude and cumulative values.

### When to Use

- **Magnitude visualization** (volume, quantity, area coverage)
- **Cumulative data** where the "filled" area conveys meaning
- **Volume trends** (resource usage, capacity)
- **Visual weight** - When you want trend AND magnitude emphasis

### XAML Implementation

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="LightBlue">
</syncfusion:SfAreaSparkline>
```

### C# Implementation

```csharp
SfAreaSparkline sparkline = new SfAreaSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    Interior = new SolidColorBrush(Colors.LightBlue),
};
```

### Area Sparkline Features

Area sparklines support the **same features as Line sparklines**:
- **Markers** - Highlight specific data points
- **Track Ball** - Interactive data inspection
- **Range Bands** - Highlight Y-axis ranges
- **Axis Controls** - Show/hide axis line
- **Segment Customization** - Color specific points

The only difference from Line sparklines is the filled area rendering.

### Styling the Fill

Control the interior and border separately:

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="#80ADD8E6">
</syncfusion:SfAreaSparkline>
```

**Tip:** Use semi-transparent colors (alpha channel) for subtle area fills:
- `#80ADD8E6` - 50% transparent light blue
- `#40FF0000` - 25% transparent red

## WinLoss Sparkline

The WinLoss Sparkline (SfWinLossSparkline) renders data as column segments specifically designed for binary outcomes: positive (win), negative (loss), and neutral (draw/tie).

### When to Use

- **Binary outcomes** (win/loss, pass/fail, up/down)
- **Trading results** (profitable/unprofitable trades)
- **Game records** (wins, losses, draws)
- **Status indicators** (success/failure over time)
- **Three-state data** (positive, negative, neutral)

### Data Requirements

WinLoss sparklines interpret Y-values as:
- **Positive values (>0):** Win/Success (typically green or positive color)
- **Negative values (<0):** Loss/Failure (typically red or negative color)
- **Zero (0):** Draw/Neutral (typically gray or neutral color)

### Data Model Example

```csharp
public class MatchDetailsModel
{
    public double Result { get; set; }  // 1 = Win, -1 = Loss, 0 = Draw
    public string Status { get; set; }
}

public class MatchDetailsViewModel
{
    public ObservableCollection<MatchDetailsModel> Match { get; set; }
    
    public MatchDetailsViewModel()
    {
        Match = new ObservableCollection<MatchDetailsModel>();
        
        Match.Add(new MatchDetailsModel { Result = 1, Status = "Win" });
        Match.Add(new MatchDetailsModel { Result = -1, Status = "Loss" });
        Match.Add(new MatchDetailsModel { Result = 0, Status = "Draw" });
        Match.Add(new MatchDetailsModel { Result = 1, Status = "Win" });
        Match.Add(new MatchDetailsModel { Result = 1, Status = "Win" });
        Match.Add(new MatchDetailsModel { Result = -1, Status = "Loss" });
    }
}
```

### XAML Implementation

```xml
<Window.DataContext>
    <local:MatchDetailsViewModel/>
</Window.DataContext>

<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding Match}" 
    YBindingPath="Result"
    Interior="Green">
</syncfusion:SfWinLossSparkline>
```

### C# Implementation

```csharp
SfWinLossSparkline sparkline = new SfWinLossSparkline()
{
    ItemsSource = new MatchDetailsViewModel().Match,
    YBindingPath = "Result",
    Interior = new SolidColorBrush(Colors.Green),
};
```

### WinLoss Sparkline Features

WinLoss sparklines support:
- **Segment Highlighting** - Highlight on mouse hover
- **Segment Customization** - Different colors for wins, losses, draws
- **Range Bands** - Highlight specific result ranges

**Important Limitations:**
- **NO Markers** - Not applicable for binary data
- **NO Track Ball** - Interactive inspection not supported
- **NO Axis Controls** - ShowAxis not available for WinLoss type

### Customizing Win/Loss Colors

Use SegmentTemplateSelector to distinguish wins, losses, and draws:

```xml
<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding Match}" 
    YBindingPath="Result">
    
    <syncfusion:SfWinLossSparkline.SegmentTemplateSelector>
        <syncfusion:SegmentTemplateSelector 
            FirstPointBrush="Yellow" 
            LastPointBrush="Yellow"
            HighPointBrush="LightGreen"
            LowPointBrush="LightCoral"
            NegativePointBrush="Red"/>
    </syncfusion:SfWinLossSparkline.SegmentTemplateSelector>
    
</syncfusion:SfWinLossSparkline>
```

## Choosing the Right Type

### Decision Matrix

| Scenario | Recommended Type | Reason |
|----------|-----------------|--------|
| Stock prices over time | Line | Continuous trend, emphasizes flow |
| Monthly sales comparison | Column | Discrete periods, individual comparison |
| CPU usage percentage | Area | Magnitude emphasis, resource visualization |
| Trading win/loss record | WinLoss | Binary outcomes, three-state display |
| Temperature trends | Line | Smooth transitions, continuous data |
| Daily website visits | Column or Line | Column for comparison, Line for trend |
| Quarterly revenue | Column | Discrete periods with clear boundaries |
| Pass/fail test results | WinLoss | Binary outcome visualization |

### Feature Availability Matrix

| Feature | Line | Column | Area | WinLoss |
|---------|------|--------|------|---------|
| Markers | ✓ | ✗ | ✓ | ✗ |
| Track Ball | ✓ | ✗ | ✓ | ✗ |
| Range Bands | ✓ | ✓ | ✓ | ✓ |
| Axis Controls | ✓ | ✓ | ✓ | ✗ |
| Segment Highlighting | ✗ | ✓ | ✗ | ✓ |
| Segment Customization | ✓ | ✓ | ✓ | ✓ |

## Type-Specific Features

### Line and Area Only

**Markers and Track Ball:**
```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    MarkerVisibility="Visible"
    ShowTrackBall="True">
</syncfusion:SfLineSparkline>
```

### Column and WinLoss Only

**Segment Highlighting:**
```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    HighlightSegment="True">
</syncfusion:SfColumnSparkline>
```

### All Types (Except WinLoss)

**Axis Controls:**
```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    ShowAxis="True"
    AxisOrigin="0">
</syncfusion:SfLineSparkline>
```

## Switching Between Types

To change sparkline types, simply replace the control type. The data binding remains the same:

```xml
<!-- Line Sparkline -->
<syncfusion:SfLineSparkline ItemsSource="{Binding Data}" YBindingPath="Value"/>

<!-- Change to Column Sparkline -->
<syncfusion:SfColumnSparkline ItemsSource="{Binding Data}" YBindingPath="Value"/>

<!-- Change to Area Sparkline -->
<syncfusion:SfAreaSparkline ItemsSource="{Binding Data}" YBindingPath="Value"/>

<!-- Change to WinLoss Sparkline -->
<syncfusion:SfWinLossSparkline ItemsSource="{Binding Data}" YBindingPath="Value"/>
```

**Note:** Ensure your data is appropriate for the type (especially for WinLoss, which requires positive/negative/zero values).

## Next Steps

- **[Markers](markers.md)** - Add markers to Line and Area sparklines
- **[Track Ball](track-ball.md)** - Enable interactive inspection
- **[Segment Customization](segment-customization.md)** - Color-code special points
- **[Axis Controls](axis-controls.md)** - Show and style axis lines
