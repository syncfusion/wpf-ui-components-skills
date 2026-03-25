# Series Configuration

## Table of Contents
- [Overview](#overview)
- [LineSeries Basics](#lineseries-basics)
- [Data Binding](#data-binding)
- [Series Customization](#series-customization)
- [Animation](#animation)
- [Series Visibility](#series-visibility)
- [Data Plotting Customization](#data-plotting-customization)
- [Multiple Series](#multiple-series)
- [Series-Specific Palettes](#series-specific-palettes)

## Overview

Series are the visual representation of transmission line data in SmithChart. The LineSeries plots data points as connected lines on the chart, showing the relationship between resistance and reactance values.

**Key Concepts:**
- **LineSeries**: The primary series type for SmithChart
- **Data Binding**: Connect your data to the chart via ItemsSource
- **Path Properties**: ResistancePath and ReactancePath map to data properties
- **Customization**: Control appearance, color, thickness, and animation
- **Multiple Series**: Compare different transmission lines on one chart

## LineSeries Basics

### Creating a LineSeries

The simplest LineSeries configuration:

```xml
<syncfusion:SfSmithChart>
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data}">
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

Code-behind equivalent:

```csharp
LineSeries series = new LineSeries();
series.ItemsSource = Data;
series.ResistancePath = "Resistance";
series.ReactancePath = "Reactance";

chart.Series.Add(series);
```

### Essential LineSeries Properties

| Property | Type | Description |
|----------|------|-------------|
| `ResistancePath` | string | Property name for X-axis (horizontal) values |
| `ReactancePath` | string | Property name for Y-axis (radial) values |
| `ItemsSource` | IEnumerable | Data collection to visualize |
| `Interior` | Brush | Line color |
| `StrokeThickness` | double | Line width |
| `Label` | string | Series name (displayed in legend) |
| `ColorModel` | SmithChartColorModel | Color model for the series |

## Data Binding

### Basic Data Binding Setup

Data binding connects your data model to the chart visualization.

**Step 1: Create Data Model**

```csharp
public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}
```

**Step 2: Create Data Collection**

```csharp
public ObservableCollection<TransmissionData> Data { get; set; }

public MainWindow()
{
    InitializeComponent();
    this.DataContext = this;
    
    Data = new ObservableCollection<TransmissionData>
    {
        new TransmissionData { Resistance = 0, Reactance = 0.05 },
        new TransmissionData { Resistance = 0.3, Reactance = 0.1 },
        new TransmissionData { Resistance = 1.0, Reactance = 0.4 },
        // ... more data points
    };
}
```

**Step 3: Bind to LineSeries**

```xml
<syncfusion:LineSeries ResistancePath="Resistance" 
                       ReactancePath="Reactance" 
                       ItemsSource="{Binding Data}">
</syncfusion:LineSeries>
```

### Path Properties Explained

**ResistancePath** - Maps to horizontal axis (X values):
- Must match property name exactly (case-sensitive)
- Represents normalized resistance values
- Typically ranges from 0 to positive values

**ReactancePath** - Maps to radial axis (Y values):
- Must match property name exactly (case-sensitive)
- Represents normalized reactance values
- Can be positive or negative

**Example with Custom Property Names:**

```csharp
public class ImpedanceData
{
    public double R { get; set; }  // Resistance
    public double X { get; set; }  // Reactance
}
```

```xml
<syncfusion:LineSeries ResistancePath="R" 
                       ReactancePath="X" 
                       ItemsSource="{Binding ImpedanceCollection}">
</syncfusion:LineSeries>
```

### Dynamic Data Updates

Use ObservableCollection for automatic UI updates when data changes:

```csharp
// Adding new point updates chart automatically
Data.Add(new TransmissionData { Resistance = 3.0, Reactance = 0.6 });

// Removing point updates chart
Data.RemoveAt(0);

// Modifying existing point (requires INotifyPropertyChanged)
Data[0].Resistance = 0.5;
```

For property-level updates, implement INotifyPropertyChanged:

```csharp
public class TransmissionData : INotifyPropertyChanged
{
    private double resistance;
    private double reactance;
    
    public double Resistance
    {
        get => resistance;
        set
        {
            resistance = value;
            OnPropertyChanged(nameof(Resistance));
        }
    }
    
    public double Reactance
    {
        get => reactance;
        set
        {
            reactance = value;
            OnPropertyChanged(nameof(Reactance));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Series Customization

### Line Color (Interior)

Change the series line color:

```xml
<syncfusion:LineSeries Interior="Orange">
</syncfusion:LineSeries>
```

```csharp
series.Interior = new SolidColorBrush(Colors.Orange);
```

**Common color options:**
```csharp
// Solid colors
series.Interior = new SolidColorBrush(Colors.Red);
series.Interior = new SolidColorBrush(Colors.Blue);
series.Interior = new SolidColorBrush(Color.FromRgb(255, 128, 0));

// Gradient brush
LinearGradientBrush gradient = new LinearGradientBrush();
gradient.GradientStops.Add(new GradientStop(Colors.Blue, 0));
gradient.GradientStops.Add(new GradientStop(Colors.Red, 1));
series.Interior = gradient;
```

### Line Thickness (StrokeThickness)

Control the width of the series line:

```xml
<syncfusion:LineSeries StrokeThickness="3">
</syncfusion:LineSeries>
```

```csharp
series.StrokeThickness = 3;
```

**Recommended values:**
- **1-2**: Thin, subtle lines
- **2-3**: Standard visibility
- **3-5**: Emphasized, bold lines
- **5+**: Very prominent (use sparingly)

### Combined Customization Example

```xml
<syncfusion:LineSeries ResistancePath="Resistance" 
                       ReactancePath="Reactance" 
                       ItemsSource="{Binding Data}"
                       Interior="Orange" 
                       StrokeThickness="3"
                       Label="TransmissionLine">
</syncfusion:LineSeries>
```

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Interior = new SolidColorBrush(Colors.Orange),
    StrokeThickness = 3,
    Label = "TransmissionLine"
};
chart.Series.Add(series);
```

## Animation

Animate the series when it first loads or when data changes.

### Enabling Animation

```xml
<syncfusion:LineSeries EnableAnimation="True" 
                       AnimationDuration="0:0:3">
</syncfusion:LineSeries>
```

```csharp
series.EnableAnimation = true;
series.AnimationDuration = TimeSpan.FromSeconds(3);
```

### Animation Properties

| Property | Type | Description |
|----------|------|-------------|
| `EnableAnimation` | bool | Enable/disable animation (default: false) |
| `AnimationDuration` | TimeSpan | Duration of animation effect |

### Animation Duration Examples

```csharp
// Fast animation (0.5 seconds)
series.AnimationDuration = TimeSpan.FromSeconds(0.5);

// Standard animation (1 second)
series.AnimationDuration = TimeSpan.FromSeconds(1);

// Slow animation (3 seconds)
series.AnimationDuration = TimeSpan.FromSeconds(3);

// Using milliseconds
series.AnimationDuration = TimeSpan.FromMilliseconds(1500);

// Using time format (minutes:seconds:milliseconds)
series.AnimationDuration = TimeSpan.Parse("0:0:2.5");
```

### When Animation Triggers

Animation occurs when:
- Chart initially loads with data
- ItemsSource is changed to a new collection
- Chart is refreshed

**Note:** Individual data point additions/removals do not trigger animation by default.

### Performance Considerations

For large datasets (>100 points):
- Use shorter animation durations (0.5-1 second)
- Consider disabling animation for better performance
- Animation overhead is minimal but can impact initial render time

## Series Visibility

Control series visibility programmatically while maintaining its legend entry.

### IsSeriesVisible Property

Hide or show series programmatically:

```xml
<syncfusion:LineSeries IsSeriesVisible="False" 
                       Label="Transmission-1">
</syncfusion:LineSeries>
```

```csharp
series.IsSeriesVisible = false; // Hide series
series.IsSeriesVisible = true;  // Show series
```

### Interactive Visibility Toggle

When a series is hidden:
- The series line is not rendered
- Legend item appears shaded/dimmed
- User can click legend item to toggle visibility
- All series data remains in memory

**Example with multiple series:**

```xml
<syncfusion:SfSmithChart>
    <!-- Initially hidden -->
    <syncfusion:LineSeries IsSeriesVisible="False" 
                           Label="Transmission-1" 
                           ItemsSource="{Binding Data1}">
    </syncfusion:LineSeries>
    
    <!-- Visible by default -->
    <syncfusion:LineSeries Label="Transmission-2" 
                           ItemsSource="{Binding Data2}">
    </syncfusion:LineSeries>
    
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

```csharp
// Hide specific series
LineSeries series1 = new LineSeries
{
    Label = "Transmission-1",
    ItemsSource = Data1,
    IsSeriesVisible = false // Initially hidden
};
chart.Series.Add(series1);

// Toggle visibility later
series1.IsSeriesVisible = !series1.IsSeriesVisible;
```

### Use Cases for Series Visibility

1. **Conditional Display**: Show/hide based on user selection
2. **Data Comparison**: Toggle between different datasets
3. **Declutter View**: Hide less important series temporarily
4. **Progressive Disclosure**: Show series one at a time

## Data Plotting Customization

### ArrangeByIndex Property

Control how data points are plotted and connected.

**Default Behavior (ArrangeByIndex = false):**
- Data points are sorted by resistance values before plotting
- Results in smooth, organized lines
- Recommended for most scenarios

**Index-Based Plotting (ArrangeByIndex = true):**
- Data points are plotted in their original order
- No sorting applied
- Useful when order matters (time-series data)

```xml
<syncfusion:LineSeries ArrangeByIndex="True">
</syncfusion:LineSeries>
```

```csharp
series.ArrangeByIndex = true;
```

### Example: Comparing Arrange Methods

**Sample Data (unsorted):**
```csharp
Data.Add(new TransmissionData { Resistance = 2.0, Reactance = 0.5 });
Data.Add(new TransmissionData { Resistance = 0.3, Reactance = 0.1 });
Data.Add(new TransmissionData { Resistance = 1.5, Reactance = 0.5 });
Data.Add(new TransmissionData { Resistance = 0.5, Reactance = 0.2 });
```

**ArrangeByIndex = false (default):**
- Points connected as: 0.3 → 0.5 → 1.5 → 2.0
- Sorted by resistance value
- Smooth, predictable line

**ArrangeByIndex = true:**
- Points connected as: 2.0 → 0.3 → 1.5 → 0.5
- Original data order preserved
- May create zigzag lines

### When to Use ArrangeByIndex

**Use ArrangeByIndex = true when:**
- Data represents time-series measurements
- Order of collection is significant
- You need to preserve original sequence

**Use ArrangeByIndex = false (default) when:**
- Standard impedance/admittance plotting
- Want smooth, predictable lines
- Data order is arbitrary

## Multiple Series

Display and compare multiple transmission lines on a single chart.

### Adding Multiple Series

```xml
<syncfusion:SfSmithChart Header="Transmission Line Comparison">
    <!-- First series -->
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data1}" 
                           Label="Transmission-1"
                           Interior="Blue"
                           ShowMarker="True">
    </syncfusion:LineSeries>
    
    <!-- Second series -->
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data2}" 
                           Label="Transmission-2"
                           Interior="Orange"
                           ShowMarker="True">
    </syncfusion:LineSeries>
    
    <!-- Third series -->
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data3}" 
                           Label="Transmission-3"
                           Interior="Green"
                           ShowMarker="True">
    </syncfusion:LineSeries>
    
    <syncfusion:SfSmithChart.Legend>
        <syncfusion:SmithChartLegend/>
    </syncfusion:SfSmithChart.Legend>
</syncfusion:SfSmithChart>
```

Code-behind approach:

```csharp
// Create multiple data collections
ObservableCollection<TransmissionData> Data1 = LoadData1();
ObservableCollection<TransmissionData> Data2 = LoadData2();
ObservableCollection<TransmissionData> Data3 = LoadData3();

// Create and add series
LineSeries series1 = new LineSeries
{
    ItemsSource = Data1,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Transmission-1",
    Interior = new SolidColorBrush(Colors.Blue),
    ShowMarker = true
};

LineSeries series2 = new LineSeries
{
    ItemsSource = Data2,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Transmission-2",
    Interior = new SolidColorBrush(Colors.Orange),
    ShowMarker = true
};

LineSeries series3 = new LineSeries
{
    ItemsSource = Data3,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Transmission-3",
    Interior = new SolidColorBrush(Colors.Green),
    ShowMarker = true
};

chart.Series.Add(series1);
chart.Series.Add(series2);
chart.Series.Add(series3);
```

### Multiple Series Best Practices

1. **Use Distinct Colors**: Ensure each series has a unique, contrasting color
2. **Add Labels**: Always set Label property for legend identification
3. **Enable Markers**: Help distinguish overlapping series
4. **Consider Line Thickness**: Vary thickness for visual hierarchy
5. **Limit Series Count**: 3-5 series maximum for readability

### Accessing Series Collection

```csharp
// Get series count
int count = chart.Series.Count;

// Access specific series
LineSeries firstSeries = chart.Series[0] as LineSeries;

// Remove a series
chart.Series.RemoveAt(0);

// Clear all series
chart.Series.Clear();

// Find series by label
var transmission1 = chart.Series.FirstOrDefault(s => s.Label == "Transmission-1");
```

## Series-Specific Palettes

Apply color palettes to individual series for automatic color cycling.

### Setting Series Palette

```xml
<syncfusion:LineSeries>
    <syncfusion:LineSeries.ColorModel>
        <syncfusion:SmithChartColorModel Palette="Metro"/>
    </syncfusion:LineSeries.ColorModel>
</syncfusion:LineSeries>
```

```csharp
series.ColorModel = new SmithChartColorModel();
series.ColorModel.Palette = ColorPalette.Metro;
```

### Available Palettes

- **Metro**: Modern, vibrant colors
- **BlueChrome**: Blue-based professional palette
- **And more...**

### When to Use Series Palette

**Use series-specific palette when:**
- Working with multiple series
- Each series has multiple data segments
- Want consistent color scheme per series
- Automatic color assignment is preferred

**Use explicit Interior color when:**
- Single series or few series
- Specific branding colors required
- Precise color control needed

### Combining Palettes and Custom Colors

```csharp
// Series 1: Uses Metro palette
LineSeries series1 = new LineSeries
{
    Label = "Series 1",
    ColorModel = new SmithChartColorModel { Palette = ColorPalette.Metro }
};

// Series 2: Uses explicit color
LineSeries series2 = new LineSeries
{
    Label = "Series 2",
    Interior = new SolidColorBrush(Colors.Orange)
};

chart.Series.Add(series1);
chart.Series.Add(series2);
```

## Common Patterns and Examples

### Pattern 1: Basic Single Series

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    Label = "Impedance",
    Interior = new SolidColorBrush(Colors.Blue),
    StrokeThickness = 2
};
chart.Series.Add(series);
```

### Pattern 2: Animated Series with Markers

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    EnableAnimation = true,
    AnimationDuration = TimeSpan.FromSeconds(1),
    ShowMarker = true,
    MarkerType = MarkerType.Circle
};
chart.Series.Add(series);
```
## Troubleshooting

### Series Not Displaying

**Issue**: Added series but no line appears  
**Solutions**:
- Verify ItemsSource has data (not null or empty)
- Check ResistancePath/ReactancePath match property names exactly
- Ensure Interior is not transparent
- Verify StrokeThickness > 0

### Incorrect Data Plotting

**Issue**: Data points connected in wrong order  
**Solution**: Set `ArrangeByIndex = false` to sort by resistance values

### Animation Not Working

**Issue**: EnableAnimation set but no animation occurs  
**Solutions**:
- Animation only triggers on initial load or ItemsSource change
- Check AnimationDuration is set to reasonable value
- Ensure ItemsSource is being set, not modified in place

### Performance Issues

**Issue**: Slow rendering with multiple series  
**Solutions**:
- Reduce data point count (sample data if needed)
- Disable animation for better performance
- Limit series count to 3-5
- Use simpler marker types

## Summary

LineSeries is the core visualization element in SmithChart, providing flexible data binding, extensive customization, and support for multiple series comparison. Key points:

- Use ResistancePath and ReactancePath for data binding
- Customize appearance with Interior and StrokeThickness
- Enable animation for engaging visualizations
- Control visibility with IsSeriesVisible
- Support multiple series for comparison scenarios
- Use ArrangeByIndex when data order matters