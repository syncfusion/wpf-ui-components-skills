# Color Bar in WPF Surface Chart

## Overview

ColorBar is used to represent the value range in surface chart via colors. It acts as a legend that maps colors to numerical values, helping users interpret the Y-axis values displayed on the surface.

The color bar is essential for understanding what the colors on the surface represent, especially in contour views and heat map visualizations.

## Basic Configuration

### Adding a Color Bar

**XAML:**
```xaml
<chart:SfSurfaceChart ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}">
    
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar ShowLabel="True" DockPosition="Right"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

**Code-Behind:**
```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";

ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;

grid.Children.Add(chart);
```

## ColorBar Properties

### DockPosition

Controls where the color bar is positioned relative to the chart.

**Available Positions:**
- `Right` - Right side of the chart (most common)
- `Left` - Left side of the chart
- `Top` - Top of the chart
- `Bottom` - Bottom of the chart
- `Floating` - Docks element at any position on panel

**XAML Examples:**
```xaml
<!-- Right side (default/common) -->
<chart:ChartColorBar DockPosition="Right"/>

<!-- Left side -->
<chart:ChartColorBar DockPosition="Left"/>

<!-- Top -->
<chart:ChartColorBar DockPosition="Top"/>

<!-- Bottom -->
<chart:ChartColorBar DockPosition="Bottom"/>

<!-- Floating -->
<chart:ChartColorBar DockPosition="Floating"/>
```

**Code-Behind:**
```csharp
ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;  // or Left, Top, Bottom
```

### ShowLabel

Controls whether value labels are displayed on the color bar.

**XAML:**
```xaml
<!-- With labels (recommended) -->
<chart:ChartColorBar ShowLabel="True" DockPosition="Right"/>

<!-- Without labels -->
<chart:ChartColorBar ShowLabel="False" DockPosition="Right"/>
```

**Code-Behind:**
```csharp
ChartColorBar colorBar = new ChartColorBar();
colorBar.ShowLabel = true;  // Show value labels
colorBar.ShowLabel = false; // Hide value labels
```

**When to Use:**
- **ShowLabel="True"** - When users need to know exact values (recommended for most cases)
- **ShowLabel="False"** - When you only want relative color indication or space is limited

## Complete Examples

### Example 1: Standard Right-Side Color Bar

```xaml
<chart:SfSurfaceChart Type="Surface"
                     ItemsSource="{Binding DataValues}"
                     XBindingPath="X"
                     YBindingPath="Y"
                     ZBindingPath="Z"
                     RowSize="{Binding RowSize}"
                     ColumnSize="{Binding ColumnSize}">
    
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Example 2: Left-Side Color Bar

```xaml
<chart:SfSurfaceChart Type="Contour">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Left" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Example 3: Horizontal Color Bar at Top

```xaml
<chart:SfSurfaceChart Type="WireframeSurface">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Top" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Example 4: Code-Behind with Configuration

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.Contour;
chart.Rotate = 0;

// Configure color bar
ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;

// Set up data
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";

grid.Children.Add(chart);
```

## Usage with Different Surface Types

### Surface Type

Color bar shows the gradient of Y-values across the 3D surface:

```xaml
<chart:SfSurfaceChart Type="Surface">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Contour Type

Color bar is **essential** for contour chart to interpret the 2D color map:

```xaml
<chart:SfSurfaceChart Type="Contour" Rotate="0">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### WireframeSurface Type

Color bar shows vertex colors in the wireframe:

```xaml
<chart:SfSurfaceChart Type="WireframeSurface">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### WireframeContour Type

Color bar interprets the 2D mesh vertex colors:

```xaml
<chart:SfSurfaceChart Type="WireframeContour" Rotate="0">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

## Integration with Palettes

The color bar automatically reflects the palette applied to the surface:

```xaml
<chart:SfSurfaceChart Palette="Metro">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

The color bar will display the Metro palette's color gradient with corresponding value labels.

## Best Practices

### 1. Always Use with Contour Chart

```xaml
<!-- ✓ GOOD: Contour with color bar -->
<chart:SfSurfaceChart Type="Contour" Rotate="0">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>

<!-- ✗ POOR: Contour without color bar (hard to interpret) -->
<chart:SfSurfaceChart Type="Contour" Rotate="0"/>
```

### 2. Enable Labels for Quantitative Analysis

```csharp
// When users need to know specific values
colorBar.ShowLabel = true;
```

### 3. Choose Position Based on Layout

```csharp
// Right side - Most common, doesn't interfere with axes
colorBar.DockPosition = ChartDock.Right;

// Left side - When right side is crowded or for RTL layouts
colorBar.DockPosition = ChartDock.Left;

// Top/Bottom - For wide charts or when vertical space is limited
colorBar.DockPosition = ChartDock.Top;  // or Bottom
```

### 4. Consistent Positioning

```csharp
// Use the same position across similar charts in your application
// for consistency and user familiarity
const ChartDock ColorBarPosition = ChartDock.Right;
colorBar.DockPosition = ColorBarPosition;
```

## Common Scenarios

### Scenario 1: Heat Map Visualization

```xaml
<chart:SfSurfaceChart Type="Contour"
                     Rotate="0"
                     ApplyGradientBrush="True"
                     BrushCount="10">
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Scenario 2: Topographical Map

```xaml
<chart:SfSurfaceChart Type="Contour"
                     Rotate="0"
                     Palette="Custom">
    <!-- Custom earth-tone palette -->
    <chart:SfSurfaceChart.ColorBar>
        <chart:ChartColorBar DockPosition="Right" ShowLabel="True"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Scenario 3: Minimal UI (No Labels)

```xaml
<chart:SfSurfaceChart Type="Surface">
    <chart:SfSurfaceChart.ColorBar>
        <!-- Just the color gradient, no labels -->
        <chart:ChartColorBar DockPosition="Right" ShowLabel="False"/>
    </chart:SfSurfaceChart.ColorBar>
</chart:SfSurfaceChart>
```

### Scenario 4: Multiple Charts with Consistent Color Bar

```csharp
// Helper method for consistent color bar creation
private ChartColorBar CreateStandardColorBar()
{
    return new ChartColorBar
    {
        DockPosition = ChartDock.Right,
        ShowLabel = true
    };
}

// Use in multiple charts
chart1.ColorBar = CreateStandardColorBar();
chart2.ColorBar = CreateStandardColorBar();
chart3.ColorBar = CreateStandardColorBar();
```

## Troubleshooting

### Color Bar Not Showing

**Problem:** Color bar defined but not visible

**Solutions:**
1. Ensure ColorBar property is set:
```csharp
if (chart.ColorBar == null)
{
    chart.ColorBar = new ChartColorBar { DockPosition = ChartDock.Right, ShowLabel = true };
}
```

2. Check if data is loaded:
```csharp
// Color bar needs data to display value range
if (chart.ItemsSource == null || chart.Data.Count == 0)
{
    // Load data first
}
```

3. Verify chart has adequate space:
```xaml
<!-- Ensure parent container allows color bar to be visible -->
<Grid>
    <chart:SfSurfaceChart Margin="10"/>
</Grid>
```

### Labels Not Appearing

**Problem:** ShowLabel="True" but labels not visible

**Solution:**
```csharp
// Explicitly set ShowLabel
colorBar.ShowLabel = true;

// Ensure there's a value range
// (happens automatically with data)
```

---
