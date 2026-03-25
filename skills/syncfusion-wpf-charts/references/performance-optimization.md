# Performance Optimization

Optimize chart performance when working with large datasets or complex visualizations.

## Fast Series Types

Use fast series for large datasets (1000+ points).

### FastLineSeries

Optimized line chart for large datasets:

```xml
<syncfusion:FastLineSeries ItemsSource="{Binding LargeDataset}"
                           XBindingPath="X"
                           YBindingPath="Y"
                           EnableAntiAliasing="True"/>
```

**Use when:** Plotting thousands of time-series data points

### FastColumnSeries

High-performance column chart:

```xml
<syncfusion:FastColumnBitmapSeries  ItemsSource="{Binding Data}"
                             XBindingPath="Category"
                             YBindingPath="Value"/>
```

### FastBarSeries

Optimized horizontal bar chart:

```xml
<syncfusion:FastBarBitmapSeries  ItemsSource="{Binding Data}"
                          XBindingPath="Category"
                          YBindingPath="Value"/>
```

### Other Fast Series

- `FastScatterBitmapSeries` - Large scatter plots
- `FastStepLineBitmapSeries` - Step line with many points
- `FastHiLoOpenCloseBitmapSeries` - High-low financial data
- `FastHiLoBitmapSeries` - OHLC with large datasets
- `FastCandleBitmapSeries` - Candlestick charts with many candles

## Fast vs Regular Series

| Feature | Regular Series | Fast Series |
|---------|---------------|-------------|
| Data points | < 1,000 | 1,000+ |
| Animations | Full support | Limited |
| Interactivity | Rich features | Basic |
| Rendering | Feature-rich | Optimized |
| Performance | Standard | High |

## Anti-Aliasing

Enable smooth rendering for fast series:

```xml
<syncfusion:FastLineSeries EnableAntiAliasing="True"/>
```

**Note:** Slightly impacts performance but improves visual quality.

## Segment Rendering

Render only visible data segments:

```xml
<syncfusion:LineSeries EnableSegmentSelection="True"
                       SegmentSpacing="0"/>
```

## Disable Animations

Turn off animations for better performance:

```xml
<syncfusion:ColumnSeries EnableAnimation="False"/>
```


## Data Sampling

Reduce data points before binding:

```csharp
public class DataSampler
{
    public static ObservableCollection<DataPoint> Sample(
        List<DataPoint> data, int maxPoints)
    {
        if (data.Count <= maxPoints)
            return new ObservableCollection<DataPoint>(data);
        
        var step = data.Count / maxPoints;
        var sampled = new ObservableCollection<DataPoint>();
        
        for (int i = 0; i < data.Count; i += step)
        {
            sampled.Add(data[i]);
        }
        
        return sampled;
    }
}
```

## Virtualization

For category axis with many categories:

```xml
<syncfusion:CategoryAxis EnableTouchMode="True"/>
```

## Optimize Data Binding

### Use Arrays for Static Data

```csharp
// Faster for static data
public double[] Values { get; set; }

// Use ObservableCollection only for dynamic updates
public ObservableCollection<DataPoint> Data { get; set; }
```

### Avoid Complex Property Paths

```csharp
// Faster
YBindingPath="Value"

// Slower
YBindingPath="Details.Performance.Value"
```

## Reduce Visual Complexity

### Limit Adornments

```xml
<!-- Only on key points -->
<syncfusion:ColumnSeries>
    <syncfusion:ColumnSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="False"
                                       ShowMarker="False"/>
    </syncfusion:ChartAdornmentsInfo>
</syncfusion:ColumnSeries>
```

### Simplify Grid Lines

```xml
<syncfusion:NumericalAxis ShowGridLines="False"/>
```

Or reduce grid line complexity:

```xml
<syncfusion:NumericalAxis Interval="1000">  <!-- Fewer lines -->
    <syncfusion:NumericalAxis.MajorGridLineStyle>
        <Style TargetType="Line">
            <Setter Property="StrokeThickness" Value="1"/>  <!-- Thinner -->
        </Style>
    </syncfusion:NumericalAxis.MajorGridLineStyle>
</syncfusion:NumericalAxis>
```

## Batch Data Updates

Suspend notifications during bulk updates:

```csharp
public void UpdateData(List<DataPoint> newData)
{
    // Suspend binding updates
    this.SuspendSeriesNotification();
    
    foreach (var point in newData)
    {
        Data.Add(point);
    }
    
    // Resume and update once
    this.ResumeSeriesNotification();
}
```

## Memory Management

### Clear Unused Data

```csharp
public void ClearOldData()
{
    // Keep only recent data
    while (Data.Count > 1000)
    {
        Data.RemoveAt(0);
    }
}
```

### Dispose Resources

```csharp
public void Cleanup()
{
    if (chart != null)
    {
        chart.Series.Clear();
        chart = null;
    }
}
```

## Optimize Zooming

```xml
<syncfusion:ChartZoomPanBehavior EnableAutoIntervalOnZooming="True"
                                 ZoomMode="X"/>  <!-- Limit to one axis -->
```

## Performance Best Practices

1. **Choose Fast Series** for datasets over 1,000 points
2. **Disable animations** when not needed
3. **Limit adornments** (labels, markers) on large datasets
4. **Use data sampling** for very large datasets
5. **Avoid complex bindings** to nested properties
6. **Disable features** you don't use (tooltips, selection, etc.)
7. **Update in batches** rather than individual points
8. **Clear old data** to manage memory
9. **Simplify visuals** (fewer grid lines, simpler styles)
10. **Test with real data volumes** to identify bottlenecks

## Performance Comparison Example

```xml
<!-- Slow: Regular series with all features -->
<syncfusion:LineSeries ItemsSource="{Binding Data}"
                       ShowTooltip="True"
                       EnableAnimation="True"
                       EnableSegmentSelection="True">
    <syncfusion:LineSeries.AdornmentsInfo>
        <syncfusion:ChartAdornmentInfo ShowLabel="True"
                                       ShowMarker="True"/>
    </syncfusion:LineSeries.AdornmentsInfo>
</syncfusion:LineSeries>

<!-- Fast: Optimized for large datasets -->
<syncfusion:FastLineSeries ItemsSource="{Binding Data}"
                           XBindingPath="X"
                           YBindingPath="Y"
                           EnableAntiAliasing="True"/>
```

## Real-time Data Updates

For streaming/real-time data:

```csharp
public class RealtimeViewModel
{
    private DispatcherTimer timer;
    public ObservableCollection<DataPoint> Data { get; set; }
    
    public RealtimeViewModel()
    {
        Data = new ObservableCollection<DataPoint>();
        
        timer = new DispatcherTimer();
        timer.Interval = TimeSpan.FromSeconds(1);
        timer.Tick += UpdateData;
        timer.Start();
    }
    
    private void UpdateData(object sender, EventArgs e)
    {
        // Add new point
        Data.Add(new DataPoint 
        { 
            X = DateTime.Now, 
            Y = GetNewValue() 
        });
        
        // Remove old points to limit memory
        if (Data.Count > 100)
            Data.RemoveAt(0);
    }
}
```

## Monitoring Performance

Measure rendering performance:

```csharp
var stopwatch = Stopwatch.StartNew();

// Update chart
chart.Series[0].ItemsSource = newData;

stopwatch.Stop();
Debug.WriteLine($"Render time: {stopwatch.ElapsedMilliseconds}ms");
```

## Key Takeaways

- **< 1,000 points:** Use regular series with full features
- **1,000 - 10,000 points:** Use fast series, disable animations
- **> 10,000 points:** Use fast series + data sampling
- **Real-time data:** Use fast series, limit data retention
- **Always test** with production-level data volumes
