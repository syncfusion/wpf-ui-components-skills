# Surface Types in WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Surface Type](#surface-type)
- [WireframeSurface Type](#wireframesurface-type)
- [Contour Type](#contour-type)
- [WireframeContour Type](#wireframecontour-type)
- [Choosing the Right Type](#choosing-the-right-type)

## Overview

Essential Surface Chart provides four types to plot three-dimensional data points, each suited for different visualization needs:

1. **Surface** - Standard 3D surface with solid rendering
2. **WireframeSurface** - Mesh/wireframe representation of the surface
3. **Contour** - Top-down 2D contour view with color mapping
4. **WireframeContour** - Mesh representation of contour view

The Type property determines how the data is rendered, allowing you to choose the most appropriate visualization for your specific use case.

## Surface Type

### Overview

Surface chart are used to explore the relationship between three-dimensional data. This is the standard solid surface rendering that creates a continuous 3D visualization of data points.

**When to Use:**
- Standard 3D data visualization
- Mathematical function plotting
- Smooth, continuous data representation
- When surface continuity is important
- Professional presentations

### XAML Implementation

```xaml
<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}"
                     Type="Surface">
</chart:SfSurfaceChart>
```

### Code-Behind Implementation

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.Surface;
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";
grid.Children.Add(chart);
```

### Sample Data for Surface Type

```csharp
public class DataViewModel
{
    public ObservableCollection<Data> DataValues { get; set; }
    public int RowSize { get; set; }
    public int ColumnSize { get; set; }
    
    public DataViewModel()
    {
        DataValues = new ObservableCollection<Data>();
        
        // Generate paraboloid surface: y = 2x² + 2z² - 4
        double inc = 8.0 / 35;
        for (double x = -4; x < 4; x += inc)
        {
            for (double z = -4; z < 4; z += inc)
            {
                double y = 2 * (x * x) + 2 * (z * z) - 4;
                DataValues.Add(new Data() { X = x, Y = y, Z = z });
            }
        }
        
        RowSize = 35;
        ColumnSize = 35;
    }
}
```

**Visual Result:** A smooth, colored 3D surface where colors represent the Y-axis values, creating a continuous visualization of the mathematical function.

## WireframeSurface Type

### Overview

You can draw the wireframe or mesh for the surface chart, showing the underlying grid structure. This type renders only the edges/lines of the surface mesh without filling the faces.

**When to Use:**
- When you need to see through the surface
- Analyzing mesh structure
- Understanding data point distribution
- Technical/engineering visualizations
- When background elements need to be visible

### XAML Implementation

```xaml
<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}"
                     Type="WireframeSurface">
</chart:SfSurfaceChart>
```

### Code-Behind Implementation

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.WireframeSurface;
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";
grid.Children.Add(chart);
```

### Customizing Wireframe Appearance

```csharp
chart.Type = SurfaceType.WireframeSurface;
chart.WireframeStroke = new SolidColorBrush(Colors.Green);
chart.WireframeStrokeThickness = 1;
```

**Visual Result:** A mesh grid structure showing the surface topology with transparent faces, allowing you to see the 3D structure from all angles.

## Contour Type

### Overview

Viewing the surface chart from the top is called contour. It is a graphical technique that represents the three-dimensional surface in a two-dimensional format, similar to topographical maps.

**When to Use:**
- Top-down view of 3D data
- Heat map visualization
- Topographical representations
- When vertical dimension is less important
- Simplified 2D analysis of 3D data
- Pattern recognition in data

### XAML Implementation

```xaml
<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}"
                     Type="Contour"
                     Rotate="0">
    
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar ShowLabel="True" DockPosition="Right"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

**Note:** Set Rotate="0" for proper top-down view.

### Code-Behind Implementation

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.Contour;
chart.Rotate = 0;

ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;

SetData();
grid.Children.Add(chart);
```

### Sample Data for Contour Type

```csharp
private void SetData()
{
    int x = 0;
    for (double i = -10; i <= 10; i++, x++)
    {
        int z = 0;
        for (double j = -10; j <= 10; j++, z++)
        {
            // Generate wave pattern: y = i*sin(j) + j*sin(i)
            double y = i * Math.Sin(j) + j * Math.Sin(i);
            chart.Data.AddPoints(x, y, z);
        }
    }
    
    chart.RowSize = 21;
    chart.ColumnSize = 21;
}
```

**Visual Result:** A flat, 2D representation with color gradients showing the Y-axis values, viewed from directly above. Perfect for identifying patterns and value distributions.

## WireframeContour Type

### Overview

You can draw the wireframe or mesh for the contour chart, combining the top-down view of contour with the mesh structure of wireframe.

**When to Use:**
- Need contour view with visible grid structure
- Analyzing data point positions in 2D
- Technical documentation
- Understanding sampling density
- When grid structure provides additional context

### XAML Implementation

```xaml
<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}"
                     Type="WireframeContour"
                     Rotate="0">
    
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar ShowLabel="True" DockPosition="Right"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Code-Behind Implementation

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.WireframeContour;
chart.Rotate = 0;

ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;

SetData();
grid.Children.Add(chart);
```

**Visual Result:** A 2D mesh grid with color-coded vertices, showing both the data distribution pattern and the underlying grid structure.

## Choosing the Right Type

### Decision Guide

**Use Surface when:**
- You need standard 3D visualization
- Surface continuity is important
- Presenting to general audiences
- Color-coding values across the surface
- Smooth, professional appearance is required

**Use WireframeSurface when:**
- You need to see through the surface
- Analyzing mesh quality or structure
- Technical/engineering contexts
- Multiple surfaces need to be visible simultaneously
- Understanding the underlying grid is important

**Use Contour when:**
- Top-down view is more useful than 3D
- Creating heat maps
- Identifying patterns or clusters
- Vertical dimension is less relevant
- Simplified 2D representation is needed
- Working with topographical or geographical data

**Use WireframeContour when:**
- Combining benefits of contour and wireframe
- Need to see grid structure in 2D view
- Analyzing data point distribution from above
- Technical documentation requiring grid visibility
- Understanding sampling density and uniformity

### Switching Types Dynamically

You can change the surface type at runtime based on user preferences:

```csharp
// Example: ComboBox selection
private void TypeComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string selectedType = (TypeComboBox.SelectedItem as ComboBoxItem)?.Content.ToString();
    
    switch (selectedType)
    {
        case "Surface":
            chart.Type = SurfaceType.Surface;
            break;
        case "Wireframe Surface":
            chart.Type = SurfaceType.WireframeSurface;
            break;
        case "Contour":
            chart.Type = SurfaceType.Contour;
            chart.Rotate = 0;
            break;
        case "Wireframe Contour":
            chart.Type = SurfaceType.WireframeContour;
            chart.Rotate = 0;
            break;
    }
}
```

### Performance Considerations

- **Surface:** Moderate rendering performance, good for most datasets
- **WireframeSurface:** Better performance with large datasets (fewer polygons to render)
- **Contour:** Best performance (2D rendering)
- **WireframeContour:** Best performance for large datasets in 2D view

For datasets with thousands of points, consider WireframeSurface or Contour types for better performance.

---