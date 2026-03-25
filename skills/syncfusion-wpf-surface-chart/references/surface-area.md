# Surface Area Customization in WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Surface Area Properties](#surface-area-properties)
- [Rotation and Tilt](#rotation-and-tilt)
- [Camera Projection](#camera-projection)
- [Header Customization](#header-customization)
- [Wall Customization](#wall-customization)
- [Wireframe Properties](#wireframe-properties)
- [Visual Effects](#visual-effects)
- [Complete Examples](#complete-examples)

## Overview

Surface area represents the entire surface chart and all its elements. It's a virtual rectangle area that includes all surface elements like axes, color bar, walls, and the data surface itself.

The SfSurfaceChart provides extensive APIs to customize the surface area based on your requirements, from basic appearance to advanced visual effects.

## Surface Area Properties

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| **EnableRotation** | bool | Enable/disable interactive rotation |
| **Rotate** | double | Rotation angle (0-360 degrees) |
| **Tilt** | double | Tilt angle for viewing perspective |
| **CameraProjection** | CameraProjection | Perspective or Orthographic view |
| **Header** | object | Chart title/header content |
| **LeftWallBrush** | Brush | Brush for left wall |
| **BottomWallBrush** | Brush | Brush for bottom wall |
| **BackWallBrush** | Brush | Brush for back wall |
| **WallThickness** | WallThickness | Thickness of wall borders |
| **WireframeStrokeThickness** | double | Wireframe line thickness |
| **WireframeStroke** | Brush | Wireframe line color |
| **Palette** | ChartColorPalette | Color palette for the surface |
| **ZoomLevel** | double | Zoom level (0.0 to 1.0) |
| **Type** | SurfaceType | Surface, Wireframe, Contour, etc. |
| **ApplyGradientBrush** | bool | Enable gradient coloring |
| **ShowContourLine** | bool | Show contour lines on surface |
| **BrushCount** | int | Number of color steps in gradient |
| **EnableZooming** | bool | Enable/disable zooming |
| **XAxis** | SurfaceAxis | X-Axis for the surface chart |
| **YAxis** | SurfaceAxis | Y-Axis for the surface chart |
| **ZAxis** | SurfaceAxis | Z-Axis for the surface chart |
| **ItemsSource** | IEnumerable | Items source for surface chart |
| **XBindingPath** | string | X member path value of items source |
| **YBindingPath** | string | Y member path value of items source |
| **ZBindingPath** | string | Z member path value of items source |
| **ColorBar** | ChartColorBar | Color bar for surface chart|

## Rotation and Tilt

### Rotate Property

Controls the rotation angle around the vertical axis (Y-axis).

**XAML:**
```xaml
<chart:SfSurfaceChart Rotate="30" />
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Rotate = 30;  // 30 degrees rotation
```

**Range:** 0 to 360 degrees
- 0° - Front view
- 90° - Side view
- 180° - Back view
- 270° - Opposite side view

### Tilt Property

Controls the tilt angle (viewing from above or below).

**XAML:**
```xaml
<chart:SfSurfaceChart Tilt="15" />
```

**Code-Behind:**
```csharp
chart.Tilt = 15;  // 15 degrees tilt
```

**Typical Values:**
- 0° - Completely flat (top-down view)
- 15-30° - Slight tilt (common for 3D visualization)
- 45-60° - Moderate tilt
- 90° - Vertical view (side view)

### EnableRotation

Allow users to rotate the chart interactively with mouse drag.

**XAML:**
```xaml
<chart:SfSurfaceChart EnableRotation="True" Rotate="20" />
```

**Code-Behind:**
```csharp
chart.EnableRotation = true;
chart.Rotate = 20;  // Initial rotation
// Users can now drag to rotate
```

### Combined Rotation and Tilt Example

```xaml
<chart:SfSurfaceChart Type="Surface"
                     EnableRotation="True"
                     Rotate="45"
                     Tilt="20">
</chart:SfSurfaceChart>
```

This provides a good initial viewing angle with the ability for users to explore the data from different perspectives.

## Camera Projection

### Perspective vs Orthographic

**CameraProjection** determines how 3D depth is rendered:

**Perspective (Default):**
- Objects farther away appear smaller (realistic)
- Provides depth perception
- Better for general visualization

**Orthographic:**
- All objects same size regardless of distance
- No depth distortion
- Better for technical/engineering diagrams

### XAML Configuration

```xaml
<chart:SfSurfaceChart CameraProjection="Perspective" />
<!-- or -->
<chart:SfSurfaceChart CameraProjection="Orthographic" />
```

### Code-Behind Configuration

```csharp
// Perspective projection (default)
chart.CameraProjection = CameraProjection.Perspective;

// Orthographic projection
chart.CameraProjection = CameraProjection.Orthographic;
```

### When to Use Each

**Use Perspective when:**
- Creating general visualizations
- Depth perception is important
- Presenting to general audiences
- Natural, realistic appearance desired

**Use Orthographic when:**
- Creating technical drawings
- Precise measurements are important
- CAD-like visualization needed
- No perspective distortion desired

## Header Customization

### Simple Text Header

```xaml
<chart:SfSurfaceChart Header="3D Surface Chart" FontSize="20" />
```

```csharp
chart.Header = "3D Surface Chart";
chart.FontSize = 20;
```

### Custom Header Object

```xaml
<chart:SfSurfaceChart>
    <chart:SfSurfaceChart.Header>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="Surface Analysis" 
                      FontSize="18" 
                      FontWeight="Bold" 
                      Margin="0,0,10,0"/>
            <TextBlock Text="(Interactive 3D View)" 
                      FontSize="12" 
                      Foreground="Gray" 
                      VerticalAlignment="Bottom"/>
        </StackPanel>
    </chart:SfSurfaceChart.Header>
</chart:SfSurfaceChart>
```

## Wall Customization

### Overview

Surface chart have three walls that bound the visualization:
- **Left Wall** - Left side boundary
- **Bottom Wall** - Floor/base
- **Back Wall** - Rear boundary

### Setting Wall Colors

**XAML:**
```xaml
<chart:SfSurfaceChart LeftWallBrush="BlanchedAlmond"
                     BottomWallBrush="LightGray"
                     BackWallBrush="WhiteSmoke">
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
chart.LeftWallBrush = new SolidColorBrush(Colors.BlanchedAlmond);
chart.BottomWallBrush = new SolidColorBrush(Colors.LightGray);
chart.BackWallBrush = new SolidColorBrush(Colors.WhiteSmoke);
```

### Wall Thickness

Control the border thickness of walls:

```xaml
<chart:SfSurfaceChart WallThickness="2" />
```

```csharp
chart.WallThickness = 2;
```

### Complete Wall Customization

```xaml
<chart:SfSurfaceChart LeftWallBrush="LightBlue"
                     BottomWallBrush="LightGreen"
                     BackWallBrush="LightYellow"
                     WallThickness="1.5">
</chart:SfSurfaceChart>
```

## Wireframe Properties

### WireframeStroke

Set the color of wireframe lines:

```xaml
<chart:SfSurfaceChart Type="WireframeSurface"
                     WireframeStroke="Green">
</chart:SfSurfaceChart>
```

```csharp
chart.Type = SurfaceType.WireframeSurface;
chart.WireframeStroke = new SolidColorBrush(Colors.Green);
```

### WireframeStrokeThickness

Set the thickness of wireframe lines:

```xaml
<chart:SfSurfaceChart Type="WireframeSurface"
                     WireframeStroke="Green"
                     WireframeStrokeThickness="1">
</chart:SfSurfaceChart>
```

```csharp
chart.WireframeStrokeThickness = 1;
```

### Wireframe with Custom Colors

```xaml
<chart:SfSurfaceChart Type="WireframeSurface"
                     WireframeStroke="DarkGreen"
                     WireframeStrokeThickness="1.5"
                     LeftWallBrush="LightGray">
</chart:SfSurfaceChart>
```

## Visual Effects

### ApplyGradientBrush

Enable smooth gradient transitions between colors:

```xaml
<chart:SfSurfaceChart ApplyGradientBrush="True" />
```

```csharp
chart.ApplyGradientBrush = true;
```

**Effect:**
- True - Smooth color gradients
- False - Discrete color bands

### BrushCount

Control the number of color steps in the gradient:

```xaml
<chart:SfSurfaceChart ApplyGradientBrush="True" BrushCount="4" />
```

```csharp
chart.ApplyGradientBrush = true;
chart.BrushCount = 4;
```

**Values:**
- Lower values (2-5) - Distinct color bands
- Higher values (10+) - Smooth gradients
- Default - Based on palette

### ShowContourLine

Display contour lines on the surface:

```xaml
<chart:SfSurfaceChart Type="Surface" ShowContourLine="True" />
```

```csharp
chart.Type = SurfaceType.Surface;
chart.ShowContourLine = true;
```

This overlays contour lines on top of the 3D surface, showing elevation levels.

### ZoomLevel

Programmatically control zoom level:

```xaml
<chart:SfSurfaceChart EnableZooming="True" ZoomLevel="0.5" />
```

```csharp
chart.EnableZooming = true;
chart.ZoomLevel = 0.5;  // 0.0 (far) to 1.0 (close)
```

## Complete Examples

### Example 1: Comprehensive Surface Area Customization

```xaml
<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}"
                     Type="WireframeSurface"
                     Header="Custom Surface Chart"
                     ApplyGradientBrush="True"
                     BrushCount="4"
                     LeftWallBrush="BlanchedAlmond"
                     BottomWallBrush="LightGray"
                     BackWallBrush="WhiteSmoke"
                     WallThickness="1.5"
                     WireframeStroke="Green"
                     WireframeStrokeThickness="1"
                     CameraProjection="Perspective"
                     Tilt="10"
                     Rotate="20"
                     EnableRotation="True"
                     EnableZooming="True">
</chart:SfSurfaceChart>
```

### Example 2: Code-Behind Complete Setup

```csharp
SfSurfaceChart chart = new SfSurfaceChart();

// Data binding
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";

// Type and visual effects
chart.Type = SurfaceType.WireframeSurface;
chart.ApplyGradientBrush = true;
chart.BrushCount = 4;

// Wall customization
chart.LeftWallBrush = new SolidColorBrush(Colors.BlanchedAlmond);
chart.BottomWallBrush = new SolidColorBrush(Colors.LightGray);
chart.BackWallBrush = new SolidColorBrush(Colors.WhiteSmoke);
chart.WallThickness = 1.5;

// Wireframe customization
chart.WireframeStroke = new SolidColorBrush(Colors.Green);
chart.WireframeStrokeThickness = 1;

// View customization
chart.CameraProjection = CameraProjection.Perspective;
chart.Tilt = 10;
chart.Rotate = 20;
chart.EnableRotation = true;
chart.EnableZooming = true;

grid.Children.Add(chart);
```

### Example 3: Contour with Surface Lines

```xaml
<chart:SfSurfaceChart Type="Contour"
                     Rotate="0"
                     ShowContourLine="True"
                     ApplyGradientBrush="True"
                     BrushCount="10"
                     ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Example 4: Technical Orthographic View

```xaml
<chart:SfSurfaceChart Type="WireframeSurface"
                     CameraProjection="Orthographic"
                     WireframeStroke="Black"
                     WireframeStrokeThickness="0.5"
                     LeftWallBrush="White"
                     BottomWallBrush="White"
                     BackWallBrush="White"
                     WallThickness="1"
                     Rotate="45"
                     Tilt="30">
</chart:SfSurfaceChart>
```

### Example 5: Dynamic View Control

```csharp
// Allow users to control view with sliders
private void RotationSlider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (chart != null)
    {
        chart.Rotate = e.NewValue;
    }
}

private void TiltSlider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (chart != null)
    {
        chart.Tilt = e.NewValue;
    }
}

private void ZoomSlider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    if (chart != null)
    {
        chart.ZoomLevel = e.NewValue;
    }
}
```

**XAML for Controls:**
```xaml
<StackPanel>
    <Label Content="Rotation:"/>
    <Slider Name="RotationSlider" 
            Minimum="0" 
            Maximum="360" 
            Value="30" 
            ValueChanged="RotationSlider_ValueChanged"/>
    
    <Label Content="Tilt:"/>
    <Slider Name="TiltSlider" 
            Minimum="0" 
            Maximum="90" 
            Value="15" 
            ValueChanged="TiltSlider_ValueChanged"/>
    
    <Label Content="Zoom:"/>
    <Slider Name="ZoomSlider" 
            Minimum="0" 
            Maximum="1" 
            Value="0.5" 
            ValueChanged="ZoomSlider_ValueChanged"/>
</StackPanel>
```

## Best Practices

### 1. Start with Good Default Angles

```csharp
// Good starting point for most visualizations
chart.Rotate = 30;
chart.Tilt = 20;
```

### 2. Enable User Control When Appropriate

```csharp
// For exploratory data analysis
chart.EnableRotation = true;
chart.EnableZooming = true;
```

### 3. Choose Projection Based on Use Case

```csharp
// General visualization
chart.CameraProjection = CameraProjection.Perspective;

// Technical/CAD applications
chart.CameraProjection = CameraProjection.Orthographic;
```

### 4. Coordinate Wall Colors with Theme

```csharp
// Neutral colors work well with any palette
chart.LeftWallBrush = new SolidColorBrush(Color.FromRgb(240, 240, 240));
chart.BottomWallBrush = new SolidColorBrush(Color.FromRgb(245, 245, 245));
chart.BackWallBrush = new SolidColorBrush(Color.FromRgb(250, 250, 250));
```

### 5. Gradient Settings for Smooth Appearance

```csharp
// Smooth, professional gradients
chart.ApplyGradientBrush = true;
chart.BrushCount = 10;  // Higher for smoother gradients
```

### 6. Wireframe Thickness for Visibility

```csharp
// Balance visibility and clutter
if (dataPointCount > 1000)
{
    chart.WireframeStrokeThickness = 0.5;  // Thinner for dense data
}
else
{
    chart.WireframeStrokeThickness = 1.5;  // Thicker for sparse data
}
```

### 7. Responsive Zoom Initialization

```csharp
// Start zoomed out for overview
chart.EnableZooming = true;
chart.ZoomLevel = 0.3;  // User can zoom in for detail
```

---
