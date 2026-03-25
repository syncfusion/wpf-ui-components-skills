# Getting Started with WPF Carousel

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [Adding Carousel via Designer](#adding-carousel-via-designer)
- [Adding Carousel via XAML](#adding-carousel-via-xaml)
- [Adding Carousel via C#](#adding-carousel-via-c)
- [Basic Configuration](#basic-configuration)
- [First Complete Example](#first-complete-example)
- [Common Setup Issues](#common-setup-issues)

## Overview

The Syncfusion WPF Carousel control provides a 3D interface for displaying objects in a circular rotating pattern. It supports data binding, custom paths, item templates, scaling, skewing, and interactive navigation features.

**Key Capabilities:**
- 3D circular item display with rotation
- Data binding support for dynamic content
- Custom geometric paths for item positioning
- Scaling and opacity effects
- Keyboard, mouse, and command-based navigation
- Item template customization
- Virtualization for large datasets

## Assembly Deployment

### Required Assemblies

To use the Carousel control, you need to add the following assembly reference:

**Assembly:** `Syncfusion.Shared.WPF.dll`

This assembly contains the Carousel control and its dependencies.

### Control Dependencies

Refer to the [Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies#carousel) documentation for a complete list of assemblies required for the Carousel control.

### NuGet Package Installation

The recommended way to add Syncfusion controls is through NuGet packages.

**Package Name:** `Syncfusion.Shared.WPF`

**Installation Steps:**

1. Open your WPF project in Visual Studio
2. Right-click on the project → **Manage NuGet Packages**
3. Search for `Syncfusion.Shared.WPF`
4. Install the latest version
5. Accept the license agreement

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Shared.WPF
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Shared.WPF
```

For more details about NuGet packages, see [Installing NuGet Packages in WPF](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages).

## Adding Carousel via Designer

You can add the Carousel control to your WPF application using the Visual Studio designer.

**Steps:**

1. Open your WPF window or user control in the designer
2. Open the **Toolbox** (View → Toolbox or Ctrl+Alt+X)
3. Locate **Syncfusion Controls for WPF** section
4. Find the **Carousel** control
5. Drag and drop the Carousel control onto your design surface

**What happens automatically:**
- The required assembly reference (`Syncfusion.Shared.WPF`) is added to your project
- The Syncfusion namespace is added to your XAML
- A Carousel control instance is created with default settings

**Using SmartTag:**

After adding the control, you can use the SmartTag feature (small arrow icon) to quickly configure common properties in design mode.

## Adding Carousel via XAML

To add the Carousel control manually in XAML, follow these steps:

### Step 1: Create a New WPF Project

Create a new WPF project in Visual Studio or ensure you have an existing WPF project.

### Step 2: Add Assembly Reference

Add the required assembly reference:
- **Syncfusion.Shared.WPF**

**In Solution Explorer:**
1. Right-click **References** → **Add Reference**
2. Browse to Syncfusion installation folder
3. Select `Syncfusion.Shared.WPF.dll`
4. Click **OK**

Or use NuGet (recommended).

### Step 3: Import Syncfusion Namespace

In your XAML file, import the Syncfusion WPF schema:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 4: Declare the Carousel Control

```xml
<Window x:Class="CarouselSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    
    <Grid Name="grid">
        <syncfusion:Carousel Name="carousel"
                             Height="300"
                             Width="500"/>
    </Grid>
</Window>
```

This creates a basic Carousel control with default settings.

## Adding Carousel via C#

To add the Carousel control programmatically in code-behind:

### Step 1: Create a New WPF Project

Create or open a WPF application project.

### Step 2: Add Assembly Reference

Add the required assembly reference:
- **Syncfusion.Shared.WPF**

### Step 3: Import Required Namespace

In your C# code file, add the using directive:

```csharp
using Syncfusion.Windows.Shared;
```

### Step 4: Create Carousel Instance

Create an instance of the Carousel control and add it to your window:

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

namespace CarouselSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create an instance of Carousel
            Carousel carousel = new Carousel();
            
            // Set height and width
            carousel.Height = 300;
            carousel.Width = 500;
            
            // Add to window's grid
            grid.Children.Add(carousel);
        }
    }
}
```

## Basic Configuration

### Setting Size

```xml
<syncfusion:Carousel Name="carousel"
                     Height="400"
                     Width="600"/>
```

```csharp
carousel.Height = 400;
carousel.Width = 600;
```

### Setting Radius (Standard Mode)

Control the circular path dimensions:

```xml
<syncfusion:Carousel RadiusX="200"
                     RadiusY="150"/>
```

```csharp
carousel.RadiusX = 200;
carousel.RadiusY = 150;
```

### Rotation Speed

Control animation speed (in milliseconds):

```xml
<syncfusion:Carousel RotationSpeed="300"/>
```

```csharp
carousel.RotationSpeed = 300;
```

## First Complete Example

Here's a complete working example with data binding:

### XAML

```xml
<Window x:Class="CarouselSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:CarouselSample"
        Title="Carousel Demo" Height="500" Width="700">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:Carousel Name="carousel"
                             Height="400"
                             Width="600"
                             ItemsSource="{Binding HeaderCollection}">
            <syncfusion:Carousel.ItemTemplate>
                <DataTemplate>
                    <Border Height="80" 
                            Width="120" 
                            BorderBrush="Purple" 
                            BorderThickness="3"
                            Background="LightBlue"
                            CornerRadius="5">
                        <TextBlock HorizontalAlignment="Center" 
                                   VerticalAlignment="Center"
                                   FontSize="16"
                                   FontWeight="Bold"
                                   Text="{Binding Header}"/>
                    </Border>
                </DataTemplate>
            </syncfusion:Carousel.ItemTemplate>
        </syncfusion:Carousel>
    </Grid>
</Window>
```

### C# Code (Model and ViewModel)

```csharp
using System.Collections.ObjectModel;

namespace CarouselSample
{
    // Model class
    public class CarouselModel
    {
        public string Header { get; set; }
    }
    
    // ViewModel class
    public class ViewModel
    {
        private ObservableCollection<CarouselModel> collection;
        
        public ObservableCollection<CarouselModel> HeaderCollection
        {
            get { return collection; }
            set { collection = value; }
        }
        
        public ViewModel()
        {
            HeaderCollection = new ObservableCollection<CarouselModel>();
            HeaderCollection.Add(new CarouselModel() { Header = "Buchanan" });
            HeaderCollection.Add(new CarouselModel() { Header = "Callahan" });
            HeaderCollection.Add(new CarouselModel() { Header = "Davolio" });
            HeaderCollection.Add(new CarouselModel() { Header = "Dodsworth" });
            HeaderCollection.Add(new CarouselModel() { Header = "Fuller" });
            HeaderCollection.Add(new CarouselModel() { Header = "King" });
            HeaderCollection.Add(new CarouselModel() { Header = "Leverling" });
            HeaderCollection.Add(new CarouselModel() { Header = "Suyama" });
        }
    }
}
```

This example creates a carousel with 8 items displayed in a circular rotating pattern.

## Common Setup Issues

### Issue 1: Carousel Not Visible

**Problem:** Carousel control doesn't appear in the window.

**Solutions:**
- Ensure Height and Width are set (Carousel doesn't auto-size by default)
- Check that the parent container has sufficient space
- Verify the control is added to the visual tree
- Check if items are actually added to the carousel

```xml
<!-- Correct: Explicit sizing -->
<syncfusion:Carousel Height="300" Width="500"/>

<!-- May not be visible without sizing -->
<syncfusion:Carousel/>
```

### Issue 2: Namespace Not Found

**Problem:** `The type 'syncfusion:Carousel' was not found.`

**Solutions:**
- Verify `Syncfusion.Shared.WPF` assembly is referenced
- Check that the namespace declaration is correct:
  ```xml
  xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
  ```
- Rebuild the project
- Ensure NuGet package is properly installed

### Issue 3: Items Not Displaying

**Problem:** Carousel is visible but items don't appear.

**Solutions:**
- Check if ItemsSource is bound correctly
- Verify the data source has items
- Ensure ItemTemplate is defined if using data binding
- Check if CarouselItem elements are properly defined

```xml
<!-- Ensure data binding is correct -->
<syncfusion:Carousel ItemsSource="{Binding YourCollection}">
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <!-- Define item appearance -->
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Issue 4: License Error

**Problem:** License validation error message appears.

**Solutions:**
- Register your Syncfusion license key in App.xaml.cs:
  ```csharp
  Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
  ```
- Ensure you have a valid Syncfusion license
- Community license users: Register your community license key

## Next Steps

Now that you have the Carousel control set up:

1. **Populate Items**: Learn how to add items using CarouselItem or data binding → [populating-items.md](populating-items.md)
2. **Navigation**: Explore keyboard, mouse, and programmatic navigation → [navigation.md](navigation.md)
3. **Visual Modes**: Understand Standard vs Custom Path modes → [standard-path.md](standard-path.md), [custom-path.md](custom-path.md)
4. **Advanced Features**: Explore effects, positioning, and optimization → [advanced-features.md](advanced-features.md)
