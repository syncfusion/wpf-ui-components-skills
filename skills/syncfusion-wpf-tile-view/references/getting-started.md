# Getting Started with WPF TileView

## Overview
This guide covers the essential steps to set up and create your first WPF TileViewControl in a Visual Studio project.

## Assembly Installation

### Required Assembly
To use TileViewControl in your WPF application, you need the following assembly:
- **Syncfusion.Shared.WPF**

### NuGet Installation
Install the package via NuGet Package Manager:
```
Install-Package Syncfusion.Shared.WPF
```

## Adding TileViewControl via Designer

The easiest way to add TileViewControl is through the Visual Studio designer:

1. Open your WPF window in the designer view
2. Locate **TileViewControl** in the Visual Studio Toolbox under Syncfusion components
3. Drag and drop it onto your designer surface
4. The required assembly (Syncfusion.Shared.WPF) is automatically added to your project

The designer also provides a **SmartTag** feature to configure basic properties directly in design mode.

## Adding TileViewControl via XAML

For manual XAML setup:

1. Create a new WPF project in Visual Studio
2. Add **Syncfusion.Shared.WPF** assembly reference
3. Import the Syncfusion schema in your XAML window:

```xaml
<Window x:Class="TileViewSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="TileView Sample" Height="450" Width="800">
    
    <Grid Name="grid">
        <syncfusion:TileViewControl Name="tileViewControl"
                                    Height="400"
                                    Width="600"/>
    </Grid>
</Window>
```

## Adding TileViewControl via C#

To programmatically create a TileViewControl:

1. Create a new WPF application
2. Add **Syncfusion.Shared.WPF** assembly reference
3. Add using statement and create the control:

```csharp
using Syncfusion.Windows.Shared;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Create TileViewControl
        TileViewControl tileViewControl = new TileViewControl();
        tileViewControl.Height = 400;
        tileViewControl.Width = 600;
        
        // Add to your Grid or Container
        grid.Children.Add(tileViewControl);
    }
}
```

## Basic Configuration

After adding TileViewControl, configure basic properties:

```xaml
<syncfusion:TileViewControl Name="tileViewControl"
                            Height="400"
                            Width="600"
                            Margin="10"
                            Background="White">
    
    <!-- TileViewItems will go here -->
    
</syncfusion:TileViewControl>
```

### Common Initial Properties
- `Height`, `Width` - Set control size
- `Margin` - Add spacing around the control
- `Background` - Set background color
- `Foreground` - Set text color

## Creating Your First Tile

Add basic tiles to your TileViewControl:

```xaml
<syncfusion:TileViewControl Name="tileViewControl">
    
    <syncfusion:TileViewItem Header="Dashboard">
        <Grid Background="#F0F0F0">
            <TextBlock VerticalAlignment="Center" HorizontalAlignment="Center">
                Dashboard Content
            </TextBlock>
        </Grid>
    </syncfusion:TileViewItem>
    
    <syncfusion:TileViewItem Header="Settings">
        <Grid Background="#F0F0F0">
            <TextBlock VerticalAlignment="Center" HorizontalAlignment="Center">
                Settings Content
            </TextBlock>
        </Grid>
    </syncfusion:TileViewItem>
    
</syncfusion:TileViewControl>
```

### TileViewItem Properties
- `Header` - Title displayed on the tile
- `Content` - Main content area (can be any UIElement)
- Default minimized size - Automatically calculated

## Minimal Working Example

Here's a complete minimal example:

```xaml
<Window x:Class="TileViewApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="TileView Minimal Example" Height="500" Width="700">
    
    <Grid>
        <syncfusion:TileViewControl>
            <syncfusion:TileViewItem Header="Tile 1">
                <TextBlock Text="Content 1" VerticalAlignment="Center" HorizontalAlignment="Center"/>
            </syncfusion:TileViewItem>
            <syncfusion:TileViewItem Header="Tile 2">
                <TextBlock Text="Content 2" VerticalAlignment="Center" HorizontalAlignment="Center"/>
            </syncfusion:TileViewItem>
            <syncfusion:TileViewItem Header="Tile 3">
                <TextBlock Text="Content 3" VerticalAlignment="Center" HorizontalAlignment="Center"/>
            </syncfusion:TileViewItem>
        </syncfusion:TileViewControl>
    </Grid>
</Window>
```

## Next Steps
- For customizing individual tiles, see **TileViewItem Features**
- To enable drag-drop, see **Arrangement and Layout**
- For styling tips, see **Appearance and Customization**
- To bind data, see **Data Binding**

## Troubleshooting

**Assembly not found?**
- Verify Syncfusion.Shared.WPF is installed via NuGet
- Check project references in Solution Explorer

**Tiles not appearing?**
- Ensure TileViewControl has `Height` and `Width` set
- Check that TileViewItems contain content
- Verify the Syncfusion schema is imported: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`

**Designer not showing?**
- Rebuild solution (Ctrl+Shift+B)
- Close and reopen XAML designer
- Check Visual Studio version compatibility
