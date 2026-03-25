# Markers in WPF Maps (SfMap)

## Table of Contents
- [Overview](#overview)
- [Adding Markers](#adding-markers)
- [Marker Model](#marker-model)
- [Default Marker Rendering](#default-marker-rendering)
- [Custom Marker Templates](#custom-marker-templates)
- [Marker Template Selector](#marker-template-selector)
- [Marker Styling Properties](#marker-styling-properties)
- [Advanced Marker Scenarios](#advanced-marker-scenarios)

## Overview

Markers are visual indicators used to highlight specific locations on maps. They're ideal for showing points of interest, store locations, weather stations, or any geographical position that needs emphasis.

**Key features:**
- Simple latitude/longitude positioning
- Default and custom visual styles
- Label support
- Template-based customization
- Conditional templates with MarkerTemplateSelector
- Data binding support

## Adding Markers

### Basic Setup

Create a model class with marker properties:

```csharp
public class MarkerModel
{
    public string Label { get; set; }
    public string Latitude { get; set; }
    public string Longitude { get; set; }
}
```

Bind the markers collection to the layer:

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           Markers="{Binding MarkerCollection}" />
```

```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApp.ShapeFiles.world1.shp";
layer.Markers = viewModel.MarkerCollection;
```

### Complete Example

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF"
        xmlns:local="clr-namespace:MyApp">
    <Grid>
        <Grid.DataContext>
            <local:MapViewModel />
        </Grid.DataContext>

        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                           Markers="{Binding Cities}" />
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

```csharp
public class City
{
    public string Label { get; set; }
    public string Latitude { get; set; }
    public string Longitude { get; set; }
}

public class MapViewModel
{
    public ObservableCollection<City> Cities { get; set; }

    public MapViewModel()
    {
        Cities = new ObservableCollection<City>
        {
            new City { Label = "New York", Latitude = "40.7128N", Longitude = "74.0060W" },
            new City { Label = "London", Latitude = "51.5074N", Longitude = "0.1278W" },
            new City { Label = "Tokyo", Latitude = "35.6762N", Longitude = "139.6503E" },
            new City { Label = "Sydney", Latitude = "33.8688S", Longitude = "151.2093E" }
        };
    }
}
```

## Marker Model

### Required Properties

Your marker model class must contain these properties:

**Latitude** - Vertical position (Y-axis)
- **Type:** string
- **Format:** Decimal degrees with N/S suffix (e.g., "40.7128N", "33.8688S")
- **Examples:** "51.5074N", "23.4500S"

**Longitude** - Horizontal position (X-axis)
- **Type:** string
- **Format:** Decimal degrees with E/W suffix (e.g., "74.0060W", "151.2093E")
- **Examples:** "0.1278W", "139.6503E"

### Optional Properties

**Label** - Text displayed with marker
- **Type:** string
- **Usage:** Shown when using default marker template

```csharp
public class MarkerData
{
    public string Label { get; set; }
    public string Latitude { get; set; }
    public string Longitude { get; set; }
    
    // Additional custom properties
    public string Category { get; set; }
    public int Population { get; set; }
    public string IconPath { get; set; }
}
```

### Coordinate Formats

```csharp
// Valid coordinate formats
new City { Latitude = "40.7128N", Longitude = "74.0060W" }      // New York
new City { Latitude = "51.5074N", Longitude = "0.1278W" }       // London
new City { Latitude = "35.6762N", Longitude = "139.6503E" }     // Tokyo
new City { Latitude = "33.8688S", Longitude = "151.2093E" }     // Sydney (Southern hemisphere)
new City { Latitude = "15.7833S", Longitude = "47.8667W" }      // Brasília
```

## Default Marker Rendering

Without custom templates, markers render with default styling:

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           Markers="{Binding Cities}"
                           MarkerIconFill="Red"
                           MarkerLabelForeground="Black" />
```

Default appearance:
- Small circle icon
- Label below icon
- Simple text formatting

## Custom Marker Templates

Use `MarkerTemplate` to fully customize marker appearance.

### Simple Custom Marker

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           Markers="{Binding Cities}">
    <syncfusion:ShapeFileLayer.MarkerTemplate>
        <DataTemplate>
            <Grid>
                <Ellipse Width="15" Height="15" Fill="Red" Stroke="White" StrokeThickness="2" />
                <TextBlock Text="{Binding Label}" 
                           Margin="0,18,0,0"
                           FontSize="12"
                           FontWeight="Bold"
                           Foreground="DarkRed"
                           HorizontalAlignment="Center" />
            </Grid>
        </DataTemplate>
    </syncfusion:ShapeFileLayer.MarkerTemplate>
</syncfusion:ShapeFileLayer>
```

### Marker with Icon Image

```xml
<syncfusion:ShapeFileLayer.MarkerTemplate>
    <DataTemplate>
        <StackPanel>
            <Image Source="/Images/pin.png" Width="24" Height="24" />
            <TextBlock Text="{Binding Label}" 
                       FontSize="10"
                       Foreground="White"
                       Background="#80000000"
                       Padding="4,2"
                       HorizontalAlignment="Center" />
        </StackPanel>
    </DataTemplate>
</syncfusion:ShapeFileLayer.MarkerTemplate>
```

### Path-Based Custom Marker

```xml
<syncfusion:ShapeFileLayer.MarkerTemplate>
    <DataTemplate>
        <Canvas Width="30" Height="40">
            <!-- Location pin shape -->
            <Path Data="M15,0 C6.7,0 0,6.7 0,15 C0,26.2 15,40 15,40 S30,26.2 30,15 C30,6.7 23.3,0 15,0 Z"
                  Fill="#E91E63"
                  Stroke="White"
                  StrokeThickness="2" />
            
            <!-- Inner circle -->
            <Ellipse Canvas.Left="10" Canvas.Top="10"
                     Width="10" Height="10"
                     Fill="White" />
            
            <!-- Label -->
            <TextBlock Text="{Binding Label}"
                       Canvas.Top="42"
                       FontSize="11"
                       FontWeight="SemiBold"
                       Foreground="#E91E63" />
        </Canvas>
    </DataTemplate>
</syncfusion:ShapeFileLayer.MarkerTemplate>
```

### Rich Marker with Multiple Data Points

```xml
<syncfusion:ShapeFileLayer.MarkerTemplate>
    <DataTemplate>
        <Border Background="White" 
                BorderBrush="#2196F3" 
                BorderThickness="2"
                CornerRadius="4"
                Padding="8,4">
            <StackPanel>
                <TextBlock Text="{Binding Label}" 
                           FontWeight="Bold" 
                           FontSize="12"
                           Foreground="#2196F3" />
                <TextBlock Text="{Binding Population, StringFormat='Pop: {0:N0}'}" 
                           FontSize="10"
                           Foreground="Gray" />
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:ShapeFileLayer.MarkerTemplate>
```

## Marker Template Selector

Use `MarkerTemplateSelector` to apply different templates based on data conditions.

### Creating Template Selector

```csharp
using System.Windows;
using System.Windows.Controls;

public class CityMarkerTemplateSelector : DataTemplateSelector
{
    public DataTemplate CapitalCityTemplate { get; set; }
    public DataTemplate RegularCityTemplate { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        if (item is City city)
        {
            return city.IsCapital ? CapitalCityTemplate : RegularCityTemplate;
        }
        return base.SelectTemplate(item, container);
    }
}
```

### Using Template Selector in XAML

```xml
<Window.Resources>
    <!-- Template for capital cities -->
    <DataTemplate x:Key="CapitalTemplate">
        <Grid>
            <Path Data="M12,2 L15,10 L23,10 L17,15 L19,23 L12,18 L5,23 L7,15 L1,10 L9,10 Z"
                  Fill="Gold" Stroke="DarkGoldenrod" StrokeThickness="1" />
            <TextBlock Text="{Binding Label}" 
                       Margin="0,28,0,0"
                       FontWeight="Bold"
                       Foreground="Gold" />
        </Grid>
    </DataTemplate>

    <!-- Template for regular cities -->
    <DataTemplate x:Key="RegularTemplate">
        <Grid>
            <Ellipse Width="10" Height="10" Fill="Blue" />
            <TextBlock Text="{Binding Label}" 
                       Margin="0,12,0,0"
                       FontSize="10"
                       Foreground="Blue" />
        </Grid>
    </DataTemplate>

    <!-- Template Selector -->
    <local:CityMarkerTemplateSelector x:Key="CityTemplateSelector"
                                      CapitalCityTemplate="{StaticResource CapitalTemplate}"
                                      RegularCityTemplate="{StaticResource RegularTemplate}" />
</Window.Resources>

<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                           Markers="{Binding Cities}"
                           MarkerTemplateSelector="{StaticResource CityTemplateSelector}" />
```

```csharp
public class City
{
    public string Label { get; set; }
    public string Latitude { get; set; }
    public string Longitude { get; set; }
    public bool IsCapital { get; set; }
}
```

## Marker Styling Properties

When using default markers (without custom template), these properties control appearance:

### MarkerIconFill

Sets the fill color for default marker icons.

**Type:** Brush  
**Default:** System default

```xml
<syncfusion:ShapeFileLayer MarkerIconFill="Red" />
```

```csharp
layer.MarkerIconFill = new SolidColorBrush(Colors.Red);
```

### MarkerLabelForeground

Sets the text color for marker labels.

**Type:** Brush  
**Default:** Black

```xml
<syncfusion:ShapeFileLayer MarkerLabelForeground="White" />
```

```csharp
layer.MarkerLabelForeground = new SolidColorBrush(Colors.White);
```

### Combined Styling

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           Markers="{Binding Cities}"
                           MarkerIconFill="DarkGreen"
                           MarkerLabelForeground="DarkGreen" />
```

## Advanced Marker Scenarios

### Scenario 1: Interactive Markers

```xml
<syncfusion:ShapeFileLayer.MarkerTemplate>
    <DataTemplate>
        <Button Background="Transparent" 
                BorderThickness="0"
                Click="OnMarkerClick"
                Tag="{Binding}">
            <StackPanel>
                <Ellipse Width="12" Height="12" Fill="Red" />
                <TextBlock Text="{Binding Label}" FontSize="10" />
            </StackPanel>
        </Button>
    </DataTemplate>
</syncfusion:ShapeFileLayer.MarkerTemplate>
```

```csharp
private void OnMarkerClick(object sender, RoutedEventArgs e)
{
    Button button = sender as Button;
    City city = button.Tag as City;
    MessageBox.Show($"Clicked: {city.Label}");
}
```

### Scenario 2: Animated Markers

```xml
<syncfusion:ShapeFileLayer.MarkerTemplate>
    <DataTemplate>
        <Grid>
            <Ellipse Width="20" Height="20" Fill="Red">
                <Ellipse.Triggers>
                    <EventTrigger RoutedEvent="Loaded">
                        <BeginStoryboard>
                            <Storyboard RepeatBehavior="Forever">
                                <DoubleAnimation Storyboard.TargetProperty="Opacity"
                                                 From="1" To="0.2" Duration="0:0:1"
                                                 AutoReverse="True" />
                            </Storyboard>
                        </BeginStoryboard>
                    </EventTrigger>
                </Ellipse.Triggers>
            </Ellipse>
            <TextBlock Text="{Binding Label}" Margin="0,22,0,0" />
        </Grid>
    </DataTemplate>
</syncfusion:ShapeFileLayer.MarkerTemplate>
```

### Scenario 3: Dynamic Marker Updates

```csharp
// Add marker at runtime
Cities.Add(new City 
{ 
    Label = "Paris", 
    Latitude = "48.8566N", 
    Longitude = "2.3522E" 
});

// Remove marker
Cities.Remove(Cities.First(c => c.Label == "Paris"));

// Update marker position
Cities[0].Latitude = "40.7589N";
Cities[0].Longitude = "73.9851W";
```

### Scenario 4: Marker Clustering

For dense marker sets, implement custom clustering logic:

```csharp
public class ClusteredMarker
{
    public string Label { get; set; }
    public string Latitude { get; set; }
    public string Longitude { get; set; }
    public int Count { get; set; } // Number of markers in cluster
}
```

```xml
<DataTemplate>
    <Grid>
        <Ellipse Width="{Binding Count}" 
                 Height="{Binding Count}"
                 Fill="Blue" Opacity="0.6" />
        <TextBlock Text="{Binding Count}" 
                   FontWeight="Bold" 
                   Foreground="White"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center" />
    </Grid>
</DataTemplate>
```

## Best Practices

1. **Coordinate accuracy** - Use precise latitude/longitude for accurate positioning
2. **Marker count** - Limit visible markers to avoid cluttering (use clustering for many markers)
3. **Template complexity** - Keep templates simple for better performance
4. **Label readability** - Use contrasting colors and appropriate font sizes
5. **Observable collections** - Use ObservableCollection for dynamic marker updates
6. **Memory management** - Clear marker collections when not needed

## Troubleshooting

### Markers not appearing

**Check:**
- Latitude/Longitude format includes N/S/E/W suffix
- Markers collection is not null or empty
- Coordinates are within map bounds
- MarkerTemplate doesn't have rendering issues

### Markers in wrong positions

**Verify:**
- Coordinate system matches shape file projection
- Latitude/Longitude values are correct
- No typos in coordinate strings (e.g., "40.7128N" not "40.7128")

### Labels not showing

**Ensure:**
- Label property exists in marker model
- Binding path in template is correct
- TextBlock is not hidden by other elements
- Foreground color contrasts with background
