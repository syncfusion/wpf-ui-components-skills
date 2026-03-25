# Interactive Features

Add user interaction capabilities to charts including tooltips, zooming, panning, crosshair, trackball, and selection.

## Table of Contents
- [Tooltip](#tooltip)
- [Zooming and Panning](#zooming-and-panning)
- [Crosshair](#crosshair)
- [Trackball](#trackball)
- [Selection](#selection)
- [Visual Data Editing](#visual-data-editing)

## Tooltip

Display data point information on hover.

### Basic Tooltip

```xml
<syncfusion:LineSeries ItemsSource="{Binding Data}"
                       XBindingPath="Month"
                       YBindingPath="Sales"
                       ShowTooltip="True"/>
```

### Custom Tooltip Template

```xml
<syncfusion:ColumnSeries ShowTooltip="True">
    <syncfusion:ColumnSeries.TooltipTemplate>
        <DataTemplate>
            <Border Background="DarkBlue" 
                    BorderBrush="White" 
                    BorderThickness="1"
                    CornerRadius="3"
                    Padding="8">
                <StackPanel>
                    <TextBlock Text="{Binding Item.Month}" 
                               Foreground="White" 
                               FontWeight="Bold"/>
                    <TextBlock Text="{Binding Item.Sales, StringFormat='${0:N0}'}" 
                               Foreground="White"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:ColumnSeries.TooltipTemplate>
</syncfusion:ColumnSeries>
```

## Zooming and Panning

Enable chart zooming and panning for data exploration.

### Enable Zooming

```xml
<syncfusion:SfChart>
    <!-- Axes and Series -->
    
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartZoomPanBehavior EnableMouseWheelZooming="True"
                                          EnablePanning="True"
                                          EnableSelectionZooming="True"
                                          EnablePinchZooming="True"/>
    </syncfusion:SfChart.Behaviors>
</syncfusion:SfChart>
```

### Zooming Modes

**Mouse Wheel Zooming:**
```xml
<syncfusion:ChartZoomPanBehavior EnableMouseWheelZooming="True"/>
```

**Selection Zooming:**
```xml
<syncfusion:ChartZoomPanBehavior EnableSelectionZooming="True"/>
```

**Pinch Zooming (Touch):**
```xml
<syncfusion:ChartZoomPanBehavior EnablePinchZooming="True"/>
```

### Zoom Mode

Control zoom direction:

```xml
<syncfusion:ChartZoomPanBehavior ZoomMode="X"/>
```

**Options:** `X`, `Y`, `XY` (default)

### Reset Zooming

```xml
<syncfusion:ChartZoomPanBehavior ResetOnDoubleTap="True"/>
```

## Crosshair

Display vertical and horizontal lines at cursor position.

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartCrosshairBehavior/>
    </syncfusion:SfChart.Behaviors>
</syncfusion:SfChart>
```

### Crosshair Customization

```xml
<syncfusion:ChartCrosshairBehavior ShowTrackballInfo="True">
    <syncfusion:ChartCrosshairBehavior.HorizontalLineStyle>
        <Style TargetType="Line">
            <Setter Property="Stroke" Value="Red"/>
            <Setter Property="StrokeThickness" Value="1"/>
            <Setter Property="StrokeDashArray" Value="2,2"/>
        </Style>
    </syncfusion:ChartCrosshairBehavior.HorizontalLineStyle>
    
    <syncfusion:ChartCrosshairBehavior.VerticalLineStyle>
        <Style TargetType="Line">
            <Setter Property="Stroke" Value="Red"/>
            <Setter Property="StrokeThickness" Value="1"/>
            <Setter Property="StrokeDashArray" Value="2,2"/>
        </Style>
    </syncfusion:ChartCrosshairBehavior.VerticalLineStyle>
</syncfusion:ChartCrosshairBehavior>
```

## Trackball

Show data point information for all series at cursor position.

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartTrackBallBehavior/>
    </syncfusion:SfChart.Behaviors>
</syncfusion:SfChart>
```

### Trackball Customization

```xml
<syncfusion:ChartTrackBallBehavior ShowLine="True" LabelDisplayMode="FloatAllPoints"
                                    LineStyle="{StaticResource lineStyle}">
</syncfusion:ChartTrackBallBehavior>
```

**LabelDisplayMode options:**
- `NearestPoint` - Show only nearest point
- `FloatAllPoints` - Show all points
- `GroupAllPoints` - Group labels together

## Selection

Enable selecting data points or series.

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartSelectionBehavior/>
    </syncfusion:SfChart.Behaviors>
</syncfusion:SfChart>
```

### Enable Selection on Series

```xml
<syncfusion:ColumnSeries EnableSegmentSelection="True"/>
```

### Selection Mode

```xml
<syncfusion:ChartSelectionBehavior SelectionMode="MouseClick"/>
```

**Options:** `MouseClick`, `MouseMove`

### Series Selection

Select entire series:

```xml
<syncfusion:LineSeries EnableSeriesSelection="True"/>
```

### Selection Style

```xml
<syncfusion:ColumnSeries EnableSegmentSelection="True" SegmentSelectionBrush="Green">
</syncfusion:ColumnSeries>
```

### Handle Selection Changed Event

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartSelectionBehavior SelectionChanged="OnSelectionChanged"/>
    </syncfusion:SfChart.Behaviors>
</syncfusion:SfChart>
```

```csharp
private void OnSelectionChanged(object sender, ChartSelectionChangedEventArgs e)
{
    var selectedIndex = e.SelectedIndex;
    var selectedSeries = e.SelectedSeries;
    // Handle selection
}
```

## Visual Data Editing

Allow users to drag data points to edit values.

```xml
<syncfusion:SfChart>
    <syncfusion:LineSeries EnableSegmentDragging="True"/>
</syncfusion:SfChart>
```

### Data Editing Events

```xml
<syncfusion:LineSeries EnableSegmentDragging="True"
                       DragStart="OnDragStart"
                       DragDelta="OnDragDelta"
                       DragEnd="OnDragEnd"/>
```

```csharp
private void OnDragEnd(object sender, ChartDragEndEventArgs e)
{
    // Update underlying data model
    var newValue = e.NewYValue  ;
}
```

## Combining Multiple Behaviors

Use multiple interactive features together:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Behaviors>
        <!-- Zooming and Panning -->
        <syncfusion:ChartZoomPanBehavior EnableMouseWheelZooming="True"
                                          EnablePanning="True"
                                          EnableSelectionZooming="True"/>
        
        <!-- Trackball -->
        <syncfusion:ChartTrackBallBehavior ShowLine="True"/>
        
        <!-- Selection -->
        <syncfusion:ChartSelectionBehavior/>
    </syncfusion:SfChart.Behaviors>
    
    <!-- Series with tooltip -->
    <syncfusion:LineSeries ShowTooltip="True"
                           EnableSegmentSelection="True"/>
</syncfusion:SfChart>
```

## Resizable Scrollbar

Add a scrollbar for navigation:

```xml
<syncfusion:SfChart>
    <syncfusion:SfChart.Behaviors>
        <syncfusion:ChartZoomPanBehavior EnableSelectionZooming="True"/>
    </syncfusion:SfChart.Behaviors>
    
    <syncfusion:SfChart.PrimaryAxis>
        <syncfusion:DateTimeAxis EnableScrollBar="True"/>
    </syncfusion:SfChart.PrimaryAxis>
</syncfusion:SfChart>
```

## Key Properties Summary

### Tooltip
- `ShowTooltip` - Enable tooltip
- `TooltipTemplate` - Custom tooltip template

### Zooming
- `EnableMouseWheelZooming` - Zoom with mouse wheel
- `EnableSelectionZooming` - Zoom by selecting region
- `EnablePinchZooming` - Touch pinch zoom
- `EnablePanning` - Pan when zoomed
- `ZoomMode` - X, Y, or XY zoom
- `ResetOnDoubleTap` - Reset zoom on double-click

### Selection
- `EnableSegmentSelection` - Select data points
- `EnableSeriesSelection` - Select entire series
- `SelectionMode` - MouseClick or MouseMove
- `SegmentSelectionBrush` - Color for selected segments

### Visual Editing
- `EnableSegmentDragging` - Allow dragging data points
- `DragStart/Delta/End` - Drag events
