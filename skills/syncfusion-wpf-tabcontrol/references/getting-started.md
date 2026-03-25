# Getting Started with WPF TabControl

This guide covers the fundamentals of setting up and creating your first TabControl application.

## Assembly References

To use TabControl in your WPF application, you need to add the following NuGet packages:

**Required Assemblies:**
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

**Installation via NuGet Package Manager:**
```powershell
Install-Package Syncfusion.Tools.WPF
Install-Package Syncfusion.Shared.WPF
```

**Adding via Visual Studio:**
1. Right-click on project → Add Reference
2. Browse to installed Syncfusion libraries
3. Select both `Syncfusion.Tools.WPF` and `Syncfusion.Shared.WPF`

## Creating TabControl via XAML

### Step 1: Import Syncfusion Schema
At the top of your XAML file, add the Syncfusion namespace:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 2: Declare TabControlExt
```xml
<Window x:Class="TabControlApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="TabControl Example" Height="450" Width="800">

    <Grid>
        <syncfusion:TabControlExt Name="tabControl" Height="300" Width="400" />
    </Grid>
</Window>
```

### Step 3: Add TabItems
```xml
<syncfusion:TabControlExt Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1">
        <TextBlock Text="Content for Tab 1" Margin="10" />
    </syncfusion:TabItemExt>
    <syncfusion:TabItemExt Header="Tab 2">
        <TextBlock Text="Content for Tab 2" Margin="10" />
    </syncfusion:TabItemExt>
    <syncfusion:TabItemExt Header="Tab 3">
        <TextBlock Text="Content for Tab 3" Margin="10" />
    </syncfusion:TabItemExt>
</syncfusion:TabControlExt>
```

## Creating TabControl via C#

### Basic Setup
```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Windows;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Create TabControl instance
        TabControlExt tabControl = new TabControlExt();
        tabControl.Height = 300;
        tabControl.Width = 400;
        
        // Add to window
        this.Content = tabControl;
    }
}
```

### Adding TabItems Programmatically
```csharp
// Create TabItems
TabItemExt tabItem1 = new TabItemExt() 
{
    Header = "Tab 1",
    Content = new TextBlock() 
    { 
        Text = "Content for Tab 1",
        Margin = new Thickness(10)
    }
};

TabItemExt tabItem2 = new TabItemExt() 
{
    Header = "Tab 2",
    Content = new TextBlock() 
    { 
        Text = "Content for Tab 2",
        Margin = new Thickness(10)
    }
};

// Add to TabControl
tabControl.Items.Add(tabItem1);
tabControl.Items.Add(tabItem2);
```

## Tab Item Structure

Each `TabItemExt` consists of:
- **Header**: The label displayed on the tab (text or custom content)
- **Content**: The main content displayed when the tab is selected
- **Optional Styling**: Background, foreground, and other visual properties

```xml
<syncfusion:TabItemExt Header="My Tab Name">
    <Grid>
        <!-- Tab content goes here -->
        <TextBlock Text="Tab content" />
    </Grid>
</syncfusion:TabItemExt>
```

## Tab Placement Options

Control where the tab headers appear using `TabStripPlacement`:

**Top Placement (Default):**
```xml
<syncfusion:TabControlExt TabStripPlacement="Top" Name="tabControl" />
```

**Bottom Placement:**
```xml
<syncfusion:TabControlExt TabStripPlacement="Bottom" Name="tabControl" />
```

**Left Placement:**
```xml
<syncfusion:TabControlExt TabStripPlacement="Left" Name="tabControl" />
```

**Right Placement:**
```xml
<syncfusion:TabControlExt TabStripPlacement="Right" Name="tabControl" />
```

**C# Equivalent:**
```csharp
tabControl.TabStripPlacement = Dock.Bottom;  // or Dock.Left, Dock.Right, Dock.Top
```

## Adding Icons to Tab Headers

Create rich tab headers with icons using nested controls:

```xml
<syncfusion:TabItemExt>
    <syncfusion:TabItemExt.Header>
        <StackPanel Orientation="Horizontal">
            <Image Source="icon.png" Width="16" Height="16" Margin="0,0,5,0" />
            <TextBlock Text="Tab with Icon" />
        </StackPanel>
    </syncfusion:TabItemExt.Header>
    <TextBlock Text="Content" Margin="10" />
</syncfusion:TabItemExt>
```

## Complex Content in Tabs

Use containers to organize complex content:

```xml
<syncfusion:TabItemExt Header="Data Grid Tab">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <TextBlock Text="Data Summary" Grid.Row="0" FontWeight="Bold" Margin="10" />
        <!-- Add your grid or other controls here -->
    </Grid>
</syncfusion:TabItemExt>
```

## Best Practices

1. **Namespace Import**: Always import the Syncfusion namespace in XAML files
2. **Assembly References**: Include both `Syncfusion.Tools.WPF` and `Syncfusion.Shared.WPF`
3. **Default Selection**: The first TabItem is selected by default; change with `SelectedItem` property
4. **Naming Convention**: Use meaningful names for tab headers (e.g., "Documents", "Settings", "Properties")
5. **Content Sizing**: Ensure tab content fits the available space; use proper containers
6. **Performance**: For many TabItems, consider lazy-loading content or using virtualization

## Common Issues

**Issue: TabControl not appearing**
- Verify that `Syncfusion.Tools.WPF` assembly is referenced
- Check that namespace is correctly imported: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`

**Issue: Namespace not recognized**
- Ensure NuGet packages are installed correctly
- Rebuild the solution after adding references

**Issue: Content not displaying**
- Check that TabItems have proper `Header` and `Content` properties
- Verify content controls are properly initialized
