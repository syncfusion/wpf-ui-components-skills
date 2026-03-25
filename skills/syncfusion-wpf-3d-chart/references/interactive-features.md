# Interactive Features in WPF SfChart3D

Interactive features enhance user engagement with 3D charts, allowing dynamic exploration of data through rotation, selection, and visual feedback.

## Dynamic Rotation

Dynamic rotation enables users to view 3D charts from different angles using mouse or touch input, helping them find the optimal viewing perspective for their data.

### Enabling Dynamic Rotation

Set the `EnableRotation` property to true:

**XAML:**
```xml
<chart:SfChart3D EnableRotation="True" Width="600" Height="500">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <chart:ColumnSeries3D 
        ItemsSource="{Binding Data}" 
        XBindingPath="Category" 
        YBindingPath="Value"/>
        
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    EnableRotation = true,
    Width = 600,
    Height = 500
};

chart3D.PrimaryAxis = new CategoryAxis3D();
chart3D.SecondaryAxis = new NumericalAxis3D();

ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = new ViewModel().Data,
    XBindingPath = "Category",
    YBindingPath = "Value"
};

chart3D.Series.Add(series);
```

**User Interaction:**
- **Mouse:** Click and drag to rotate
- **Touch:** Touch and drag on touch-enabled devices
- Rotation persists after user releases input

### Setting Initial Rotation

Control the initial viewing angle with rotation properties:

**XAML:**
```xml
<chart:SfChart3D 
    EnableRotation="True"
    Rotation="45" 
    Tilt="-30"
    PerspectiveAngle="90"
    Depth="100"
    Width="500" 
    Height="500">
    
    <!-- Axes and series configuration -->
    
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    EnableRotation = true,
    Rotation = 45,          // Horizontal rotation angle (0-360)
    Tilt = -30,             // Vertical tilt angle
    PerspectiveAngle = 90,  // 3D perspective (0-180)
    Depth = 100,            // Chart depth for 3D effect
    Width = 500,
    Height = 500
};
```

**Rotation Properties:**

- **Rotation** - Horizontal rotation angle (0-360 degrees)
  - 0° - Front view
  - 45° - Angled view
  - 90° - Side view
  - 180° - Back view

- **Tilt** - Vertical tilt angle
  - Positive values tilt upward
  - Negative values tilt downward
  - Typical range: -45 to 45

- **PerspectiveAngle** - 3D depth perspective (0-180 degrees)
  - 0° - Flat (orthographic view)
  - 90° - Moderate 3D effect
  - 180° - Strong 3D effect

- **Depth** - Overall chart depth
  - Default: 100
  - Higher values increase 3D prominence
  - Affects Z-axis depth

### Best Viewing Angles

**Column/Bar Charts:**
```xml
Rotation="43" Tilt="10" PerspectiveAngle="100"
```

**Circular Charts (Pie/Doughnut):**
```xml
Rotation="45" Tilt="-30" PerspectiveAngle="90" Depth="30"
```

**Manhattan Charts:**
```xml
Rotation="29" Tilt="15" PerspectiveAngle="50"
```

### Dynamic Rotation Tips

**When to enable:**
- Complex 3D data requiring multiple viewing angles
- Manhattan charts with multiple series
- Interactive dashboards
- Exploration-focused visualizations

**When to disable:**
- Static reports and presentations
- When specific angle is critical
- Touch-unfriendly environments
- Print/PDF outputs

## Segment Selection

Segment selection highlights individual data points when clicked, providing visual feedback for user interaction.

### Enabling Segment Selection

Set the `SegmentSelectionBrush` property on the series:

**XAML:**
```xml
<chart:SfChart3D 
    EnableRotation="True"  
    PerspectiveAngle="50"   
    Rotation="29" 
    Depth="100" 
    Palette="BlueChrome" 
    Width="300" 
    Height="280">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Enable segment selection -->
    <chart:ColumnSeries3D 
        SegmentSelectionBrush="SkyBlue" 
        ItemsSource="{Binding DataPoints}"  
        XBindingPath="Year" 
        YBindingPath="India">               
    </chart:ColumnSeries3D>
    
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    EnableRotation = true,
    PerspectiveAngle = 50,
    Rotation = 29,
    Depth = 100,
    Palette = ChartColorPalette.BlueChrome,
    Width = 300,
    Height = 280
};

chart3D.PrimaryAxis = new CategoryAxis3D();
chart3D.SecondaryAxis = new NumericalAxis3D();

ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = new StockViewModel().DataPoints,
    XBindingPath = "Year",
    YBindingPath = "India",
    SegmentSelectionBrush = new SolidColorBrush(Colors.SkyBlue),
    SegmentSpacing = 0.5
};

chart3D.Series.Add(series);
```

**Behavior:**
- Click a segment to highlight it
- Selected segment changes to SegmentSelectionBrush color
- Click again to deselect
- Only one segment selected at a time per series

### Selection Customization

**Different selection colors per series:**
```xml
<chart:ColumnSeries3D 
    SegmentSelectionBrush="LightGreen"
    ItemsSource="{Binding Sales}"/>
    
<chart:ColumnSeries3D 
    SegmentSelectionBrush="LightCoral"
    ItemsSource="{Binding Costs}"/>
```

## Series Selection

Series selection highlights an entire series, useful for multi-series charts where you want to emphasize one dataset.

### Enabling Series Selection

Enable with `EnableSeriesSelection` and optionally set initial selection:

**XAML:**
```xml
<chart:SfChart3D 
    EnableRotation="True"  
    PerspectiveAngle="50" 
    EnableSeriesSelection="True" 
    SeriesSelectedIndex="0"
    Rotation="29" 
    Depth="100" 
    Palette="BlueChrome" 
    Width="300" 
    Height="280">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Series 1 -->
    <chart:ColumnSeries3D 
        SeriesSelectionBrush="LightGreen" 
        SegmentSpacing="0.5" 
        ItemsSource="{Binding Demands}"  
        XBindingPath="Category" 
        YBindingPath="Value">
    </chart:ColumnSeries3D>
    
    <!-- Series 2 -->
    <chart:ColumnSeries3D 
        SegmentSpacing="0.5" 
        SeriesSelectionBrush="SkyBlue" 
        ItemsSource="{Binding Demands}"  
        XBindingPath="Category" 
        YBindingPath="Value">
    </chart:ColumnSeries3D>
    
</chart:SfChart3D>
```

**C#:**
```csharp
SfChart3D chart3D = new SfChart3D()
{
    EnableSeriesSelection = true,
    EnableRotation = true,
    PerspectiveAngle = 50,
    SeriesSelectedIndex = 0,  // Pre-select first series
    Rotation = 29,
    Depth = 100,
    Palette = ChartColorPalette.BlueChrome,
    Width = 300,
    Height = 280
};

chart3D.PrimaryAxis = new CategoryAxis3D();
chart3D.SecondaryAxis = new NumericalAxis3D();

ColumnSeries3D series1 = new ColumnSeries3D()
{
    ItemsSource = new StockViewModel().Demands,
    XBindingPath = "Category",
    YBindingPath = "Value",
    SeriesSelectionBrush = new SolidColorBrush(Colors.LightGreen),
    SegmentSpacing = 0.5
};

ColumnSeries3D series2 = new ColumnSeries3D()
{
    ItemsSource = new StockViewModel().Demands,
    XBindingPath = "Category",
    YBindingPath = "Value",
    SeriesSelectionBrush = new SolidColorBrush(Colors.SkyBlue),
    SegmentSpacing = 0.5
};

chart3D.Series.Add(series1);
chart3D.Series.Add(series2);
```

**Properties:**
- **EnableSeriesSelection** - Enable series selection feature
- **SeriesSelectedIndex** - Index of initially selected series (0-based)
- **SeriesSelectionBrush** - Color applied when series is selected

**Behavior:**
- Click any segment in a series to select entire series
- All segments in selected series change to SeriesSelectionBrush
- Click another series to change selection
- Click selected series again to deselect

### Selection Changed Event

Handle selection changes programmatically:

**C#:**
```csharp
chart3D.SelectionChanged += Chart3D_SelectionChanged;

private void Chart3D_SelectionChanged(object sender, ChartSelectionChangedEventArgs e)
{
    // e.SelectedIndex - Currently selected series index
    // e.PreviousSelectedIndex - Previously selected series index
    
    if (e.SelectedIndex >= 0)
    {
        var selectedSeries = chart3D.Series[e.SelectedIndex];
        // Handle selection logic
    }
}
```

## Dynamic Explode (Circular Series)

Circular series (Pie and Doughnut) support exploding segments to emphasize specific data points.

### Explode on Mouse Click

Enable interactive segment explosion:

**XAML:**
```xml
<chart:SfChart3D 
    EnableRotation="True"  
    Tilt="-30" 
    Rotation="45"
    Depth="30" 
    PerspectiveAngle="90" 
    Width="500" 
    Height="500">
    
    <chart:SfChart3D.PrimaryAxis>
        <chart:CategoryAxis3D Header="Year"/>
    </chart:SfChart3D.PrimaryAxis>
    
    <chart:SfChart3D.SecondaryAxis>
        <chart:NumericalAxis3D Header="Sales"/>
    </chart:SfChart3D.SecondaryAxis>
    
    <!-- Enable explode on click -->
    <chart:PieSeries3D  
        ExplodeOnMouseClick="True" 
        ItemsSource="{Binding CategoricalData}"  
        XBindingPath="Year" 
        YBindingPath="Iron">
    </chart:PieSeries3D>
    
</chart:SfChart3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    ExplodeOnMouseClick = true
};

chart3D.Series.Add(series);
```

**Behavior:**
- Click a segment to explode it outward
- Click exploded segment to return to normal position
- Only one segment exploded at a time (unless ExplodeAll is true)

### Explode Specific Segment

Programmatically explode a segment by index:

**XAML:**
```xml
<chart:PieSeries3D  
    ExplodeIndex="2"
    ExplodeRadius="10"
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:PieSeries3D>
```

**C#:**
```csharp
PieSeries3D series = new PieSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    ExplodeIndex = 2,      // Explode third segment (0-based)
    ExplodeRadius = 10     // Distance of explosion
};

chart3D.Series.Add(series);
```

### Explode All Segments

Explode all segments simultaneously:

**XAML:**
```xml
<chart:DoughnutSeries3D  
    ExplodeAll="True"
    ExplodeRadius="3"
    ItemsSource="{Binding CategoricalData}"  
    XBindingPath="Year" 
    YBindingPath="Iron">
</chart:DoughnutSeries3D>
```

**C#:**
```csharp
DoughnutSeries3D series = new DoughnutSeries3D()
{
    ItemsSource = new CategoryDataViewModel().CategoricalData,
    XBindingPath = "Year",
    YBindingPath = "Iron",
    ExplodeAll = true,
    ExplodeRadius = 3
};

chart3D.Series.Add(series);
```

**Properties:**
- **ExplodeOnMouseClick** - Enable click-to-explode interaction
- **ExplodeIndex** - Index of segment to explode (0-based)
- **ExplodeAll** - Explode all segments
- **ExplodeRadius** - Distance of explosion (default: 30)

## Animation Support

SfChart3D supports animations for series loading and data updates, adding visual polish to your visualizations.

**Note:** Animation properties are inherited from the base SfChart control. Refer to 2D chart documentation for detailed animation configuration.

**Basic Animation:**
```xml
<chart:ColumnSeries3D 
    EnableAnimation="True"
    ItemsSource="{Binding Data}"/>
```

## Tooltips

Tooltips display data point information on hover or tap, configured at the series level.

**Enable Tooltips:**

**XAML:**
```xml
<chart:ColumnSeries3D 
    Label="Sales"
    ShowTooltip="True"
    ItemsSource="{Binding UsersList}" 
    XBindingPath="TimeStamp" 
    YBindingPath="NoOfUsers">
</chart:ColumnSeries3D>
```

**C#:**
```csharp
ColumnSeries3D series = new ColumnSeries3D()
{
    ItemsSource = new UsersViewModel().UsersList,
    XBindingPath = "TimeStamp",
    YBindingPath = "NoOfUsers",
    Label = "Sales",
    ShowTooltip = true
};
```

**Tooltip Content:**
- Series Label
- X value
- Y value
- (Z value if depth axis is used)

## User Interaction Patterns

### Pattern 1: Explore with Rotation
```xml
<chart:SfChart3D 
    EnableRotation="True"
    Rotation="45" 
    Tilt="15">
    <!-- Chart configuration -->
</chart:SfChart3D>
```
**Use Case:** Data exploration, Manhattan charts

### Pattern 2: Highlight on Select
```xml
<chart:ColumnSeries3D 
    SegmentSelectionBrush="Yellow"
    ShowTooltip="True">
    <!-- Series configuration -->
</chart:ColumnSeries3D>
```
**Use Case:** Data analysis, presentations

### Pattern 3: Interactive Circular Chart
```xml
<chart:PieSeries3D 
    ExplodeOnMouseClick="True"
    ShowTooltip="True"
    ExplodeRadius="8"
    EnableSmartLabels="True">
    <!-- Series configuration -->
</chart:PieSeries3D>
```
**Use Case:** Market share, budget allocation

### Pattern 4: Multi-Series Comparison
```xml
<chart:SfChart3D 
    EnableSeriesSelection="True"
    EnableRotation="True">
    <chart:ColumnSeries3D SeriesSelectionBrush="LightGreen"/>
    <chart:ColumnSeries3D SeriesSelectionBrush="LightCoral"/>
</chart:SfChart3D>
```
**Use Case:** Multi-dataset analysis

## Best Practices

### Performance

**Optimize for smooth interaction:**
- Limit data points for responsive rotation (500-1000 optimal)
- Use simpler series types for large datasets
- Disable animations for very large datasets
- Test rotation performance on target hardware

### User Experience

**Rotation:**
- Start with intuitive viewing angles (Rotation="45", Tilt="15")
- Provide rotation instructions for first-time users
- Allow rotation reset (set specific angles programmatically)

**Selection:**
- Use contrasting selection colors
- Combine with tooltips for detailed info
- Provide clear visual feedback
- Consider deselection behavior

**Explode:**
- Use moderate ExplodeRadius (5-10 pixels)
- Combine with tooltips and labels
- Consider ExplodeOnMouseClick for interactive scenarios
- Use ExplodeIndex to highlight important segments by default

### Accessibility

**Interactive features considerations:**
- Provide keyboard alternatives for mouse interactions
- Include ARIA labels for screen readers
- Don't rely solely on color for selection indication
- Test with various input devices

### Common Issues

**Issue: Rotation lag or stuttering**
- Reduce data point count
- Simplify visual effects
- Check hardware acceleration settings
- Optimize data binding

**Issue: Selection not visible**
- Use highly contrasting selection colors
- Increase selection brush opacity
- Verify SegmentSelectionBrush is set
- Check if palette is overriding selection

**Issue: Explode not working**
- Ensure series is PieSeries3D or DoughnutSeries3D
- Verify ExplodeOnMouseClick="True" or ExplodeIndex is set
- Check ExplodeRadius is > 0
- Test with simplified chart configuration

**Issue: Tooltips not appearing**
- Verify ShowTooltip="True"
- Check if data binding is working
- Ensure series has Label property set
- Test mouse hover position
