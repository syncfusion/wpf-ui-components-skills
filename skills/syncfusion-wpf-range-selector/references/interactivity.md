# Interactivity in Range Selectors

## Table of Contents
- [Overview](#overview)
- [Zoom and Pan Properties](#zoom-and-pan-properties)
- [Data Selection](#data-selection)
- [Events](#events)
- [Thumb Customization](#thumb-customization)
- [Bar Customization](#bar-customization)
- [Performance Features](#performance-features)

## Overview

The SfDateTimeRangeNavigator provides rich interactive features that allow users to explore large time-based datasets through zooming, panning, and range selection. The control features a resizable scrollbar that users can drag and resize to select specific time ranges, with all changes reflected in synchronized visualizations.

**Key Interactive Capabilities:**
- **Zooming** - Resize the scrollbar to focus on specific time periods
- **Panning** - Drag the scrollbar to navigate through the timeline
- **Range Selection** - Select precise date-time ranges with visual feedback
- **Data Filtering** - Retrieve only the data within the selected range
- **Event Handling** - Respond to user interactions programmatically
- **Visual Customization** - Style thumbs, bars, and other interactive elements

## Zoom and Pan Properties

### Core Zoom Properties

These properties control the zoom level and position of the selected range:

#### ZoomPosition
Represents the starting position of the selected range as a value between 0.0 and 1.0.
- **0.0** = Start of the entire data range
- **1.0** = End of the entire data range
- **0.5** = Middle of the data range

```xml
<syncfusion:SfDateTimeRangeNavigator 
    x:Name="RangeNavigator"
    ZoomPosition="0.2"
    ItemsSource="{Binding Data}"
    XBindingPath="Date"/>
```

#### ZoomFactor
Represents the width of the selected range as a value between 0.0 and 1.0.
- **1.0** = Entire data range selected
- **0.5** = Half of the data range selected
- **0.1** = 10% of the data range selected

```xml
<syncfusion:SfDateTimeRangeNavigator 
    x:Name="RangeNavigator"
    ZoomFactor="0.3"
    ItemsSource="{Binding Data}"
    XBindingPath="Date"/>
```

### Synchronizing with Chart Axes

The most common pattern is to bind the navigator's zoom properties to a chart's axis for synchronized navigation:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="150"/>
    </Grid.RowDefinitions>
    
    <!-- Main Chart -->
    <syncfusion:SfChart Grid.Row="0" x:Name="Chart">
        <syncfusion:SfChart.PrimaryAxis>
            <syncfusion:DateTimeAxis 
                Header="Date"
                ZoomPosition="{Binding ElementName=RangeNavigator, Path=ZoomPosition, Mode=TwoWay}"
                ZoomFactor="{Binding ElementName=RangeNavigator, Path=ZoomFactor, Mode=TwoWay}"/>
        </syncfusion:SfChart.PrimaryAxis>
        
        <syncfusion:SfChart.SecondaryAxis>
            <syncfusion:NumericalAxis Header="Value"/>
        </syncfusion:SfChart.SecondaryAxis>
        
        <syncfusion:LineSeries 
            ItemsSource="{Binding Data}"
            XBindingPath="Date"
            YBindingPath="Value"/>
    </syncfusion:SfChart>
    
    <!-- Range Navigator -->
    <syncfusion:SfDateTimeRangeNavigator 
        Grid.Row="1"
        x:Name="RangeNavigator"
        ItemsSource="{Binding Data}"
        XBindingPath="Date">
        
        <syncfusion:SfDateTimeRangeNavigator.Content>
            <syncfusion:SfLineSparkline 
                ItemsSource="{Binding Data}"
                YBindingPath="Value"/>
        </syncfusion:SfDateTimeRangeNavigator.Content>
    </syncfusion:SfDateTimeRangeNavigator>
</Grid>
```

**Key Points:**
- Use `Mode=TwoWay` binding for bidirectional synchronization
- Chart zooms when navigator changes
- Navigator updates when chart is zoomed programmatically

### Range Boundary Properties

#### Minimum and Maximum
Set the absolute boundaries of the navigator range:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    Minimum="2024-01-01"
    Maximum="2024-12-31"/>
```

```csharp
rangeNavigator.Minimum = new DateTime(2024, 1, 1);
rangeNavigator.Maximum = new DateTime(2024, 12, 31);
```

#### ViewRangeStart and ViewRangeEnd (Read-Only)
Get the actual start and end date-time values of the current selection:

```csharp
private void OnRangeChanged(object sender, EventArgs e)
{
    var navigator = sender as SfDateTimeRangeNavigator;
    
    DateTime rangeStart = navigator.ViewRangeStart;
    DateTime rangeEnd = navigator.ViewRangeEnd;
    
    MessageBox.Show($"Selected range: {rangeStart:d} to {rangeEnd:d}");
}
```

## Data Selection

### SelectedData Property

The `SelectedData` property returns a collection containing only the data points within the current selected range. This is extremely useful for filtering or analyzing specific portions of your dataset.

```csharp
public void ProcessSelectedRange()
{
    var selectedItems = RangeNavigator.SelectedData;
    
    if (selectedItems != null)
    {
        var dataPoints = selectedItems.Cast<DataPoint>().ToList();
        
        double average = dataPoints.Average(d => d.Value);
        double max = dataPoints.Max(d => d.Value);
        double min = dataPoints.Min(d => d.Value);
        
        ResultTextBlock.Text = $"Selected {dataPoints.Count} points\n" +
                              $"Average: {average:F2}\n" +
                              $"Max: {max:F2}\n" +
                              $"Min: {min:F2}";
    }
}
```

### Real-Time Data Filtering Example

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="150"/>
    </Grid.RowDefinitions>
    
    <syncfusion:SfChart Grid.Row="0" ItemsSource="{Binding ElementName=RangeNavigator, Path=SelectedData}">
        <syncfusion:SfChart.PrimaryAxis>
            <syncfusion:DateTimeAxis/>
        </syncfusion:SfChart.PrimaryAxis>
        <syncfusion:SfChart.SecondaryAxis>
            <syncfusion:NumericalAxis/>
        </syncfusion:SfChart.SecondaryAxis>
        <syncfusion:ColumnSeries XBindingPath="Date" YBindingPath="Value"/>
    </syncfusion:SfChart>
    
    <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="10">
        <TextBlock Text="Selected Data Points: "/>
        <TextBlock Text="{Binding ElementName=RangeNavigator, Path=SelectedData.Count}"/>
    </StackPanel>
    
    <syncfusion:SfDateTimeRangeNavigator 
        Grid.Row="2"
        x:Name="RangeNavigator"
        ItemsSource="{Binding Data}"
        XBindingPath="Date">
        <syncfusion:SfDateTimeRangeNavigator.Content>
            <syncfusion:SfLineSparkline ItemsSource="{Binding Data}" YBindingPath="Value"/>
        </syncfusion:SfDateTimeRangeNavigator.Content>
    </syncfusion:SfDateTimeRangeNavigator>
</Grid>
```

### ShowGridLines

Display vertical gridlines within the content area to align with time intervals:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ShowGridLines="True"/>
```

## Events

### ValueChanged Event

Triggered whenever the scrollbar position or size changes (user drag, resize, or programmatic change):

```xml
<syncfusion:SfDateTimeRangeNavigator 
    x:Name="RangeNavigator"
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    ValueChanged="OnRangeNavigatorValueChanged"/>
```

```csharp
private void OnRangeNavigatorValueChanged(object sender, EventArgs e)
{
    var navigator = sender as SfDateTimeRangeNavigator;
    
    // Get selected range
    DateTime start = navigator.ViewRangeStart;
    DateTime end = navigator.ViewRangeEnd;
    
    // Get zoom properties
    double position = navigator.ZoomPosition;
    double factor = navigator.ZoomFactor;
    
    // Update UI or perform calculations
    UpdateDashboard(start, end);
    
    // Get filtered data
    var selectedData = navigator.SelectedData;
    ProcessSelectedData(selectedData);
}
```

### LowerBarLabelsCreated Event

Triggered when the lower label bar's labels are generated or updated:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    LowerBarLabelsCreated="OnLowerBarLabelsCreated"/>
```

```csharp
private void OnLowerBarLabelsCreated(object sender, LowerBarLabelsCreatedEventArgs e)
{
    // Access and modify lower bar labels
    foreach (var label in e.Labels)
    {
        // Customize label text
        if (label.LabelContent is DateTime date)
        {
            label.LabelContent = date.ToString("MMM dd");
        }
    }
}
```

### UpperBarLabelsCreated Event

Triggered when the upper (higher) label bar's labels are generated or updated:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    UpperBarLabelsCreated="OnUpperBarLabelsCreated"/>
```

```csharp
private void OnUpperBarLabelsCreated(object sender, UpperBarLabelsCreatedEventArgs e)
{
    // Access and modify upper bar labels
    foreach (var label in e.Labels)
    {
        // Customize label appearance or content
        if (label.LabelContent is DateTime date)
        {
            label.LabelContent = date.Year.ToString();
        }
    }
}
```

## Thumb Customization

The navigator has two thumbs (left and right) that users drag to resize the selected range. These can be extensively customized.

### Thumb Structure

Each thumb consists of:
- **Symbol** - The visual handle users interact with
- **Line** - The vertical line extending through the content area

### SymbolTemplate

Customize the thumb symbol appearance using DataTemplates:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    x:Name="RangeNavigator"
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.Resources>
        <DataTemplate x:Key="CustomThumbSymbol">
            <Grid>
                <!-- Outer circle -->
                <Ellipse Height="40" Width="40" 
                         Fill="DodgerBlue" 
                         Stroke="White"
                         StrokeThickness="3"
                         VerticalAlignment="Center"/>
                
                <!-- Inner dot -->
                <Ellipse Height="8" Width="8" 
                         Fill="White" 
                         VerticalAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfDateTimeRangeNavigator.Resources>
    
    <!-- Apply to left thumb -->
    <syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
        <syncfusion:ThumbStyle SymbolTemplate="{StaticResource CustomThumbSymbol}"/>
    </syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
    
    <!-- Apply to right thumb -->
    <syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
        <syncfusion:ThumbStyle SymbolTemplate="{StaticResource CustomThumbSymbol}"/>
    </syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### Different Symbols for Each Thumb

```xml
<syncfusion:SfDateTimeRangeNavigator.Resources>
    <!-- Left thumb: Triangle pointing right -->
    <DataTemplate x:Key="LeftThumbSymbol">
        <Path Fill="Green" 
              Data="M 0,0 L 30,15 L 0,30 Z"
              Stroke="White"
              StrokeThickness="2"/>
    </DataTemplate>
    
    <!-- Right thumb: Triangle pointing left -->
    <DataTemplate x:Key="RightThumbSymbol">
        <Path Fill="Green" 
              Data="M 30,0 L 0,15 L 30,30 Z"
              Stroke="White"
              StrokeThickness="2"/>
    </DataTemplate>
</syncfusion:SfDateTimeRangeNavigator.Resources>

<syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
    <syncfusion:ThumbStyle SymbolTemplate="{StaticResource LeftThumbSymbol}"/>
</syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>

<syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
    <syncfusion:ThumbStyle SymbolTemplate="{StaticResource RightThumbSymbol}"/>
</syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
```

### LineStyle

Customize the vertical line that extends from the thumb through the content:

```xml
<Grid.Resources>
    <Style TargetType="Line" x:Key="LeftLineStyle">
        <Setter Property="Stroke" Value="Red"/>
        <Setter Property="StrokeThickness" Value="3"/>
        <Setter Property="StrokeDashArray" Value="4,2"/>
    </Style>
    
    <Style TargetType="Line" x:Key="RightLineStyle">
        <Setter Property="Stroke" Value="Blue"/>
        <Setter Property="StrokeThickness" Value="3"/>
        <Setter Property="StrokeDashArray" Value="4,2"/>
    </Style>
</Grid.Resources>

<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
        <syncfusion:ThumbStyle LineStyle="{StaticResource LeftLineStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
    
    <syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
        <syncfusion:ThumbStyle LineStyle="{StaticResource RightLineStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
    
</syncfusion:SfDateTimeRangeNavigator>
```

### Complete Thumb Customization Example

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date">
    
    <syncfusion:SfDateTimeRangeNavigator.Resources>
        <Style TargetType="Line" x:Key="ThumbLineStyle">
            <Setter Property="Stroke" Value="#FF4081"/>
            <Setter Property="StrokeThickness" Value="2"/>
        </Style>
        
        <DataTemplate x:Key="ThumbSymbol">
            <Grid>
                <Rectangle Height="50" Width="10" 
                          Fill="#FF4081" 
                          RadiusX="5" RadiusY="5"
                          Stroke="White"
                          StrokeThickness="2"/>
                <Line X1="5" Y1="15" X2="5" Y2="35" 
                      Stroke="White" StrokeThickness="2"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfDateTimeRangeNavigator.Resources>
    
    <syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
        <syncfusion:ThumbStyle 
            SymbolTemplate="{StaticResource ThumbSymbol}"
            LineStyle="{StaticResource ThumbLineStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.LeftThumbStyle>
    
    <syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
        <syncfusion:ThumbStyle 
            SymbolTemplate="{StaticResource ThumbSymbol}"
            LineStyle="{StaticResource ThumbLineStyle}"/>
    </syncfusion:SfDateTimeRangeNavigator.RightThumbStyle>
    
</syncfusion:SfDateTimeRangeNavigator>
```

## Bar Customization

### Grid Line Customization

Customize the vertical gridlines in the higher and lower label bars:

```xml
<Grid.Resources>
    <Style TargetType="Line" x:Key="HigherBarGridStyle">
        <Setter Property="Stroke" Value="Gray"/>
        <Setter Property="StrokeThickness" Value="1"/>
        <Setter Property="Opacity" Value="0.5"/>
    </Style>
    
    <Style TargetType="Line" x:Key="LowerBarGridStyle">
        <Setter Property="Stroke" Value="LightGray"/>
        <Setter Property="StrokeThickness" Value="1"/>
        <Setter Property="StrokeDashArray" Value="2,2"/>
    </Style>
</Grid.Resources>

<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    HigherBarGridLineStyle="{StaticResource HigherBarGridStyle}"
    LowerBarGridLineStyle="{StaticResource LowerBarGridStyle}"/>
```

### Tick Line Customization

Customize the small tick marks on label bars:

```xml
<Grid.Resources>
    <Style TargetType="Line" x:Key="HigherTickStyle">
        <Setter Property="Stroke" Value="DarkGray"/>
        <Setter Property="StrokeThickness" Value="2"/>
    </Style>
    
    <Style TargetType="Line" x:Key="LowerTickStyle">
        <Setter Property="Stroke" Value="Gray"/>
        <Setter Property="StrokeThickness" Value="1.5"/>
    </Style>
</Grid.Resources>

<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding Data}"
    XBindingPath="Date"
    HigherBarTickLineStyle="{StaticResource HigherTickStyle}"
    LowerBarTickLineStyle="{StaticResource LowerTickStyle}"/>
```

## Performance Features

### EnableDeferredUpdate

For large datasets or complex content, enable deferred updates to improve scrolling performance. When enabled, the content and events update only after the user stops interacting:

```xml
<syncfusion:SfDateTimeRangeNavigator 
    ItemsSource="{Binding LargeDataset}"
    XBindingPath="Date"
    EnableDeferredUpdate="True"
    DeferredUpdateDelay="500"/>
```

**How it works:**
1. User starts dragging or resizing the scrollbar
2. Updates are paused during the interaction
3. After user stops (500ms delay), updates are applied
4. Reduces expensive re-renders during active dragging

### DeferredUpdateDelay

Set the delay in milliseconds before applying updates after user interaction stops:

```csharp
rangeNavigator.EnableDeferredUpdate = true;
rangeNavigator.DeferredUpdateDelay = 300; // 300ms delay
```

**Recommended values:**
- **100-300ms** - Responsive feel for moderate datasets
- **500-1000ms** - Better performance for very large datasets
- **Test with your data** - Balance responsiveness vs performance

## Complete Interactive Example

```xml
<Window x:Class="RangeNavigatorDemo.InteractiveWindow"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
            <RowDefinition Height="150"/>
        </Grid.RowDefinitions>
        
        <!-- Info Panel -->
        <Border Grid.Row="0" Background="LightGray" Padding="10" Margin="0,0,0,10">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                
                <StackPanel Grid.Column="0">
                    <TextBlock Text="Range Start:" FontWeight="Bold"/>
                    <TextBlock x:Name="RangeStartText"/>
                </StackPanel>
                
                <StackPanel Grid.Column="1">
                    <TextBlock Text="Range End:" FontWeight="Bold"/>
                    <TextBlock x:Name="RangeEndText"/>
                </StackPanel>
                
                <StackPanel Grid.Column="2">
                    <TextBlock Text="Data Points:" FontWeight="Bold"/>
                    <TextBlock Text="{Binding ElementName=RangeNavigator, Path=SelectedData.Count}"/>
                </StackPanel>
            </Grid>
        </Border>
        
        <!-- Chart -->
        <syncfusion:SfChart Grid.Row="1" x:Name="Chart">
            <syncfusion:SfChart.PrimaryAxis>
                <syncfusion:DateTimeAxis 
                    ZoomPosition="{Binding ElementName=RangeNavigator, Path=ZoomPosition, Mode=TwoWay}"
                    ZoomFactor="{Binding ElementName=RangeNavigator, Path=ZoomFactor, Mode=TwoWay}"/>
            </syncfusion:SfChart.PrimaryAxis>
            
            <syncfusion:SfChart.SecondaryAxis>
                <syncfusion:NumericalAxis/>
            </syncfusion:SfChart.SecondaryAxis>
            
            <syncfusion:LineSeries 
                ItemsSource="{Binding Data}"
                XBindingPath="Date"
                YBindingPath="Value"/>
        </syncfusion:SfChart>
        
        <!-- Range Navigator -->
        <syncfusion:SfDateTimeRangeNavigator 
            Grid.Row="2"
            x:Name="RangeNavigator"
            ItemsSource="{Binding Data}"
            XBindingPath="Date"
            ShowGridLines="True"
            EnableDeferredUpdate="True"
            DeferredUpdateDelay="300"
            ValueChanged="OnNavigatorValueChanged">
            
            <syncfusion:SfDateTimeRangeNavigator.Content>
                <syncfusion:SfLineSparkline 
                    ItemsSource="{Binding Data}"
                    YBindingPath="Value"/>
            </syncfusion:SfDateTimeRangeNavigator.Content>
            
        </syncfusion:SfDateTimeRangeNavigator>
    </Grid>
</Window>
```

```csharp
private void OnNavigatorValueChanged(object sender, EventArgs e)
{
    var navigator = sender as SfDateTimeRangeNavigator;
    
    RangeStartText.Text = navigator.ViewRangeStart.ToString("MMM dd, yyyy");
    RangeEndText.Text = navigator.ViewRangeEnd.ToString("MMM dd, yyyy");
}
```

## Summary

You've learned how to:
- ✓ Use ZoomPosition and ZoomFactor for programmatic control
- ✓ Synchronize navigator with chart axes using two-way binding
- ✓ Retrieve selected data with SelectedData property
- ✓ Handle ValueChanged and label creation events
- ✓ Customize thumb symbols and lines
- ✓ Style higher and lower bars with gridlines and ticks
- ✓ Optimize performance with deferred updates
- ✓ Build complete interactive dashboards

Next: Explore [label-customization.md](label-customization.md) for interval types and label styling options.
