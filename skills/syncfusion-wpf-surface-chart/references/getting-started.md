# Getting Started with WPF Surface Chart

## Table of Contents
- [Overview](#overview)
- [Visual Structure](#visual-structure)
- [Installation and Setup](#installation-and-setup)
- [Creating Surface Chart from XAML](#creating-surface-chart-from-xaml)
- [Creating Surface Chart from Code-Behind](#creating-surface-chart-from-code-behind)
- [Complete Examples](#complete-examples)
- [Theme Support](#theme-support)

## Overview

The Syncfusion WPF Surface Chart (SfSurfaceChart) displays a three-dimensional surface that connects a set of data points. It's ideal for visualizing relationships between three variables and exploring mathematical functions or scientific data in 3D space.

**Key Features:**
- ColorBar represents a range of values
- Built-in palettes
- Supports gradient brushes
- Perspective and Orthographic view
- Contour and wireframe support

## Visual Structure

Understanding the elements of SfSurfaceChart:

**Main Elements:**
- **Surface Header** – Title of the surface chart
- **Wall** – Walls that bound the surface chart (left, bottom, back)
- **Major Gridlines** – Surface axis gridlines
- **Color Bar** – Displays the value range in color
- **Axis Label** – Labels for surface axes
- **Axis Header** – Headers for X, Y, and Z axes

These elements work together to provide a complete 3D visualization with clear value representation.

## Installation and Setup
 
### Step 1: Create WPF Application
 
Create a [WPF app for C# and .NET 8](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/overview/) or later.
 
### Step 2: Install NuGet Package
 
Add reference to the [Syncfusion.SfChart.WPF](https://www.nuget.org/packages/Syncfusion.SfChart.WPF) NuGet package.
 
**Package Manager Console:**
```
Install-Package Syncfusion.SfChart.WPF
```
 
**NuGet Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.SfChart.WPF"
3. Click Install

## Creating Surface Chart from XAML

### Step 1: Initialize the Surface Chart

Add the SfSurfaceChart control with the Syncfusion namespace:

```xaml
<syncfusion:SfSurfaceChart>
</syncfusion:SfSurfaceChart>
```

### Step 2: Add from Toolbox (Alternative)

You can drag the SfSurfaceChart control from the Toolbox to your design surface:
- Select **View > Toolbox** if not visible
- Locate SfSurfaceChart in the toolbox
- Drag to the desired location

This automatically adds the Syncfusion reference and xmlns namespace to your XAML.

### Step 3: Prepare Data Model

Create a data model to hold X, Y, Z values:

```csharp
public class Data
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Z { get; set; }
}
```

Create a ViewModel with data collection:

```csharp
public class ViewModel
{
    public ObservableCollection<Data> DataValues { get; set; }
    public int RowSize { get; set; }
    public int ColumnSize { get; set; }
    
    public ViewModel()
    {
        DataValues = new ObservableCollection<Data>();
        
        // Sample data - 3x3 grid
        DataValues.Add(new Data() { X = 0, Y = 3, Z = 0 });
        DataValues.Add(new Data() { X = 0, Y = 2, Z = 1 });
        DataValues.Add(new Data() { X = 0, Y = 1, Z = 2 });
        
        DataValues.Add(new Data() { X = 1, Y = 2, Z = 0 });
        DataValues.Add(new Data() { X = 1, Y = 1, Z = 1 });
        DataValues.Add(new Data() { X = 1, Y = 2, Z = 2 });
        
        DataValues.Add(new Data() { X = 2, Y = 1, Z = 0 });
        DataValues.Add(new Data() { X = 2, Y = 2, Z = 1 });
        DataValues.Add(new Data() { X = 2, Y = 3, Z = 2 });
        
        RowSize = 3;
        ColumnSize = 3;
    }
}
```

### Step 4: Bind Data to Surface Chart

```xaml
<syncfusion:SfSurfaceChart ItemsSource="{Binding DataValues}"
                          XBindingPath="X"
                          YBindingPath="Y"
                          ZBindingPath="Z"
                          RowSize="{Binding RowSize}"
                          ColumnSize="{Binding ColumnSize}">
</syncfusion:SfSurfaceChart>
```

### Step 5: Add Header

```xaml
<syncfusion:SfSurfaceChart Header="Simple Surface" FontSize="20" />
```

### Step 6: Add Axes

Define X, Y, and Z axes (optional - default axes are created automatically):

```xaml
<syncfusion:SfSurfaceChart ItemsSource="{Binding DataValues}"  
                           XBindingPath="X"  
                           YBindingPath="Y" ZBindingPath="Z" RowSize="{Binding RowSize}"
                           ColumnSize="{Binding ColumnSize}">
        <syncfusion:SfSurfaceChart.XAxis>
            <syncfusion:SurfaceAxis Header="X-Axis" />
        </syncfusion:SfSurfaceChart.XAxis>

        <syncfusion:SfSurfaceChart.YAxis>
            <syncfusion:SurfaceAxis Header="Y-Axis" LabelFormat="0.0"/>
        </syncfusion:SfSurfaceChart.YAxis>

        <syncfusion:SfSurfaceChart.ZAxis>
            <syncfusion:SurfaceAxis Header="Z-Axis"/>
        </syncfusion:SfSurfaceChart.ZAxis>
</syncfusion:SfSurfaceChart>
```

**Note:** SfSurfaceChart supports default axes - all axes are generated automatically even when not explicitly defined.

### Step 7: Add Surface Type

```xaml
<syncfusion:SfSurfaceChart Type="Surface" />
```

### Step 8: Add Color Bar

```xaml
<syncfusion:SfSurfaceChart.ColorBar>
    <syncfusion:ChartColorBar ShowLabel="True" DockPosition="Right"/>
</syncfusion:SfSurfaceChart.ColorBar>
```

### Complete XAML Example

```xaml
<syncfusion:SfSurfaceChart Type="Surface" 
                          Tilt="15" 
                          Rotate="30"
                          ItemsSource="{Binding DataValues}"
                          XBindingPath="X"
                          YBindingPath="Y"
                          ZBindingPath="Z"
                          RowSize="{Binding RowSize}"
                          ColumnSize="{Binding ColumnSize}">
    
    <syncfusion:SfSurfaceChart.XAxis>
        <syncfusion:SurfaceAxis Header="X-Axis" />
    </syncfusion:SfSurfaceChart.XAxis>
    
    <syncfusion:SfSurfaceChart.YAxis>
        <syncfusion:SurfaceAxis Header="Y-Axis" LabelFormat="0.0"/>
    </syncfusion:SfSurfaceChart.YAxis>
    
    <syncfusion:SfSurfaceChart.ZAxis>
        <syncfusion:SurfaceAxis Header="Z-Axis"/>
    </syncfusion:SfSurfaceChart.ZAxis>
    
    <syncfusion:SfSurfaceChart.ColorBar>
        <syncfusion:ChartColorBar DockPosition="Right"/>
    </syncfusion:SfSurfaceChart.ColorBar>
</syncfusion:SfSurfaceChart>
```

## Creating Surface Chart from Code-Behind

### Step 1: Initialize Surface Chart

```csharp
SfSurfaceChart surface = new SfSurfaceChart();
```

### Step 2: Add Data Using AddPoints Method

You can directly add data points using the AddPoints(x, y, z) method:

```csharp
SfSurfaceChart surface = new SfSurfaceChart();

// First Row
surface.Data.AddPoints(0, 3, 0);
surface.Data.AddPoints(0, 2, 1);
surface.Data.AddPoints(0, 2, 2);

// Second Row
surface.Data.AddPoints(1, 2, 0);
surface.Data.AddPoints(1, 1, 1);
surface.Data.AddPoints(1, 2, 2);

// Third Row
surface.Data.AddPoints(2, 1, 0);
surface.Data.AddPoints(2, 2, 1);
surface.Data.AddPoints(2, 3, 2);

surface.RowSize = 3;
surface.ColumnSize = 3;

grid.Children.Add(surface);
```

### Step 3: Add Header

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Header = "Simple Surface";
chart.FontSize = 20;
grid.Children.Add(chart);
```

### Step 4: Add Axes

```csharp
SfSurfaceChart chart = new SfSurfaceChart();

// Add X axis
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.Header = "X-Axis";
chart.XAxis = xAxis;

// Add Y axis
SurfaceAxis yAxis = new SurfaceAxis();
yAxis.Header = "Y-Axis";
yAxis.LabelFormat = "0.0";
chart.YAxis = yAxis;

// Add Z axis
SurfaceAxis zAxis = new SurfaceAxis();
zAxis.Header = "Z-Axis";
chart.ZAxis = zAxis;

grid.Children.Add(chart);
```

**Note:** SfSurfaceChart supports default axes where all axes are generated automatically, even when not defined explicitly.

### Step 5: Add Surface Type

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.Type = SurfaceType.Surface;
grid.Children.Add(chart);
```

### Step 6: Add ColorBar

```csharp
ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
chart.ColorBar = colorBar;
```

## Complete Examples

### Example 1: Using ItemsSource (Data Binding)

```csharp
SfSurfaceChart chart = new SfSurfaceChart();
chart.SetBinding(SfSurfaceChart.ItemsSourceProperty, "DataValues");
chart.SetBinding(SfSurfaceChart.RowSizeProperty, "RowSize");
chart.SetBinding(SfSurfaceChart.ColumnSizeProperty, "ColumnSize");
chart.XBindingPath = "X";
chart.YBindingPath = "Y";
chart.ZBindingPath = "Z";
grid.Children.Add(chart);
```

### Example 2: Using Direct Data Points

```csharp
SfSurfaceChart surface = new SfSurfaceChart();
surface.Header = "Simple Surface";
surface.Tilt = 15;
surface.Rotate = 30;
surface.Type = SurfaceType.Surface;

// First Row
surface.Data.AddPoints(0, 3, 0);
surface.Data.AddPoints(0, 2, 1);
surface.Data.AddPoints(0, 2, 2);

// Second Row
surface.Data.AddPoints(1, 2, 0);
surface.Data.AddPoints(1, 1, 1);
surface.Data.AddPoints(1, 2, 2);

// Third Row
surface.Data.AddPoints(2, 1, 0);
surface.Data.AddPoints(2, 2, 1);
surface.Data.AddPoints(2, 3, 2);

surface.RowSize = 3;
surface.ColumnSize = 3;

// Add X axis
SurfaceAxis xAxis = new SurfaceAxis();
xAxis.Header = "X-Axis";
surface.XAxis = xAxis;

// Add Y axis
SurfaceAxis yAxis = new SurfaceAxis();
yAxis.Header = "Y-Axis";
surface.YAxis = yAxis;

// Add Z axis
SurfaceAxis zAxis = new SurfaceAxis();
zAxis.Header = "Z-Axis";
surface.ZAxis = zAxis;

// Add color bar
ChartColorBar colorBar = new ChartColorBar();
colorBar.DockPosition = ChartDock.Right;
colorBar.ShowLabel = true;
surface.ColorBar = colorBar;

this.Content = surface;
```

## Theme Support

WPF SurfaceChart supports various built-in themes for consistent application styling.

**Applying Themes:**

- **Using SfSkinManager:** Apply themes across the application
- **Using ThemeStudio:** Create custom themes to match your brand

Available themes include Metro, Office 2019, Material Design, and more.

**Theme Application:**
Refer to Syncfusion's theming documentation for:
- Applying themes using SfSkinManager
- Creating custom themes using ThemeStudio

Themes provide consistent colors, fonts, and styling across all Syncfusion controls, including the Surface Chart.

---
