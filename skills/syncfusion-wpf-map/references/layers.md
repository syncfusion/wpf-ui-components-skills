# Layers in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [Layer Types](#layer-types)
- [ShapeFileLayer Configuration](#shapefilelayer-configuration)
- [Shape Settings](#shape-settings)
- [Customizing Selected Shapes](#customizing-selected-shapes)
- [Layer Visibility and Background](#layer-visibility-and-background)

## Overview

The WPF Maps control is built using a layered architecture. Each layer can render different types of geographical data, and multiple layers can be combined to create rich visualizations.

The Maps control supports two types of layers:
1. **ShapeFileLayer** - Renders vector shapes from shape files
2. **ImageryLayer** - Displays map tiles from providers (OpenStreetMap, Bing Maps)

## Layer Types

### ShapeFileLayer

ShapeFileLayer is used to render custom vector shapes from shape files with full control over styling and data binding.

**Key features:**
- Vector shape rendering
- Data binding support
- Custom styling and colors
- Selection support
- Markers and bubbles
- Legends

### ImageryLayer

ImageryLayer displays raster map tiles from online map providers. See the map-providers.md reference for detailed information.

## ShapeFileLayer Configuration

### Basic Setup

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp" />
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

```csharp
SfMap map = new SfMap();
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.world1.shp";
map.Layers.Add(layer);
```

### Multiple Layers

You can add multiple layers to create overlays:

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <!-- Base layer: Country boundaries -->
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.countries.shp">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="LightGray" ShapeStroke="White" ShapeStrokeThickness="1" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
        
        <!-- Overlay layer: Major cities -->
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.cities.shp">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Red" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

## Shape Settings

ShapeSettings control the visual appearance of shapes in the layer.

### Basic Properties

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeFill="LightBlue" 
                                 ShapeStroke="DarkBlue" 
                                 ShapeStrokeThickness="2" />
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";

ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeFill = new SolidColorBrush(Colors.LightBlue);
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.DarkBlue);
shapeSetting.ShapeStrokeThickness = 2;
layer.ShapeSettings = shapeSetting;
```

### ShapeFill Property

Sets the fill color for all shapes in the layer.

**Type:** Brush  
**Default:** System default

```xml
<syncfusion:ShapeSetting ShapeFill="#E0F7FA" />
```

```csharp
shapeSetting.ShapeFill = (SolidColorBrush)new BrushConverter().ConvertFrom("#E0F7FA");
```

**Gradient fill:**

```xml
<syncfusion:ShapeSetting>
    <syncfusion:ShapeSetting.ShapeFill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="DarkBlue" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:ShapeSetting.ShapeFill>
</syncfusion:ShapeSetting>
```

### ShapeStroke Property

Sets the border color for shapes.

**Type:** Brush  
**Default:** Transparent

```xml
<syncfusion:ShapeSetting ShapeStroke="White" />
```

```csharp
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.White);
```

### ShapeStrokeThickness Property

Sets the border width for shapes.

**Type:** double  
**Default:** 0

```xml
<syncfusion:ShapeSetting ShapeStrokeThickness="1.5" />
```

```csharp
shapeSetting.ShapeStrokeThickness = 1.5;
```

### Complete Example

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF">
    <Grid>
        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp">
                    <syncfusion:ShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeFill="#B3E5FC" 
                                                 ShapeStroke="#01579B" 
                                                 ShapeStrokeThickness="1" />
                    </syncfusion:ShapeFileLayer.ShapeSettings>
                </syncfusion:ShapeFileLayer>
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

## Customizing Selected Shapes

Enable interactive selection and customize the appearance of selected shapes.

### Enabling Selection

Set the `EnableSelection` property on ShapeFileLayer:

```xml
<syncfusion:ShapeFileLayer EnableSelection="True" Uri="MyApp.ShapeFiles.usa_state.shp">
```

```csharp
layer.EnableSelection = true;
```

### Selection Properties

**SelectedShapeColor** - Fill color for selected shapes  
**SelectedShapeStroke** - Border color for selected shapes  
**SelectedShapeStrokeThickness** - Border thickness for selected shapes

```xml
<syncfusion:ShapeFileLayer EnableSelection="True" Uri="MyApp.ShapeFiles.usa_state.shp">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeFill="LightGray"
                                 ShapeStroke="White"
                                 ShapeStrokeThickness="1"
                                 SelectedShapeColor="Orange"
                                 SelectedShapeStroke="Red"
                                 SelectedShapeStrokeThickness="2" />
    </syncfusion:ShapeFileLayer.ShapeSettings>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.EnableSelection = true;
layer.Uri = "MyApp.ShapeFiles.usa_state.shp";

ShapeSetting shapeSetting = new ShapeSetting();
shapeSetting.ShapeFill = new SolidColorBrush(Colors.LightGray);
shapeSetting.ShapeStroke = new SolidColorBrush(Colors.White);
shapeSetting.ShapeStrokeThickness = 1;
shapeSetting.SelectedShapeColor = new SolidColorBrush(Colors.Orange);
shapeSetting.SelectedShapeStroke = new SolidColorBrush(Colors.Red);
shapeSetting.SelectedShapeStrokeThickness = 2;
layer.ShapeSettings = shapeSetting;
```

### Selection Behavior

**Single selection (default):**
- Click a shape to select it
- Previously selected shape is deselected
- Click selected shape again to deselect

**Handling selection events:**

```xml
<syncfusion:ShapeFileLayer EnableSelection="True"
                           ShapeSelected="OnShapeSelected">
```

```csharp
private void OnShapeSelected(object sender, ShapeSelectedEventArgs e)
{
    // Access selected shape data
    var shapeName = e.Data as string;
    MessageBox.Show($"Selected: {shapeName}");
}
```

## Layer Visibility and Background

### Background Color

```xml
<syncfusion:ShapeFileLayer Background="AliceBlue" Uri="MyApp.ShapeFiles.world1.shp" />
```

```csharp
layer.Background = new SolidColorBrush(Colors.AliceBlue);
```

### Layer Opacity

```xml
<syncfusion:ShapeFileLayer Opacity="0.8" Uri="MyApp.ShapeFiles.world1.shp" />
```

```csharp
layer.Opacity = 0.8;
```

### Visibility

```xml
<syncfusion:ShapeFileLayer Visibility="Collapsed" Uri="MyApp.ShapeFiles.world1.shp" />
```

```csharp
layer.Visibility = Visibility.Collapsed; // or Visibility.Visible
```

## Advanced Layer Configuration

### Complete Styled Layer Example

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer x:Name="stateLayer"
                                   Uri="MyApp.ShapeFiles.usa_state.shp"
                                   Background="White"
                                   EnableSelection="True">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="#E3F2FD"
                                         ShapeStroke="#1976D2"
                                         ShapeStrokeThickness="1"
                                         SelectedShapeColor="#FFC107"
                                         SelectedShapeStroke="#F57C00"
                                         SelectedShapeStrokeThickness="2" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Programmatic Layer Manipulation

```csharp
// Add layer dynamically
ShapeFileLayer newLayer = new ShapeFileLayer();
newLayer.Uri = "MyApp.ShapeFiles.cities.shp";
map.Layers.Add(newLayer);

// Remove layer
map.Layers.Remove(existingLayer);

// Clear all layers
map.Layers.Clear();

// Access specific layer
ShapeFileLayer firstLayer = map.Layers[0] as ShapeFileLayer;
```

## Best Practices

1. **Use appropriate stroke thickness** - Thin strokes (0.5-1) for dense shapes, thicker (2-3) for sparse
2. **Contrast selection colors** - Make selected shapes clearly distinguishable
3. **Layer ordering** - Add base layers first, overlays last
4. **Performance** - Minimize layer count for complex shape files
5. **Naming** - Use x:Name for layers that need programmatic access

## Troubleshooting

### Shapes appear without borders
Set ShapeStroke and ensure ShapeStrokeThickness > 0.

### Selection not working
Verify EnableSelection="True" and selection color properties are set.

### Layer not visible
Check Visibility property and ensure Uri points to valid shape file.

### Overlapping layers obscure base
Reorder layers - base layers should be added first.
