# Advanced Features

## Table of Contents
- [Orientation](#orientation)
- [Flow Direction](#flow-direction)
- [Tooltips](#tooltips)
- [Data Binding](#data-binding)
- [MVVM Pattern Support](#mvvm-pattern-support)
- [Performance Optimization](#performance-optimization)

## Orientation

The bullet graph can be displayed in either horizontal or vertical orientation using the `Orientation` property.

### Orientation Property

| Value | Description |
|-------|-------------|
| `Horizontal` | Default. Graph extends from left to right |
| `Vertical` | Graph extends from top to bottom |

### Horizontal Orientation (Default)

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Orientation="Horizontal" 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="6"
    ComparativeMeasure="8">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="3" RangeStroke="#EBEBEB"/>
        <syncfusion:QualitativeRange RangeEnd="7" RangeStroke="#D8D8D8"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#7F7F7F"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Orientation = Orientation.Horizontal;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 6;
bulletGraph.ComparativeMeasure = 8;

bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 3,
    RangeStroke = (Brush)new BrushConverter().ConvertFrom("#EBEBEB")
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 7,
    RangeStroke = (Brush)new BrushConverter().ConvertFrom("#D8D8D8")
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 10,
    RangeStroke = (Brush)new BrushConverter().ConvertFrom("#7F7F7F")
});
```

### Vertical Orientation

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Orientation="Vertical" 
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="6"
    ComparativeMeasure="8"
    Height="300">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="3" RangeStroke="#EBEBEB"/>
        <syncfusion:QualitativeRange RangeEnd="7" RangeStroke="#D8D8D8"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#7F7F7F"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.Orientation = Orientation.Vertical;
bulletGraph.Height = 300;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
bulletGraph.FeaturedMeasure = 6;
bulletGraph.ComparativeMeasure = 8;

bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 3,
    RangeStroke = (Brush)new BrushConverter().ConvertFrom("#EBEBEB")
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 7,
    RangeStroke = (Brush)new BrushConverter().ConvertFrom("#D8D8D8")
});
bulletGraph.QualitativeRanges.Add(new QualitativeRange()
{
    RangeEnd = 10,
    RangeStroke = (Brush)new BrushConverter().ConvertFrom("#7F7F7F")
});
```

### When to Use Each Orientation

**Horizontal (default):**
- Standard dashboard layouts
- Wide display areas
- Multiple stacked bullet graphs
- Traditional left-to-right reading order
- Most common use case

**Vertical:**
- Tall, narrow display areas
- Space-constrained layouts
- Mimicking vertical gauges or thermometers
- Columnar dashboard designs
- When comparing multiple metrics side-by-side

## Flow Direction

The flow direction controls the direction in which the bullet graph is rendered. This is particularly useful for right-to-left (RTL) language support.

### FlowDirection Property

| Value | Horizontal Orientation | Vertical Orientation |
|-------|----------------------|---------------------|
| `LeftToRight` | Scale flows left to right (default) | Scale flows top to bottom (default) |
| `RightToLeft` | Scale flows right to left (RTL support) | Scale flows bottom to top |

### Default Flow Direction (LeftToRight)

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Orientation="Horizontal" 
    FlowDirection="LeftToRight"
    Minimum="0" 
    Maximum="10" 
    Interval="2">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="3" RangeStroke="#EBEBEB"/>
        <syncfusion:QualitativeRange RangeEnd="7" RangeStroke="#7F7F7F"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#D8D8D8"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.FlowDirection = FlowDirection.LeftToRight;
bulletGraph.Orientation = Orientation.Horizontal;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
```

### Right-to-Left Flow Direction

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    Orientation="Horizontal" 
    FlowDirection="RightToLeft"
    Minimum="0" 
    Maximum="10" 
    Interval="2">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="3" RangeStroke="#EBEBEB"/>
        <syncfusion:QualitativeRange RangeEnd="7" RangeStroke="#7F7F7F"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#D8D8D8"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.FlowDirection = FlowDirection.RightToLeft;
bulletGraph.Orientation = Orientation.Horizontal;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.Interval = 2;
```

### RTL Support for Localization

For applications supporting Arabic, Hebrew, or other RTL languages:

```xml
<Window FlowDirection="RightToLeft">
    <Grid>
        <syncfusion:SfBulletGraph 
            FlowDirection="RightToLeft"
            Minimum="0" 
            Maximum="100"
            FeaturedMeasure="75">
            
            <syncfusion:SfBulletGraph.Caption>
                <TextBlock Text="الإيرادات السنوية" FontSize="14"/>
            </syncfusion:SfBulletGraph.Caption>
        </syncfusion:SfBulletGraph>
    </Grid>
</Window>
```

## Tooltips

Tooltips display values when users hover over or touch the featured measure, comparative measure, or qualitative ranges.

### Enabling Tooltips

Set the `ShowToolTip` property to `true` to enable tooltips:

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    ShowToolTip="True"
    Minimum="0" 
    Maximum="10" 
    Interval="2"
    FeaturedMeasure="5"
    FeaturedMeasureBarStroke="Black"
    ComparativeMeasure="4" 
    QualitativeRangesSize="30"
    MinorTicksPerInterval="3" 
    MinorTickSize="8">
    
    <syncfusion:SfBulletGraph.QualitativeRanges>
        <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="Red" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="Yellow" RangeOpacity="1"/>
        <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="1"/>
    </syncfusion:SfBulletGraph.QualitativeRanges>
</syncfusion:SfBulletGraph>
```

**C#:**
```csharp
SfBulletGraph bulletGraph = new SfBulletGraph();
bulletGraph.ShowToolTip = true;
bulletGraph.Minimum = 0;
bulletGraph.Maximum = 10;
bulletGraph.FeaturedMeasure = 5;
bulletGraph.ComparativeMeasure = 4;
bulletGraph.FeaturedMeasureBarStroke = new SolidColorBrush(Colors.Black);
```

### Custom Featured Measure Tooltip

Customize the tooltip appearance for the featured measure using `FeaturedMeasureToolTipTemplate`:

**XAML:**
```xml
<Grid x:Name="grid">
    <Grid.Resources>
        <DataTemplate x:Key="featuredTooltipTemplate">
            <Border BorderBrush="#D3D3D3" BorderThickness="1.5" Background="#232323" CornerRadius="5">
                <TextBlock Text="{Binding}" FontSize="14" Foreground="#D3D3D3" Margin="12,8"/>
            </Border>
        </DataTemplate>
    </Grid.Resources>

    <syncfusion:SfBulletGraph 
        ShowToolTip="True"
        FeaturedMeasureToolTipTemplate="{StaticResource featuredTooltipTemplate}"
        Minimum="0" 
        Maximum="10" 
        FeaturedMeasure="5"
        ComparativeMeasure="4"
        FeaturedMeasureBarStroke="Black"
        QualitativeRangesSize="30">
        
        <syncfusion:SfBulletGraph.QualitativeRanges>
            <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="Red" RangeOpacity="1"/>
            <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="Yellow" RangeOpacity="1"/>
            <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="1"/>
        </syncfusion:SfBulletGraph.QualitativeRanges>
    </syncfusion:SfBulletGraph>
</Grid>
```

**C#:**
```csharp
bulletGraph.ShowToolTip = true;
bulletGraph.FeaturedMeasureToolTipTemplate = grid.Resources["featuredTooltipTemplate"] as DataTemplate;
```

### Custom Comparative Measure Tooltip

Customize the tooltip for the comparative measure using `ComparativeMeasureToolTipTemplate`:

**XAML:**
```xml
<Grid x:Name="grid">
    <Grid.Resources>
        <DataTemplate x:Key="comparativeTooltipTemplate">
            <Border BorderBrush="#D3D3D3" BorderThickness="1.5" Background="#232323" CornerRadius="5">
                <TextBlock Text="{Binding}" FontSize="14" Foreground="#D3D3D3" Margin="12,8"/>
            </Border>
        </DataTemplate>
    </Grid.Resources>

    <syncfusion:SfBulletGraph 
        ShowToolTip="True"
        ComparativeMeasureToolTipTemplate="{StaticResource comparativeTooltipTemplate}"
        FeaturedMeasure="5"
        ComparativeMeasure="4">
    </syncfusion:SfBulletGraph>
</Grid>
```

**C#:**
```csharp
bulletGraph.ComparativeMeasureToolTipTemplate = grid.Resources["comparativeTooltipTemplate"] as DataTemplate;
```

### Custom Qualitative Range Tooltip

Customize the tooltip for qualitative ranges using `QualitativeRangeToolTipTemplate`:

**XAML:**
```xml
<Grid x:Name="grid">
    <Grid.Resources>
        <DataTemplate x:Key="rangeTooltipTemplate">
            <Border BorderBrush="Black" BorderThickness="1.5" CornerRadius="5">
                <Border Background="{Binding RangeStroke}" Opacity="0.7" CornerRadius="5">
                    <StackPanel Orientation="Horizontal" Margin="12,8">
                        <TextBlock Text="{Binding RangeEnd}" FontSize="14" Foreground="Black"/>
                    </StackPanel>
                </Border>
            </Border>
        </DataTemplate>
    </Grid.Resources>

    <syncfusion:SfBulletGraph 
        ShowToolTip="True"
        QualitativeRangeToolTipTemplate="{StaticResource rangeTooltipTemplate}"
        FeaturedMeasure="5"
        ComparativeMeasure="4"
        QualitativeRangesSize="30">
        
        <syncfusion:SfBulletGraph.QualitativeRanges>
            <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="Red" RangeOpacity="1"/>
            <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="Yellow" RangeOpacity="1"/>
            <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="Green" RangeOpacity="1"/>
        </syncfusion:SfBulletGraph.QualitativeRanges>
    </syncfusion:SfBulletGraph>
</Grid>
```

**C#:**
```csharp
bulletGraph.QualitativeRangeToolTipTemplate = grid.Resources["rangeTooltipTemplate"] as DataTemplate;
```

### Tooltip Best Practices

1. **Enable tooltips for data clarity:** Especially when precise values are important
2. **Keep tooltip content concise:** Display the value and optionally a unit
3. **Use consistent styling:** Match tooltip design with application theme
4. **Consider touch devices:** Tooltips display on hold/press for touch interaction
5. **Test visibility:** Ensure tooltips contrast well with backgrounds

## Data Binding

Bullet graphs support data binding for dynamic updates and MVVM implementations.

### Binding Measure Values

**ViewModel:**
```csharp
public class RevenueViewModel : INotifyPropertyChanged
{
    private double _currentRevenue;
    private double _targetRevenue;

    public double CurrentRevenue
    {
        get => _currentRevenue;
        set
        {
            _currentRevenue = value;
            OnPropertyChanged(nameof(CurrentRevenue));
        }
    }

    public double TargetRevenue
    {
        get => _targetRevenue;
        set
        {
            _targetRevenue = value;
            OnPropertyChanged(nameof(TargetRevenue));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    FeaturedMeasure="{Binding CurrentRevenue}"
    ComparativeMeasure="{Binding TargetRevenue}"
    Minimum="0" 
    Maximum="500" 
    Interval="100">
</syncfusion:SfBulletGraph>
```

**Code-behind:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = new RevenueViewModel
        {
            CurrentRevenue = 375,
            TargetRevenue = 400
        };
    }
}
```

### Binding Range Collections

```csharp
public class BulletGraphViewModel : INotifyPropertyChanged
{
    public ObservableCollection<QualitativeRange> PerformanceRanges { get; set; }

    public BulletGraphViewModel()
    {
        PerformanceRanges = new ObservableCollection<QualitativeRange>
        {
            new QualitativeRange { RangeEnd = 40, RangeStroke = new SolidColorBrush(Colors.Red) },
            new QualitativeRange { RangeEnd = 75, RangeStroke = new SolidColorBrush(Colors.Yellow) },
            new QualitativeRange { RangeEnd = 100, RangeStroke = new SolidColorBrush(Colors.Green) }
        };
    }
}
```

**XAML:**
```xml
<syncfusion:SfBulletGraph 
    QualitativeRanges="{Binding PerformanceRanges}"
    FeaturedMeasure="65">
</syncfusion:SfBulletGraph>
```

## MVVM Pattern Support

The bullet graph is fully compatible with the MVVM architectural pattern.

### Complete MVVM Example

**ViewModel:**
```csharp
public class DashboardViewModel : INotifyPropertyChanged
{
    private double _revenue;
    private double _revenueTarget;
    private string _revenueCaption;

    public double Revenue
    {
        get => _revenue;
        set { _revenue = value; OnPropertyChanged(nameof(Revenue)); }
    }

    public double RevenueTarget
    {
        get => _revenueTarget;
        set { _revenueTarget = value; OnPropertyChanged(nameof(RevenueTarget)); }
    }

    public string RevenueCaption
    {
        get => _revenueCaption;
        set { _revenueCaption = value; OnPropertyChanged(nameof(RevenueCaption)); }
    }

    public DashboardViewModel()
    {
        Revenue = 375;
        RevenueTarget = 400;
        RevenueCaption = "Revenue YTD";
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

**XAML:**
```xml
<Window x:Class="BulletGraphMVVM.MainWindow"
        DataContext="{StaticResource DashboardViewModel}">
    <Grid>
        <syncfusion:SfBulletGraph 
            FeaturedMeasure="{Binding Revenue}"
            ComparativeMeasure="{Binding RevenueTarget}"
            Minimum="0" 
            Maximum="500"
            Interval="100">
        </syncfusion:SfBulletGraph>
    </Grid>
</Window>
```

## Performance Optimization

### Tips for Optimal Performance

1. **Limit range count:** Use 3-5 qualitative ranges maximum
2. **Avoid excessive minor ticks:** More than 9 per interval can impact rendering
3. **Reuse templates:** Define DataTemplates as resources for tooltip customization
4. **Minimize updates:** Batch property changes when updating multiple values
5. **Use appropriate scale lengths:** Match visualization size to available space

### Efficient Updates

When updating multiple properties:

```csharp
// Inefficient: Multiple re-renders
bulletGraph.FeaturedMeasure = 75;
bulletGraph.ComparativeMeasure = 80;
bulletGraph.Minimum = 0;

// Better: Use BeginInit/EndInit pattern if available, or batch in ViewModel
// Update ViewModel properties which trigger single render
viewModel.UpdateValues(75, 80, 0);
```

### Memory Considerations

- Bullet graphs are lightweight controls
- Each instance uses minimal memory
- Safe to use multiple instances in dashboards (10-20+ without issues)
- Dispose properly when removing from visual tree

## Troubleshooting

**Issue:** Vertical bullet graph not displaying correctly  
**Solution:** Ensure Height is explicitly set for vertical orientation. Container must provide vertical space.

**Issue:** RTL flow direction not working  
**Solution:** Verify `FlowDirection` is set on both the Window/parent and the bullet graph itself for proper RTL support.

**Issue:** Tooltips not showing  
**Solution:** Confirm `ShowToolTip="True"` is set. Check that tooltip templates are defined in accessible resources.

**Issue:** Data binding not updating  
**Solution:** Verify ViewModel implements `INotifyPropertyChanged` and raises property changed events correctly.
