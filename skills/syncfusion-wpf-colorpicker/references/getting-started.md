# Getting Started with WPF ColorPicker

This guide covers setup, installation, and basic implementation of the Syncfusion WPF ColorPicker control.

## Assembly Dependencies

The ColorPicker requires the following assembly to function:
- **Syncfusion.Shared.WPF**

For NuGet installation, refer to [Syncfusion WPF NuGet packages documentation](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages).

## Adding ColorPicker via Designer

The easiest way to add ColorPicker to your application:

1. Open your WPF project in Visual Studio
2. Open the Toolbox panel
3. Locate **ColorPicker** under Syncfusion controls
4. Drag and drop it onto your designer view
5. The required assembly (Syncfusion.Shared.WPF) will be added automatically

![WPF Color Picker Drag and dropped from ToolBox](../../../../../../../docs/Getting-Started.md)

## Adding ColorPicker via XAML

To manually add ColorPicker in XAML:

### Step 1: Create a New WPF Project
Create a new WPF application in Visual Studio.

### Step 2: Add Assembly Reference
Add the Syncfusion.Shared.WPF assembly to your project references.

### Step 3: Import Syncfusion Schema
In your XAML file, import the Syncfusion WPF schema:
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 4: Declare ColorPicker
```xaml
<Window x:Class="ColorPickerSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="ColorPicker Sample" Height="450" Width="800">
    <Grid Name="grid">
        <syncfusion:ColorPicker Name="colorPicker" 
                                Height="100" 
                                Width="280"/>
    </Grid>
</Window>
```

This creates a basic ColorPicker control with default settings (solid color mode, color palette visible, alpha channel visible).

## Adding ColorPicker via C#

To add ColorPicker programmatically in C#:

### Step 1: Include Required Namespace
```csharp
using Syncfusion.Windows.Shared;
```

### Step 2: Create and Configure
```csharp
ColorPicker colorPicker = new ColorPicker();
colorPicker.Width = 300;
colorPicker.Height = 100;

// Add to your container (Grid, StackPanel, etc.)
grid.Children.Add(colorPicker);
```

## Control Structure

The ColorPicker consists of the following main components:

- **Color Picker Region** - Interactive area to select hue and saturation by dragging
- **Hue Spectrum Slider** - Vertical slider for hue value selection
- **Value Inputs** - RGB, HSV, or Hex value editors (depending on active tab)
- **Alpha Slider** - Horizontal slider for opacity control (if IsAlphaVisible is True)
- **Color Palette Dropdown** - Built-in color palette for quick selection (if IsColorPaletteVisible is True)
- **Brush Mode Buttons** - Buttons to switch between Solid, Linear Gradient, and Radial Gradient modes (if EnableSolidToGradientSwitch is True)

## Basic Configuration

### Set Initial Color
```xaml
<syncfusion:ColorPicker Color="Yellow"/>
```

Or in C#:
```csharp
colorPicker.Color = Colors.Yellow;
```

### Control Visibility of Components

Hide the alpha channel:
```xaml
<syncfusion:ColorPicker IsAlphaVisible="False"/>
```

Hide the color palette:
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="False"/>
```

Hide brush mode switching:
```xaml
<syncfusion:ColorPicker EnableSolidToGradientSwitch="False"/>
```

### Set Dimensions
```csharp
colorPicker.Width = 400;
colorPicker.Height = 350;
```

## Common Configuration Combinations

### Minimal (Solid Colors Only)
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="True"
                        IsAlphaVisible="False"
                        EnableSolidToGradientSwitch="False"/>
```
Use this for simple solid color selection without gradients or transparency.

### Full-Featured
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="True"
                        IsAlphaVisible="True"
                        EnableSolidToGradientSwitch="True"/>
```
Use this for complete color and gradient selection with all options.

### Compact (No Palette)
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="False"
                        Width="200"
                        Height="150"/>
```
Use this for space-constrained UI layouts.

## Verify Installation

To verify the ColorPicker is working:

1. Add a ColorPicker control to your window
2. Run the application
3. You should see a colored square area and controls below it
4. Click on the colored area to change the color
5. Verify you can adjust RGB, HSV, or Hex values
6. If visible, verify the color palette dropdown works

If the control doesn't appear, check:
- Syncfusion.Shared.WPF assembly is added to project references
- Correct schema URI is used: `http://schemas.syncfusion.com/wpf`
- Build the project to ensure no compilation errors

## Next Steps

- To select and work with colors: See **Solid and Gradient Colors** reference
- To customize behavior: See **Customization Options** reference
- To handle color changes: See **Events and Integration** reference
- To apply themes: See **Appearance and Theming** reference
