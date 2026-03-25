# Rendering Types and User Interactions

## Overview

This guide covers SmithChart rendering types and interactive features:

**Rendering Types:**
- Impedance rendering (default)
- Admittance rendering
- Understanding the differences

**User Interactions:**
- Tooltips for data points
- Custom tooltip templates
- Interactive features

## Rendering Types

SmithChart supports two rendering modes that determine how data is visualized.

### RenderingType Property

Control how transmission line data is plotted:

```xml
<syncfusion:SfSmithChart RenderingType="Impedance">
</syncfusion:SfSmithChart>
```

```csharp
chart.RenderingType = RenderingType.Impedance;  // Default
// or
chart.RenderingType = RenderingType.Admittance;
```

### Impedance Rendering (Default)

**Characteristics:**
- Normalized resistance circles drawn from **right to left**
- Normalized reactance curves drawn from **right to left**
- Axis labels range from **left to right**
- Standard Smith Chart representation
- Most common for impedance analysis

**Example:**

```xml
<syncfusion:SfSmithChart RenderingType="Impedance">
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data}"/>
</syncfusion:SfSmithChart>
```

```csharp
SfSmithChart chart = new SfSmithChart();
chart.RenderingType = RenderingType.Impedance;

LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance"
};
chart.Series.Add(series);
```

**Use Impedance when:**
- Analyzing impedance matching
- Standard transmission line impedance measurements
- Working with load impedance
- Following conventional Smith Chart usage

### Admittance Rendering

**Characteristics:**
- Normalized resistance circles drawn from **left to right**
- Normalized reactance curves drawn from **left to right**
- Axis labels range from **right to left**
- Inverted Smith Chart representation
- Used for admittance analysis

**Example:**

```xml
<syncfusion:SfSmithChart RenderingType="Admittance">
    <syncfusion:LineSeries ResistancePath="Resistance" 
                           ReactancePath="Reactance" 
                           ItemsSource="{Binding Data}"/>
</syncfusion:SfSmithChart>
```

```csharp
SfSmithChart chart = new SfSmithChart();
chart.RenderingType = RenderingType.Admittance;

LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance"
};
chart.Series.Add(series);
```

**Use Admittance when:**
- Analyzing admittance (inverse of impedance)
- Working with parallel circuits
- Conductance and susceptance measurements
- Specific engineering requirements

### Impedance vs Admittance Comparison

| Aspect | Impedance | Admittance |
|--------|-----------|------------|
| Direction | Right to Left | Left to Right |
| Label Order | Left to Right | Right to Left |
| Use Case | Standard impedance | Inverse (admittance) |
| Common Usage | Most common | Specialized |
| Data Input | Same (Z data) | Same (Y data) |

### Switching Rendering Types

You can change rendering type at runtime:

```csharp
// Toggle between modes
if (chart.RenderingType == RenderingType.Impedance)
{
    chart.RenderingType = RenderingType.Admittance;
}
else
{
    chart.RenderingType = RenderingType.Impedance;
}
```

**Note:** Changing rendering type redraws the chart automatically. All other features (series, markers, labels, legend) work the same in both modes.

### Data Considerations

**Important:** The same data model works for both rendering types:
- ResistancePath and ReactancePath remain the same
- Data values stay unchanged
- Only visualization orientation changes

```csharp
// Same data works for both rendering types
public class TransmissionData
{
    public double Resistance { get; set; }
    public double Reactance { get; set; }
}

public class TransmissionViewModel
{
    private ObservableCollection<TransmissionData> _data;

    public ObservableCollection<TransmissionData> Data
    {
        get { return _data; }
        set
        {
            _data = value;
            OnPropertyChanged(nameof(Data));
        }
    }

    public TransmissionViewModel()
    {
        // Initialize with sample transmission line data
        Data = new ObservableCollection<TransmissionData>
        {
            new TransmissionData { Resistance = 0.0, Reactance = 0.05 },
            new TransmissionData { Resistance = 0.3, Reactance = 0.1 },
            new TransmissionData { Resistance = 0.5, Reactance = 0.2 },
            new TransmissionData { Resistance = 1.0, Reactance = 0.4 },
            new TransmissionData { Resistance = 1.5, Reactance = 0.5 },
            new TransmissionData { Resistance = 2.0, Reactance = 0.5 },
            new TransmissionData { Resistance = 2.5, Reactance = 0.4 },
            new TransmissionData { Resistance = 3.5, Reactance = 0.0 }
        };
    }
}

// Use with Impedance
chart.RenderingType = RenderingType.Impedance;
series.ItemsSource = transmissionData;

// Or use with Admittance
chart.RenderingType = RenderingType.Admittance;
series.ItemsSource = transmissionData;  // Same data
```

## User Interactions

### Tooltips

Tooltips provide information about data points on hover.

#### Enabling Tooltips

```xml
<syncfusion:LineSeries ShowToolTip="True" 
                       ToolTipDuration="0:0:3">
</syncfusion:LineSeries>
```

```csharp
series.ShowToolTip = true;
series.ToolTipDuration = TimeSpan.FromSeconds(3);
```

#### Tooltip Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowToolTip` | bool | false | Enable/disable tooltips |
| `ToolTipDuration` | TimeSpan | - | How long tooltip stays visible |
| `ToolTipTemplate` | DataTemplate | null | Custom tooltip layout |

#### Tooltip Duration

Control how long tooltips remain visible:

```csharp
// Short display (1 second)
series.ToolTipDuration = TimeSpan.FromSeconds(1);

// Standard display (3 seconds)
series.ToolTipDuration = TimeSpan.FromSeconds(3);

// Long display (5 seconds)
series.ToolTipDuration = TimeSpan.FromSeconds(5);

// Very brief (500 milliseconds)
series.ToolTipDuration = TimeSpan.FromMilliseconds(500);
```

#### Default Tooltip Appearance

By default, tooltips display:
- Resistance value
- Reactance value
- Simple formatted text

Example default tooltip content:
```
Resistance: 1.5
Reactance: 0.5
```

### Custom Tooltip Templates

Create rich, customized tooltips with DataTemplate.

#### Basic Custom Tooltip

```xml
<syncfusion:SfSmithChart>
    <syncfusion:SfSmithChart.Resources>
        <DataTemplate x:Key="tooltipTemplate">
            <Border CornerRadius="4" 
                    Background="{Binding Interior}" 
                    BorderBrush="Yellow" 
                    BorderThickness="2">
                <Grid HorizontalAlignment="Center" Margin="8,8,8,8">
                    <Grid.RowDefinitions>
                        <RowDefinition/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition/>
                        <ColumnDefinition/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    
                    <TextBlock Grid.Row="0" Grid.Column="0" 
                               Text="Resistance" 
                               Foreground="Black"  
                               FontSize="13"/>
                    <TextBlock Grid.Row="0" Grid.Column="1" 
                               Text=" : " 
                               Foreground="Black"  
                               FontSize="13" 
                               Margin="0,-1,0,0"/>
                    <TextBlock Grid.Row="0" Grid.Column="2" 
                               Text="{Binding Resistance}" 
                               Foreground="Black"  
                               FontSize="13" 
                               Margin="3,0,0,0"/>
                    
                    <TextBlock Grid.Row="1" Grid.Column="0" 
                               Text="Reactance" 
                               Foreground="Black"  
                               FontSize="13"/>
                    <TextBlock Grid.Row="1" Grid.Column="1" 
                               Text=" : " 
                               Foreground="Black" 
                               FontSize="13" 
                               Margin="0,-1,0,0"/>
                    <TextBlock Grid.Row="1" Grid.Column="2" 
                               Text="{Binding Reactance}" 
                               Margin="3,0,0,0" 
                               Foreground="Black" 
                               FontSize="13"/>
                </Grid>
            </Border>
        </DataTemplate>
    </syncfusion:SfSmithChart.Resources>
    
    <syncfusion:LineSeries ShowToolTip="True" 
                           ToolTipTemplate="{StaticResource tooltipTemplate}"
                           ItemsSource="{Binding Data}" 
                           ResistancePath="Resistance" 
                           ReactancePath="Reactance">
    </syncfusion:LineSeries>
</syncfusion:SfSmithChart>
```

```csharp
DataTemplate tooltipTemplate = /* define template */;
series.ShowToolTip = true;
series.ToolTipTemplate = tooltipTemplate;
```

#### Tooltip Template Binding Context

Available binding properties in tooltip template:
- **Resistance**: Resistance value
- **Reactance**: Reactance value
- **Interior**: Series color
- **ForeColor**: Suggested text color
- **Background**: Default tooltip background
- **BorderBrush**: Default tooltip border

#### Advanced Tooltip Examples
**Color-Coded Tooltip:**
```xml
<DataTemplate x:Key="coloredTooltip">
    <Border Background="{Binding Interior}" 
            BorderBrush="White" 
            BorderThickness="2" 
            CornerRadius="8" 
            Padding="12,6">
        <StackPanel>
            <TextBlock Foreground="White" 
                       FontWeight="Bold" 
                       FontSize="11"
                       TextAlignment="Center">
                <Run Text="Z = "/>
                <Run Text="{Binding ResistancePath}"/>
                <Run Text=" + j"/>
                <Run Text="{Binding ReactancePath}"/>
            </TextBlock>
        </StackPanel>
    </Border>
</DataTemplate>
```

**Detailed Tooltip with Calculations:**
```xml
<DataTemplate x:Key="detailedTooltip">
    <Border Background="LightYellow" 
            BorderBrush="Orange" 
            BorderThickness="2" 
            CornerRadius="6" 
            Padding="10">
        <StackPanel>
            <TextBlock FontWeight="Bold" 
                       FontSize="12" 
                       Foreground="DarkBlue">
                Impedance Point
            </TextBlock>
            <Line Stroke="Orange" 
                  StrokeThickness="1" 
                  X2="120" 
                  Margin="0,4"/>
            <TextBlock FontSize="11">
                <Run Text="Resistance: "/>
                <Run Text="{Binding ResistancePath, StringFormat='{}{0:F2}'}" 
                     FontWeight="Bold"/>
                <Run Text=" Ω"/>
            </TextBlock>
            <TextBlock FontSize="11">
                <Run Text="Reactance: "/>
                <Run Text="{Binding ReactancePath, StringFormat='{}{0:F2}'}" 
                     FontWeight="Bold"/>
                <Run Text=" Ω"/>
            </TextBlock>
        </StackPanel>
    </Border>
</DataTemplate>
```

### Tooltip Best Practices

1. **Keep tooltips concise** - Show only essential information
2. **Use readable fonts** - Minimum 10-11pt font size
3. **Ensure contrast** - Text must be readable against background
4. **Add appropriate padding** - Don't cram content
5. **Consider duration** - 2-3 seconds is usually sufficient
6. **Test with real data** - Ensure all values display correctly
7. **Match series color** - Use {Binding Interior} for consistency

### Interactive Tooltip Example

```csharp
LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    ShowMarker = true,
    MarkerType = MarkerType.Circle,
    ShowToolTip = true,
    ToolTipDuration = TimeSpan.FromSeconds(3),
    Label = "Transmission Line"
};

// Custom tooltip will show on hover over data points
chart.Series.Add(series);
```

## Common Patterns

### Pattern 1: Impedance Chart with Tooltips

```csharp
SfSmithChart chart = new SfSmithChart
{
    Header = "Impedance Analysis",
    RenderingType = RenderingType.Impedance
};

LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Resistance",
    ReactancePath = "Reactance",
    ShowMarker = true,
    ShowToolTip = true,
    ToolTipDuration = TimeSpan.FromSeconds(3)
};

chart.Series.Add(series);
```

### Pattern 2: Admittance Chart

```csharp
SfSmithChart chart = new SfSmithChart
{
    Header = "Admittance Analysis",
    RenderingType = RenderingType.Admittance
};

LineSeries series = new LineSeries
{
    ItemsSource = Data,
    ResistancePath = "Conductance",  // G (admittance real part)
    ReactancePath = "Susceptance",   // B (admittance imaginary part)
    ShowMarker = true
};

chart.Series.Add(series);
```

### Pattern 3: Interactive Comparison Chart

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Toggle Button -->
    <Button Grid.Row="0" Content="Switch Rendering Type" 
            Click="ToggleRendering_Click" Margin="10"/>
    
    <!-- Chart -->
    <syncfusion:SfSmithChart x:Name="chart" Grid.Row="1" 
                             RenderingType="Impedance">
        <syncfusion:LineSeries ShowToolTip="True" 
                               ShowMarker="True"
                               ItemsSource="{Binding Data}"
                               ResistancePath="Resistance"
                               ReactancePath="Reactance"/>
    </syncfusion:SfSmithChart>
</Grid>
```

```csharp
private void ToggleRendering_Click(object sender, RoutedEventArgs e)
{
    if (chart.RenderingType == RenderingType.Impedance)
    {
        chart.RenderingType = RenderingType.Admittance;
        chart.Header = "Admittance View";
    }
    else
    {
        chart.RenderingType = RenderingType.Impedance;
        chart.Header = "Impedance View";
    }
}
```

### Pattern 4: Rich Tooltip Information

```xml
<syncfusion:SfSmithChart.Resources>
    <DataTemplate x:Key="richTooltip">
        <Border Background="White" 
                BorderBrush="{Binding Interior}" 
                BorderThickness="3" 
                CornerRadius="8" 
                Padding="15">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="5"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <!-- Header -->
                <TextBlock Grid.Row="0" 
                           Text="Transmission Line Data" 
                           FontWeight="Bold" 
                           FontSize="12"
                           Foreground="{Binding Interior}"/>
                
                <!-- Separator -->
                <Rectangle Grid.Row="1" 
                           Fill="{Binding Interior}" 
                           Height="2" 
                           Margin="0,3"/>
                
                <!-- Resistance -->
                <StackPanel Grid.Row="2" Orientation="Horizontal">
                    <TextBlock Text="Resistance: " 
                               FontSize="11" 
                               Foreground="Gray"/>
                    <TextBlock Text="{Binding ResistancePath, StringFormat='{}{0:F2} Ω'}" 
                               FontSize="11" 
                               FontWeight="Bold" 
                               Foreground="Black"/>
                </StackPanel>
                
                <!-- Reactance -->
                <StackPanel Grid.Row="3" Orientation="Horizontal" Margin="0,3,0,0">
                    <TextBlock Text="Reactance: " 
                               FontSize="11" 
                               Foreground="Gray"/>
                    <TextBlock Text="{Binding ReactancePath, StringFormat='{}{0:F2} Ω'}" 
                               FontSize="11" 
                               FontWeight="Bold" 
                               Foreground="Black"/>
                </StackPanel>
            </Grid>
        </Border>
    </DataTemplate>
</syncfusion:SfSmithChart.Resources>

<syncfusion:LineSeries ShowToolTip="True"
                       ToolTipTemplate="{StaticResource richTooltip}"
                       ToolTipDuration="0:0:4"/>
```

## Troubleshooting

### Tooltips Not Showing

**Issue**: ShowToolTip is true but tooltips don't appear  
**Solutions**:
- Ensure markers are visible or cursor is over the data line
- Check if ToolTipDuration is set to a reasonable value
- Verify data points exist (ItemsSource not empty)
- Try hovering directly over data markers

### Tooltip Template Not Displaying

**Issue**: Custom template defined but not showing  
**Solutions**:
- Verify ToolTipTemplate resource key matches
- Ensure template is in correct Resources section
- Check binding paths (Resistance, Reactance, Interior)
- Test with default tooltip first

### Wrong Rendering Orientation

**Issue**: Chart appears backwards or inverted  
**Solutions**:
- Check RenderingType is set correctly
- Impedance: standard orientation (right to left)
- Admittance: inverted orientation (left to right)
- Verify you're using the intended mode for your analysis

### Rendering Type Not Changing

**Issue**: Setting RenderingType has no effect  
**Solutions**:
- Ensure property is set before data is loaded
- Try setting in constructor or XAML
- Verify chart is refreshing after property change
- Check if binding is preventing direct set

## Summary

Rendering types and interactions enhance SmithChart functionality:

**Rendering Types:**
- **Impedance**: Standard mode, right-to-left plotting, most common
- **Admittance**: Inverted mode, left-to-right plotting, for admittance analysis
- Same data works for both modes
- Easy runtime switching between modes

**User Interactions:**
- **Tooltips**: Show data point information on hover
- **Duration Control**: Configure tooltip display time
- **Custom Templates**: Create rich, formatted tooltips
- **Enhanced UX**: Improve data point inspection and analysis

These features make SmithChart interactive and adaptable to different analysis requirements, whether working with impedance or admittance data.