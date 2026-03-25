---
name: syncfusion-wpf-surface-chart
description: Guide for implementing Syncfusion WPF Surface Chart (SfSurfaceChart) components for three-dimensional data visualization. Use this skill always when the user mentions 3D charts, surface chart, surface visualization, 3D data plotting, contour charts, wireframe charts, SfSurfaceChart, or needs to visualize three-dimensional data points with X, Y, Z coordinates. Apply this when user requests 3D data visualization, mathematical surface plotting, topographical charts, or any scenario involving three axes of data.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Surface Chart
The `SfSurfaceChart` is a three-dimensional surface that connects a set of data points.

## When to Use This Skill

Use this skill when the user needs to:
- Visualize three-dimensional data with X, Y, and Z coordinates
- Create 3D surface plots for mathematical functions or scientific data
- Display topographical or elevation maps
- Show contour plots or wireframe visualizations
- Implement interactive 3D charts with rotation and zooming
- Render heat maps with color-coded values
- Visualize relationships between three variables
- Create mesh or grid-based 3D visualizations

The Syncfusion WPF Surface Chart (SfSurfaceChart) is specifically designed for rendering three-dimensional data in a grid format, supporting multiple visualization types including standard surfaces, wireframes, and contour views.

## Component Overview

**SfSurfaceChart** is a powerful 3D visualization control that displays data points in three-dimensional space. It connects data points to create a continuous surface, making it ideal for exploring relationships between three variables.

**Key Capabilities:**
- Four surface types: Surface, WireframeSurface, Contour, WireframeContour
- Multiple data binding methods (ItemsSource or direct data points)
- Three customizable axes (X, Y, Z)
- Color bars for value representation
- Predefined and custom color palettes
- Interactive rotation, tilting, and zooming
- Perspective and orthographic camera projections
- Gradient and solid color rendering

**Visual Elements:**
- Surface Header - Title of the chart
- Wall - Bounding walls around the chart (left, bottom, back)
- Major Gridlines - Axis gridlines
- Color Bar - Value range indicator with color mapping
- Axis Labels - Labels for X, Y, Z axes
- Axis Headers - Headers for each axis

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly reference and installation
- Visual structure of surface chart elements
- Creating surface chart from XAML
- Creating surface chart from code-behind
- Basic initialization and setup
- Adding headers, axes, and color bars
- Complete working examples
- Theme support and customization

### Surface Types
📄 **Read:** [references/surface-types.md](references/surface-types.md)
- Overview of 4 surface type options
- Surface - Standard 3D surface visualization
- WireframeSurface - Mesh/wireframe rendering
- Contour - Top-down 2D contour view
- WireframeContour - Mesh contour visualization
- Choosing the right type for your data
- Type property configuration
- Visual comparisons and use cases

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Data structure requirements (grid format)
- Using ItemsSource property with collections
- XBindingPath, YBindingPath, ZBindingPath mapping
- RowSize and ColumnSize configuration
- Direct data input via Data.AddPoints method
- Creating data models and ViewModels
- Binding in XAML vs code-behind
- Performance considerations

### Axes Configuration
📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)
- X, Y, and Z axis setup
- Default vs explicit axis definitions
- Header and HeaderTemplate customization
- LabelTemplate and LabelFormat options
- Minimum and Maximum range settings
- Interval and tick configuration
- GridLines customization (stroke, thickness)
- EdgeLabelsDrawingMode options
- RangePadding settings
- Complete axis styling examples

### Surface Area Customization
📄 **Read:** [references/surface-area.md](references/surface-area.md)
- Surface area properties overview
- Rotation and Rotate property
- Tilt property for viewing angle
- EnableRotation for interactivity
- CameraProjection (Perspective/Orthographic)
- Header and title customization
- Wall brushes (LeftWallBrush, BottomWallBrush, BackWallBrush)
- WallThickness configuration
- Wireframe stroke and thickness
- ShowContourLine option
- BrushCount for color gradients
- ApplyGradientBrush setting

### Color Bar Configuration
📄 **Read:** [references/color-bar.md](references/color-bar.md)
- ColorBar purpose and value representation
- DockPosition property (Left, Right, Top, Bottom)
- ShowLabel property for value display
- Adding and configuring color bars
- Integration with palettes
- XAML and code-behind setup

### Palettes and Colors
📄 **Read:** [references/palettes.md](references/palettes.md)
- Palette overview and purpose
- 12 predefined palettes (Metro, AutumnBrights, FloraHues, Pineapple, etc.)
- Applying predefined palettes via Palette property
- Creating custom palettes with ChartColorModel
- CustomBrushes collection
- Gradient vs solid color rendering
- Visual palette examples

### User Interactions
📄 **Read:** [references/user-interactions.md](references/user-interactions.md)
- Zooming capabilities and EnableZooming
- ZoomLevel property for programmatic zoom
- Pinch zooming support
- Rotation with EnableRotation
- Rotate property for programmatic rotation
- Tilt property for viewing angle
- Interactive vs programmatic control
- Combining rotation, tilt, and zoom

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)
- Common issues and solutions
- Data not displaying correctly
- RowSize and ColumnSize mismatches
- Axis range and scaling problems
- Performance optimization tips
- Color bar visibility issues
- Wireframe rendering problems
- Assembly reference errors

## Quick Start Example

### Basic Surface Chart in XAML

```xaml
<Window xmlns:chart="clr-namespace:Syncfusion.UI.Xaml.Charts;assembly=Syncfusion.SfChart.WPF">
    <Grid>
        <chart:SfSurfaceChart Type="Surface" 
                             ItemsSource="{Binding DataValues}"
                             XBindingPath="X" 
                             YBindingPath="Y" 
                             ZBindingPath="Z"
                             RowSize="{Binding RowSize}"
                             ColumnSize="{Binding ColumnSize}"
                             Header="3D Surface Chart"
                             Rotate="30" 
                             Tilt="15">
            
            <!-- X Axis -->
            <chart:SfSurfaceChart.XAxis>
                <chart:SurfaceAxis Header="X-Axis" />
            </chart:SfSurfaceChart.XAxis>
            
            <!-- Y Axis -->
            <chart:SfSurfaceChart.YAxis>
                <chart:SurfaceAxis Header="Y-Axis" LabelFormat="0.0" />
            </chart:SfSurfaceChart.YAxis>
            
            <!-- Z Axis -->
            <chart:SfSurfaceChart.ZAxis>
                <chart:SurfaceAxis Header="Z-Axis" />
            </chart:SfSurfaceChart.ZAxis>
            
            <!-- Color Bar -->
            <chart:SfSurfaceChart.ColorBar>
                <chart:ChartColorBar DockPosition="Right" ShowLabel="True" />
            </chart:SfSurfaceChart.ColorBar>
        </chart:SfSurfaceChart>
    </Grid>
</Window>
```

### Data Model and ViewModel

```csharp
// Data Model
public class Data
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Z { get; set; }
}

// ViewModel
public class ViewModel
{
    public ObservableCollection<Data> DataValues { get; set; }
    public int RowSize { get; set; }
    public int ColumnSize { get; set; }
    
    public ViewModel()
    {
        DataValues = new ObservableCollection<Data>();
        
        // Generate sample data for mathematical surface
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

### Basic Surface Chart in Code-Behind

```csharp
using Syncfusion.UI.Xaml.Charts;

// Create surface chart
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.Surface;
chart.Header = "3D Surface Chart";
chart.Rotate = 30;
chart.Tilt = 15;

// Add data directly using AddPoints
for (double x = -4; x < 4; x += 0.2)
{
    for (double z = -4; z < 4; z += 0.2)
    {
        double y = 2 * (x * x) + 2 * (z * z) - 4;
        chart.Data.AddPoints(x, y, z);
    }
}

chart.RowSize = 40;
chart.ColumnSize = 40;

// Configure axes
chart.XAxis = new SurfaceAxis { Header = "X-Axis" };
chart.YAxis = new SurfaceAxis { Header = "Y-Axis", LabelFormat = "0.0" };
chart.ZAxis = new SurfaceAxis { Header = "Z-Axis" };

// Add color bar
ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;

// Add to grid
grid.Children.Add(chart);
```

## Common Patterns

### Pattern 1: Mathematical Function Visualization

When visualizing mathematical functions in 3D:

```csharp
// Generate data for z = sin(x) * cos(y)
for (double x = -Math.PI; x <= Math.PI; x += 0.1)
{
    for (double y = -Math.PI; y <= Math.PI; y += 0.1)
    {
        double z = Math.Sin(x) * Math.Cos(y);
        chart.Data.AddPoints(x, z, y);
    }
}

chart.RowSize = 63;  // Number of x points
chart.ColumnSize = 63;  // Number of y points
```

### Pattern 2: Contour Chart for Top-Down View

When user needs a 2D contour view of 3D data:

```xaml
<chart:SfSurfaceChart Type="Contour" 
                     Rotate="0"
                     ItemsSource="{Binding DataValues}"
                     XBindingPath="X" 
                     YBindingPath="Y" 
                     ZBindingPath="Z">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Pattern 3: Interactive Chart with User Control

When user needs to interact with the chart:

```xaml
<chart:SfSurfaceChart EnableRotation="True" 
                     EnableZooming="True"
                     Type="Surface">
    <!-- Chart will respond to mouse drag (rotation) and mouse wheel (zoom) -->
</chart:SfSurfaceChart>
```

### Pattern 4: Custom Color Palette

When user needs specific colors:

```xaml
<chart:SfSurfaceChart Palette="Custom">
    <chart:SfSurfaceChart.ColorModel>
        <chart:ChartColorModel>
            <chart:ChartColorModel.CustomBrushes>
                <SolidColorBrush Color="Blue"/>
                <SolidColorBrush Color="Lime"/>
                <SolidColorBrush Color="Yellow"/>
                <SolidColorBrush Color="OrangeRed"/>
            </chart:ChartColorModel.CustomBrushes>
        </chart:ChartColorModel>
    </chart:SfSurfaceChart.ColorModel>
</chart:SfSurfaceChart>
```

### Pattern 5: Wireframe Visualization

When user needs to see the mesh structure:

```csharp
chart.Type = SurfaceType.WireframeSurface;
chart.WireframeStroke = new SolidColorBrush(Colors.Green);
chart.WireframeStrokeThickness = 1;
```

## Key Properties

### Essential Properties
- **Type** - Surface type (Surface, WireframeSurface, Contour, WireframeContour)
- **ItemsSource** - Data collection for binding
- **XBindingPath, YBindingPath, ZBindingPath** - Property paths for X, Y, Z values
- **RowSize, ColumnSize** - Grid dimensions for data
- **Data** - Direct data collection using AddPoints method

### Visual Properties
- **Header** - Chart title
- **Rotate** - Rotation angle (0-360)
- **Tilt** - Tilt angle for viewing perspective
- **Palette** - Color scheme (Metro, AutumnBrights, FloraHues, etc.)
- **ColorBar** - Value range indicator with colors

### Interaction Properties
- **EnableRotation** - Allow mouse-drag rotation
- **EnableZooming** - Allow mouse-wheel zoom
- **ZoomLevel** - Programmatic zoom level

### Axis Properties
- **XAxis, YAxis, ZAxis** - Axis configuration objects
- Each axis supports: Header, LabelFormat, Minimum, Maximum, Interval, etc.

### Customization Properties
- **CameraProjection** - Perspective or Orthographic view
- **LeftWallBrush, BottomWallBrush, BackWallBrush** - Wall colors
- **WallThickness** - Wall border thickness
- **WireframeStroke, WireframeStrokeThickness** - Wireframe appearance
- **ApplyGradientBrush** - Enable gradient coloring
- **BrushCount** - Number of color steps in gradient

## Common Use Cases

### Use Case 1: Scientific Data Visualization
**Scenario:** Researcher needs to visualize experimental data with X, Y coordinates and measured Z values.

**Approach:** Use ItemsSource binding with data collection, configure appropriate axes with scientific notation, and use a clear color palette like Metro or BlueChrome for professional appearance.

### Use Case 2: Mathematical Function Plotting
**Scenario:** Student needs to visualize a 3D mathematical function like z = f(x, y).

**Approach:** Generate data programmatically using nested loops, use Data.AddPoints for direct input, enable rotation for exploration, and use wireframe or standard surface type for clarity.

### Use Case 3: Topographical Mapping
**Scenario:** Display elevation data or terrain visualization.

**Approach:** Use Contour type for 2D representation with color gradients, or Surface type for 3D terrain. Apply earth-toned custom palette (green-brown-white for elevation levels).

### Use Case 4: Heat Map Visualization
**Scenario:** Show intensity or value distribution across two dimensions.

**Approach:** Use Contour type with Rotate="0" for flat view, configure ColorBar with ShowLabel="True", and apply gradient brushes with appropriate BrushCount for smooth transitions.

### Use Case 5: Interactive Data Exploration
**Scenario:** User needs to explore complex 3D dataset from multiple angles.

**Approach:** Enable both EnableRotation and EnableZooming, provide initial good viewing angle with Rotate and Tilt, and consider adding UI controls to switch between Surface types dynamically.

### Use Case 6: Engineering Analysis
**Scenario:** Visualize stress, temperature, or pressure distribution in engineering simulations.

**Approach:** Use Surface or WireframeSurface type, apply custom color palette matching engineering standards (e.g., blue for cold/low, red for hot/high), and configure axes with appropriate units and precision.

---

## Next Steps

1. **For initial setup:** Read [getting-started.md](references/getting-started.md)
2. **To choose visualization type:** Read [surface-types.md](references/surface-types.md)
3. **For data integration:** Read [data-binding.md](references/data-binding.md)
4. **For detailed customization:** Navigate to specific feature references as needed

Remember: SfSurfaceChart requires data in grid format with consistent RowSize and ColumnSize. The order of data points matters - they should be organized in row-major or column-major format for proper surface rendering.
