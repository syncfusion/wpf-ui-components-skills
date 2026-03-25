# Getting Started with SfBusyIndicator

This guide covers installation, basic setup, and initial implementation of the Syncfusion WPF Busy Indicator control.

## Installation

### Via NuGet Package Manager

Install the SfBusyIndicator package using one of these methods:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.SfBusyIndicator.WPF
```

**NuGet Package Manager UI:**
1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.SfBusyIndicator.WPF"
4. Click Install

**.NET CLI:**
```bash
dotnet add package Syncfusion.SfBusyIndicator.WPF
```

## Assembly References

The control requires these assembly references (automatically added via NuGet):

- **Syncfusion.SfBusyIndicator.WPF.dll** - Main control assembly
- **Syncfusion.Shared.WPF.dll** - Shared utilities (dependency)

To verify references, check your project's Dependencies > Assemblies in Solution Explorer.

## Namespace Imports

### XAML Namespace

Add the namespace declaration to your Window or UserControl:

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Notification;assembly=Syncfusion.SfBusyIndicator.WPF"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
</Window>
```

Or use a shorter alias:

```xml
<Window xmlns:notification="clr-namespace:Syncfusion.Windows.Controls.Notification;assembly=Syncfusion.SfBusyIndicator.WPF">
</Window>
```

### C# Code-Behind

Add the using directive in your code files:

```csharp
using Syncfusion.Windows.Controls.Notification;
```

## License Configuration

Syncfusion components require a valid license key. Register it during application startup:

```csharp
// In App.xaml.cs
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

**License Key Sources:**
- **Trial:** Available when you download the trial from Syncfusion website
- **Licensed:** Found in your Syncfusion account dashboard
- **Community:** Free license available for qualifying users

**Important:** The license must be registered before any Syncfusion control is instantiated.

## Basic Implementation

### XAML Implementation

Create a basic busy indicator in XAML:

```xml
<Window x:Class="BusyIndicatorApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Notification;assembly=Syncfusion.SfBusyIndicator.WPF"
        Title="Busy Indicator Demo" Height="450" Width="800">
    
    <Grid Background="CornflowerBlue">
        <syncfusion:SfBusyIndicator IsBusy="True"/>
    </Grid>
</Window>
```

### Code-Behind Implementation

Create the control programmatically:

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Notification;

namespace BusyIndicatorApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create busy indicator
            SfBusyIndicator busyIndicator = new SfBusyIndicator();
            busyIndicator.IsBusy = true;
            
            // Add to grid
            Grid grid = new Grid();
            grid.Background = System.Windows.Media.Brushes.CornflowerBlue;
            grid.Children.Add(busyIndicator);
            
            this.Content = grid;
        }
    }
}
```

### VB.NET Implementation

```vb
Imports Syncfusion.Windows.Controls.Notification

Public Class MainWindow
    Public Sub New()
        InitializeComponent()
        
        ' Create busy indicator
        Dim busyIndicator As New SfBusyIndicator()
        busyIndicator.IsBusy = True
        
        ' Add to grid
        Dim grid As New Grid()
        grid.Background = Brushes.CornflowerBlue
        grid.Children.Add(busyIndicator)
        
        Me.Content = grid
    End Sub
End Class
```

## Minimal Working Example

This complete example shows a functional busy indicator that appears on button click:

```xml
<Window x:Class="BusyIndicatorApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Notification;assembly=Syncfusion.SfBusyIndicator.WPF"
        Title="Busy Indicator Demo" Height="300" Width="400">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <Button Grid.Row="0" Content="Show Busy Indicator" 
                Click="OnShowBusyClick" Margin="10"/>
        
        <Grid Grid.Row="1" Background="CornflowerBlue">
            <syncfusion:SfBusyIndicator x:Name="busyIndicator" 
                                         IsBusy="False"
                                         AnimationType="DoubleCircle"
                                         Header="Loading..."
                                         Foreground="White"/>
        </Grid>
    </Grid>
</Window>
```

```csharp
private async void OnShowBusyClick(object sender, RoutedEventArgs e)
{
    busyIndicator.IsBusy = true;
    
    // Simulate work
    await Task.Delay(3000);
    
    busyIndicator.IsBusy = false;
}
```

## Theme Support

The SfBusyIndicator supports Syncfusion's theme system for consistent styling across your application.

### Applying Themes

**Method 1: Using SfSkinManager (Recommended)**

```csharp
// In App.xaml.cs or Window constructor
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

Available themes:
- MaterialLight, MaterialDark
- FluentLight, FluentDark
- Office2019Colorful, Office2019Black, Office2019White
- Office2016Colorful, Office2016White, Office2016DarkGray
- And more...

**Method 2: XAML Theme Application**

```xml
<syncfusion:ChromelessWindow 
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialLight}">
    
    <Grid>
        <syncfusion:SfBusyIndicator IsBusy="True"/>
    </Grid>
</syncfusion:ChromelessWindow>
```

**Method 3: Resource Dictionary**

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Syncfusion.Themes.MaterialLight.WPF;component/MSControl/SfBusyIndicator.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Verification

To verify successful setup:

1. **Build the project** - No errors should appear
2. **Run the application** - The busy indicator should display
3. **Check animation** - Animation should play when IsBusy is True

If you encounter issues, verify:
- NuGet package is installed correctly
- License key is registered (check Output window for license errors)
- Namespace is correctly imported
- Assembly references are present

## GitHub Samples

Explore complete working examples:

- **Getting Started Sample:** https://github.com/SyncfusionExamples/wpf-BusyIndicator-examples/tree/master/Samples/Getting-Started
- **All Samples:** https://github.com/SyncfusionExamples/wpf-BusyIndicator-examples

## Next Steps

Now that you have the basic setup working:

- **Configure animations** - See `animation-types.md` for 37+ animation options
- **Control display** - See `isbusy-property.md` for managing visibility
- **Add headers** - See `header-customization.md` for custom text/templates
- **Adjust sizing** - See `sizing.md` for ViewboxHeight and ViewboxWidth
- **Advanced customization** - See `methods.md` for extending the control

## Common Setup Issues

**Issue:** "The type 'SfBusyIndicator' was not found"
- **Solution:** Verify namespace import and assembly reference

**Issue:** License error messages in output window
- **Solution:** Register valid license key in App.xaml.cs OnStartup

**Issue:** Control not visible
- **Solution:** Ensure IsBusy is set to True and container has sufficient size

**Issue:** Animation not playing
- **Solution:** Check that AnimationType is set and IsBusy is True
