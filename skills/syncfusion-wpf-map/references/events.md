# Events in WPF Maps (SfMap)

The Maps control provides various events that enable you to respond to user interactions and state changes. These events allow you to implement custom behaviors when users interact with map elements.

## Available Events

### Selection Events

**ShapeSelected**
- **Trigger:** When a shape is clicked by the user
- **Arguments:** `ShapeSelectedEventArgs`
- **Use case:** Handle shape selection, show details, navigate

**MarkerSelected**
- **Trigger:** When a marker is clicked
- **Arguments:** `MarkerSelectedEventArgs`
- **Use case:** Display marker information, navigation

### Zoom Events

**ZoomedOut**
- **Trigger:** When map zoomed out
- **Arguments:** `ZoomEventArgs`
- **Use case:** Update UI based on zoom, load detail data

**ZoomedIn**
- **Trigger:** When map zoomed in.
- **Arguments:** `ZoomEventArgs`
- **Use case:** Update UI based on zoom, load detail data

## Shape Selection Events

### ShapeSelected Event

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                   EnableSelection="True"
                                   ShapesSelected="ShapeFileLayer_ShapeSelected">
        </syncfusion:ShapeFileLayer>
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

```csharp
private void ShapeFileLayer_ShapeSelected(object sender, SelectionEventArgs e)
{
    // Get the data object for the selected shape
    var data = e.Items;
    
    // Get the shape object
    var shape = e.Shape;
    
    // Example: Show shape information
    if (data != null)
    {
        MessageBox.Show($"Selected: {data}");
    }
}
```
## Marker Events

### MarkerSelected Event
```csharp
    public class Marker
    {
        public string Label { get; set; }
        public string Longitude { get; set; }
        public string Latitude { get; set; }
    }

    public class ViewModel
    {
        public ObservableCollection<Marker> MarkerCollection { get; set; }
        public ViewModel()
        {
            this.MarkerCollection = new ObservableCollection<Marker>
            {
                new Marker() { Label = "California", Latitude = "37", Longitude = "-120" },
                new Marker() { Label = "USA", Latitude = "38.8833N", Longitude = "77.0167W" },
                new Marker() { Label = "Brazil", Latitude = "15.7833S", Longitude = "47.8667W" },
                new Marker() { Label = "India", Latitude = "21.0000N", Longitude = "78.0000E" },
                new Marker() { Label = "China", Latitude = "35.0000N", Longitude = "103.0000E" },
                new Marker() { Label = "Indonesia", Latitude = "6.1750S", Longitude = "106.8283E" },
                new Marker() { Label = "Africa", Latitude = "-8.7832S", Longitude = "34.5085E" },
            };
        }
    }
```

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           Markers="{Binding MarkerCollection}"
                           MarkerSelected="ShapeFileLayer_MarkerSelected">
</syncfusion:ShapeFileLayer>
```

```csharp
private void ShapeFileLayer_MarkerSelected(object sender, MarkerSelectedEventArgs e)
{
    // Get the selected marker
    var marker = e.SelectedMarker;
    
    if (marker != null)
    {
        // Display marker information in intenally definied TextBlocks
        MarkerLabel.Text = marker.Label;
        MarkerCoordinates.Text = $"{marker.Latitude}, {marker.Longitude}";
    }
}
```

## Complete Examples

### Example 1: Selection with Detail Panel

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="300" />
    </Grid.ColumnDefinitions>
    
    <syncfusion:SfMap Grid.Column="0">
        <syncfusion:SfMap.Layers>
            <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                                       ItemsSource="{Binding States}"
                                       ShapeIDPath="Name"
                                       ShapeIDTableField="STATE_NAME"
                                       ShapeSelected="ShapeFileLayer_ShapeSelected">
                <syncfusion:ShapeFileLayer.ShapeSettings>
                    <syncfusion:ShapeSetting ShapeValuePath="Population"
                                             EnableSelection="True"
                                             SelectedShapeColor="Orange" />
                </syncfusion:ShapeFileLayer.ShapeSettings>
            </syncfusion:ShapeFileLayer>
        </syncfusion:SfMap.Layers>
    </syncfusion:SfMap>
    
    <Border Grid.Column="1" Background="#F5F5F5" Padding="20">
        <StackPanel x:Name="DetailPanel" Visibility="Collapsed">
            <TextBlock Text="State Information" FontSize="18" FontWeight="Bold" Margin="0,0,0,10" />
            <TextBlock x:Name="StateName" FontSize="16" Margin="0,0,0,5" />
            <TextBlock x:Name="StatePopulation" Margin="0,0,0,5" />
            <TextBlock x:Name="StateArea" />
        </StackPanel>
    </Border>
</Grid>
```

```csharp
public partial class MapWindow : Window
{
    public MapWindow()
    {
        InitializeComponent();
    }

    private void ShapeFileLayer_ShapeSelected(object sender, SelectionEventArgs e)
    {
        if (e.Items is StateData state)
        {
            DetailPanel.Visibility = Visibility.Visible;
            StateName.Text = state.Name;
            StatePopulation.Text = $"Population: {state.Population:N0}";
            StateArea.Text = $"Area: {state.Area:N0} sq mi";
        }
    }
}

public class StateData : INotifyPropertyChanged
{
    public string Name { get; set; }
    public int Population { get; set; }
    public double Area { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Example 2: Zoom-Based Detail Loading

```xml
<syncfusion:SfMap ZoomedIn="map_ZoomedIn"
                  ZoomedOut="map_ZoomedOut"
                  EnableZoom="True"
                  MinZoom="1"
                  MaxZoom="10">
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer x:Name="MainLayer"
                                   Uri="MyApp.ShapeFiles.world1.shp"
                                   ItemsSource="{Binding Countries}" />
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

```csharp
public partial class ZoomMapWindow : Window
{
    private const double DetailZoomThreshold = 4.0;
    private bool detailsLoaded = false;

    public ZoomMapWindow()
    {
        InitializeComponent();
    }

    private void map_ZoomedIn(object sender, ZoomEventArgs e)
    {
        double zoomLevel = e.NewZoomLevel;
        
        // Load detailed markers when zooming in
        if (zoomLevel >= DetailZoomThreshold && !detailsLoaded)
        {
            LoadDetailedMarkers();
            detailsLoaded = true;
        }
        // Remove detailed markers when zooming out
        else if (zoomLevel < DetailZoomThreshold && detailsLoaded)
        {
            ClearDetailedMarkers();
            detailsLoaded = false;
        }
    }

    private void map_ZoomedOut(object sender, ZoomEventArgs e)
    {
        double zoomLevel = e.NewZoomLevel;
        
        // Load detailed markers when zooming in
        if (zoomLevel >= DetailZoomThreshold && !detailsLoaded)
        {
            LoadDetailedMarkers();
            detailsLoaded = true;
        }
        // Remove detailed markers when zooming out
        else if (zoomLevel < DetailZoomThreshold && detailsLoaded)
        {
            ClearDetailedMarkers();
            detailsLoaded = false;
        }
    }

    private void LoadDetailedMarkers()
    {
        MainLayer.Markers.Clear();
        
        // Add detailed city markers
        foreach (var city in GetCityData())
        {
            MainLayer.Markers.Add(new MapMarker
            {
                Label = city.Name,
                Latitude = city.Latitude,
                Longitude = city.Longitude
            });
        }
    }

    private void ClearDetailedMarkers()
    {
        MainLayer.Markers.Clear();
    }

    private List<CityData> GetCityData()
    {
        return new List<CityData>
        {
            new CityData { Name = "New York", Latitude = "40.7128N", Longitude = "74.0060W" },
            new CityData { Name = "London", Latitude = "51.5074N", Longitude = "0.1278W" },
            new CityData { Name = "Tokyo", Latitude = "35.6762N", Longitude = "139.6503E" }
        };
    }
}
```

## Best Practices

1. **Check data type** - Cast `e.Items` to expected type before accessing properties
2. **Null checks** - Always verify event arguments are not null
3. **UI thread** - Update UI elements on dispatcher thread if needed
4. **Unsubscribe** - Remove event handlers when disposing controls
5. **Performance** - Avoid heavy operations in frequently-fired events (ZoomLevelChanged)
6. **State management** - Track selection state to avoid redundant updates
7. **Event coordination** - Use multiple events together for rich interactions

## Troubleshooting

### Event not firing

**Check:**
- Event handler is properly subscribed in XAML or code
- EnableSelection is true for selection events
- EnableZoom is true for zoom events
- Layer is visible and has data

### Cannot access data in event handler

**Verify:**
- ItemsSource is bound to data collection
- ShapeIDPath and ShapeIDTableField are correctly set
- Data object type matches expected type in cast

### UI not updating in event handler

**Fix:**
- Ensure UI updates are on dispatcher thread
- Check binding modes and property notifications
- Verify element visibility and parent container layout
