# Getting Started with WPF Bullet Graph

This guide covers installation, setup, and creating your first bullet graph in WPF applications using the Syncfusion `SfBulletGraph` control.

## Installation Options

### Option 1: Using Syncfusion Reference Manager

The Syncfusion Reference Manager is a Visual Studio extension that simplifies adding Syncfusion controls to your project.

**Steps:**
1. Create a WPF application in Visual Studio
2. Right-click on the project and select **Syncfusion Reference Manager**
3. In the Reference Manager wizard, search for **SfBulletGraph**
4. Select the **SfBulletGraph** control and click **Done**
5. Click **OK** to add the required assemblies

The assemblies will be automatically added to your project:
- `Syncfusion.SfBulletGraph.WPF.dll`
- Required dependencies

**Note:** Syncfusion Reference Manager is available in versions 11.3.0.30 and later, supports assemblies from version 10.4.0.71 onwards, and works with Visual Studio 2015 and later.

### Option 2: Using NuGet Package Manager

Install the bullet graph package using NuGet Package Manager Console:

```powershell
Install-Package Syncfusion.SfBulletGraph.WPF
```

Or use the NuGet Package Manager UI in Visual Studio:
1. Right-click on **References** → **Manage NuGet Packages**
2. Search for **Syncfusion.SfBulletGraph.WPF**
3. Click **Install**

### Option 3: Manual Assembly Reference

Add a reference to the assembly manually:
1. Right-click on **References** → **Add Reference**
2. Browse to the Syncfusion installation folder
3. Add `Syncfusion.SfBulletGraph.WPF.dll`

**Default installation path:**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\Assemblies for WPF\
```

## Required Assembly and Namespace

**Assembly:** `Syncfusion.SfBulletGraph.WPF`  
**Namespace:** `Syncfusion.UI.Xaml.BulletGraph`

## Adding Namespace References

### XAML Namespace Declaration

Add the namespace reference in your XAML window or user control:

**Option 1: Using Syncfusion's global namespace (recommended)**
```xml
<Window x:Class="YourApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <!-- Your content here -->
</Window>
```

**Option 2: Using specific namespace and assembly**
```xml
<Window x:Class="YourApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:bulletgraph="clr-namespace:Syncfusion.UI.Xaml.BulletGraph;assembly=Syncfusion.SfBulletGraph.WPF">
    <!-- Your content here -->
</Window>
```

### C# Namespace Import

Add the using directive in your C# code files:

```csharp
using Syncfusion.UI.Xaml.BulletGraph;
```

## Creating Your First Bullet Graph

### Minimal XAML Example

```xml
<Window x:Class="BulletGraphDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Bullet Graph Demo" Height="350" Width="525">
    <Grid x:Name="LayoutRoot">
        <syncfusion:SfBulletGraph/>
    </Grid>
</Window>
```

This creates a default bullet graph with standard settings.

### Minimal C# Example

```csharp
using System.Windows;
using Syncfusion.UI.Xaml.BulletGraph;

namespace BulletGraphDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            SfBulletGraph bulletGraph = new SfBulletGraph();
            this.LayoutRoot.Children.Add(bulletGraph);
        }
    }
}
```

## Basic Configured Example

A more practical example with configured properties:

### XAML Implementation

```xml
<Window x:Class="BulletGraphDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Revenue Dashboard" Height="350" Width="600">
    <Grid>
        <syncfusion:SfBulletGraph 
            Orientation="Horizontal" 
            Minimum="0" 
            Maximum="10" 
            Interval="2"
            ComparativeMeasure="7" 
            FeaturedMeasure="5" 
            FeaturedMeasureBarStroke="Black" 
            MajorTickStroke="Red"
            MinorTickStroke="Green" 
            MinorTicksPerInterval="3"
            MajorTickSize="15" 
            MinorTickSize="10">
            
            <syncfusion:SfBulletGraph.QualitativeRanges>
                <syncfusion:QualitativeRange RangeEnd="4.5" RangeStroke="#EBEBEB"/>
                <syncfusion:QualitativeRange RangeEnd="7.5" RangeStroke="#D8D8D8"/>
                <syncfusion:QualitativeRange RangeEnd="10" RangeStroke="#7F7F7F"/>
            </syncfusion:SfBulletGraph.QualitativeRanges>
        </syncfusion:SfBulletGraph>
    </Grid>
</Window>
```

### C# Implementation

```csharp
using System.Windows;
using System.Windows.Media;
using Syncfusion.UI.Xaml.BulletGraph;

namespace BulletGraphDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            CreateBulletGraph();
        }
        
        private void CreateBulletGraph()
        {
            SfBulletGraph bulletGraph = new SfBulletGraph();
            bulletGraph.Minimum = 0;
            bulletGraph.Maximum = 10;
            bulletGraph.Interval = 2;
            bulletGraph.FeaturedMeasure = 5;
            bulletGraph.ComparativeMeasure = 7;
            bulletGraph.MajorTickStroke = new SolidColorBrush(Colors.Red);
            bulletGraph.MinorTickStroke = new SolidColorBrush(Colors.Green);
            bulletGraph.FeaturedMeasureBarStroke = new SolidColorBrush(Colors.Black);
            bulletGraph.MinorTicksPerInterval = 3;
            bulletGraph.MinorTickSize = 10;
            bulletGraph.MajorTickSize = 15;
            bulletGraph.Orientation = Orientation.Horizontal;
            
            // Add qualitative ranges
            QualitativeRange range1 = new QualitativeRange();
            range1.RangeEnd = 4.5;
            range1.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#EBEBEB");
            
            QualitativeRange range2 = new QualitativeRange();
            range2.RangeEnd = 7.5;
            range2.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#D8D8D8");
            
            QualitativeRange range3 = new QualitativeRange();
            range3.RangeEnd = 10;
            range3.RangeStroke = (Brush)new BrushConverter().ConvertFrom("#7F7F7F");
            
            bulletGraph.QualitativeRanges.Add(range1);
            bulletGraph.QualitativeRanges.Add(range2);
            bulletGraph.QualitativeRanges.Add(range3);
            
            this.LayoutRoot.Children.Add(bulletGraph);
        }
    }
}
```

## Theme Support

The bullet graph supports Syncfusion's built-in themes for consistent styling across your application.

### Applying Themes Using SfSkinManager

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialDark}">
    <Grid>
        <syncfusion:SfBulletGraph Minimum="0" Maximum="10" FeaturedMeasure="6"/>
    </Grid>
</Window>
```

## Default Bullet Graph Appearance

When you create a bullet graph with default settings, it displays:
- A horizontal quantitative scale
- Default tick marks and labels
- Standard sizing and colors
- No featured or comparative measures (until you set them)
- No qualitative ranges (until you add them)

To customize the appearance, refer to the other reference documents for specific features.

## Next Steps

- **Configure the quantitative scale** → Read `quantitative-scale.md`
- **Set featured and comparative measures** → Read `measures.md`
- **Add qualitative ranges** → Read `ranges.md`
- **Add captions** → Read `caption.md`
- **Enable advanced features** → Read `advanced-features.md`

## Troubleshooting

**Issue:** Assembly not found error  
**Solution:** Ensure you've installed the correct NuGet package or added the assembly reference. Verify the Syncfusion version matches your project's target framework.

**Issue:** Namespace not recognized in XAML  
**Solution:** Check that the namespace declaration is correct and the assembly reference is properly added to the project.

**Issue:** Bullet graph not visible  
**Solution:** Ensure the parent container has a defined size. The bullet graph needs space to render. Set explicit Height/Width or use a layout container that provides available space.
