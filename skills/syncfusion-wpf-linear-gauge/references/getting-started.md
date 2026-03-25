# Getting Started with SfLinearGauge

A step-by-step guide to setting up and implementing your first Syncfusion WPF Linear Gauge control. This guide covers installation, basic configuration, and creating a complete working gauge with scale, pointers, and ranges.

## Installation and Setup

### Method 1: Adding Gauge Reference from NuGet

The recommended approach is to install the Syncfusion WPF Gauge package from NuGet:

1. **Open NuGet Package Manager** in Visual Studio
2. **Search for:** `Syncfusion.SfGauge.WPF`
3. **Install** the package

**Package Manager Console:**
```powershell
Install-Package Syncfusion.SfGauge.WPF
```

**NuGet Package Manager UI:**
- Tools → NuGet Package Manager → Manage NuGet Packages for Solution
- Browse tab → Search "Syncfusion.SfGauge.WPF"
- Select your project and click Install

The package automatically adds the required assembly references.

### Method 2: Adding Gauge Reference from Toolbox

If you have Syncfusion controls installed:

1. **Open** the Visual Studio Toolbox
2. **Locate** the SfLinearGauge control in the Syncfusion Controls section
3. **Drag and drop** it onto your XAML designer
4. The required assemblies and namespaces are added automatically

### Method 3: Adding Gauge Assemblies Manually

If you prefer manual assembly references:

**Location:** `{Installed location}/{version}/WPF/Assemblies`

**Required assemblies:**
- `Syncfusion.SfGauge.WPF.dll` - Core gauge controls
- `Syncfusion.Shared.WPF.dll` - Shared utilities (dependency)

**Add reference in Visual Studio:**
1. Right-click **References** in Solution Explorer
2. Select **Add Reference...**
3. Browse to the assemblies location
4. Select the required DLLs and click **Add**

**Assembly dependencies:**
For more details on dependencies, see [Syncfusion WPF Gauge Dependencies](https://help.syncfusion.com/wpf/control-dependencies#sfgauge).

## Namespace Imports

### XAML Namespace

Add the gauge namespace to your Window or UserControl:

```xml
<Window x:Class="YourApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf"
        Title="Linear Gauge Demo" Height="450" Width="800">
    <!-- Your content here -->
</Window>
```

**Namespace breakdown:**
- **xmlns:gauge** - Your chosen prefix (can be any name)
- **clr-namespace:** - The .NET namespace
- **Syncfusion.UI.Xaml.Gauges** - The gauge namespace
- **assembly=Syncfusion.SfGauge.Wpf** - The assembly name

### C# Namespace

Add the using directive in your code-behind or view model:

```csharp
using Syncfusion.UI.Xaml.Gauges;
using System.Windows.Media;
```

## Initialize Empty Linear Gauge

Start with the simplest possible gauge:

### XAML

```xml
<gauge:SfLinearGauge />
```

This creates an empty linear gauge with no scale. While valid, it won't display anything meaningful.

### C#

```csharp
SfLinearGauge linearGauge = new SfLinearGauge();
this.Content = linearGauge;
```

## Configuring the Scale

The scale is the foundation of the gauge. It defines the value range, appearance, and layout.

### Basic Scale Setup

Add a **MainScale** with essential properties:

```xml
<gauge:SfLinearGauge>
    <gauge:SfLinearGauge.MainScale>
        <gauge:LinearScale 
            ScaleBarStroke="#E0E0E0"
            LabelStroke="#424242"
            MajorTickStroke="Gray"
            MajorTickSize="15"
            MajorTickStrokeThickness="1"
            MinorTickStroke="Gray"
            MinorTickSize="7"
            MinorTickStrokeThickness="1"
            MinorTicksPerInterval="3"
            ScaleBarLength="300"
            ScaleBarSize="10">
        </gauge:LinearScale>
    </gauge:SfLinearGauge.MainScale>
</gauge:SfLinearGauge>
```

**C# equivalent:**

```csharp
SfLinearGauge linearGauge = new SfLinearGauge();

LinearScale scale = new LinearScale
{
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
    MajorTickStroke = new SolidColorBrush(Colors.Gray),
    MajorTickSize = 15,
    MajorTickStrokeThickness = 1,
    MinorTickStroke = new SolidColorBrush(Colors.Gray),
    MinorTickSize = 7,
    MinorTickStrokeThickness = 1,
    MinorTicksPerInterval = 3,
    ScaleBarLength = 300,
    ScaleBarSize = 10
};

linearGauge.MainScale = scale;
this.Content = linearGauge;
```

**Key properties explained:**
- **ScaleBarStroke** - Background color of the scale bar
- **ScaleBarSize** - Thickness of the scale bar (height in horizontal orientation)
- **ScaleBarLength** - Length of the scale bar (width in horizontal orientation)
- **LabelStroke** - Color of numeric labels
- **MajorTickStroke/MinorTickStroke** - Tick mark colors
- **MajorTickSize/MinorTickSize** - Tick mark lengths
- **MinorTicksPerInterval** - Number of minor ticks between major ticks (default interval)

### Setting Scale Range

Define minimum and maximum values:

```xml
<gauge:LinearScale 
    Minimum="0" 
    Maximum="100"
    Interval="10"
    ScaleBarStroke="#E0E0E0"
    ScaleBarSize="10" />
```

**C#:**

```csharp
LinearScale scale = new LinearScale
{
    Minimum = 0,
    Maximum = 100,
    Interval = 10,
    ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
    ScaleBarSize = 10
};
```

**Default values:**
- Minimum: 0
- Maximum: 100
- Interval: Auto-calculated (approximately 3 labels per 100 pixels)

## Adding a Symbol Pointer

Symbol pointers are shapes that mark specific values on the scale.

### Basic Symbol Pointer

```xml
<gauge:LinearScale.Pointers>
    <gauge:LinearPointer 
        PointerType="SymbolPointer"
        Value="60"
        SymbolPointerHeight="10"
        SymbolPointerWidth="10"
        SymbolPointerPosition="Below"
        SymbolPointerStroke="#757575" />
</gauge:LinearScale.Pointers>
```

**C#:**

```csharp
LinearPointer symbolPointer = new LinearPointer
{
    PointerType = LinearPointerType.SymbolPointer,
    Value = 60,
    SymbolPointerHeight = 10,
    SymbolPointerWidth = 10,
    SymbolPointerPosition = LinearSymbolPointersPosition.Below,
    SymbolPointerStroke = new SolidColorBrush(Color.FromRgb(117, 117, 117))
};

scale.Pointers.Add(symbolPointer);
```

**Symbol pointer properties:**
- **Value** - The value on the scale where the pointer appears
- **SymbolPointerHeight/Width** - Size of the pointer symbol
- **SymbolPointerPosition** - Above, Below (default), or Cross
- **SymbolPointerStroke** - Color of the pointer

## Adding a Bar Pointer

Bar pointers fill the scale from the start to the pointer value, ideal for progress indication.

### Basic Bar Pointer

```xml
<gauge:LinearScale.Pointers>
    <gauge:LinearPointer 
        PointerType="BarPointer"
        Value="50"
        BarPointerStroke="#757575"
        BarPointerStrokeThickness="10" />
</gauge:LinearScale.Pointers>
```

**C#:**

```csharp
LinearPointer barPointer = new LinearPointer
{
    PointerType = LinearPointerType.BarPointer,
    Value = 50,
    BarPointerStroke = new SolidColorBrush(Color.FromRgb(117, 117, 117)),
    BarPointerStrokeThickness = 10
};

scale.Pointers.Add(barPointer);
```

**Bar pointer properties:**
- **Value** - Where the bar ends on the scale
- **BarPointerStroke** - Color of the bar
- **BarPointerStrokeThickness** - Width of the bar

## Adding Ranges

Ranges highlight specific sections of the scale with colors, useful for thresholds.

### Basic Range

```xml
<gauge:LinearScale.Ranges>
    <gauge:LinearRange 
        StartValue="0"
        EndValue="40"
        RangeStroke="#27BEB7"
        StartWidth="10"
        EndWidth="10"
        RangeOffset="0.4" />
    
    <gauge:LinearRange 
        StartValue="40"
        EndValue="100"
        RangeStroke="LightCyan"
        StartWidth="10"
        EndWidth="10"
        RangeOffset="0.4" />
</gauge:LinearScale.Ranges>
```

**C#:**

```csharp
// First range (0-40)
LinearRange range1 = new LinearRange
{
    StartValue = 0,
    EndValue = 40,
    RangeStroke = new SolidColorBrush(Color.FromRgb(39, 190, 183)),
    StartWidth = 10,
    EndWidth = 10,
    RangeOffset = 0.4
};
scale.Ranges.Add(range1);

// Second range (40-100)
LinearRange range2 = new LinearRange
{
    StartValue = 40,
    EndValue = 100,
    RangeStroke = new SolidColorBrush(Colors.LightCyan),
    StartWidth = 10,
    EndWidth = 10,
    RangeOffset = 0.4
};
scale.Ranges.Add(range2);
```

**Range properties:**
- **StartValue/EndValue** - Range boundaries on the scale
- **RangeStroke** - Color of the range
- **StartWidth/EndWidth** - Range thickness (can differ for tapered effect)
- **RangeOffset** - Distance from scale bar (multiplier)

## Complete Example

Here's a fully functional linear gauge combining all elements:

### XAML

```xml
<Window x:Class="LinearGaugeDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf"
        Title="Linear Gauge Demo" Height="300" Width="600">
    <Grid>
        <gauge:SfLinearGauge>
            <gauge:SfLinearGauge.MainScale>
                <gauge:LinearScale 
                    LabelStroke="#424242"
                    MajorTickStroke="Gray"
                    MajorTickSize="15"
                    MajorTickStrokeThickness="1"
                    MinorTickStroke="Gray"
                    MinorTickSize="7"
                    MinorTickStrokeThickness="1"
                    MinorTicksPerInterval="3"
                    ScaleBarStroke="#E0E0E0"
                    ScaleBarLength="300"
                    ScaleBarSize="10">

                    <!-- Pointers -->
                    <gauge:LinearScale.Pointers>
                        <!-- Symbol pointer at 60 -->
                        <gauge:LinearPointer 
                            PointerType="SymbolPointer"
                            Value="60"
                            SymbolPointerHeight="10"
                            SymbolPointerWidth="10"
                            SymbolPointerPosition="Below"
                            SymbolPointerStroke="#757575" />
                        
                        <!-- Bar pointer at 50 -->
                        <gauge:LinearPointer 
                            PointerType="BarPointer"
                            Value="50"
                            BarPointerStroke="#757575"
                            BarPointerStrokeThickness="10" />
                    </gauge:LinearScale.Pointers>

                    <!-- Ranges -->
                    <gauge:LinearScale.Ranges>
                        <!-- Green range (0-40) -->
                        <gauge:LinearRange 
                            StartValue="0"
                            EndValue="40"
                            RangeStroke="#27BEB7"
                            StartWidth="10"
                            EndWidth="10"
                            RangeOffset="0.4" />
                        
                        <!-- Cyan range (40-100) -->
                        <gauge:LinearRange 
                            StartValue="40"
                            EndValue="100"
                            RangeStroke="LightCyan"
                            RangeOffset="0.4"
                            StartWidth="10"
                            EndWidth="10" />
                    </gauge:LinearScale.Ranges>
                </gauge:LinearScale>
            </gauge:SfLinearGauge.MainScale>
        </gauge:SfLinearGauge>
    </Grid>
</Window>
```

### C# Complete Example

```csharp
using Syncfusion.UI.Xaml.Gauges;
using System.Windows;
using System.Windows.Media;

namespace LinearGaugeDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            CreateLinearGauge();
        }

        private void CreateLinearGauge()
        {
            // Create the gauge
            SfLinearGauge linearGauge = new SfLinearGauge();

            // Create and configure the scale
            LinearScale scale = new LinearScale
            {
                LabelStroke = new SolidColorBrush(Color.FromRgb(66, 66, 66)),
                MajorTickStroke = new SolidColorBrush(Colors.Gray),
                MajorTickSize = 15,
                MajorTickStrokeThickness = 1,
                MinorTickStroke = new SolidColorBrush(Colors.Gray),
                MinorTickSize = 7,
                MinorTickStrokeThickness = 1,
                MinorTicksPerInterval = 3,
                ScaleBarStroke = new SolidColorBrush(Color.FromRgb(224, 224, 224)),
                ScaleBarLength = 300,
                ScaleBarSize = 10
            };

            // Add symbol pointer
            LinearPointer symbolPointer = new LinearPointer
            {
                PointerType = LinearPointerType.SymbolPointer,
                Value = 60,
                SymbolPointerHeight = 10,
                SymbolPointerWidth = 10,
                SymbolPointerPosition = LinearSymbolPointersPosition.Below,
                SymbolPointerStroke = new SolidColorBrush(Color.FromRgb(117, 117, 117))
            };
            scale.Pointers.Add(symbolPointer);

            // Add bar pointer
            LinearPointer barPointer = new LinearPointer
            {
                PointerType = LinearPointerType.BarPointer,
                Value = 50,
                BarPointerStroke = new SolidColorBrush(Color.FromRgb(117, 117, 117)),
                BarPointerStrokeThickness = 10
            };
            scale.Pointers.Add(barPointer);

            // Add first range (0-40)
            LinearRange range1 = new LinearRange
            {
                StartValue = 0,
                EndValue = 40,
                RangeStroke = new SolidColorBrush(Color.FromRgb(39, 190, 183)),
                StartWidth = 10,
                EndWidth = 10,
                RangeOffset = 0.4
            };
            scale.Ranges.Add(range1);

            // Add second range (40-100)
            LinearRange range2 = new LinearRange
            {
                StartValue = 40,
                EndValue = 100,
                RangeStroke = new SolidColorBrush(Colors.LightCyan),
                RangeOffset = 0.4,
                StartWidth = 10,
                EndWidth = 10
            };
            scale.Ranges.Add(range2);

            // Set the scale to the gauge
            linearGauge.MainScale = scale;
            
            // Add to window
            this.Content = linearGauge;
        }
    }
}
```

## Theme Support

Apply professional themes to your Linear Gauge using SfSkinManager.

### Using SfSkinManager

```csharp
using Syncfusion.SfSkinManager;

// In your window or application startup
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

### Applying Theme in XAML

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    
    <gauge:SfLinearGauge>
        <!-- Your gauge configuration -->
    </gauge:SfLinearGauge>
</Window>
```

## Sample Projects

Explore complete examples:
- [Getting Started Sample on GitHub](https://github.com/SyncfusionExamples/WPF-UG-getting-started-samples/tree/master/GettingStartedLinearGauge)

## Next Steps

Now that you have a basic Linear Gauge running, explore advanced features:

1. **[Scale Configuration](scale-configuration.md)** - Customize scale appearance, intervals, and positioning
2. **[Pointers](pointers.md)** - Learn about pointer types, animations, and multiple pointers
3. **[Ranges](ranges.md)** - Create color-coded zones and threshold indicators
4. **[Labels and Ticks](labels-and-ticks.md)** - Format labels and configure tick marks
5. **[Orientation](orientation.md)** - Switch between horizontal and vertical layouts

## Troubleshooting

### Gauge Not Appearing

**Issue:** The gauge control doesn't render.

**Solutions:**
1. Verify the NuGet package is installed correctly
2. Check that the namespace is imported properly
3. Ensure MainScale is set with at least basic properties
4. Set explicit Width and Height on the SfLinearGauge or parent container

### Assembly Load Errors

**Issue:** "Could not load file or assembly 'Syncfusion.SfGauge.WPF'"

**Solutions:**
1. Clean and rebuild your solution
2. Verify the assembly reference points to the correct version
3. Check that all dependent assemblies are present
4. Ensure the target framework matches (e.g., .NET Framework 4.5+)

### Scale Not Visible

**Issue:** The scale bar doesn't show any visual elements.

**Solutions:**
1. Set ScaleBarStroke to a visible color (not transparent)
2. Set ScaleBarSize to a reasonable value (e.g., 10)
3. Verify that ScaleBarLength is set or the container has adequate space
4. Check that Minimum and Maximum are properly configured

---

**You now have a working Linear Gauge!** Continue to the other reference files to unlock the full potential of this versatile control.
