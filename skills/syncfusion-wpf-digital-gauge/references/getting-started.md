# Getting Started with Digital Gauge

This guide covers the installation, setup, and basic usage of the SfDigitalGauge control in WPF applications.

## Installation Methods

There are three methods to add SfDigitalGauge to your WPF project. Choose the method that best fits your workflow.

### Method 1: NuGet Package Manager (Recommended)

The easiest way to add SfDigitalGauge is through NuGet:

1. Open your WPF project in Visual Studio
2. Right-click on the project → **Manage NuGet Packages**
3. Search for `Syncfusion.SfGauge.WPF`
4. Click **Install**

**Package:** [Syncfusion.SfGauge.WPF](https://www.nuget.org/packages/Syncfusion.SfGauge.WPF)

**Benefits:**
- Automatic dependency management
- Easy updates
- No manual file handling

### Method 2: Toolbox Drag-and-Drop

For visual designers who prefer working with the designer:

1. Open the **Toolbox** in Visual Studio
2. Locate **SfDigitalGauge** in the Syncfusion WPF controls section
3. Drag and drop onto your XAML designer

**What happens automatically:**
- Assembly references are added
- Namespace declarations are added to XAML
- Control is placed in the designer

### Method 3: Manual Assembly Reference

For advanced scenarios or custom build configurations:

1. Navigate to the Syncfusion installation directory:
   ```
   {Installed location}/{version}/WPF/Assemblies
   ```

2. Add reference to:
   - `Syncfusion.SfGauge.Wpf.dll`

3. Check [Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies#sfgauge) for additional required assemblies

**When to use:** Custom build processes, specific version requirements, offline environments.

---

## Namespace Setup

### XAML Namespace

Add the gauge namespace to your Window or UserControl:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf">
    
    <!-- Your content here -->
    
</Window>
```

### C# Namespace

Add the using directive to your code-behind:

```csharp
using Syncfusion.UI.Xaml.Gauges;
```

---

## Initializing SfDigitalGauge

### XAML Declaration

The simplest way to create a digital gauge:

```xaml
<gauge:SfDigitalGauge />
```

This creates a default digital gauge with no value displayed.

### Code-Behind Creation

Creating the control programmatically:

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
this.Content = digitalGauge;
```

For adding to existing containers:

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
myGrid.Children.Add(digitalGauge);
```

Or with specific positioning:

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
Grid.SetRow(digitalGauge, 1);
Grid.SetColumn(digitalGauge, 2);
myGrid.Children.Add(digitalGauge);
```

---

## Displaying Values

The `Value` property accepts any string to display on the digital gauge.

### Basic Value Display

```xaml
<gauge:SfDigitalGauge Value="1 2 3 4" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "1 2 3 4";
```

**Note:** Spaces in the value are preserved and displayed as gaps between characters.

### Common Value Patterns

**Numeric values:**
```csharp
digitalGauge.Value = "12345";
```

**Time display:**
```csharp
digitalGauge.Value = "11:59:50";
```

**Text with numbers:**
```csharp
digitalGauge.Value = "SPEED 088";
```

**Mixed content:**
```csharp
digitalGauge.Value = "TEMP 72°F";
```

---

## Setting Character Type

The `CharacterType` property determines how characters are rendered using different segment configurations.

### Available Character Types

```csharp
public enum CharacterType
{
    SegmentSeven,              // 7-segment (numbers only)
    SegmentFourteen,           // 14-segment (numbers + letters)
    SegmentSixteen,            // 16-segment (numbers + letters)
    EightCrossEightDotMatrix   // 8×8 dot matrix (full character set)
}
```

### Example: 8×8 Dot Matrix Display

```xaml
<gauge:SfDigitalGauge Value="1 2 3 4" 
                      CharacterType="EightCrossEightDotMatrix" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Value = "1 2 3 4";
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
```

**See [character-types.md](character-types.md) for detailed comparison of all character types.**

---

## Basic Configuration

Configure common properties to customize the appearance:

```xaml
<gauge:SfDigitalGauge Value="11:59:50 PM"
                      Height="100"
                      Width="375"
                      CharacterHeight="60"
                      CharacterWidth="25"
                      CharacterType="EightCrossEightDotMatrix"
                      CharacterStroke="#146CED"
                      DimmedBrush="#F2F2F2"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center" />
```

```csharp
SfDigitalGauge digitalGauge = new SfDigitalGauge();
digitalGauge.Height = 100;
digitalGauge.Width = 375;
digitalGauge.Value = "11:59:50 PM";
digitalGauge.CharacterHeight = 60;
digitalGauge.CharacterWidth = 25;
digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#F2F2F2");
digitalGauge.HorizontalAlignment = HorizontalAlignment.Center;
digitalGauge.VerticalAlignment = VerticalAlignment.Center;
```

**Result:** A styled digital clock display with blue segments and light gray dimmed background.

---

## Complete Getting Started Example

### XAML

```xaml
<Window x:Class="DigitalGaugeDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:gauge="clr-namespace:Syncfusion.UI.Xaml.Gauges;assembly=Syncfusion.SfGauge.Wpf"
        Title="Digital Gauge Demo" Height="200" Width="400">
    <Grid>
        <gauge:SfDigitalGauge Value="11:59:50 PM"
                              Height="100"
                              Width="375"
                              CharacterHeight="60"
                              CharacterWidth="25"
                              CharacterType="EightCrossEightDotMatrix"
                              CharacterStroke="#146CED"
                              DimmedBrush="#F2F2F2"
                              HorizontalAlignment="Center"
                              VerticalAlignment="Center" />
    </Grid>
</Window>
```

### Code-Behind

```csharp
using System.Windows;
using Syncfusion.UI.Xaml.Gauges;
using System.Windows.Media;

namespace DigitalGaugeDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Alternative: Create in code
            // CreateDigitalGaugeProgrammatically();
        }
        
        private void CreateDigitalGaugeProgrammatically()
        {
            SfDigitalGauge digitalGauge = new SfDigitalGauge();
            digitalGauge.Height = 100;
            digitalGauge.Width = 375;
            digitalGauge.Value = "11:59:50 PM";
            digitalGauge.CharacterHeight = 60;
            digitalGauge.CharacterWidth = 25;
            digitalGauge.CharacterType = CharacterType.EightCrossEightDotMatrix;
            digitalGauge.CharacterStroke = (SolidColorBrush)new BrushConverter().ConvertFrom("#146CED");
            digitalGauge.DimmedBrush = (SolidColorBrush)new BrushConverter().ConvertFrom("#F2F2F2");
            digitalGauge.HorizontalAlignment = HorizontalAlignment.Center;
            digitalGauge.VerticalAlignment = VerticalAlignment.Center;
            
            this.Content = digitalGauge;
        }
    }
}
```

---

## Theme Support

SfDigitalGauge supports Syncfusion's built-in themes for consistent styling across your application.

### Apply Theme Using SfSkinManager

```csharp
using Syncfusion.SfSkinManager;

// Set theme for the window
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

---

## Next Steps

Now that you have SfDigitalGauge set up and running:

1. **Explore character types** → [character-types.md](character-types.md)
2. **Customize appearance** → [customization.md](customization.md)
3. **Apply transformations** → [transformations.md](transformations.md)
4. **Advanced styling** → [segment-styling.md](segment-styling.md)

---

## Resources

- [Complete Sample Code](https://github.com/SyncfusionExamples/WPF-UG-getting-started-samples/tree/master/GettingStartedDigitalGauge)
- [Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies#sfgauge)
- [Official Documentation](https://help.syncfusion.com/wpf/digital-gauge/getting-started)
