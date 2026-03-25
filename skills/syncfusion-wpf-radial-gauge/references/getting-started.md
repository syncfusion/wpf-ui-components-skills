# Getting Started with Circular Gauges

This guide walks you through setting up and creating your first Syncfusion WPF Radial Gauge (SfCircularGauge) component.

## Installation

### Method 1: NuGet Package Manager (Recommended)

Install the Syncfusion.SfGauge.WPF package from NuGet:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.SfGauge.WPF
```

**Visual Studio NuGet UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.SfGauge.WPF"
3. Click Install

**Package Name:** `Syncfusion.SfGauge.WPF`

### Method 2: Toolbox

Drag the SfCircularGauge control from the toolbox to your designer. This automatically adds required references and namespaces.

### Method 3: Manual Assembly Reference

Add the following assemblies from the Syncfusion installation directory:

**Location:** `{Installed location}/{version}/WPF/Assemblies`

**Required Assemblies:**
- `Syncfusion.SfGauge.Wpf.dll`
- `Syncfusion.Shared.Wpf.dll` (dependency)

## Namespace Imports

### XAML

Add the namespace declaration to your Window or UserControl:

```xml
<Window xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    
    <!-- Your gauge here -->
    
</Window>
```

**Namespace:** `clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf`

### C# Code-Behind

Add using directive:

```csharp
using Syncfusion.UI.Xaml.Gauges;
```

## License Configuration

Syncfusion components require a valid license key. Register it in your application startup:

**App.xaml.cs:**
```csharp
using Syncfusion.Licensing;

public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Register license key
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        base.OnStartup(e);
    }
}
```

**Get Your License:**
- **Community License:** Free for qualifying individuals and businesses
- **Trial License:** 30-day evaluation
- **Commercial License:** For production use

Visit: https://www.syncfusion.com/sales/products

## Creating Your First Circular Gauge

### Step 1: Initialize Empty Gauge

**XAML:**
```xml
<gauge:SfCircularGauge />
```

**C#:**
```csharp
SfCircularGauge gauge = new SfCircularGauge();
this.Content = gauge;
```

This creates an empty gauge container. You'll add scales and pointers next.

### Step 2: Add a Scale

A scale defines the value range and appearance of the gauge.

**XAML:**
```xml
<gauge:SfCircularGauge>
    <gauge:SfCircularGauge.Scales>
        <gauge:CircularScale StartValue="0" 
                             EndValue="100" 
                             Interval="10"/>
    </gauge:SfCircularGauge.Scales>
</gauge:SfCircularGauge>
```

**C#:**
```csharp
SfCircularGauge gauge = new SfCircularGauge();

CircularScale scale = new CircularScale
{
    StartValue = 0,
    EndValue = 100,
    Interval = 10
};

gauge.Scales.Add(scale);
this.Content = gauge;
```

**Key Properties:**
- `StartValue` - Minimum value (default: 0)
- `EndValue` - Maximum value (default: 100)
- `Interval` - Label interval (default: Auto)

### Step 3: Add a Pointer

Pointers indicate the current value on the scale.

**XAML:**
```xml
<gauge:CircularScale StartValue="0" EndValue="100">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer PointerType="NeedlePointer" 
                               Value="60"/>
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

**C#:**
```csharp
CircularPointer pointer = new CircularPointer
{
    PointerType = PointerType.NeedlePointer,
    Value = 60
};
scale.Pointers.Add(pointer);
```

### Step 4: Add a Header (Optional)

**XAML:**
```xml
<gauge:SfCircularGauge HeaderAlignment="Custom"
                       GaugeHeaderPosition="0.63,0.75">
    <gauge:SfCircularGauge.GaugeHeader>
        <TextBlock Text="Temperature"
                   FontSize="14"
                   FontWeight="SemiBold"/>
    </gauge:SfCircularGauge.GaugeHeader>
    
    <!-- Scales here -->
</gauge:SfCircularGauge>
```

**C#:**
```csharp
TextBlock header = new TextBlock
{
    Text = "Temperature",
    FontSize = 14,
    FontWeight = FontWeights.SemiBold
};
gauge.GaugeHeader = header;
gauge.HeaderAlignment = HeaderAlignment.Custom;
gauge.GaugeHeaderPosition = new Point(0.63, 0.75);
```

## Complete Minimal Example

Here's a complete working gauge with all basic elements:

### XAML

```xml
<Window x:Class="GaugeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf"
        Title="My First Gauge" Height="400" Width="400">
    
    <Grid>
        <gauge:SfCircularGauge Height="300" 
                               Width="300"
                               HeaderAlignment="Custom"
                               GaugeHeaderPosition="0.63,0.75">
            
            <!-- Header -->
            <gauge:SfCircularGauge.GaugeHeader>
                <TextBlock Text="Temperature (°C)"
                           FontSize="14"
                           FontWeight="SemiBold"
                           Foreground="#333"/>
            </gauge:SfCircularGauge.GaugeHeader>
            
            <!-- Scale -->
            <gauge:SfCircularGauge.Scales>
                <gauge:CircularScale StartValue="0" 
                                     EndValue="100" 
                                     Interval="10"
                                     ShowRim="True"
                                     RimStroke="LightGray"
                                     RimStrokeThickness="3">
                    
                    <!-- Pointer -->
                    <gauge:CircularScale.Pointers>
                        <gauge:CircularPointer PointerType="NeedlePointer"
                                               Value="72"
                                               NeedlePointerStroke="#666"
                                               NeedlePointerStrokeThickness="5"
                                               KnobFill="#666"/>
                    </gauge:CircularScale.Pointers>
                    
                </gauge:CircularScale>
            </gauge:SfCircularGauge.Scales>
        </gauge:SfCircularGauge>
    </Grid>
    
</Window>
```

### C# Code-Behind

```csharp
using System.Windows;
using System.Windows.Media;
using Syncfusion.UI.Xaml.Gauges;

namespace GaugeApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Alternative: Create gauge programmatically
            // CreateGaugeProgrammatically();
        }
        
        private void CreateGaugeProgrammatically()
        {
            // Create gauge
            SfCircularGauge gauge = new SfCircularGauge
            {
                Height = 300,
                Width = 300,
                HeaderAlignment = HeaderAlignment.Custom,
                GaugeHeaderPosition = new Point(0.63, 0.75)
            };
            
            // Create header
            TextBlock header = new TextBlock
            {
                Text = "Temperature (°C)",
                FontSize = 14,
                FontWeight = FontWeights.SemiBold,
                Foreground = new SolidColorBrush(Color.FromRgb(51, 51, 51))
            };
            gauge.GaugeHeader = header;
            
            // Create scale
            CircularScale scale = new CircularScale
            {
                StartValue = 0,
                EndValue = 100,
                Interval = 10,
                ShowRim = true,
                RimStroke = new SolidColorBrush(Colors.LightGray),
                RimStrokeThickness = 3
            };
            
            // Create pointer
            CircularPointer pointer = new CircularPointer
            {
                PointerType = PointerType.NeedlePointer,
                Value = 72,
                NeedlePointerStroke = new SolidColorBrush(Color.FromRgb(102, 102, 102)),
                NeedlePointerStrokeThickness = 5,
                KnobFill = new SolidColorBrush(Color.FromRgb(102, 102, 102))
            };
            
            scale.Pointers.Add(pointer);
            gauge.Scales.Add(scale);
            
            // Set as window content
            this.Content = gauge;
        }
    }
}
```

### App.xaml.cs (License Registration)

```csharp
using System.Windows;
using Syncfusion.Licensing;

namespace GaugeApp
{
    public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            // Register Syncfusion license
            SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");
            
            base.OnStartup(e);
        }
    }
}
```

## Basic Configuration Options

### Sizing the Gauge

**Fixed Size:**
```xml
<gauge:SfCircularGauge Height="400" Width="400"/>
```

**Responsive Size:**
```xml
<gauge:SfCircularGauge HorizontalAlignment="Stretch" 
                       VerticalAlignment="Stretch"/>
```

### Changing Scale Range

```xml
<gauge:CircularScale StartValue="-40" 
                     EndValue="140" 
                     Interval="20"/>
```

### Pointer Types

**Needle Pointer (Default):**
```xml
<gauge:CircularPointer PointerType="NeedlePointer" Value="50"/>
```

**Range Pointer (Arc):**
```xml
<gauge:CircularPointer PointerType="RangePointer" Value="75"/>
```

**Symbol Pointer (Marker):**
```xml
<gauge:CircularPointer PointerType="SymbolPointer" 
                       Symbol="InvertedTriangle" 
                       Value="60"/>
```

## Troubleshooting

### Issue: Gauge Not Displaying

**Possible Causes:**
1. Missing NuGet package
2. Incorrect namespace
3. Missing license key (trial expired)

**Solution:**
- Verify package installation
- Check namespace: `xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf"`
- Register valid license key

### Issue: Pointer Not Visible

**Possible Causes:**
1. Pointer value outside scale range
2. Pointer visibility set to Hidden
3. No pointers added to scale

**Solution:**
```xml
<gauge:CircularScale StartValue="0" EndValue="100">
    <gauge:CircularScale.Pointers>
        <gauge:CircularPointer Value="50"/> <!-- Value within range -->
    </gauge:CircularScale.Pointers>
</gauge:CircularScale>
```

### Issue: Labels Not Showing

**Possible Causes:**
1. Interval set too large
2. LabelStroke set to Transparent

**Solution:**
```xml
<gauge:CircularScale Interval="10" 
                     LabelStroke="Black"/>
```

## Next Steps

Now that you have a basic gauge running:

1. **Configure Scales:** Learn about angles, sweep direction, multiple scales
   → Read: [scales-and-configuration.md](scales-and-configuration.md)

2. **Customize Pointers:** Explore needle types, range pointers, symbols
   → Read: [pointers.md](pointers.md)

3. **Add Ranges:** Create visual zones for different value ranges
   → Read: [ranges.md](ranges.md)

4. **Style Your Gauge:** Customize labels, ticks, rim, and themes
   → Read: [labels-and-ticks.md](labels-and-ticks.md), [rim-and-appearance.md](rim-and-appearance.md)

## Sample Code Repository

Find complete getting started samples:
https://github.com/SyncfusionExamples/WPF-UG-getting-started-samples/tree/master/GettingStartedCircularGauge
