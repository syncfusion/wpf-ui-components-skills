# Getting Started with SfDomainUpDown

Complete guide to installing, configuring, and creating your first WPF DomainUpDown control.

## Overview

The `SfDomainUpDown` control is a WPF input control that allows users to cycle through a predefined list of items using spin buttons (up/down arrows). It provides a compact way to select from a fixed set of values without requiring a dropdown list.

**Use Cases:**
- Selecting from a fixed list of strings (days, months, status values)
- Navigating through employee names or contact lists
- Choosing predefined categories or types
- Any scenario requiring sequential item selection

## Assembly Deployment

### Required Assemblies

To use the `SfDomainUpDown` control, you need the following assemblies:

1. **Syncfusion.SfInput.WPF** - Core input control library
2. **Syncfusion.SfShared.WPF** - Shared utilities and base classes

### Installation Methods

#### Method 1: NuGet Package Manager (Recommended)

Open Package Manager Console and run:

```powershell
Install-Package Syncfusion.SfInput.WPF
```

This automatically installs all dependencies including `Syncfusion.SfShared.WPF`.

#### Method 2: NuGet Package Manager UI

1. Right-click your project → **Manage NuGet Packages**
2. Search for `Syncfusion.SfInput.WPF`
3. Click **Install**

#### Method 3: Syncfusion Reference Manager

If you have Syncfusion Visual Studio extensions installed:

1. Right-click project → **Syncfusion Reference Manager**
2. Select **WPF** platform
3. Check **SfDomainUpDown** control
4. Click **OK** to add references

#### Method 4: Manual Assembly Reference

1. Download Syncfusion Essential Studio for WPF
2. Navigate to installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{version}\Assemblies\{framework-version}`)
3. Add references to:
   - `Syncfusion.SfInput.WPF.dll`
   - `Syncfusion.SfShared.WPF.dll`

## Creating Your First SfDomainUpDown

### Method 1: Adding Control via Designer

The easiest way to add `SfDomainUpDown` to your application:

1. **Open Visual Studio Designer** for your XAML page
2. **Open Toolbox** (View → Toolbox or Ctrl+Alt+X)
3. **Locate SfDomainUpDown** in the Syncfusion WPF Toolbox category
4. **Drag and drop** the control onto your design surface

The required assemblies will be added automatically, and the namespace will be imported.

**Result in XAML:**
```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown" 
                           Height="30" 
                           Width="150"/>
```

### Method 2: Adding Control Manually in XAML

For more control over the implementation:

**Step 1: Import the Namespace**

Add the Syncfusion namespace to your Window or Page:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="DomainUpDown Example" Height="450" Width="800">
```

**Alternative namespace format:**
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.WPF"
```

**Step 2: Declare the Control**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:SfDomainUpDown x:Name="domainUpDown"
                                   Height="30" 
                                   Width="150" 
                                   Value="40"
                                   HorizontalAlignment="Center"
                                   VerticalAlignment="Center"/>
    </Grid>
</Window>
```

### Method 3: Adding Control Manually in C#

For programmatic creation:

**Step 1: Add Using Directive**

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;
```

**Step 2: Create and Configure the Control**

```csharp
namespace DomainUpDownExample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create the control
            SfDomainUpDown domainUpDown = new SfDomainUpDown();
            
            // Configure properties
            domainUpDown.Height = 30;
            domainUpDown.Width = 150;
            domainUpDown.Value = 40;
            domainUpDown.HorizontalAlignment = HorizontalAlignment.Center;
            domainUpDown.VerticalAlignment = VerticalAlignment.Center;
            
            // Add to the window
            this.Content = domainUpDown;
        }
    }
}
```

## Basic Configuration

### Setting the Initial Value

Set the current value using the `Value` property:

**XAML:**
```xml
<syncfusion:SfDomainUpDown Value="James"/>
```

**C#:**
```csharp
domainUpDown.Value = "James";
```

### Populating Items (Simple List)

Add items directly in XAML:

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown" Value="James">
    <syncfusion:SfDomainUpDown.ItemsSource>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>Lucas</sys:String>
            <sys:String>James</sys:String>
            <sys:String>Jacob</sys:String>
            <sys:String>Michael</sys:String>
        </x:Array>
    </syncfusion:SfDomainUpDown.ItemsSource>
</syncfusion:SfDomainUpDown>
```

Or in C#:

```csharp
domainUpDown.ItemsSource = new List<string> 
{ 
    "Lucas", 
    "James", 
    "Jacob", 
    "Michael" 
};
domainUpDown.Value = "James";
```

## Complete Working Example

Here's a full working example to get you started:

**MainWindow.xaml:**
```xml
<Window x:Class="DomainUpDownDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="DomainUpDown Demo" Height="300" Width="400">
    <Grid>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center" Spacing="20">
            <TextBlock Text="Select a Day:" FontWeight="Bold" FontSize="14"/>
            
            <syncfusion:SfDomainUpDown x:Name="daySelector" 
                                       Height="35" 
                                       Width="200"
                                       Value="Monday">
                <syncfusion:SfDomainUpDown.ItemsSource>
                    <x:Array Type="sys:String">
                        <sys:String>Monday</sys:String>
                        <sys:String>Tuesday</sys:String>
                        <sys:String>Wednesday</sys:String>
                        <sys:String>Thursday</sys:String>
                        <sys:String>Friday</sys:String>
                        <sys:String>Saturday</sys:String>
                        <sys:String>Sunday</sys:String>
                    </x:Array>
                </syncfusion:SfDomainUpDown.ItemsSource>
            </syncfusion:SfDomainUpDown>
            
            <TextBlock Text="{Binding ElementName=daySelector, Path=Value, StringFormat='Selected: {0}'}" 
                       FontSize="12"
                       HorizontalAlignment="Center"/>
        </StackPanel>
    </Grid>
</Window>
```

## Common Configuration Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Value` | `object` | `null` | Currently selected item |
| `ItemsSource` | `IEnumerable` | `null` | Collection of items |
| `Height` | `double` | `Auto` | Control height |
| `Width` | `double` | `Auto` | Control width |
| `HorizontalAlignment` | `HorizontalAlignment` | `Stretch` | Horizontal positioning |
| `VerticalAlignment` | `VerticalAlignment` | `Stretch` | Vertical positioning |

## Best Practices

1. **Always Set ItemsSource:** Populate items before setting the initial value
2. **Valid Initial Value:** Ensure `Value` exists in `ItemsSource` to avoid errors
3. **Use Namespaces:** Prefer the schema URL (`http://schemas.syncfusion.com/wpf`) for cleaner XAML
4. **Name Your Controls:** Use `x:Name` for accessing controls in code-behind
5. **Size Appropriately:** Set explicit `Height` and `Width` for consistent layouts

## Troubleshooting

### Control Doesn't Appear

**Problem:** Control not visible in the designer or at runtime.

**Solutions:**
- Verify assembly references are correctly added
- Check that namespace import is correct
- Ensure control has explicit size or is in a proper layout container
- Verify license is valid (trial or licensed)

### Items Don't Show

**Problem:** Control is visible but items don't cycle when clicking spin buttons.

**Solutions:**
- Verify `ItemsSource` is populated
- Check that items are of the correct type
- Ensure `Value` is set to a valid item from `ItemsSource`

### Namespace Not Recognized

**Problem:** XAML shows error for `syncfusion:` prefix.

**Solutions:**
- Clean and rebuild the solution
- Verify NuGet packages are restored
- Check that assembly versions match
- Try using the CLR namespace format instead of schema URL

## Next Steps

Now that you have the basic control set up, explore:

- **[Data Population](data-population.md)** - Bind complex objects with custom templates
- **[Spin Button Alignment](spin-button-alignment.md)** - Customize button positioning
- **[Appearance and Styling](appearance-styling.md)** - Theme and customize the control
- **[Advanced Features](advanced-features.md)** - AutoReverse, gestures, and more
