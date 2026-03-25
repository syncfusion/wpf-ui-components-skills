# Multilayer Support in WPF Maps (SfMap)

The Maps control supports multiple layers that can be stacked to create rich, complex visualizations. This includes sublayers for vector shapes and KML file support for geographical data.

## Overview

Multilayer support enables:
- **Base layer + overlay layers** - Combine imagery with shape data
- **Sublayers** - Add additional shape files as sublayers
- **KML files** - Import and display KML geographic data
- **Layer ordering** - Control which layers appear on top
- **Independent configuration** - Each layer has its own styling and data

## SubShapeFileLayers

Add sublayers to overlay additional shape data on the main layer.

### Adding Sublayers

Use the `SubShapeFileLayers` collection:

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.countries.shp">
    <syncfusion:ShapeFileLayer.SubShapeFileLayers>
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.cities.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Red" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
    </syncfusion:ShapeFileLayer.SubShapeFileLayers>
</syncfusion:ShapeFileLayer>
```

```csharp
ShapeFileLayer mainLayer = new ShapeFileLayer();
mainLayer.Uri = "MyApp.ShapeFiles.countries.shp";

SubShapeFileLayer subLayer = new SubShapeFileLayer();
subLayer.Uri = "MyApp.ShapeFiles.cities.shp";

ShapeSetting subLayerSetting = new ShapeSetting();
subLayerSetting.ShapeFill = new SolidColorBrush(Colors.Red);
subLayer.ShapeSettings = subLayerSetting;

mainLayer.SubShapeFileLayers.Add(subLayer);
```

### Multiple Sublayers

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp">
    <syncfusion:ShapeFileLayer.SubShapeFileLayers>
        <!-- Major cities -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.major_cities.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Red" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
        
        <!-- Rivers -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.rivers.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeStroke="Blue" ShapeStrokeThickness="2" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
        
        <!-- Roads -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.highways.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeStroke="Gray" ShapeStrokeThickness="1" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
    </syncfusion:ShapeFileLayer.SubShapeFileLayers>
</syncfusion:ShapeFileLayer>
```

## KML File Support

KML (Keyhole Markup Language) files contain geographical data in XML format.

### Loading KML Files

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp">
    <syncfusion:ShapeFileLayer.SubShapeFileLayers>
        <syncfusion:SubShapeFileLayer Uri="MyApp.KMLFiles.locations.kml" />
    </syncfusion:ShapeFileLayer.SubShapeFileLayers>
</syncfusion:ShapeFileLayer>
```

## Layer Ordering

Layers are rendered in the order they're added:
1. First layer added = Bottom layer
2. Last layer added = Top layer

## Complete Examples

### Example 1: Country Map with City Sublayer

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                   ItemsSource="{Binding States}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="STATE_NAME">
            <!-- State styling -->
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="LightGray"
                                         ShapeStroke="White"
                                         ShapeStrokeThickness="1" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
            
            <!-- City sublayer -->
            <syncfusion:ShapeFileLayer.SubShapeFileLayers>
                <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.us_cities.shp">
                    <syncfusion:SubShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeFill="DarkRed" />
                    </syncfusion:SubShapeFileLayer.ShapeSettings>
                </syncfusion:SubShapeFileLayer>
            </syncfusion:ShapeFileLayer.SubShapeFileLayers>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 2: Geographical Features Overlay

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.continent.shp">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeFill="#E0E0E0" ShapeStroke="White" ShapeStrokeThickness="2" />
    </syncfusion:ShapeFileLayer.ShapeSettings>
    
    <syncfusion:ShapeFileLayer.SubShapeFileLayers>
        <!-- Mountains -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.mountains.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Brown" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
        
        <!-- Lakes -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.lakes.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Blue" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
        
        <!-- Forests -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.ShapeFiles.forests.shp">
            <syncfusion:SubShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Green" Opacity="0.6" />
            </syncfusion:SubShapeFileLayer.ShapeSettings>
        </syncfusion:SubShapeFileLayer>
    </syncfusion:ShapeFileLayer.SubShapeFileLayers>
</syncfusion:ShapeFileLayer>
```

### Example 3: KML Overlay on Shape File

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp">
    <syncfusion:ShapeFileLayer.ShapeSettings>
        <syncfusion:ShapeSetting ShapeFill="LightGray" ShapeStroke="White" ShapeStrokeThickness="0.5" />
    </syncfusion:ShapeFileLayer.ShapeSettings>
    
    <syncfusion:ShapeFileLayer.SubShapeFileLayers>
        <!-- KML with tourist attractions -->
        <syncfusion:SubShapeFileLayer Uri="MyApp.KMLFiles.attractions.kml">
            <syncfusion:SubShapeFileLayer.ItemsTemplate>
                <DataTemplate>
                    <StackPanel>
                        <Image Source="/Images/marker-star.png" Width="20" Height="20" />
                        <TextBlock Text="{Binding Name}" 
                                   FontSize="9"
                                   FontWeight="Bold"
                                   Foreground="DarkBlue"
                                   HorizontalAlignment="Center"
                                   Margin="0,2,0,0" />
                    </StackPanel>
                </DataTemplate>
            </syncfusion:SubShapeFileLayer.ItemsTemplate>
        </syncfusion:SubShapeFileLayer>
    </syncfusion:ShapeFileLayer.SubShapeFileLayers>
</syncfusion:ShapeFileLayer>
```

### Example 4: Programmatic Layer Management

```csharp
public class MultiLayerMapViewModel
{
    private SfMap map;
    private ShapeFileLayer mainLayer;

    public void InitializeMap()
    {
        map = new SfMap();
        
        // Base layer
        mainLayer = new ShapeFileLayer();
        mainLayer.Uri = "MyApp.ShapeFiles.countries.shp";
        map.Layers.Add(mainLayer);
    }

    public void AddCitySubLayer()
    {
        SubShapeFileLayer cityLayer = new SubShapeFileLayer();
        cityLayer.Uri = "MyApp.ShapeFiles.cities.shp";
        
        ShapeSetting citySetting = new ShapeSetting();
        citySetting.ShapeFill = new SolidColorBrush(Colors.Red);
        cityLayer.ShapeSettings = citySetting;
        
        mainLayer.SubShapeFileLayers.Add(cityLayer);
    }

    public void AddRiverSubLayer()
    {
        SubShapeFileLayer riverLayer = new SubShapeFileLayer();
        riverLayer.Uri = "MyApp.ShapeFiles.rivers.shp";
        
        ShapeSetting riverSetting = new ShapeSetting();
        riverSetting.ShapeStroke = new SolidColorBrush(Colors.Blue);
        riverSetting.ShapeStrokeThickness = 2;
        riverLayer.ShapeSettings = riverSetting;
        
        mainLayer.SubShapeFileLayers.Add(riverLayer);
    }

    public void RemoveSubLayer(SubShapeFileLayer layer)
    {
        mainLayer.SubShapeFileLayers.Remove(layer);
    }

    public void ClearSubLayers()
    {
        mainLayer.SubShapeFileLayers.Clear();
    }
}
```

## Best Practices

1. **Layer order** - Add base layers first, detail layers last
2. **Transparency** - Use semi-transparent fills for overlays to show base layer
3. **Performance** - Limit sublayer count (3-5) for optimal rendering
4. **Stroke visibility** - Use contrasting strokes to separate overlapping shapes
5. **File organization** - Store shape files and KML files in separate folders
6. **Data consistency** - Ensure coordinate systems match across all layers
7. **Testing** - Test layer visibility and order at different zoom levels

## Troubleshooting

### Sublayer not visible

**Check:**
- Uri points to valid shape file
- Shape file is added as embedded resource
- ShapeFill or ShapeStroke properties are set
- Layer is not obscured by opaque layers above it

### KML file not loading

**Verify:**
- KML file format is valid XML
- File is added as embedded resource
- Uri path is correct (namespace.folder.filename.kml)
- Coordinate data in KML is supported

### Layers in wrong order

**Adjust:**
- Reorder layers in XAML or code
- Base layers should be added first
- Overlay layers added last appear on top

### Performance issues with multiple layers

**Optimize:**
- Reduce number of sublayers
- Simplify shape files (fewer vertices)
- Use appropriate zoom levels
- Consider imagery layers instead of multiple shape files
