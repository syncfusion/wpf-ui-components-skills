# Getting Started with PercentTextBox

This guide covers the initial setup and basic implementation of the Syncfusion WPF PercentTextBox control.

## Assembly Deployment

To use the PercentTextBox control, you need to add the required assembly reference to your project.

### Required Assembly
- **Syncfusion.Shared.WPF** - Core assembly for PercentTextBox and related controls

### NuGet Package Installation

Install the NuGet package in your WPF application:

1. Right-click on your project in Solution Explorer
2. Select **Manage NuGet Packages**
3. Search for **Syncfusion.Shared.WPF**
4. Click **Install**

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.WPF
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Shared.WPF
```

### Control Dependencies

The PercentTextBox control is part of the Syncfusion Shared library and has minimal dependencies. When you add the control via designer or reference the assembly, the following will be included automatically:
- Syncfusion.Shared.WPF assembly
- Required Syncfusion theme resources (if using theme integration)

## Adding PercentTextBox via Designer

The easiest way to add PercentTextBox to your application is through the Visual Studio designer:

1. **Open the Toolbox** in Visual Studio
2. **Locate Syncfusion Controls** section (if not visible, right-click Toolbox → Choose Items → browse to Syncfusion.Shared.WPF.dll)
3. **Drag PercentTextBox** from the toolbox onto your XAML designer surface
4. The control will be added to your view, and the assembly reference will be added automatically

**Result:** The designer will generate the XAML with proper namespace declaration and control instantiation.

## Adding PercentTextBox via XAML

For more control over the implementation, add PercentTextBox directly in XAML:

### Step 1: Create WPF Project
Create a new WPF project in Visual Studio (.NET Framework or .NET Core/.NET 5+).

### Step 2: Add Assembly Reference
Add a reference to **Syncfusion.Shared.WPF** assembly:
- Right-click **References** → **Add Reference**
- Browse to Syncfusion assemblies location or use NuGet

### Step 3: Import Syncfusion Schema
Import the Syncfusion WPF schema in your XAML file:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="PercentTextBoxSample.MainWindow"
        Title="PercentTextBox Sample" Height="350" Width="525">
    <!-- Content here -->
</Window>
```

**Key namespace:** `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`

### Step 4: Declare PercentTextBox Control

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="PercentTextBoxSample.MainWindow"
        Title="PercentTextBox Sample" Height="350" Width="525">
    <Grid>
        <!-- Basic PercentTextBox -->
        <syncfusion:PercentTextBox x:Name="percentTextBox" 
                                   Width="150" 
                                   Height="30"
                                   VerticalAlignment="Center"
                                   HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

### Complete XAML Example

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="PercentTextBoxSample.MainWindow"
        Title="PercentTextBox Sample" Height="350" Width="525">
    <StackPanel Margin="20" VerticalAlignment="Center">
        <TextBlock Text="Enter Percentage:" Margin="0,0,0,5"/>
        <syncfusion:PercentTextBox x:Name="percentTextBox" 
                                   Width="150" 
                                   Height="30"
                                   PercentValue="50"
                                   HorizontalAlignment="Left"/>
    </StackPanel>
</Window>
```

## Adding PercentTextBox via C#

You can also create and configure PercentTextBox programmatically in code-behind:

### Step 1: Create WPF Application
Create a new WPF application project in Visual Studio.

### Step 2: Add Assembly Reference
Add a reference to **Syncfusion.Shared.WPF** assembly (via NuGet or direct reference).

### Step 3: Include Required Namespace
Add the Syncfusion namespace to your C# file:

```csharp
using Syncfusion.Windows.Shared;
```

### Step 4: Create Instance and Configure

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

namespace PercentTextBoxSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create an instance of PercentTextBox
            PercentTextBox percentTextBox = new PercentTextBox();
            
            // Set dimensions
            percentTextBox.Width = 150;
            percentTextBox.Height = 30;
            
            // Set initial value
            percentTextBox.PercentValue = 50;
            
            // Add to window (if window content is not set)
            this.Content = percentTextBox;
        }
    }
}
```

### Adding to Existing Layout

If you already have a layout (Grid, StackPanel, etc.), add the control to it:

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Shared;

namespace PercentTextBoxSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create PercentTextBox
            PercentTextBox percentTextBox = new PercentTextBox
            {
                Width = 150,
                Height = 30,
                PercentValue = 75.5,
                VerticalAlignment = VerticalAlignment.Center,
                HorizontalAlignment = HorizontalAlignment.Center
            };
            
            // Add to existing Grid (assuming Grid named "mainGrid")
            mainGrid.Children.Add(percentTextBox);
        }
    }
}
```

## Setting Basic Properties

### Width and Height

Set control dimensions using Width and Height properties:

**XAML:**
```xml
<syncfusion:PercentTextBox Width="200" Height="35"/>
```

**C#:**
```csharp
percentTextBox.Width = 200;
percentTextBox.Height = 35;
```

### PercentValue Property

The primary property for setting and getting the percentage value:

**XAML:**
```xml
<syncfusion:PercentTextBox PercentValue="85.5"/>
```

**C#:**
```csharp
percentTextBox.PercentValue = 85.5;
```

**Important:** Always use the `PercentValue` property, **NOT** the `Text` property from the base TextBox class.

❌ **Don't do this:**
```csharp
percentTextBox.Text = "50%"; // This won't work properly
```

✅ **Do this:**
```csharp
percentTextBox.PercentValue = 50; // Correct approach
```

## Basic Usage Examples

### Simple Percentage Display

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="25.5"/>
```

**Result:** Displays "25.5 %" (formatted according to current culture)

### With Min/Max Limits

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="50"
                           MinValue="0"
                           MaxValue="100"/>
```

**Result:** User input is restricted between 0 and 100

### Read-Only Display

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                           Width="150"
                           Height="30"
                           PercentValue="33.33"
                           IsReadOnly="True"/>
```

**Result:** Displays percentage but prevents user editing

## Common Gotchas

### 1. Using Text Property
**Problem:** Setting the `Text` property directly doesn't work as expected.

**Solution:** Always use `PercentValue` property for setting values.

### 2. Missing Assembly Reference
**Problem:** Control not recognized or namespace error.

**Solution:** Ensure Syncfusion.Shared.WPF assembly is referenced and namespace is imported.

### 3. Value Not Updating
**Problem:** PercentValue doesn't update when bound to ViewModel.

**Solution:** Use `UpdateSourceTrigger=PropertyChanged` in binding:
```xml
<syncfusion:PercentTextBox PercentValue="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}"/>
```

### 4. Null Value Display
**Problem:** Control shows "0" instead of empty/null when value is null.

**Solution:** Enable `UseNullOption` property:
```xml
<syncfusion:PercentTextBox UseNullOption="True" NullValue="{x:Null}"/>
```

## Next Steps

After basic setup, explore these topics:

- **Value Management** - Learn about data binding, null handling, and value change events
- **Validation** - Configure min/max restrictions and validation timing
- **Formatting** - Apply culture-specific formats and custom separators
- **Appearance** - Customize colors, backgrounds, and visual styling
- **Interactive Features** - Enable scroll intervals, mouse wheel, and range adorner
