# Axes Configuration in WPF SfChart3D

## Table of Contents
- [Overview](#overview)
- [Axis Types](#axis-types)
- [Depth Axis (Z-Axis)](#depth-axis-z-axis)
- [Manhattan Charts](#manhattan-charts)
- [Axis Customization](#axis-customization)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

ChartAxis is used to locate data points inside the 3D chart area. Charts typically have three axes for 3D visualization:

- **PrimaryAxis (X-Axis)** - Horizontal axis for X values
- **SecondaryAxis (Y-Axis)** - Vertical axis for Y values  
- **DepthAxis (Z-Axis)** - Horizontal depth axis for Z values (optional)

The axes measure and categorize data, providing context for the 3D visualization. SfChart3D supports multiple axis types to handle different data formats.

## Axis Types

SfChart3D supports five axis types, each optimized for specific data scenarios:

### Numerical Axis

**NumericalAxis3D** plots numerical values and can be defined for any axis (Primary, Secondary, or Depth).

**When to use:**
- Numeric data (integers, decimals)
- Measurements and quantities
- Mathematical values

**Basic Implementation:**

**XAML:**
```xml
<chart:SfChart3D>
    <!-- Primary Axis (X-Axis) -->
    <chart:SfChart3D.PrimaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <!-- Secondary Axis (Y-Axis) -->
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    PrimaryAxis = new NumericalAxis3D(),
    SecondaryAxis = new NumericalAxis3D()
};
```

**Properties:**
- **Interval** - Spacing between axis labels
- **Minimum** - Starting value
- **Maximum** - Ending value

### Category Axis

**CategoryAxis3D** is an index-based axis that plots values based on the index of the data point collection. Points are equally spaced.

**When to use:**
- Categorical data (product names, regions, months)
- Non-numeric X values
- Fixed categories with equal spacing

**Basic Implementation:**

**XAML:**
```xml
<chart:SfChart3D>
    <!-- Primary Axis (X-Axis) -->
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    PrimaryAxis = new CategoryAxis3D()
};
```

**Example Data:**
```csharp
public class SalesData
{
    public string Product { get; set; }  // Category data for X-axis
    public double Sales { get; set; }
}
```

### DateTime Axis

**DateTimeAxis3D** plots DateTime values, commonly used for time-series data like stock market charts or financial analysis.

**When to use:**
- Time-series data
- Date-based analysis
- Financial charts
- Historical trends

**Basic Implementation:**

**XAML:**
```xml
<chart:SfChart3D>
    <!-- Primary Axis (X-Axis) -->
    <chart:SfChart3D.PrimaryAxis>
        <chart:DateTimeAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    PrimaryAxis = new DateTimeAxis3D()
};
```

**Example Data:**
```csharp
public class TimeSeriesData
{
    public DateTime Date { get; set; }  // DateTime for X-axis
    public double Value { get; set; }
}
```

**Properties:**
- **IntervalType** - Day, Month, Year, Hour, etc.
- **Interval** - Number of interval units
- **LabelFormat** - Date format string (e.g., "MMM dd")

### TimeSpan Axis

**TimeSpanAxis3D** plots time duration values with millisecond precision.

**When to use:**
- Duration measurements
- Elapsed time data
- Performance metrics (execution time)
- Time differences

**Basic Implementation:**

**XAML:**
```xml
<chart:SfChart3D>
    <!-- Primary Axis (X-Axis) -->
    <chart:SfChart3D.PrimaryAxis>
        <chart:TimeSpanAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    PrimaryAxis = new TimeSpanAxis3D()
};
```

**Example Data:**
```csharp
public class DurationData
{
    public TimeSpan Duration { get; set; }  // TimeSpan for X-axis
    public double Performance { get; set; }
}
```

**Limitation:** Only accepts TimeSpan values (hh:mm:ss), not DateTime values.

### Logarithmic Axis

**LogarithmicAxis3D** plots values on a logarithmic scale with base 10 by default.

**When to use:**
- Exponential data
- Wide value ranges
- Scientific data
- Orders of magnitude visualization

**Basic Implementation:**

**XAML:**
```xml
<chart:SfChart3D>
    <!-- Secondary Axis (Y-Axis) -->
    <chart:SfChart3D.SecondaryAxis>
        <chart:LogarithmicAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    SecondaryAxis = new LogarithmicAxis3D()
};
```

**Properties:**
- **LogarithmicBase** - Base for logarithmic calculation (default: 10)

## Depth Axis (Z-Axis)

The **DepthAxis** enables true 3D plotting with X, Y, and Z coordinates, adding depth dimension to your charts.

**Supported Series:**
- ColumnSeries3D
- BarSeries3D
- LineSeries3D
- ScatterSeries3D
- StackingColumnSeries3D
- StackingBarSeries3D

### Basic 3D Plotting

**When to use:**
- Three-dimensional data (X, Y, Z coordinates)
- Scientific visualizations
- Spatial data analysis
- Multi-variable comparisons

**XAML:**
```xml
<chart:SfChart3D Margin="120,20,120,30" Rotation="43" Tilt="10"
                 EnableRotation="True" PerspectiveAngle="100">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Depth Axis (Z-Axis) -->
    <chart:SfChart3D.DepthAxis>
        <chart:NumericalAxis3D Interval="1"/>
    </chart:SfChart3D.DepthAxis>
    
    <!-- Series with Z data binding -->
    <chart:ColumnSeries3D 
        XBindingPath="XValue"                                
        YBindingPath="YValue"
        ZBindingPath="ZValue"
        ItemsSource="{Binding Data}"/>
        
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D();
chart3D.Rotation = 43;
chart3D.Tilt = 10;
chart3D.Margin = new Thickness(120, 20, 120, 30);
chart3D.PerspectiveAngle = 100;
chart3D.EnableRotation = true;

// Configure axes
chart3D.PrimaryAxis = new NumericalAxis3D();
chart3D.SecondaryAxis = new NumericalAxis3D();

NumericalAxis3D depthAxis = new NumericalAxis3D();
depthAxis.Interval = 1;
chart3D.DepthAxis = depthAxis;

// Add series with Z binding
ColumnSeries3D series = new ColumnSeries3D();
series.ItemsSource = new ViewModel().Data;
series.XBindingPath = "XValue";
series.YBindingPath = "YValue";
series.ZBindingPath = "ZValue";  // Z-axis data

chart3D.Series.Add(series);
```

**Data Model for 3D Plotting:**
```csharp
public class Data3D
{
    public double XValue { get; set; }
    public double YValue { get; set; }
    public double ZValue { get; set; }  // Depth axis value
}

public class ViewModel
{
    public ObservableCollection<Data3D> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<Data3D>
        {
            new Data3D { XValue = 1, YValue = 20, ZValue = 5 },
            new Data3D { XValue = 2, YValue = 35, ZValue = 8 },
            new Data3D { XValue = 3, YValue = 28, ZValue = 12 },
            new Data3D { XValue = 4, YValue = 42, ZValue = 15 }
        };
    }
}
```

**Key Point:** When DepthAxis is not explicitly defined, it's automatically created based on ZBindingPath data type.

## Manhattan Charts

Manhattan charts display multiple series across the depth axis, creating a 3D visualization of multiple datasets.

**When to use:**
- Comparing multiple related datasets
- Multi-product analysis over time
- 3D trend comparisons
- Portfolio visualizations

### Implementation

**XAML:**
```xml
<chart:SfChart3D Rotation="43" EnableRotation="True">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Depth Axis for Manhattan Chart -->
    <chart:SfChart3D.DepthAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.DepthAxis>
    
    <!-- Multiple series -->
    <chart:LineSeries3D 
        XBindingPath="XValue"                                
        YBindingPath="YValue"                         
        ItemsSource="{Binding Data1}" 
        Label="First"/>
    
    <chart:LineSeries3D 
        XBindingPath="XValue"                                
        YBindingPath="YValue"                         
        ItemsSource="{Binding Data2}" 
        Label="Second"/>
        
    <chart:LineSeries3D 
        XBindingPath="XValue"                                
        YBindingPath="YValue"                         
        ItemsSource="{Binding Data3}" 
        Label="Third"/>
        
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    Rotation = 43,
    EnableRotation = true
};

chart3D.PrimaryAxis = new CategoryAxis3D();
chart3D.SecondaryAxis = new NumericalAxis3D();

// Depth axis as Category for series labels
NumericalAxis3D depthAxis = new CategoryAxis3D();
chart3D.DepthAxis = depthAxis;

// Add multiple series
LineSeries3D series1 = new LineSeries3D();
series1.ItemsSource = new ViewModel().Data1;
series1.XBindingPath = "XValue";
series1.YBindingPath = "YValue";
series1.Label = "First";

LineSeries3D series2 = new LineSeries3D();
series2.ItemsSource = new ViewModel().Data2;
series2.XBindingPath = "XValue";
series2.YBindingPath = "YValue";
series2.Label = "Second";

LineSeries3D series3 = new LineSeries3D();
series3.ItemsSource = new ViewModel().Data3;
series3.XBindingPath = "XValue";
series3.YBindingPath = "YValue";
series3.Label = "Third";

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
chart3D.Series.Add(series3);
```

**Manhattan Chart Behavior:**
- DepthAxis uses CategoryAxis3D
- Axis labels map to series Label property
- If Label not defined, displays "Series1", "Series2", etc.
- Each series appears at a different depth position

## Axis Customization

### Headers

Add descriptive headers to axes:

**XAML:**
```xml
<chart:SfChart3D>
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D Header="Month"/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D Header="Sales (in thousands)"/>
    </chart:SfChart3D.SecondaryAxis>
    
    <chart:SfChart3D.DepthAxis>
        <chart:NumericalAxis3D Header="Product Category"/>
    </chart:SfChart3D.DepthAxis>
</chart:SfChart3D>
```

**C#:**
```csharp
chart3D.PrimaryAxis = new CategoryAxis3D() { Header = "Month" };
chart3D.SecondaryAxis = new NumericalAxis3D() { Header = "Sales (in thousands)" };
chart3D.DepthAxis = new NumericalAxis3D() { Header = "Product Category" };
```

### Intervals

Control the spacing between axis labels:

**XAML:**
```xml
<chart:NumericalAxis3D Interval="10" Minimum="0" Maximum="100"/>
```

**C#:**
```csharp
NumericalAxis3D axis = new NumericalAxis3D()
{
    Interval = 10,
    Minimum = 0,
    Maximum = 100
};
```

### Ranges

Set explicit min/max values:

**XAML:**
```xml
<chart:NumericalAxis3D Minimum="0" Maximum="1000"/>
```

**C#:**
```csharp
axis.Minimum = 0;
axis.Maximum = 1000;
```

### Font Customization

**XAML:**
```xml
<chart:CategoryAxis3D Header="Time" FontSize="14">
</chart:CategoryAxis3D>
```

**C#:**
```csharp
CategoryAxis3D axis = new CategoryAxis3D()
{
    Header = "Time",
    FontSize = 14
};
```

## Edge Cases and Troubleshooting

### Issue: Depth Axis Not Appearing

**Causes:**
- ZBindingPath not set on series
- Series type doesn't support depth axis
- DepthAxis not configured

**Solution:**
```xml
<!-- Ensure all three are present -->
<chart:SfChart3D.DepthAxis>
    <chart:NumericalAxis3D/>
</chart:SfChart3D.DepthAxis>

<chart:ColumnSeries3D 
    XBindingPath="XValue"
    YBindingPath="YValue"
    ZBindingPath="ZValue">  <!-- Must have ZBindingPath -->
</chart:ColumnSeries3D>
```

### Issue: Manhattan Chart Labels Not Showing

**Cause:** Series Label property not set

**Solution:**
```xml
<chart:LineSeries3D Label="Product A" .../>
<chart:LineSeries3D Label="Product B" .../>
```

### Issue: Axis Range Too Narrow or Wide

**Cause:** Automatic range calculation not optimal

**Solution:** Set explicit ranges:
```xml
<chart:NumericalAxis3D Minimum="0" Maximum="100" Interval="20"/>
```

### Issue: DateTime Axis Labels Overlapping

**Cause:** Too many date labels

**Solution:** Adjust interval:
```xml
<chart:DateTimeAxis3D IntervalType="Months" Interval="2" LabelFormat="MMM yyyy"/>
```

### Issue: Category Axis Shows Index Numbers

**Cause:** XBindingPath pointing to numeric property instead of string

**Solution:** Ensure XBindingPath maps to string property:
```csharp
// Data model
public class SalesData
{
    public string Category { get; set; }  // String for CategoryAxis
    public double Value { get; set; }
}
```

### Best Practices

**Axis Selection:**
- Use **NumericalAxis3D** for numeric measurements
- Use **CategoryAxis3D** for discrete categories
- Use **DateTimeAxis3D** for time series
- Use **TimeSpanAxis3D** for durations only
- Use **LogarithmicAxis3D** for exponential data

**Depth Axis:**
- Explicitly define DepthAxis for better control
- Match axis type to Z data type
- Set appropriate Interval for clarity
- Test rotation angles for best viewing

**Manhattan Charts:**
- Limit to 3-5 series for clarity
- Use meaningful Label properties
- Apply consistent data structure across series
- Enable rotation for data exploration

**Performance:**
- Avoid excessive Interval density
- Use appropriate axis ranges
- Consider data sampling for large datasets
- Simplify axis styling for better performance
