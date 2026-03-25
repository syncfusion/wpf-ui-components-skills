# Annotations in WPF Maps (SfMap)

Annotations allow you to add custom text, shapes, or images at specific positions on the map. They're useful for highlighting points of interest, adding labels, or displaying additional information.

## Overview

Annotations are custom visual elements positioned using X/Y coordinates on the map canvas. Unlike markers (which use latitude/longitude), annotations use pixel-based positioning.

**Key features:**
- Text annotations for labels
- Shape annotations (rectangles, ellipses, custom paths)
- Image annotations
- Custom templates for rich content
- Absolute positioning
- Visibility control

## Adding Annotations

### Annotations Collection

Annotations are added to the `Annotations` collection of ShapeFileLayer.

```xml
<syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp">
    <syncfusion:ShapeFileLayer.Annotations>
        <!-- Annotation items go here -->
    </syncfusion:ShapeFileLayer.Annotations>
</syncfusion:ShapeFileLayer>
```

## Text Annotations

Add text labels at specific positions.

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations
        AnnotationLabel="North America"
        AnnotationLabelFontFamily="Segoe UI"
        AnnotationLabelFontSize="14"
        AnnotationLabelForeground="#8C1313"
        Latitude="40.4230"
        Longitude="-112.7372" />
</syncfusion:ShapeFileLayer.Annotations>
```

## Shape Annotations

### Rectangle Annotation

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations AnnotationLabel="North America"
                                AnnotationLabelFontFamily="Segoe UI"
                                AnnotationLabelFontSize="14"
                                AnnotationLabelForeground="#8C1313"
                                AnnotationLabelFontStyle="Oblique"
                                Latitude="40.4230"
                                Longitude="-112.7372" >
        <syncfusion:MapAnnotations.AnnotationSymbol>
            <Rectangle Canvas.Left="150" Canvas.Top="100"
                        Width="200" Height="100"
                        Fill="Transparent"
                        Stroke="Red"
                        StrokeThickness="2"
                        StrokeDashArray="4 2" />
        </syncfusion:MapAnnotations.AnnotationSymbol>
    </syncfusion:MapAnnotations>
</syncfusion:ShapeFileLayer.Annotations>
```

### Ellipse/Circle Annotation

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations AnnotationLabel="North America"
                                AnnotationLabelFontFamily="Segoe UI"
                                AnnotationLabelFontSize="14"
                                AnnotationLabelForeground="#8C1313"
                                AnnotationLabelFontStyle="Oblique"
                                Latitude="40.4230"
                                Longitude="-112.7372" >
        <syncfusion:MapAnnotations.AnnotationSymbol>
            <Ellipse Canvas.Left="300" Canvas.Top="200"
                        Width="50" Height="50"
                        Fill="#80FF0000"
                        Stroke="Red"
                        StrokeThickness="2" />
        </syncfusion:MapAnnotations.AnnotationSymbol>
    </syncfusion:MapAnnotations>
</syncfusion:ShapeFileLayer.Annotations>
```

### Custom Path Annotation

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations AnnotationLabel="North America"
                                AnnotationLabelFontFamily="Segoe UI"
                                AnnotationLabelFontSize="14"
                                AnnotationLabelForeground="#8C1313"
                                AnnotationLabelFontStyle="Oblique"
                                Latitude="40.4230"
                                Longitude="-112.7372" >
        <syncfusion:MapAnnotations.AnnotationSymbol>
            <Path Data="M 0,0 L 100,0 L 50,50 Z"
                    Canvas.Left="250" 
                    Canvas.Top="180"
                    Fill="Yellow"
                    Stroke="Black"
                    StrokeThickness="1" />
        </syncfusion:MapAnnotations.AnnotationSymbol>
    </syncfusion:MapAnnotations>
</syncfusion:ShapeFileLayer.Annotations>
```

## Image Annotations

Add images or icons to the map.

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations AnnotationLabel="North America"
                                AnnotationLabelFontFamily="Segoe UI"
                                AnnotationLabelFontSize="14"
                                AnnotationLabelForeground="#8C1313"
                                AnnotationLabelFontStyle="Oblique"
                                Latitude="40.4230"
                                Longitude="-112.7372" >
        <syncfusion:MapAnnotations.AnnotationSymbol>
            <Image Source="/Images/landmark.png"
                    Canvas.Left="320" Canvas.Top="240"
                    Width="32" Height="32" />
        </syncfusion:MapAnnotations.AnnotationSymbol>
    </syncfusion:MapAnnotations>
</syncfusion:ShapeFileLayer.Annotations>
```

### Image with Label

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations AnnotationLabel="North America"
                                AnnotationLabelFontFamily="Segoe UI"
                                AnnotationLabelFontSize="14"
                                AnnotationLabelForeground="#8C1313"
                                AnnotationLabelFontStyle="Oblique"
                                Latitude="40.4230"
                                Longitude="-112.7372" >
        <syncfusion:MapAnnotations.AnnotationSymbol>
            <StackPanel Canvas.Left="400" Canvas.Top="300">
                <Image Source="/Images/airport.png" Width="24" Height="24" />
                <TextBlock Text="International Airport"
                            FontSize="11"
                            Foreground="Black"
                            HorizontalAlignment="Center"
                            Margin="0,4,0,0" />
            </StackPanel>
        </syncfusion:MapAnnotations.AnnotationSymbol>
    </syncfusion:MapAnnotations>
</syncfusion:ShapeFileLayer.Annotations>
```

## Complex Annotations

### Callout/Pointer Annotation

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations AnnotationLabel="North America"
                                AnnotationLabelFontFamily="Segoe UI"
                                AnnotationLabelFontSize="14"
                                AnnotationLabelForeground="#8C1313"
                                AnnotationLabelFontStyle="Oblique"
                                Latitude="40.4230"
                                Longitude="-112.7372" >
        <syncfusion:MapAnnotations.AnnotationSymbol>
            <Grid Canvas.Left="350" Canvas.Top="250">
                <Path Data="M 0,0 L 20,20 L 20,0 L 140,0 L 140,40 L 20,40 L 20,20 Z"
                        Fill="White"
                        Stroke="Black"
                        StrokeThickness="1" />
                <TextBlock Text="Important Location"
                            Canvas.Left="52" 
                            Canvas.Top="10"
                            FontSize="12"
                            FontWeight="Bold" />
            </Grid>
        </syncfusion:MapAnnotations.AnnotationSymbol>
    </syncfusion:MapAnnotations>
</syncfusion:ShapeFileLayer.Annotations>
```

### Information Box Annotation

```xml
<syncfusion:MapAnnotations.AnnotationSymbol>
    <Border Canvas.Left="100" Canvas.Top="50"
            Background="#F5F5F5"
            BorderBrush="#2196F3"
            BorderThickness="2"
            CornerRadius="6"
            Padding="12,8">
        <StackPanel Width="200">
            <TextBlock Text="Map Statistics"
                    FontSize="14"
                    FontWeight="Bold"
                    Foreground="#2196F3"
                    Margin="0,0,0,8" />
            <TextBlock Text="Total States: 50"
                    FontSize="11"
                    Margin="0,0,0,4" />
            <TextBlock Text="Total Population: 331M"
                    FontSize="11" />
        </StackPanel>
    </Border>
</syncfusion:MapAnnotations.AnnotationSymbol>
```

## Positioning
### Dynamic Positioning

```csharp
var annotation = new MapAnnotations
{
    AnnotationLabel = "Dynamic Annotation",
    Latitude = 40.4230,
    Longitude = -112.7372,
    AnnotationLabelFontSize = 14,
    AnnotationLabelForeground = Brushes.Red
};

layer.Annotations.Add(annotation);

annotation.Latitude = 45.0;
annotation.Longitude = -100.0;
```

## Visibility Control

### Toggle Visibility

```csharp
// Hide annotation
annotation.Visibility = Visibility.Collapsed;

// Show annotation
annotation.Visibility = Visibility.Visible;
```

## Complete Examples

### Example 1: Map with Title and Legend Annotations

```xml
<Grid>
    <!-- Map -->
    <syncfusion:SfMap>
        <syncfusion:SfMap.Layers>
            <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp">
                <syncfusion:ShapeFileLayer.Annotations>
                    <syncfusion:MapAnnotations >
                    <syncfusion:MapAnnotations.AnnotationSymbol>
                            <Canvas IsHitTestVisible="False">
                                <!-- Title -->
                                <TextBlock Text="United States Population Map"
                                        Canvas.Left="20"
                                        Canvas.Top="20"
                                        FontSize="20"
                                        FontWeight="Bold"
                                        Foreground="#1976D2" />

                                <!-- Subtitle -->
                                <TextBlock Text="Data as of 2020 Census"
                                        Canvas.Left="20"
                                        Canvas.Top="50"
                                        FontSize="12"
                                        Foreground="Gray" />

                                <!-- Scale indicator (static example) -->
                                <StackPanel Canvas.Left="20" Canvas.Bottom="20">
                                    <Rectangle Width="100" Height="4" Fill="Black" />
                                    <TextBlock Text="100 miles" FontSize="10" HorizontalAlignment="Center" />
                                </StackPanel>
                            </Canvas>
                        </syncfusion:MapAnnotations.AnnotationSymbol>
                    </syncfusion:MapAnnotations>
                <syncfusion:ShapeFileLayer.Annotations/>
            </syncfusion:ShapeFileLayer>
        </syncfusion:SfMap.Layers>
    </syncfusion:SfMap>
</Grid>
```

### Example 2: Highlight Region with Annotations

```xml
<syncfusion:ShapeFileLayer.Annotations>
    <syncfusion:MapAnnotations >
    <!-- Highlight circle -->
    <Ellipse Canvas.Left="280" Canvas.Top="170"
             Width="80" Height="80"
             Fill="Transparent"
             Stroke="Red"
             StrokeThickness="3" />
    </syncfusion:MapAnnotations >
    <syncfusion:MapAnnotations >
    <!-- Arrow pointing to region -->
    <Path Data="M 0,0 L 50,0 L 50,-10 L 70,5 L 50,20 L 50,10 L 0,10 Z"
          Canvas.Left="200" Canvas.Top="195"
          Fill="Red" />
    </syncfusion:MapAnnotations >
    <syncfusion:MapAnnotations >
    <!-- Label -->
    <Border Canvas.Left="50" Canvas.Top="190"
            Background="White"
            BorderBrush="Red"
            BorderThickness="2"
            Padding="8,4">
        <TextBlock Text="High Population Density Area"
                   FontSize="12"
                   FontWeight="Bold"
                   Foreground="Red" />
    </Border>
    </syncfusion:MapAnnotations >
</syncfusion:ShapeFileLayer.Annotations>
```

### Example 3: Programmatic Annotation Management

```csharp
public class MapViewModel
{
    private ShapeFileLayer layer;

    public void AddAnnotation(string text, double x, double y)
    {
        MapAnnotations annotation = new MapAnnotations
        {
            AnnotationLabel = text,
            AnnotationLabelFontSize = 12,
            AnnotationLabelForeground="#8C1313",
            AnnotationLabelFontFamily="Segoe UI"
        };

        Canvas.SetLeft(annotation, x);
        Canvas.SetTop(annotation, y);

        layer.Annotations.Add(annotation);
    }

    public void RemoveAnnotation(MapAnnotations annotation)
    {
        layer.Annotations.Remove(annotation);
    }

    public void ClearAnnotations()
    {
        layer.Annotations.Clear();
    }
}
```

## Best Practices

1. **Positioning** - Use absolute coordinates; test at different zoom levels
2. **Contrast** - Ensure annotations are visible against map background
3. **Borders** - Add borders or backgrounds to text annotations for readability
4. **Size** - Keep annotations reasonably sized to avoid cluttering
5. **Z-index** - Control layering with Panel.ZIndex if annotations overlap
6. **Performance** - Minimize complex annotations for maps with frequent updates
7. **Responsive** - Consider annotation visibility at different zoom levels

## Troubleshooting

### Annotations not visible

**Check:**
- Canvas.Left and Canvas.Top are within map boundaries
- Visibility is set to Visible
- Foreground/Fill color contrasts with background
- Not obscured by other elements

### Annotations in wrong position

**Verify:**
- Coordinates are correct (top-left origin)
- Using Canvas.Left/Top (not Margin)
- No conflicting layout properties

### Annotations don't move with zoom/pan

**Expected behavior** - Annotations use absolute positioning
- Use markers (with Latitude/Longitude) for geo-positioned elements that move with the map
- Annotations remain fixed relative to the map canvas
