# Getting Started with WPF Calculator

## Table of Contents
- [Assembly Dependencies](#assembly-dependencies)
- [Creating a New Project](#creating-a-new-project)
- [Adding via Designer](#adding-via-designer)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C#](#adding-via-c)
- [Setting Display Text](#setting-display-text)
- [Setting Default Value](#setting-default-value)

## Assembly Dependencies

To use SfCalculator in your WPF application, you need to add the following assembly references to your project:

**Required Assemblies:**
- `Syncfusion.SfInput.WPF`
- `Syncfusion.Shared.WPF`

**Installation via NuGet:**
```
Install-Package Syncfusion.SfInput.WPF
```

These assemblies contain the calculator control and all required dependencies. You can add them manually to your project references or install via NuGet Package Manager.

## Creating a New Project

1. Open Visual Studio
2. Create a new **WPF Application** project
3. Right-click on the project → Select **Add Reference**
4. Browse to the Syncfusion assemblies location (usually in Program Files or NuGet packages)
5. Add `Syncfusion.SfInput.WPF` and `Syncfusion.Shared.WPF`

Visual Studio will automatically add the necessary dependencies when you use the control.

## Adding via Designer

The easiest way to add SfCalculator is using the Visual Studio Designer:

1. Open your **MainWindow.xaml** in design view
2. In the **Toolbox**, search for "SfCalculator"
3. Drag and drop it onto your Window
4. Adjust size and position as needed

The required assemblies will be automatically added to your project when you drag the control onto the designer.

**Result:** Your XAML will automatically include the necessary namespace and control declaration.

## Adding via XAML

To manually add SfCalculator in XAML:

1. In your XAML file, add the Syncfusion namespace at the top:
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

2. Declare the SfCalculator control in your Grid or parent container:
```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="SfCalculatorSample.MainWindow"
        Title="Calculator Sample" 
        Height="350" 
        Width="300">
    <Grid>
        <syncfusion:SfCalculator x:Name="sfCalculator" 
                                 HorizontalAlignment="Center" 
                                 VerticalAlignment="Center" 
                                 Width="200" 
                                 Height="300"/>
    </Grid>
</Window>
```

**Key Attributes:**
- `x:Name` - Identifier for code-behind access
- `Width/Height` - Size of the calculator
- `HorizontalAlignment/VerticalAlignment` - Position in parent container

## Adding via C#

To create and add SfCalculator programmatically:

1. Import the namespace:
```csharp
using Syncfusion.Windows.Controls.Input;
```

2. Create an instance and add to your window:
```csharp
namespace SfCalculatorSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create calculator instance
            SfCalculator sfCalculator = new SfCalculator();
            
            // Optional: Configure properties
            sfCalculator.Width = 200;
            sfCalculator.Height = 300;
            sfCalculator.HorizontalAlignment = HorizontalAlignment.Center;
            sfCalculator.VerticalAlignment = VerticalAlignment.Center;
            
            // Add to window
            this.Content = sfCalculator;
        }
    }
}
```

**Benefits of programmatic approach:**
- Full control over initialization
- Easy to create multiple instances
- Suitable for dynamic UI generation
- Better for complex applications

## Setting Display Text

The **DisplayText** property shows watermark or placeholder text in the calculator's value display box.

**XAML Approach:**
```xaml
<syncfusion:SfCalculator DisplayText="Enter calculation" />
```

**C# Approach:**
```csharp
SfCalculator calculator = new SfCalculator();
calculator.DisplayText = "Enter calculation";
```

**Use Cases:**
- Show hint text to users
- Display instructions
- Indicate field purpose
- Professional UI polish

## Setting Default Value

The **DefaultValue** property sets the initial value displayed when the calculator starts.

**XAML Approach:**
```xaml
<syncfusion:SfCalculator DefaultValue="0" />
```

**C# Approach:**
```csharp
SfCalculator calculator = new SfCalculator();
calculator.DefaultValue = 100;
```

**Important Notes:**
- Default value is shown in decimal format
- Useful for calculator pre-population
- Does not affect calculation logic
- Can be changed at runtime
- Read the Value property to get calculated results (this is read-only)

**Complete Initialization Example:**
```csharp
SfCalculator calculator = new SfCalculator()
{
    Width = 250,
    Height = 350,
    DefaultValue = 0,
    DisplayText = "Calculator is ready",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};

this.Content = calculator;
```
