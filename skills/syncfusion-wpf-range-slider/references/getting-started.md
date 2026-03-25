# Getting Started with WPF Range Slider

This guide covers installation, basic setup, and initial configuration of the Syncfusion WPF Range Slider (SfRangeSlider) control.

## Installation

### NuGet Package Installation

Install the required package via NuGet Package Manager:

```bash
# Using Package Manager Console
Install-Package Syncfusion.SfInput.WPF

# Using .NET CLI
dotnet add package Syncfusion.SfInput.WPF
```

### Required Assemblies

The SfRangeSlider control requires the following assemblies:
- **Syncfusion.SfInput.WPF.dll** - Main assembly containing SfRangeSlider
- **Syncfusion.SfShared.WPF.dll** - Shared dependency assembly

**Namespace:** `Syncfusion.Windows.Controls.Input`

## Adding SfRangeSlider to Your Application

### Method 1: Using XAML Designer

1. Open your WPF project in Visual Studio
2. Locate the SfRangeSlider control in the toolbox
3. Drag and drop it onto your XAML designer view
4. The required assembly references will be added automatically

Add the namespace declaration to your XAML file:

```xaml
<Window x:Class="YourNamespace.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf">
    
    <Grid>
        <editors:SfRangeSlider
            Width="500"
            HorizontalAlignment="Center"
            VerticalAlignment="Center"
            Minimum="0"
            Maximum="100"
            Value="50" />
    </Grid>
</Window>
```

### Method 2: Manual XAML Code

Add the control directly in XAML without using the designer:

```xaml
<Window x:Class="RangeSliderApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Range Slider Demo" Height="450" Width="800">
    
    <Grid>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <!-- Basic Range Slider -->
            <editors:SfRangeSlider
                Width="400"
                Margin="0,0,0,20"
                Minimum="0"
                Maximum="100"
                Value="50" />
            
            <!-- Range Slider with dual thumbs -->
            <editors:SfRangeSlider
                Width="400"
                Minimum="0"
                Maximum="100"
                RangeStart="30"
                RangeEnd="70"
                ShowRange="True" />
        </StackPanel>
    </Grid>
</Window>
```

### Method 3: Programmatic Creation (C#)

Create and configure the SfRangeSlider control in code-behind:

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            CreateRangeSlider();
        }

        private void CreateRangeSlider()
        {
            // Create a grid container
            Grid parentGrid = new Grid();

            // Create the range slider
            SfRangeSlider rangeSlider = new SfRangeSlider()
            {
                Width = 500,
                HorizontalAlignment = HorizontalAlignment.Center,
                VerticalAlignment = VerticalAlignment.Center,
                Minimum = 0,
                Maximum = 100,
                Value = 50
            };

            // Add to container
            parentGrid.Children.Add(rangeSlider);
            this.Content = parentGrid;
        }
    }
}
```

## Basic Configuration

### Setting Minimum and Maximum Values

The `Minimum` and `Maximum` properties define the value range for the slider:

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Minimum = 0,
    Maximum = 100
};
```

**Default values:**
- `Minimum` = 0
- `Maximum` = 100

### Setting Initial Value (Single Thumb Mode)

Use the `Value` property for single thumb selection:

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    Value="50" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Minimum = 0,
    Maximum = 100,
    Value = 50
};
```

### Setting Initial Range (Dual Thumb Mode)

Use `RangeStart` and `RangeEnd` properties with `ShowRange="True"`:

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="30"
    RangeEnd="70"
    ShowRange="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Minimum = 0,
    Maximum = 100,
    ShowRange = true,
    RangeStart = 30,
    RangeEnd = 70
};
```

## Applying Themes

### Using SfSkinManager

Apply built-in themes to the SfRangeSlider using SfSkinManager:

```xaml
<Window x:Class="RangeSliderApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SkinManager;assembly=Syncfusion.SkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    
    <Grid>
        <editors:SfRangeSlider Width="400" Minimum="0" Maximum="100" Value="50" />
    </Grid>
</Window>
```

**Available themes:**
- MaterialLight
- MaterialDark
- Office2019Colorful
- Office2019Black
- Office2019White
- FluentLight
- FluentDark

### Applying Theme in Code-Behind

```csharp
using Syncfusion.SkinManager;

public MainWindow()
{
    InitializeComponent();
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}
```

## Complete Example

Here's a complete working example with multiple configurations:

```xaml
<Window x:Class="RangeSliderApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Range Slider Getting Started" Height="500" Width="800">
    
    <Grid Margin="50">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>

        <!-- Example 1: Basic Single Value Slider -->
        <StackPanel Grid.Row="0" Margin="0,0,0,40">
            <TextBlock Text="Single Value Slider" FontWeight="Bold" Margin="0,0,0,10"/>
            <editors:SfRangeSlider
                Width="500"
                Minimum="0"
                Maximum="100"
                Value="50" />
        </StackPanel>

        <!-- Example 2: Range Selection Slider -->
        <StackPanel Grid.Row="1" Margin="0,0,0,40">
            <TextBlock Text="Range Selection Slider" FontWeight="Bold" Margin="0,0,0,10"/>
            <editors:SfRangeSlider
                Width="500"
                Minimum="0"
                Maximum="100"
                RangeStart="25"
                RangeEnd="75"
                ShowRange="True" />
        </StackPanel>

        <!-- Example 3: Slider with Custom Range -->
        <StackPanel Grid.Row="2" Margin="0,0,0,40">
            <TextBlock Text="Custom Range (1-1000)" FontWeight="Bold" Margin="0,0,0,10"/>
            <editors:SfRangeSlider
                Width="500"
                Minimum="1"
                Maximum="1000"
                Value="500" />
        </StackPanel>

        <!-- Example 4: Decimal Values -->
        <StackPanel Grid.Row="3">
            <TextBlock Text="Decimal Values (0-10)" FontWeight="Bold" Margin="0,0,0,10"/>
            <editors:SfRangeSlider
                Width="500"
                Minimum="0"
                Maximum="10"
                Value="5.5" />
        </StackPanel>
    </Grid>
</Window>
```

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace RangeSliderApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

## Common Issues and Solutions

### Issue: Control Not Visible

**Problem:** SfRangeSlider doesn't appear in the UI.

**Solutions:**
1. Verify assembly references are added
2. Check namespace declaration in XAML
3. Ensure Width or Height is set (control has no default size)
4. Verify the control is added to a visible container

### Issue: License Warning

**Problem:** License prompt appears when running the application.

**Solution:** Register your Syncfusion license key in App.xaml.cs:

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Register Syncfusion license
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        base.OnStartup(e);
    }
}
```

For development/testing, you can use the community license: [https://www.syncfusion.com/products/communitylicense](https://www.syncfusion.com/products/communitylicense)

### Issue: Value Not Changing

**Problem:** Slider thumb doesn't move or value doesn't update.

**Solutions:**
1. Verify `Minimum` < `Maximum`
2. Check if initial `Value` is within min/max range
3. Ensure control is not disabled

## Next Steps

Now that you have the basic setup working, explore these features:

- **Range Selection** - Read `range-selection.md` for dual thumb functionality
- **Ticks and Snapping** - Read `ticks-and-snapping.md` for tick marks and snap-to behavior
- **Labels** - Read `labels.md` for value labels and custom labels
- **Tooltips** - Read `tooltips.md` for tooltip customization
- **Customization** - Read `customization.md` for styling options
- **Events** - Read `events.md` for handling value changes

## Additional Resources

- [Official Documentation](https://help.syncfusion.com/wpf/range-slider/getting-started)
- [API Reference](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfRangeSlider.html)
- [NuGet Package](https://www.nuget.org/packages/Syncfusion.SfInput.WPF)
- [Community License](https://www.syncfusion.com/products/communitylicense)
