# Range Band

## Overview

The range band feature highlights a specific Y-axis range within the sparkline, making it easy to visualize target zones, acceptable ranges, or thresholds. Range bands are rendered as colored rectangular areas spanning the specified Y-value range.

**Range Band Availability:**
- ✓ **Line Sparkline** (SfLineSparkline)
- ✓ **Column Sparkline** (SfColumnSparkline)
- ✓ **Area Sparkline** (SfAreaSparkline)
- ✓ **WinLoss Sparkline** (SfWinLossSparkline)

Range bands are useful for:
- Highlighting target or goal ranges
- Showing acceptable/unacceptable value zones
- Visualizing thresholds (e.g., "green zone" for good performance)
- Comparing data against standards or benchmarks
- Identifying outliers that fall outside expected ranges

## Basic Range Band Configuration

Configure a range band using three properties:
- **BandRangeStart** - Upper Y-value boundary
- **BandRangeEnd** - Lower Y-value boundary
- **RangeBandBrush** - Fill color for the range band

### XAML Implementation

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    BandRangeStart="2000"
    BandRangeEnd="-1000"
    RangeBandBrush="Green">
</syncfusion:SfLineSparkline>
```

### C# Implementation

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.Windows.Media;

SfLineSparkline sparkline = new SfLineSparkline()
{
    ItemsSource = new SparkViewModel().UsersList,
    YBindingPath = "NoOfUsers",
    BandRangeStart = 2000,
    BandRangeEnd = -1000,
    RangeBandBrush = new SolidColorBrush(Colors.Green)
};
```

### Understanding Range Direction

**Important:** `BandRangeStart` is typically the **upper** value, and `BandRangeEnd` is the **lower** value.

```
BandRangeStart = 2000  ←─── Upper boundary
         ║
         ║  Highlighted Range (Green)
         ║
BandRangeEnd = -1000   ←─── Lower boundary
```

The range band fills the area between these two Y-values across the entire sparkline width.

## Range Band with Different Sparkline Types

### Line Sparkline with Range Band

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="#4a4a4a"
    BandRangeStart="5000"
    BandRangeEnd="2000"
    RangeBandBrush="LightGreen">
</syncfusion:SfLineSparkline>
```

**Result:** Line sparkline with a light green band highlighting values between 2000 and 5000.

### Column Sparkline with Range Band

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="SteelBlue"
    BandRangeStart="4000"
    BandRangeEnd="1000"
    RangeBandBrush="#8000FF00">
</syncfusion:SfColumnSparkline>
```

**Result:** Column sparkline with semi-transparent green band showing target range. Columns that reach into the band are easily identifiable.

### Area Sparkline with Range Band

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding UsersList}" 
    YBindingPath="NoOfUsers"
    Interior="LightBlue"
    BandRangeStart="6000"
    BandRangeEnd="3000"
    RangeBandBrush="#60FFFF00">
</syncfusion:SfAreaSparkline>
```

**Result:** Area sparkline with semi-transparent yellow band highlighting target zone.

### WinLoss Sparkline with Range Band

```xml
<syncfusion:SfWinLossSparkline 
    ItemsSource="{Binding Match}" 
    YBindingPath="Result"
    Interior="Green"
    BandRangeStart="1"
    BandRangeEnd="0"
    RangeBandBrush="#4000FF00">
</syncfusion:SfWinLossSparkline>
```

**Result:** WinLoss sparkline with band highlighting positive outcomes (wins).

## Common Range Band Patterns

### Target Zone (Green Zone)

Highlight acceptable or target performance range:

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding PerformanceData}" 
    YBindingPath="Score"
    BandRangeStart="80"
    BandRangeEnd="100"
    RangeBandBrush="#4000FF00"
    Interior="Orange">
</syncfusion:SfLineSparkline>
```

**Use case:** Scores between 80-100 are "good" (green zone), making it easy to see when performance falls below target.

### Warning Zone (Yellow/Orange)

Highlight cautionary range:

```xml
<syncfusion:SfColumnSparkline 
    ItemsSource="{Binding TemperatureData}" 
    YBindingPath="Temperature"
    BandRangeStart="30"
    BandRangeEnd="25"
    RangeBandBrush="#60FFA500"
    Interior="Red">
</syncfusion:SfColumnSparkline>
```

**Use case:** Temperatures between 25-30°C are in warning range.

### Threshold Line (Single Value)

Create a threshold line by setting start and end to the same value:

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding SalesData}" 
    YBindingPath="Amount"
    BandRangeStart="5000"
    BandRangeEnd="5000"
    RangeBandBrush="Red"
    Interior="Blue">
</syncfusion:SfLineSparkline>
```

**Result:** A horizontal line at Y=5000 showing a threshold or target.

### Negative Value Zone

Highlight negative value regions:

```xml
<syncfusion:SfAreaSparkline 
    ItemsSource="{Binding ProfitData}" 
    YBindingPath="Profit"
    BandRangeStart="0"
    BandRangeEnd="-5000"
    RangeBandBrush="#60FF0000"
    Interior="Green"
    BorderBrush="DarkGreen"
    BorderThickness="1">
</syncfusion:SfAreaSparkline>
```

**Use case:** Highlight loss range (negative profit) in red.

## Color and Transparency

### Solid Colors

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    BandRangeStart="100"
    BandRangeEnd="50"
    RangeBandBrush="LightGreen">
</syncfusion:SfLineSparkline>
```

### Semi-Transparent Colors

Use alpha channel for subtle highlighting (recommended):

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    BandRangeStart="100"
    BandRangeEnd="50"
    RangeBandBrush="#4000FF00">
    <!-- #40 = 25% opacity, 00FF00 = Green -->
</syncfusion:SfLineSparkline>
```

**Alpha Channel Values:**
- `#FF` - 100% opaque (solid)
- `#CC` - 80% opaque
- `#99` - 60% opaque
- `#66` - 40% opaque
- `#40` - 25% opaque (recommended for subtle bands)
- `#33` - 20% opaque
- `#00` - Fully transparent

### Gradient Brushes

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    BandRangeStart="100"
    BandRangeEnd="50">
    
    <syncfusion:SfLineSparkline.RangeBandBrush>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#8000FF00" Offset="0"/>
            <GradientStop Color="#40FFFF00" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfLineSparkline.RangeBandBrush>
    
</syncfusion:SfLineSparkline>
```

**Result:** Vertical gradient from green (top) to yellow (bottom) within the range band.

## Multiple Range Zones (Workaround)

Sparklines support only **one range band** at a time. For multiple zones, consider these approaches:

### Approach 1: Multiple Sparklines (Recommended)

Stack sparklines with different range bands:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Base sparkline with data -->
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding Data}" 
        YBindingPath="Value"
        Interior="Blue">
    </syncfusion:SfLineSparkline>
    
    <!-- Overlay: Green zone -->
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding Data}" 
        YBindingPath="Value"
        Interior="Transparent"
        BandRangeStart="80"
        BandRangeEnd="100"
        RangeBandBrush="#3000FF00"
        IsHitTestVisible="False">
    </syncfusion:SfLineSparkline>
    
    <!-- Overlay: Yellow zone -->
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding Data}" 
        YBindingPath="Value"
        Interior="Transparent"
        BandRangeStart="50"
        BandRangeEnd="80"
        RangeBandBrush="#30FFFF00"
        IsHitTestVisible="False">
    </syncfusion:SfLineSparkline>
    
    <!-- Overlay: Red zone -->
    <syncfusion:SfLineSparkline 
        ItemsSource="{Binding Data}" 
        YBindingPath="Value"
        Interior="Transparent"
        BandRangeStart="0"
        BandRangeEnd="50"
        RangeBandBrush="#30FF0000"
        IsHitTestVisible="False">
    </syncfusion:SfLineSparkline>
</Grid>
```

**Note:** Set `IsHitTestVisible="False"` on overlay sparklines so mouse events reach the base sparkline.

### Approach 2: Choose Most Important Zone

If multiple zones aren't critical, highlight only the most important range:

```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    BandRangeStart="80"
    BandRangeEnd="100"
    RangeBandBrush="LightGreen"
    Interior="Red">
    <!-- Red sparkline with green "success zone" -->
</syncfusion:SfLineSparkline>
```

## Dynamic Range Bands

Bind range values to ViewModel properties for dynamic updates:

### ViewModel

```csharp
using System.ComponentModel;

public class RangeViewModel : INotifyPropertyChanged
{
    private double _rangeStart = 5000;
    private double _rangeEnd = 2000;
    
    public double RangeStart
    {
        get => _rangeStart;
        set
        {
            _rangeStart = value;
            OnPropertyChanged(nameof(RangeStart));
        }
    }
    
    public double RangeEnd
    {
        get => _rangeEnd;
        set
        {
            _rangeEnd = value;
            OnPropertyChanged(nameof(RangeEnd));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### XAML Binding

```xml
<Window.DataContext>
    <local:RangeViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Controls to adjust range -->
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
        <TextBlock Text="Start:" VerticalAlignment="Center" Margin="5"/>
        <TextBox Text="{Binding RangeStart, UpdateSourceTrigger=PropertyChanged}" Width="80"/>
        
        <TextBlock Text="End:" VerticalAlignment="Center" Margin="15,0,5,0"/>
        <TextBox Text="{Binding RangeEnd, UpdateSourceTrigger=PropertyChanged}" Width="80"/>
    </StackPanel>
    
    <!-- Sparkline with bound range band -->
    <syncfusion:SfLineSparkline 
        Grid.Row="1"
        ItemsSource="{Binding UsersList}" 
        YBindingPath="NoOfUsers"
        BandRangeStart="{Binding RangeStart}"
        BandRangeEnd="{Binding RangeEnd}"
        RangeBandBrush="LightGreen">
    </syncfusion:SfLineSparkline>
</Grid>
```

**Result:** Range band updates in real-time as user changes Start/End values.

## Best Practices

### When to Use Range Bands

**Use range bands when:**
- Visualizing target zones or acceptable ranges
- Comparing data against thresholds or benchmarks
- Highlighting "good" vs. "bad" value regions
- Showing safety margins or tolerance zones
- Indicating standard deviations or confidence intervals

**Avoid range bands when:**
- Range is not meaningful for the data
- Too many overlapping zones are needed (sparklines support only one)
- The band would cover the entire sparkline (no contrast)

### Color Selection

**Semantic Colors:**
- **Green** - Success, target, acceptable range
- **Yellow/Orange** - Warning, caution, borderline
- **Red** - Danger, failure, unacceptable range
- **Blue** - Neutral, information, reference

**Transparency Guidelines:**
- Use 20-40% opacity for subtle highlighting
- Use 50-60% opacity for moderate emphasis
- Use 70-80% opacity for strong emphasis
- Avoid 100% opacity (can obscure sparkline data)

### Range Band Sizing

**Appropriate Range Size:**
- Band should be meaningful portion of Y-axis range
- Avoid bands that are too narrow (< 5% of range) - hard to see
- Avoid bands covering entire sparkline (>80% of range) - loses contrast

**Example - Good Range:**
```xml
<!-- Data ranges from 0 to 10000 -->
<!-- Band covers 20% of range -->
<syncfusion:SfLineSparkline 
    BandRangeStart="6000"
    BandRangeEnd="4000"
    RangeBandBrush="#4000FF00">
</syncfusion:SfLineSparkline>
```

### Combining with Other Features

**Range Band + Markers:**
```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    MarkerVisibility="Visible"
    BandRangeStart="80"
    BandRangeEnd="100"
    RangeBandBrush="LightGreen">
    
    <syncfusion:SfLineSparkline.MarkerTemplateSelector>
        <syncfusion:MarkerTemplateSelector 
            HighPointBrush="Red"
            LowPointBrush="Blue"
            MarkerHeight="10" 
            MarkerWidth="10"/>
    </syncfusion:SfLineSparkline.MarkerTemplateSelector>
    
</syncfusion:SfLineSparkline>
```

**Use case:** Range band shows target zone, markers highlight extremes.

**Range Band + Track Ball:**
```xml
<syncfusion:SfLineSparkline 
    ItemsSource="{Binding Data}" 
    YBindingPath="Value"
    ShowTrackBall="True"
    BandRangeStart="80"
    BandRangeEnd="100"
    RangeBandBrush="LightGreen">
</syncfusion:SfLineSparkline>
```

**Use case:** Range band provides context, track ball shows exact values on hover.

## Next Steps

- **[Segment Customization](segment-customization.md)** - Color-code individual data points
- **[Axis Controls](axis-controls.md)** - Show axis lines with range bands
- **[Markers](markers.md)** - Combine range bands with point markers
