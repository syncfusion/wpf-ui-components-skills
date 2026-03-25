# User Interaction in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [Tooltips](#tooltips)
- [Zooming](#zooming)
- [Panning](#panning)
- [Selection](#selection)
- [Keyboard Support](#keyboard-support)
- [Complete Examples](#complete-examples)

## Overview

The WPF Maps control supports rich user interactions including tooltips, zoom, pan, and shape selection. These features enhance the user experience and enable exploration of geographical data.

**Supported interactions:**
- Tooltips on shapes, bubbles, and markers
- Mouse wheel zooming
- Click-and-drag panning
- Shape selection with visual feedback
- Keyboard shortcuts for zoom/pan
- Touch support for mobile/tablet scenarios

## Tooltips

Tooltips display additional information when hovering over map elements.

### ToolTipSettings Configuration

Enable tooltips using `ToolTipSettings` property on ShapeFileLayer.

**ValuePath** - Property to display in tooltip

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           ItemsSource="{Binding States}"
                           ShapeIDPath="Name"
                           ShapeIDTableField="STATE_NAME">
    <syncfusion:ShapeFileLayer.ToolTipSettings>
        <syncfusion:ToolTipSetting ValuePath="Population" />
    </syncfusion:ShapeFileLayer.ToolTipSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";
layer.ItemsSource = viewModel.States;
layer.ShapeIDPath = "Name";
layer.ShapeIDTableField = "STATE_NAME";

ToolTipSetting tooltipSetting = new ToolTipSetting();
tooltipSetting.ValuePath = "Population";
layer.ToolTipSettings = tooltipSetting;
```

### Custom Tooltip Template

For rich tooltips, use `ItemsTemplate`:

```xml
<syncfusion:ShapeFileLayer.ToolTipSettings>
    <syncfusion:ToolTipSetting ValuePath="Population" />
</syncfusion:ShapeFileLayer.ToolTipSettings>

<syncfusion:ShapeFileLayer.ItemsTemplate>
    <DataTemplate>
        <Border Background="#212121" 
                BorderBrush="#757575" 
                BorderThickness="1"
                CornerRadius="4"
                Padding="10,6">
            <StackPanel>
                <TextBlock Text="{Binding Name}" 
                           FontWeight="Bold" 
                           FontSize="13"
                           Foreground="White" />
                <TextBlock Text="{Binding Population, StringFormat='Population: {0:N0}'}" 
                           FontSize="11"
                           Foreground="#BDBDBD"
                           Margin="0,4,0,0" />
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:ShapeFileLayer.ItemsTemplate>
```

### Tooltip for Different Elements

**Shape tooltips:**
```xml
<syncfusion:ShapeFileLayer.ToolTipSettings>
    <syncfusion:ToolTipSetting ValuePath="Population" />
</syncfusion:ShapeFileLayer.ToolTipSettings>
```

**Bubble tooltips:** Automatically shown when BubbleMarkerSetting is configured  
**Marker tooltips:** Use MarkerTemplate to include tooltip content

## Zooming

Control zoom functionality to allow users to focus on map regions.

### Enable Zooming

Set `EnableZoom` property on SfMap.

**Type:** bool  
**Default:** true

```xml
<syncfusion:SfMap EnableZoom="True" />
```

```csharp
map.EnableZoom = true;
```

### Zoom Level Properties

**MinZoom** - Minimum zoom level (double, default: 1)  
**MaxZoom** - Maximum zoom level (double, default: 10)  
**ZoomLevel** - Current zoom level (double, default: 1)

```xml
<syncfusion:SfMap EnableZoom="True"
                  MinZoom="1"
                  MaxZoom="20"
                  ZoomLevel="5" />
```

```csharp
map.EnableZoom = true;
map.MinZoom = 1;
map.MaxZoom = 20;
map.ZoomLevel = 5;
```

### Zoom Methods

**Mouse wheel:**
- Scroll up to zoom in
- Scroll down to zoom out

**Programmatic zooming:**
```csharp
// Zoom in
map.ZoomLevel += 1;

// Zoom out
map.ZoomLevel -= 1;

// Set specific zoom
map.ZoomLevel = 8;
```

### Zoom to Specific Area

```csharp
// Define area bounds
double minX = -130;
double maxX = -70;
double minY = 25;
double maxY = 50;

// Calculate zoom level and center
// Implementation depends on your coordinate system
```

## Panning

Enable map panning to navigate across the geographical area.

### Enable Panning

Set `EnablePan` property on SfMap.

**Type:** bool  
**Default:** true

```xml
<syncfusion:SfMap EnablePan="True" />
```

```csharp
map.EnablePan = true;
```

### Pan Methods

**Mouse:**
- Click and drag to pan

**Touch:**
- Touch and drag to pan

**Keyboard:**
- Arrow keys to pan (when enabled)

## Selection

Enable interactive shape selection.

### Enable Selection

Set `EnableSelection` on ShapeFileLayer.

**Type:** bool  
**Default:** false

```xml
<syncfusion:ShapeFileLayer EnableSelection="True" />
```

```csharp
layer.EnableSelection = true;
```

### Selection Visual Customization

Use `SelectedShapeColor`, `SelectedShapeStroke`, and `SelectedShapeStrokeThickness` in ShapeSetting:

```xml
<syncfusion:ShapeFileLayer EnableSelection="True">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeFill="LightGray"
                                 ShapeStroke="White"
                                 ShapeStrokeThickness="1"
                                 SelectedShapeColor="Orange"
                                 SelectedShapeStroke="DarkOrange"
                                 SelectedShapeStrokeThickness="2" />
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeFill = new SolidColorBrush(Colors.LightGray);
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.White);
shapeSetting.ShapeStrokeThickness = 1;
shapeSetting.SelectedShapeColor = new SolidColorBrush(Colors.Orange);
shapeSetting.SelectedShapeStroke = new SolidColorBrush(Colors.DarkOrange);
shapeSetting.SelectedShapeStrokeThickness = 2;
```

## Complete Examples

### Example 1: Interactive Map with All Features

```xml
<syncfusion:SfMap EnableZoom="True"
                  EnablePan="True"
                  MinZoom="1"
                  MaxZoom="15"
                  ZoomLevel="3">
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                   ItemsSource="{Binding States}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="STATE_NAME"
                                   EnableSelection="True">
            
            <!-- Shape settings with selection colors -->
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="#E3F2FD"
                                         ShapeStroke="#1976D2"
                                         ShapeStrokeThickness="1"
                                         SelectedShapeColor="#FFC107"
                                         SelectedShapeStroke="#F57C00"
                                         SelectedShapeStrokeThickness="2" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
            
            <!-- Tooltips -->
            <syncfusion:ShapeFileLayer.ToolTipSettings>
                <syncfusion:ToolTipSetting ValuePath="Population" />
            </syncfusion:ShapeFileLayer.ToolTipSettings>
            
            <!-- Custom tooltip template -->
            <syncfusion:ShapeFileLayer.ItemsTemplate>
                <DataTemplate>
                    <Border Background="White" 
                            BorderBrush="Gray" 
                            BorderThickness="1"
                            CornerRadius="3"
                            Padding="8,4">
                        <StackPanel>
                            <TextBlock Text="{Binding Name}" 
                                       FontWeight="Bold" 
                                       FontSize="12" />
                            <TextBlock Text="{Binding Population, StringFormat='Pop: {0:N0}'}" 
                                       FontSize="10"
                                       Foreground="Gray" />
                        </StackPanel>
                    </Border>
                </DataTemplate>
            </syncfusion:ShapeFileLayer.ItemsTemplate>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 2: Zoom Controls in UI

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Zoom controls -->
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
        <Button Content="Zoom In" Click="ZoomIn_Click" Width="80" Margin="0,0,5,0"/>
        <Button Content="Zoom Out" Click="ZoomOut_Click" Width="80" Margin="0,0,5,0"/>
        <Button Content="Reset" Click="ResetZoom_Click" Width="80" Margin="0,0,10,0"/>
        <TextBlock Text="{Binding ElementName=map, Path=ZoomLevel, StringFormat='Zoom: {0:F1}x'}" 
                   VerticalAlignment="Center"
                   FontWeight="Bold"/>
    </StackPanel>
    
    <!-- Map -->
    <syncfusion:SfMap x:Name="map" Grid.Row="1"
                      EnableZoom="True"
                      EnablePan="True"
                      MinZoom="1"
                      MaxZoom="20"
                      ZoomLevel="1">
        <syncfusion:SfMap.Layers>
            <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp" />
        </syncfusion:SfMap.Layers>
    </syncfusion:SfMap>
</Grid>
```

```csharp
private void ZoomIn_Click(object sender, RoutedEventArgs e)
{
    if (map.ZoomLevel < map.MaxZoom)
    {
        map.ZoomLevel += 1;
    }
}

private void ZoomOut_Click(object sender, RoutedEventArgs e)
{
    if (map.ZoomLevel > map.MinZoom)
    {
        map.ZoomLevel -= 1;
    }
}

private void ResetZoom_Click(object sender, RoutedEventArgs e)
{
    map.ZoomLevel = 1;
}
```

### Example 3: Selection Event Handling

```xml
<syncfusion:ShapeFileLayer EnableSelection="True"
                           ShapeSelected="OnShapeSelected" />
```

```csharp
private void OnShapeSelected(object sender, SelectionEventArgs e)
{
    // Access selected shape data
    var selectedData = e.Items as StateData;
    if (selectedData != null)
    {
        MessageBox.Show($"Selected: {selectedData.Name}\nPopulation: {selectedData.Population:N0}");
    }
}
```

## Best Practices

1. **Enable zoom/pan** for large or detailed maps
2. **Set reasonable zoom limits** (MinZoom=1, MaxZoom=15-20)
3. **Provide visual feedback** for selection with contrasting colors
4. **Keep tooltips concise** to avoid cluttering the view
5. **Use custom templates** for rich tooltip content
6. **Test touch interactions** if targeting tablets/touch devices
7. **Provide UI controls** for zoom/reset if keyboard not available
8. **Consider performance** with complex tooltips on maps with many shapes

## Troubleshooting

### Zooming not working

**Check:**
- EnableZoom is set to true
- ZoomLevel is within MinZoom/MaxZoom range
- Mouse wheel events not blocked by parent containers

### Panning not responsive

**Verify:**
- EnablePan is set to true
- Map has sufficient space to pan
- Click events not consumed by other elements

### Tooltips not appearing

**Ensure:**
- ToolTipSettings is configured
- ValuePath matches property in data source
- Mouse hover is not blocked by overlays

### Selection not highlighting shapes

**Confirm:**
- EnableSelection is true
- SelectedShapeColor properties are set
- Colors contrast with default shape fill
