---
name: syncfusion-wpf-map
description: Implement Syncfusion WPF Maps (SfMap) control for geographical data visualization in Windows Presentation Foundation applications. Use this when displaying geographical data, rendering shape files, or integrating map layers with markers and bubbles. This skill covers ShapeFileLayer, ImageryLayer, map customization, legends, OpenStreetMap/Bing Maps integration, and interactive map features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Maps (SfMap)

The Syncfusion WPF Maps control is a powerful component for visualizing geographical data in Windows Presentation Foundation applications. It provides comprehensive support for rendering shape files, displaying map imagery, adding markers and bubbles, and creating interactive data visualizations with rich customization options.

## When to Use This Skill

Use this skill when you need to:
- **Render geographical maps** using shape files (.shp, .dbf)
- **Visualize statistical data** on maps with color coding
- **Add interactive elements** like markers, bubbles, and tooltips
- **Display map imagery** from OpenStreetMap or Bing Maps
- **Create choropleth maps** with data-driven color mapping
- **Implement map legends** for data interpretation
- **Enable user interaction** with zoom, pan, and selection
- **Build dashboards** with geographical data visualization
- **Support multilayer maps** with overlays and sublayers
- **Customize map appearance** with themes and templates

## Component Overview

**SfMap** is a graphical representation control that displays geographical data with the following capabilities:

### Core Features
- **Multiple layer types:** ShapeFileLayer for vector shapes, ImageryLayer for tiles
- **Data binding:** Connect business objects to map shapes
- **Visual elements:** Markers, bubbles, annotations, labels
- **Color mapping:** Range-based and categorical color coding
- **Legends:** Automatic legend generation with customization
- **User interaction:** Zoom, pan, selection, tooltips
- **Multilayer support:** Sublayers and KML overlays

### Key Use Cases
- Weather data visualization
- Population density maps
- Sales territory analysis
- Political/election maps
- Building/floor plan layouts
- Resource availability mapping

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet (Syncfusion.SfMaps.WPF)
- Required assemblies and namespace imports
- Creating your first SfMap control
- Understanding shape file structure (.shp, .dbf)
- Configuring Uri property for shape files
- Basic XAML and C# implementation
- Minimal working example

### Layer Configuration

📄 **Read:** [references/layers.md](references/layers.md)
- ShapeFileLayer vs ImageryLayer
- ShapeSettings configuration
- ShapeFill, ShapeStroke, ShapeStrokeThickness properties
- EnableSelection for interactive selection
- Customizing selected shapes (SelectedShapeColor, SelectedShapeStroke)
- Layer background and visual properties
- Multiple layers management

📄 **Read:** [references/map-providers.md](references/map-providers.md)
- ImageryLayer for map tiles
- OpenStreetMap integration
- Bing Maps configuration
- Custom tile providers
- Zoom levels and caching

### Data Binding and Population

📄 **Read:** [references/data-binding.md](references/data-binding.md)
- ItemsSource binding to collections
- ShapeIDPath and ShapeIDTableField mapping
- ShapeValuePath for numerical data
- ShapeColorValuePath for color binding
- ObservableCollection with INotifyPropertyChanged
- MVVM pattern implementation
- Binding troubleshooting

### Visual Elements

📄 **Read:** [references/markers.md](references/markers.md)
- Adding markers to maps
- Marker model properties (Label, Latitude, Longitude)
- Default marker rendering
- MarkerTemplate customization
- MarkerTemplateSelector for conditional styling
- MarkerIconFill and MarkerLabelForeground
- Positioning and alignment

📄 **Read:** [references/bubbles.md](references/bubbles.md)
- BubbleMarkerSetting configuration
- ValuePath for data-driven sizing
- MinSize and MaxSize properties
- ColorValuePath and ColorMappings
- AutoFillColor vs custom Fill
- Stroke, StrokeThickness, Opacity
- Bubble legends

📄 **Read:** [references/shapes-color-customization.md](references/shapes-color-customization.md)
- Shape color properties
- AutoFillColors property
- RangeColorMapping for continuous data
- EqualsColorMapping for categorical data
- Tree map-like visualization
- Color palettes and gradients
- Custom color logic

📄 **Read:** [references/legends.md](references/legends.md)
- LegendVisibility and positioning
- LegendPosition enum values
- LegendPositionX and LegendPositionY for custom placement
- LegendHeader for titles
- LegendType (Layers vs Bubbles)
- LegendIcon shapes (Rectangle, Circle, Diamond, Triangle)
- LegendColumnSplit for multi-column layout
- Custom legend templates

📄 **Read:** [references/map-labels.md](references/map-labels.md)
- LabelPath for shape labeling
- ShapeLabelTemplate customization
- Label positioning and visibility
- Font styling and formatting
- Zoom-based label display

### Interactivity

📄 **Read:** [references/user-interaction.md](references/user-interaction.md)
- ToolTipSettings configuration
- EnableZoom and EnablePan properties
- MinZoom and MaxZoom constraints
- ZoomLevel property
- EnableSelection for shape selection
- Mouse, touch, and keyboard interactions
- Custom tooltip templates
- Programmatic zoom and pan

📄 **Read:** [references/events.md](references/events.md)
- ShapeSelected event
- ShapeSelectionChanged event
- BubbleMarkerSelected event
- MarkerSelected event
- ZoomLevelChanged event
- Event arguments and data access

### Advanced Features

📄 **Read:** [references/annotations.md](references/annotations.md)
- Annotations collection
- Text, shape, and image annotations
- Coordinate positioning
- Custom annotation templates
- Visibility and interaction

📄 **Read:** [references/multilayer-support.md](references/multilayer-support.md)
- SubShapeFileLayers collection
- Adding multiple shape layers
- KML file support
- KmlItemsTemplate customization
- Layer ordering and z-index
- Combining vector and imagery layers

## Quick Start Example

Here's a minimal example to create a map with data binding and color mapping:

### XAML

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF">
    <Grid>
        <Grid.DataContext>
            <local:MapViewModel />
        </Grid.DataContext>

        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.world1.shp"
                                           ItemsSource="{Binding Countries}"
                                           ShapeIDPath="Name"
                                           ShapeIDTableField="NAME"
                                           EnableSelection="True">
                    
                    <syncfusion:ShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeValuePath="Population"
                                                 ShapeStroke="White"
                                                 ShapeStrokeThickness="1">
                            <syncfusion:ShapeSetting.FillSetting>
                                <syncfusion:ShapeFillSetting AutoFillColors="False">
                                    <syncfusion:ShapeFillSetting.ColorMappings>
                                        <syncfusion:RangeColorMapping From="0" To="1000000" Color="#FEE5D9" />
                                        <syncfusion:RangeColorMapping From="1000000" To="10000000" Color="#FCAE91" />
                                        <syncfusion:RangeColorMapping From="10000000" To="100000000" Color="#FB6A4A" />
                                        <syncfusion:RangeColorMapping From="100000000" To="1000000000" Color="#CB181D" />
                                    </syncfusion:ShapeFillSetting.ColorMappings>
                                </syncfusion:ShapeFillSetting>
                            </syncfusion:ShapeSetting.FillSetting>
                        </syncfusion:ShapeSetting>
                    </syncfusion:ShapeFileLayer.ShapeSettings>                 
                </syncfusion:ShapeFileLayer>
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

### C# ViewModel

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class Country : INotifyPropertyChanged
{
    private string name;
    private int population;

    public string Name
    {
        get { return name; }
        set { name = value; OnPropertyChanged(nameof(Name)); }
    }

    public int Population
    {
        get { return population; }
        set { population = value; OnPropertyChanged(nameof(Population)); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class MapViewModel
{
    public ObservableCollection<Country> Countries { get; set; }

    public MapViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "United States", Population = 331000000 },
            new Country { Name = "China", Population = 1400000000 },
            new Country { Name = "India", Population = 1380000000 },
            new Country { Name = "Brazil", Population = 212000000 }
        };
    }
}
```

## Common Patterns

### Pattern 1: Adding Markers with Custom Templates

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp"
                           Markers="{Binding Cities}">
    <syncfusion:ShapeFileLayer.MarkerTemplate>
        <DataTemplate>
            <Grid>
                <Ellipse Width="12" Height="12" Fill="Red" />
                <TextBlock Text="{Binding Label}" 
                           Margin="0,15,0,0"
                           FontSize="11"
                           Foreground="Black" />
            </Grid>
        </DataTemplate>
    </syncfusion:ShapeFileLayer.MarkerTemplate>
</syncfusion:ShapeFileLayer>
```

```csharp
public class City
{
    public string Label { get; set; }
    public string Latitude { get; set; }
    public string Longitude { get; set; }
}
```

### Pattern 2: Implementing Tooltips

```xml
<syncfusion:ShapeFileLayer.ToolTipSettings>
    <syncfusion:ToolTipSetting ValuePath="Population" />
</syncfusion:ShapeFileLayer.ToolTipSettings>
```

### Pattern 3: Enabling Interactive Zoom and Pan

```xml
<syncfusion:SfMap EnableZoom="True" 
                  EnablePan="True"
                  MinZoom="1"
                  MaxZoom="20"
                  ZoomLevel="5" />
```

### Pattern 4: Creating Legends

```xml
<syncfusion:ShapeFileLayer LegendVisibility="Visible"
                           LegendPosition="BottomLeft"
                           LegendHeader="Population Density"
                           LegendIcon="Rectangle"
                           LegendColumnSplit="2" />
```

## Key Properties

### SfMap Properties
- **EnableZoom** (bool): Enable/disable zooming functionality
- **EnablePan** (bool): Enable/disable panning functionality
- **MinZoom** (double): Minimum zoom level
- **MaxZoom** (double): Maximum zoom level
- **ZoomLevel** (double): Current zoom level
- **Layers** (ObservableCollection&lt;MapLayer&gt;): Collection of map layers

### ShapeFileLayer Properties
- **Uri** (string): Path to shape file (.shp) as embedded resource
- **ItemsSource** (IEnumerable): Data source for binding
- **ShapeIDPath** (string): Property path in data source for shape matching
- **ShapeIDTableField** (string): Field name in .dbf file for matching
- **EnableSelection** (bool): Enable shape selection
- **Markers** (IEnumerable): Collection of marker objects
- **LegendVisibility** (Visibility): Show/hide legend
- **LegendPosition** (LegendPosition): Standard position for legend
- **LegendPositionX/Y** (double): Custom legend coordinates
- **LegendHeader** (string): Legend title text
- **LegendIcon** (LegendIcons): Icon shape for legend items
- **LegendType** (LegendType): Legend for shapes or bubbles
- **LegendColumnSplit** (int): Number of legend columns
- **ToolTipSettings** (ToolTipSetting): Tooltip configuration

### ShapeSetting Properties
- **ShapeFill** (Brush): Default fill color for shapes
- **ShapeStroke** (Brush): Border color for shapes
- **ShapeStrokeThickness** (double): Border thickness
- **ShapeValuePath** (string): Property for data values
- **ShapeColorValuePath** (string): Property for color values
- **SelectedShapeColor** (Brush): Fill color for selected shapes
- **SelectedShapeStroke** (Brush): Border color for selected shapes
- **SelectedShapeStrokeThickness** (double): Border thickness for selected shapes
- **FillSetting** (ShapeFillSetting): Color mapping configuration

### BubbleMarkerSetting Properties
- **ValuePath** (string): Property for bubble size values
- **ColorValuePath** (string): Property for bubble colors
- **MinSize** (double): Minimum bubble diameter
- **MaxSize** (double): Maximum bubble diameter
- **Fill** (Brush): Bubble fill color
- **Stroke** (Brush): Bubble border color
- **StrokeThickness** (double): Bubble border thickness
- **Opacity** (double): Bubble transparency (0-1)
- **AutoFillColor** (bool): Enable automatic color assignment
- **ColorMappings** (ObservableCollection): Range/equals color mappings

## Common Use Cases

### Use Case 1: Population Density Map
Display population data with color-coded regions and interactive tooltips showing detailed statistics.

### Use Case 2: Sales Territory Visualization
Show sales performance across geographical regions with bubbles representing revenue and colors indicating growth rates.

### Use Case 3: Election Results Map
Visualize voting results with categorical colors for different candidates and legends for interpretation.

### Use Case 4: Weather Data Dashboard
Display temperature, precipitation, or other weather metrics on maps with real-time data updates.

### Use Case 5: Store Location Finder
Mark store locations with custom markers and enable zoom/pan for users to explore different areas.

### Use Case 6: Building Floor Plans
Render building layouts using custom shape files with room labeling and occupancy data visualization.

---

**Next Steps:** Navigate to the specific reference files above based on your implementation needs. Start with `getting-started.md` for initial setup, then explore other references as needed.
