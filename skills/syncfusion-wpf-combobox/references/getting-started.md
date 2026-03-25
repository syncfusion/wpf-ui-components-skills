# Getting Started with WPF ComboBoxAdv

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [Creating Application with ComboBoxAdv](#creating-application-with-comboboxadv)
- [Adding Control via Designer](#adding-control-via-designer)
- [Adding Control Manually in XAML](#adding-control-manually-in-xaml)
- [Adding Control Manually in C#](#adding-control-manually-in-c)
- [Adding Items](#adding-items)
- [Data Binding Basics](#data-binding-basics)
- [Item Templates](#item-templates)
- [Theme Support](#theme-support)
- [Troubleshooting](#troubleshooting)

## Overview

The `ComboBoxAdv` control provides a flexible dropdown component for WPF applications that allows users to type a value or choose options from a predefined list. This guide covers installation, basic setup, and initial configuration.

**Key Capabilities:**
- Multiple ways to add items (static, data binding)
- Designer and code-based configuration
- Template support for custom item display
- Built-in theme support
- MVVM-compatible architecture

## Assembly Deployment

### Required Assembly

To use `ComboBoxAdv`, add the following assembly reference:

- **Syncfusion.Shared.WPF**

### NuGet Package Installation

1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Shared.WPF"
4. Install the package

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.WPF
```

### Control Dependencies

The ComboBoxAdv control depends on:
- `Syncfusion.Shared.WPF` assembly
- .NET Framework 4.5 or higher (or .NET Core 3.1+)

For more details, see the [control dependencies documentation](https://help.syncfusion.com/wpf/control-dependencies#comboboxadv).

## Creating Application with ComboBoxAdv

### Project Setup Steps

1. **Create a new WPF project** in Visual Studio
2. **Install NuGet package** (see Assembly Deployment above)
3. **Add the control** via designer, XAML, or C#
4. **Configure properties** and bind data as needed

## Adding Control via Designer

The easiest way to add `ComboBoxAdv` to your application:

1. Open your XAML file in the Designer view
2. Locate **ComboBoxAdv** in the Toolbox (under Syncfusion Controls)
3. Drag and drop it onto your design surface
4. The required assemblies are added automatically

**Benefits:**
- Visual positioning and sizing
- Automatic assembly references
- Property configuration via Properties window

## Adding Control Manually in XAML

### Step 1: Add Assembly Reference

Add the `Syncfusion.Shared.WPF` assembly to your project references.

### Step 2: Import Syncfusion Namespace

Add the Syncfusion WPF schema to your XAML:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Or use the CLR namespace:

```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Tools.Controls;assembly=Syncfusion.Shared.WPF"
```

### Step 3: Declare ComboBoxAdv

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:ComboBoxAdv Height="30" Width="150"
                                HorizontalAlignment="Center"
                                VerticalAlignment="Center"/>
    </Grid>
</Window>
```

## Adding Control Manually in C#

### Step 1: Add Assembly Reference

Add the `Syncfusion.Shared.WPF` assembly to your project references.

### Step 2: Import Namespace

```csharp
using Syncfusion.Windows.Tools.Controls;
```

### Step 3: Create and Add Control

```csharp
using System.Windows;
using Syncfusion.Windows.Tools.Controls;

namespace ComboBoxExample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create ComboBoxAdv instance
            ComboBoxAdv comboBoxAdv = new ComboBoxAdv();
            comboBoxAdv.Height = 30;
            comboBoxAdv.Width = 150;
            comboBoxAdv.DefaultText = "Choose Items";
            
            // Add to window
            this.Content = comboBoxAdv;
        }
    }
}
```

## Adding Items

### Method 1: Using ComboBoxItemAdv (Static Items)

Add items directly in XAML:

```xml
<syncfusion:ComboBoxAdv Height="30" Width="200"
                        HorizontalAlignment="Center" 
                        VerticalAlignment="Center">
    <syncfusion:ComboBoxItemAdv Content="Denmark" />
    <syncfusion:ComboBoxItemAdv Content="New Zealand" />
    <syncfusion:ComboBoxItemAdv Content="Canada" />
    <syncfusion:ComboBoxItemAdv Content="Russia" />
    <syncfusion:ComboBoxItemAdv Content="Japan" />
</syncfusion:ComboBoxAdv>
```

Or in C#:

```csharp
ComboBoxAdv comboBoxAdv = new ComboBoxAdv() 
{ 
    Height = 30,
    Width = 200,
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center 
};

ComboBoxItemAdv item1 = new ComboBoxItemAdv() { Content = "Denmark" };
ComboBoxItemAdv item2 = new ComboBoxItemAdv() { Content = "New Zealand" };
ComboBoxItemAdv item3 = new ComboBoxItemAdv() { Content = "Canada" };
ComboBoxItemAdv item4 = new ComboBoxItemAdv() { Content = "Russia" };
ComboBoxItemAdv item5 = new ComboBoxItemAdv() { Content = "Japan" };

comboBoxAdv.Items.Add(item1);
comboBoxAdv.Items.Add(item2);
comboBoxAdv.Items.Add(item3);
comboBoxAdv.Items.Add(item4);
comboBoxAdv.Items.Add(item5);

this.Content = comboBoxAdv;
```

### Method 2: Data Binding (Recommended)

See [Data Binding Basics](#data-binding-basics) below.

## Data Binding Basics

For dynamic data, use the `ItemsSource` property with a data model and ViewModel.

### Creating a Data Model

```csharp
public class PopulationInfo
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Population { get; set; }
    public double Growth { get; set; }
}
```

### Creating a ViewModel

```csharp
using System.Collections.ObjectModel;

public class PopulationViewModel
{
    public ObservableCollection<PopulationInfo> PopulationDetails { get; set; }

    public PopulationViewModel()
    {
        PopulationDetails = new ObservableCollection<PopulationInfo>
        {
            new PopulationInfo { Continent = "Asia", Country = "Indonesia", Growth = 3, Population = 237641326 },
            new PopulationInfo { Continent = "Asia", Country = "Russia", Growth = 2, Population = 152518015 },
            new PopulationInfo { Continent = "North America", Country = "United States", Growth = 4, Population = 315645000 },
            new PopulationInfo { Continent = "North America", Country = "Canada", Growth = 1, Population = 35056064 },
            new PopulationInfo { Continent = "Europe", Country = "Germany", Growth = 1, Population = 81993000 },
            new PopulationInfo { Continent = "Europe", Country = "France", Growth = 1, Population = 65605000 }
        };
    }
}
```

### Binding to ComboBoxAdv

```xml
<Window xmlns:local="clr-namespace:YourNamespace">
    <Window.DataContext>
        <local:PopulationViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:ComboBoxAdv x:Name="comboBoxAdv" 
                                Height="30" Width="200"
                                ItemsSource="{Binding PopulationDetails}"/>
    </Grid>
</Window>
```

### Setting DisplayMemberPath

To display a specific property from your data object:

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding PopulationDetails}"
                        DisplayMemberPath="Country"
                        Height="30" Width="200"/>
```

**Result:** Shows country names in the dropdown.

## Item Templates

For custom item display beyond simple text, use `ItemTemplate`:

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding PopulationDetails}"
                        Height="30" Width="250">
    <syncfusion:ComboBoxAdv.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding Country}" FontWeight="Bold"/>
                <TextBlock Text=" - "/>
                <TextBlock Text="{Binding Continent}" Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:ComboBoxAdv.ItemTemplate>
</syncfusion:ComboBoxAdv>
```

**Benefits:**
- Complex layouts (multiple TextBlocks, images, etc.)
- Custom styling per item
- Rich visual representation

## Theme Support

ComboBoxAdv supports various built-in themes for consistent application styling.

### Using SfSkinManager

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:Theme ThemeName="MaterialLight"/>
    </syncfusionskin:SfSkinManager.Theme>
    
    <syncfusion:ComboBoxAdv Height="30" Width="200"/>
</Window>
```

### Available Themes

- MaterialLight / MaterialDark
- FluentLight / FluentDark
- Office2019Colorful / Office2019Black / Office2019White
- Office2016Colorful / Office2016White / Office2016DarkGray
- VisualStudio2015 / VisualStudio2013
- And more...

### Creating Custom Themes

Use Syncfusion's [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio) to create custom color schemes.

For more details, see [references/customization-blendability.md](customization-blendability.md).

## Troubleshooting

### Issue: Control Not Appearing

**Cause:** Missing assembly reference or namespace import

**Solution:**
1. Verify `Syncfusion.Shared.WPF` is referenced
2. Check namespace declaration: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`
3. Rebuild the project

### Issue: Items Not Displaying

**Cause:** Missing `DisplayMemberPath` or `ItemTemplate` for complex objects

**Solution:**
- Set `DisplayMemberPath` to the property you want to display
- Or define an `ItemTemplate` for custom rendering

### Issue: Data Binding Not Working

**Cause:** DataContext not set or property path incorrect

**Solution:**
1. Verify DataContext is set (Window.DataContext or Grid.DataContext)
2. Check property names match your model exactly (case-sensitive)
3. Use `ObservableCollection` for dynamic updates
4. Implement `INotifyPropertyChanged` in ViewModel for property changes

### Issue: Theme Not Applying

**Cause:** SfSkinManager not configured or theme assembly missing

**Solution:**
1. Add `Syncfusion.SfSkinManager.WPF` assembly reference
2. Apply theme to Window or control
3. Ensure theme assembly is included in your project

## See Also

- [Data Binding](data-binding.md) - Comprehensive data binding guide
- [Selection and MultiSelection](selection-multiselection.md) - Selection features
- [Customization and Blendability](customization-blendability.md) - Styling and themes
- [Syncfusion WPF ComboBoxAdv Documentation](https://help.syncfusion.com/wpf/combobox/getting-started)
