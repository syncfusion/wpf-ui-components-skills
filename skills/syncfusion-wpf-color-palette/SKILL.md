---
name: syncfusion-wpf-color-palette
description: Implements the Syncfusion WPF Color Palette (SfColorPalette) control for color selection interfaces with swatches. Use this when implementing color picker functionality, color swatches, or color binding in WPF applications. Covers setup, color selection, data binding, swatch navigation, appearance customization, and theming.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Color Palettes

The `SfColorPalette` is a versatile color picker component that allows users to select colors from organized swatches. It provides multiple color sets, binding support, and extensive customization options.

## When to Use This Skill

Use this skill when you need to:

- **Add a color picker interface** to your WPF application
- **Allow users to select colors** from predefined swatches
- **Bind selected colors** to UI elements (brushes, backgrounds, etc.)
- **Navigate between different color swatches** (Material, Office, Metro, Grayscale, etc.)
- **Customize the appearance** (foreground, background, flow direction)
- **Apply themes** or customize the color palette styling

## Quick Overview

The Color Palette component consists of:

- **Color Swatches**: Different organized color sets (Material, Office, etc.)
- **Swatch Button**: Located at top-right to switch between color packages
- **Color Items**: Individual colors available in the current swatch
- **Tooltip Preview**: Shows the selected color when hovering
- **SelectedColor Property**: Returns the currently selected color as a `Color` object

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly references and NuGet package setup
- Adding the control via designer, XAML, and C#
- Basic Color Palette creation
- Selecting colors and handling events

### Color Binding
📄 **Read:** [references/color-binding.md](references/color-binding.md)
- Binding the SelectedColor property to other objects
- Creating color to brush converters
- Two-way data binding with the UI
- Binding selected colors to shapes and controls

### Swatches Navigation
📄 **Read:** [references/swatches-navigation.md](references/swatches-navigation.md)
- Available color swatches overview
- Switching between different swatch collections
- Understanding swatch button functionality
- Customizing swatch availability

### Appearance and Theming
📄 **Read:** [references/appearance-theming.md](references/appearance-theming.md)
- Customizing foreground and background colors
- Setting flow direction (LTR/RTL support)
- Applying built-in themes with SfSkinManager
- Creating custom themes with Theme Studio

## Quick Start Example

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    Title="Color Palette Example" Height="400" Width="500">
    <Grid>
        <StackPanel Margin="20">
            <TextBlock Text="Select a Color:" FontSize="14" Margin="0,0,0,10"/>
            <syncfusion:SfColorPalette 
                x:Name="colorPalette"
                Height="250"
                Width="300"
                HorizontalAlignment="Left"/>
            
            <TextBlock Text="Selected Color:" FontSize="14" Margin="0,20,0,10"/>
            <Rectangle 
                Fill="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"
                Height="50"
                Width="300"
                HorizontalAlignment="Left"/>
        </StackPanel>
    </Grid>
</Window>
```

## Common Patterns

### Pattern 1: Basic Color Selection
Use when you need a simple color picker without advanced binding:

```csharp
// Create and add the control
SfColorPalette colorPalette = new SfColorPalette();
colorPalette.Height = 300;
colorPalette.Width = 200;
this.Content = colorPalette;

// Get the selected color
Color selectedColor = colorPalette.SelectedColor;
```

### Pattern 2: Binding to UI Elements
Use when you want the selected color to automatically update UI elements:

**XAML:**
```xaml
<Grid.Resources>
    <local:ColorToSolidColorBrushValueConverter x:Key="ColorToBrushConverter"/>
</Grid.Resources>

<Rectangle Fill="{Binding ElementName=colorPalette, Path=SelectedColor, Converter={StaticResource ColorToBrushConverter}}"/>
<syncfusion:SfColorPalette x:Name="colorPalette"/>
```

**C# Converter:**
```csharp
public class ColorToSolidColorBrushValueConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value is Color color)
            return new SolidColorBrush(color);
        return null;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return true;
    }
}
```

### Pattern 3: Styled Color Palette
Use when you need to customize appearance with themes and colors:

```xaml
<syncfusion:SfColorPalette 
    x:Name="colorPalette"
    Foreground="Blue"
    Background="AliceBlue"
    FlowDirection="LeftToRight"
    Height="250"
    Width="300"/>
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `SelectedColor` | `Color` | — | Gets the currently selected color |
| `Foreground` | `Brush` | Gray | Foreground color of the palette UI |
| `Background` | `Brush` | Snow | Background color of the palette |
| `FlowDirection` | `FlowDirection` | LeftToRight | Layout direction (LTR or RTL) |

## Common Use Cases

- **Theme Customizer**: Allow users to pick custom colors for application themes
- **Design Tools**: Integrated color picker for graphics or UI design applications
- **Branding Tools**: Select brand colors for design systems
- **Data Visualization**: Pick colors for chart series, legends, or annotations
- **Accessibility Tools**: Provide quick color selection for color blindness solutions
