# Striplines

Striplines are vertical or horizontal bands that highlight specific regions on chart axes.

## Basic Stripline

Add a stripline to highlight a specific value range:

```xml
<syncfusion:NumericalAxis>
    <syncfusion:NumericalAxis.StripLines>
        <syncfusion:ChartStripLine Start="40000" 
                                   Width="20000"
                                   Background="LightBlue"
                                   Opacity="0.5"/>
    </syncfusion:NumericalAxis.StripLines>
</syncfusion:NumericalAxis>
```

## Stripline Properties

### Start and Width

```xml
<syncfusion:ChartStripLine Start="50000"
                           Width="10000"
                           Background="LightGreen"/>
```

- **Start:** Beginning value on the axis
- **Width:** Width in axis units

### Color and Opacity

```xml
<syncfusion:ChartStripLine Start="0"
                           Width="30000"
                           Background="Red"
                           Opacity="0.3"/>
```

## Stripline with Border

```xml
<syncfusion:ChartStripLine Start="60000"
                           Width="20000"
                           Background="Yellow"
                           BorderBrush="Orange"
                           BorderThickness="2"/>
```

## Stripline Label

Add text labels to striplines:

```xml
<syncfusion:ChartStripLine Start="40000"
                           Width="20000"
                           Background="LightBlue"
                           Text="Target Range"
                           LabelAngle ="0">
</syncfusion:ChartStripLine>
```

### Label Positioning

```xml
<syncfusion:ChartStripLine Text="Average"
                           LabelHorizontalAlignment="Center"
                           LabelVerticalAlignment="Middle"/>
```

## Multiple Striplines

Add several striplines to one axis:

```xml
<syncfusion:NumericalAxis>
    <syncfusion:NumericalAxis.StripLines>
        <!-- Below target -->
        <syncfusion:ChartStripLine Start="0" 
                                   Width="40000"
                                   Background="LightCoral"
                                   Opacity="0.3"
                                   Text="Below Target"/>
        
        <!-- Target zone -->
        <syncfusion:ChartStripLine Start="40000" 
                                   Width="20000"
                                   Background="LightGreen"
                                   Opacity="0.3"
                                   Text="Target Zone"/>
        
        <!-- Above target -->
        <syncfusion:ChartStripLine Start="60000" 
                                   Width="40000"
                                   Background="LightBlue"
                                   Opacity="0.3"
                                   Text="Exceeded"/>
    </syncfusion:NumericalAxis.StripLines>
</syncfusion:NumericalAxis>
```

## Recurring Striplines

Create repeating striplines:

```xml
<syncfusion:ChartStripLine Start="0"
                           Width="10000"
                           RepeatEvery="20000"
                           RepeatUntil="100000"
                           Background="LightGray"
                           Opacity="0.2"/>
```

This creates striplines at 0-10k, 20k-30k, 40k-50k, etc.

## Stripline on DateTime Axis

```xml
<syncfusion:DateTimeAxis>
    <syncfusion:DateTimeAxis.StripLines>
        <syncfusion:ChartStripLine x:Name="stripLine"  
                                   Width="7"
                                   Background="Yellow"
                                   Opacity="0.3"
                                   Text="Holiday Week"/>
    </syncfusion:DateTimeAxis.StripLines>
</syncfusion:DateTimeAxis>
```

```C#
stripLine.Start = new DateTime(2009, 1, 3).ToOADate(); 
```


## Stripline on Category Axis

```xml
<syncfusion:CategoryAxis>
    <syncfusion:CategoryAxis.StripLines>
        <syncfusion:ChartStripLine Start="2"
                                   Width="2"
                                   Background="LightGreen"
                                   Opacity="0.4"
                                   Text="Q2"/>
    </syncfusion:CategoryAxis.StripLines>
</syncfusion:CategoryAxis>
```

## Stripline Segmentation

Segment striplines for stacked series:

```xml
<syncfusion:ChartStripLine Start="10"
                           Width="5"
                           IsSegmented="True"
                           SegmentStartValue="100"
                           SegmentEndValue="200"
                           Background="LightBlue"/>
```

## Complete Example

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:CategoryAxis/>
    </syncfusion:SfChart.PrimaryAxis>
    
    <syncfusion:SfChart.SecondaryAxis>
        <syncfusion:NumericalAxis>
            <syncfusion:NumericalAxis.StripLines>
                <!-- Critical zone -->
                <syncfusion:ChartStripLine Start="0"
                                           Width="30000"
                                           Background="Red"
                                           Opacity="0.2"
                                           Text="Critical"
                                           LabelVerticalAlignment="Top">
                  
                </syncfusion:ChartStripLine>
                
                <!-- Warning zone -->
                <syncfusion:ChartStripLine Start="30000"
                                           Width="20000"
                                           Background="Yellow"
                                           Opacity="0.2"
                                           Text="Warning"/>
                
                <!-- Safe zone -->
                <syncfusion:ChartStripLine Start="50000"
                                           Width="50000"
                                           Background="Green"
                                           Opacity="0.2"
                                           Text="Safe"/>
            </syncfusion:NumericalAxis.StripLines>
        </syncfusion:NumericalAxis>
    </syncfusion:SfChart.SecondaryAxis>
    
    <!-- Series -->
    <syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                             XBindingPath="Month"
                             YBindingPath="Value"/>
</syncfusion:SfChart>
```

## Common Stripline Patterns

### Pattern 1: Average Line

```xml
<syncfusion:ChartStripLine Start="50000"
                           Width="0"
                           BorderBrush="Red"
                           BorderThickness="2"
                           Text="Average"
                           LabelHorizontalAlignment="End"/>
```

### Pattern 2: Target Zone

```xml
<syncfusion:ChartStripLine Start="45000"
                           Width="10000"
                           Background="LightGreen"
                           Opacity="0.3"
                           BorderBrush="Green"
                           BorderThickness="1"
                           Text="Target: 45K - 55K"/>
```

### Pattern 3: Alternating Bands

```xml
<syncfusion:ChartStripLine Start="0"
                           Width="20000"
                           RepeatEvery="40000"
                           RepeatUntil="100000"
                           Background="LightGray"
                           Opacity="0.1"/>
```

## Key Properties

| Property | Description |
|----------|-------------|
| `Start` | Starting value on axis |
| `Width` | Width in axis units |
| `WidthType` | Unit type for DateTime axis |
| `Background` | Fill color |
| `Opacity` | Transparency (0-1) |
| `BorderBrush` | Border color |
| `BorderThickness` | Border width |
| `Text` | Label text |
| `LabelAngle` | Label rotation |
| `LabelHorizontalAlignment` | Label horizontal position |
| `LabelVerticalAlignment` | Label vertical position |
| `RepeatEvery` | Repeat interval |
| `RepeatUntil` | Repeat until value |
| `IsSegmented` | Enable segmentation |
