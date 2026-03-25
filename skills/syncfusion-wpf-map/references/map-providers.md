# Map Providers in WPF Maps (SfMap)

Maps control supports imagery layers that display map tiles from online providers like OpenStreetMap and Bing Maps. This provides real-world map backgrounds without needing shape files.

## ImageryLayer Overview

ImageryLayer displays raster map tiles from tile servers, offering:
- Street maps from OpenStreetMap
- Satellite imagery from Bing Maps
- Hybrid views (satellite + labels)
- Custom tile providers
- Automatic tile loading based on zoom level

## OpenStreetMap Configuration

OpenStreetMap provides free, open-source map tiles.

### Basic Setup

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ImageryLayer LayerType="OSM"/>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

```csharp
SfMap map = new SfMap();
ImageryLayer imageryLayer = new ImageryLayer();
imageryLayer.LayerType = LayerType.OSM;
map.Layers.Add(imageryLayer);
```

**Default:** ImageryLayer uses OpenStreetMap tiles by default.

### With Zoom and Pan

```xml
<syncfusion:SfMap EnableZoom="True" 
                  EnablePan="True"
                  MinZoom="1"
                  MaxZoom="18"
                  ZoomLevel="5">
    <syncfusion:SfMap.Layers>
        <syncfusion:ImageryLayer LayerType="OSM" />
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

## Bing Maps Configuration

Bing Maps provides aerial, road, and hybrid imagery (requires API key).

### Getting Bing Maps API Key

1. Visit https://www.bingmapsportal.com/
2. Create an account or sign in
3. Create a new key under "My Account" → "My Keys"
4. Copy the API key

### Bing Maps Setup

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ImageryLayer BingMapKey="YOUR_BING_MAPS_API_KEY"
        LayerType="Bing"
        BingMapStyle="Aerial" />
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

```csharp
ImageryLayer imageryLayer = new ImageryLayer();
imageryLayer.BingMapKey = "YOUR_BING_MAPS_API_KEY";
imageryLayer.LayerType = LayerType.Bing;
imageryLayer.BingMapStyle = BingMapStyle.Aerial;
map.Layers.Add(imageryLayer);
```

### BingMapStyle

**BingMapStyle enum values:**
- **Road** - Street map view
- **Aerial** - Satellite imagery
- **AerialWithLabels** - Satellite with labels

```xml
<!-- Road map -->
<syncfusion:ImageryLayer BingMapKey="YOUR_KEY" LayerType="Bing" BingMapStyle="Road" />

<!-- Aerial/Satellite -->
<syncfusion:ImageryLayer BingMapKey="YOUR_KEY" LayerType="Bing" BingMapStyle="Aerial" />

<!-- Hybrid (Satellite + Labels) -->
<syncfusion:ImageryLayer BingMapKey="YOUR_KEY" BingMapStyle="AerialWithLabels" />
```

## Combining Imagery with Shape Layers

Overlay shape layers on imagery for enhanced visualization.

```xml
<syncfusion:SfMap EnableZoom="True" EnablePan="True" ZoomLevel="4">
    <syncfusion:SfMap.Layers>
        <!-- Base imagery layer -->
        <syncfusion:ImageryLayer LayerType="OSM" />
        
        <!-- Overlay shape layer -->
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                   ItemsSource="{Binding States}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="STATE_NAME">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeFill="Transparent"
                                         ShapeStroke="Red"
                                         ShapeStrokeThickness="2" />
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

## Custom Tile Provider

Implement custom tile providers for specialized map sources.

### Custom Tile URL Template

Some tile providers use URL templates:

```
http://tile-server.com/{z}/{x}/{y}.png
```

Where:
- `{z}` - Zoom level
- `{x}` - Tile X coordinate
- `{y}` - Tile Y coordinate

**Note:** Check provider's terms of service before using.

## Zoom Levels and Tile Loading

### Zoom Level Ranges

**OpenStreetMap:** Zoom 1-18  
**Bing Maps:** Zoom 1-21

Higher zoom = more detail, more tiles loaded.

### Performance Considerations

- Tiles are cached automatically
- Lower zoom levels load fewer tiles (better performance)
- Internet connection required for initial tile load

## Complete Examples

### Example 1: OpenStreetMap with Markers

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF"
        xmlns:local="clr-namespace:MyApp">
    <Grid>
        <Grid.DataContext>
            <local:CityViewModel />
        </Grid.DataContext>

        <syncfusion:SfMap EnableZoom="True" EnablePan="True" ZoomLevel="6">
            <syncfusion:SfMap.Layers>
                <syncfusion:ImageryLayer Markers="{Binding Cities}" LayerType="OSM">
                    <syncfusion:ImageryLayer.MarkerTemplate>
                        <DataTemplate>
                            <Grid>
                                <Ellipse Width="16" Height="16" Fill="Red" Stroke="White" StrokeThickness="2" />
                                <TextBlock Text="{Binding Label}" 
                                           Margin="0,20,0,0"
                                           FontSize="11"
                                           FontWeight="Bold"
                                           Foreground="Red" />
                            </Grid>
                        </DataTemplate>
                    </syncfusion:ImageryLayer.MarkerTemplate>
                </syncfusion:ImageryLayer>
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

### Example 2: Bing Aerial with Data Overlay

```xml
<syncfusion:SfMap EnableZoom="True" EnablePan="True" ZoomLevel="5">
    <syncfusion:SfMap.Layers>
        <!-- Bing aerial imagery -->
        <syncfusion:ImageryLayer BingMapKey="YOUR_BING_MAPS_KEY"
                                 LayerType="Bing" />
        
        <!-- Data overlay with semi-transparent shapes -->
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.counties.shp"
                                   ItemsSource="{Binding Counties}"
                                   ShapeIDPath="Name"
                                   ShapeIDTableField="NAME">
            <syncfusion:ShapeFileLayer.ShapeSettings>
                <syncfusion:ShapeSetting ShapeValuePath="Population">
                    <syncfusion:ShapeSetting.FillSetting>
                        <syncfusion:ShapeFillSetting AutoFillColors="False">
                            <syncfusion:ShapeFillSetting.ColorMappings>
                                <syncfusion:RangeColorMapping From="0" To="50000" Color="#8000FF00" LegendLabel="Low" />
                                <syncfusion:RangeColorMapping From="50000" To="200000" Color="#80FFFF00" LegendLabel="Medium" />
                                <syncfusion:RangeColorMapping From="200000" To="1000000" Color="#80FF0000" LegendLabel="High" />
                            </syncfusion:ShapeFillSetting.ColorMappings>
                        </syncfusion:ShapeFillSetting>
                    </syncfusion:ShapeSetting.FillSetting>
                </syncfusion:ShapeSetting>
            </syncfusion:ShapeFileLayer.ShapeSettings>
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### Example 3: Switchable Map Types

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
        <RadioButton Content="OpenStreetMap" IsChecked="True" Checked="UseOSM_Checked" Margin="0,0,10,0"/>
        <RadioButton Content="Bing Road" Checked="UseBingRoad_Checked" Margin="0,0,10,0"/>
        <RadioButton Content="Bing Aerial" Checked="UseBingAerial_Checked" Margin="0,0,10,0"/>
    </StackPanel>
    
    <syncfusion:SfMap x:Name="map" Grid.Row="1" EnableZoom="True" EnablePan="True" ZoomLevel="4" />
</Grid>
```

```csharp
private void UseOSM_Checked(object sender, RoutedEventArgs e)
{
    map.Layers.Clear();
    ImageryLayer layer = new ImageryLayer();
    layer.LayerType = LayerType.OSM;
    map.Layers.Add(layer);
}

private void UseBingRoad_Checked(object sender, RoutedEventArgs e)
{
    map.Layers.Clear();
    ImageryLayer layer = new ImageryLayer();
    layer.BingMapKey = "YOUR_BING_MAPS_KEY";
    layer.LayerType = LayerType.Bing;
    layer.BingMapStyle = BingMapStyle.Road;
    map.Layers.Add(layer);
}

private void UseBingAerial_Checked(object sender, RoutedEventArgs e)
{
    map.Layers.Clear();
    ImageryLayer layer = new ImageryLayer();
    layer.BingMapKey = "YOUR_BING_MAPS_KEY";
    layer.LayerType = LayerType.Bing;
    layer.BingMapStyle = BingMapStyle.Aerial;
    map.Layers.Add(layer);
}
```

## Adding Azure Maps

Azure Maps uses a URL template to load the tiles.
```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ImageryLayer 
            UrlTemplate="https://atlas.microsoft.com/map/tile?api-version=2024-04-01
                         &tilesetId=microsoft.base.road
                         &zoom={z}&x={x}&y={y}
                         &subscription-key=YOUR_AZURE_MAPS_KEY"/>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```
```csharp
SfMap map = new SfMap();
ImageryLayer layer = new ImageryLayer();

layer.UrlTemplate =
    "https://atlas.microsoft.com/map/tile?api-version=2024-04-01" +
    "&tilesetId=microsoft.base.road" +
    "&zoom={z}&x={x}&y={y}" +
    "&subscription-key=YOUR_AZURE_MAPS_KEY";

map.Layers.Add(layer);
this.Content = map
```
### Adding Sublayers for Azure Maps
```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>

        <syncfusion:ImageryLayer
            UrlTemplate="https://atlas.microsoft.com/map/tile?api-version=2024-04-01
                         &tilesetId=microsoft.base.road
                         &zoom={z}&x={x}&y={y}
                         &subscription-key=YOUR_AZURE_MAPS_KEY">

            <syncfusion:ImageryLayer.SubShapeFileLayers>
                <syncfusion:SubShapeFileLayer Uri="DataMarkers.ShapeFiles.Africa.shp">
                    <syncfusion:SubShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeStroke="#C1C1C1"
                                                 ShapeStrokeThickness="0.5"
                                                 ShapeFill="Chocolate"/>
                    </syncfusion:SubShapeFileLayer.ShapeSettings>
                </syncfusion:SubShapeFileLayer>
            </syncfusion:ImageryLayer.SubShapeFileLayers>

        </syncfusion:ImageryLayer>

    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```
## Best Practices

1. **Internet connection** - Imagery layers require active internet connection
2. **Caching** - Tiles are cached automatically; first load may be slow
3. **Zoom limits** - Respect provider's zoom level limits
4. **API keys** - Keep Bing Maps keys secure, don't hardcode in production
5. **Overlay transparency** - Use alpha channel colors (#80RRGGBB) for overlays
6. **Terms of service** - Review and comply with provider's usage terms
7. **Fallback** - Consider fallback for offline scenarios

## Troubleshooting

### Tiles not loading

**Check:**
- Internet connection is active
- Firewall allows connection to tile servers
- API key is valid (for Bing Maps)
- Zoom level is within provider's supported range

### Slow initial load

**Normal behavior** - First load downloads tiles
- Subsequent views use cached tiles
- Lower initial zoom level for faster load

### Bing Maps not appearing

**Verify:**
- BingMapKey is set and valid
- LayerType is specified (Road, Aerial, or Hybrid)
- API key has not exceeded usage limits

### Overlays not visible on imagery

**Ensure:**
- Shape layer added after imagery layer
- ShapeFill has transparency or use Stroke only
- Coordinates match imagery projection
