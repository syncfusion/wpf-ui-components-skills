# Getting Started with WPF Maps (SfMap)

This guide walks you through the installation, setup, and basic implementation of the Syncfusion WPF Maps control.

## Installation

### Method 1: NuGet Package Manager

Install the SfMap control from NuGet:

```powershell
Install-Package Syncfusion.SfMaps.WPF
```

Or search for **Syncfusion.SfMaps.WPF** in the NuGet Package Manager UI in Visual Studio.

### Method 2: Toolbox

Drag the SfMap control from the Visual Studio toolbox and drop it onto the designer. This automatically:
- References required assemblies
- Adds namespace declarations
- Creates the control markup

### Method 3: Manual Assembly References

Add the following assemblies manually from the Syncfusion installation directory:

**Location:** `{Installed location}/{version}/WPF/Assemblies`

**Required assemblies:**
- `Syncfusion.SfMaps.WPF.dll`
- `Syncfusion.Shared.WPF.dll`

## Namespace Imports

Add the Maps namespace to your XAML:

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

In C# code-behind or ViewModel:

```csharp
using Syncfusion.UI.Xaml.Maps;
```

## Creating Your First Map

### XAML Approach

```xml
<syncfusion:SfMap>
    <syncfusion:SfMap.Layers>
        <syncfusion:ShapeFileLayer Uri="MyApp.ShapeFiles.usa_state.shp" />
    </syncfusion:SfMap.Layers>
</syncfusion:SfMap>
```

### C# Approach

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        SfMap map = new SfMap();
        ShapeFileLayer layer = new ShapeFileLayer();
        layer.Uri = "MyApp.ShapeFiles.usa_state.shp";
        map.Layers.Add(layer);
        
        this.Content = map;
    }
}
```

### Expression Blend Approach

1. Create a WPF project in Expression Blend
2. Reference required assemblies (Syncfusion.SfMaps.WPF, Syncfusion.Shared.WPF)
3. Search for SfMap in the Toolbox
4. Drag SfMap to the designer
5. Configure properties in the Properties panel

## Understanding Shape Files

Maps control reads and renders geographical data from shape files.

### Shape File Structure

A shape file is a collection of files that store spatial and attribute data:

**Required files:**
- **Main file (.shp)** - Contains vector geometry (points, lines, polygons)
- **dBase file (.dbf)** - Contains attribute data for shapes

**Optional files:**
- **Index file (.shx)** - Positional index
- **Projection file (.prj)** - Coordinate system information

**8.3 Naming Convention:**
All files must have the same prefix (e.g., `usa_state.shp`, `usa_state.dbf`).

### Adding Shape Files to Your Project

1. **Create a folder** in your project (e.g., `ShapeFiles`)
2. **Add shape files** (.shp and .dbf) to the folder
3. **Set Build Action** to **Embedded Resource** for both files:
   - Right-click each file in Solution Explorer
   - Select Properties
   - Set "Build Action" to "Embedded Resource"

### Uri Property Format

The Uri property locates the shape file added as an embedded resource:

**Format:** `Namespace.FolderName.FileName.shp`

**Example:**
```xml
<syncfusion:ShapeFileLayer Uri="MyApplication.ShapeFiles.usa_state.shp" />
```

**Breakdown:**
- `MyApplication` - Root namespace of your project
- `ShapeFiles` - Folder name containing shape files
- `usa_state.shp` - Shape file name

**C# Example:**
```csharp
ShapeFileLayer layer = new ShapeFileLayer();
layer.Uri = "MyApplication.ShapeFiles.world1.shp";
```

## Complete Working Example

### XAML

```xml
<Window x:Class="MapsDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Maps;assembly=Syncfusion.SfMaps.WPF"
        Title="My First Map" Height="600" Width="900">
    
    <Grid>
        <syncfusion:SfMap>
            <syncfusion:SfMap.Layers>
                <syncfusion:ShapeFileLayer Uri="MapsDemo.ShapeFiles.usa_state.shp">
                    <syncfusion:ShapeFileLayer.ShapeSettings>
                        <syncfusion:ShapeSetting ShapeFill="LightBlue" 
                                                 ShapeStroke="White" 
                                                 ShapeStrokeThickness="1" />
                    </syncfusion:ShapeFileLayer.ShapeSettings>
                </syncfusion:ShapeFileLayer>
            </syncfusion:SfMap.Layers>
        </syncfusion:SfMap>
    </Grid>
</Window>
```

### C# Code-Behind

```csharp
using System.Windows;
using Syncfusion.UI.Xaml.Maps;
using System.Windows.Media;

namespace MapsDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            CreateMapProgrammatically();
        }

        private void CreateMapProgrammatically()
        {
            // Create SfMap instance
            SfMap map = new SfMap();

            // Create ShapeFileLayer
            ShapeFileLayer layer = new ShapeFileLayer();
            layer.Uri = "MapsDemo.ShapeFiles.usa_state.shp";

            // Configure ShapeSettings
            ShapeSetting shapeSetting = new ShapeSetting();
            shapeSetting.ShapeFill = new SolidColorBrush(Colors.LightBlue);
            shapeSetting.ShapeStroke = new SolidColorBrush(Colors.White);
            shapeSetting.ShapeStrokeThickness = 1;
            layer.ShapeSettings = shapeSetting;

            // Add layer to map
            map.Layers.Add(layer);

            // Set as window content
            this.Content = map;
        }
    }
}
```

## Verifying Installation

After setup, you should see:
- Map renders successfully without errors
- Shapes display with correct borders and fills
- No missing assembly warnings in Output window

## Common Setup Issues

### Issue: "Could not load file or assembly"
**Solution:** Ensure all required assemblies are referenced and their versions match.

### Issue: "Uri not found" or blank map
**Solution:** 
- Verify shape files are set to "Embedded Resource"
- Check Uri format matches your namespace and folder structure
- Ensure both .shp and .dbf files are included

### Issue: Shapes not displaying correctly
**Solution:**
- Verify shape file is not corrupted
- Ensure coordinate system is supported
- Check .dbf file contains required attribute data

## Next Steps

Now that you have a basic map rendering, explore:
- Data binding to populate shape attributes
- Adding markers to highlight locations
- Color mapping for data visualization
- Legends for data interpretation
- User interaction with zoom and selection

Refer to other reference files for detailed guidance on each feature.

## Licensing

Remember to register your Syncfusion license key in `App.xaml.cs`:

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        base.OnStartup(e);
    }
}
```

Get your license key from the Syncfusion website or use the free Community License if eligible.
