---
name: syncfusion-wpf-colorpicker
description: Implement Syncfusion WPF ColorPicker for color and gradient selection. Use this skill whenever users need to add color picking functionality, select solid colors, create linear/radial gradients, customize palettes, apply themes, or handle color-changed events in WPF applications. Covers XAML and C# implementations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF ColorPicker

The [ColorPicker](https://www.syncfusion.com/wpf-controls/colorpicker) is a WPF user interface control for selecting and adjusting color values. It supports multiple color specifications including RGB (Red Green Blue), HSV (Hue Saturation Value), and hexadecimal codes, with built-in gradient editor, eyedropper, color palette, and theming support.

## When to Use This Skill

Use the ColorPicker when you need to:
- Allow users to select solid colors in WPF applications
- Create linear or radial gradient brushes with multiple color stops
- Edit color values manually using RGB, HSV, or Hex input
- Include an eyedropper tool for picking colors from anywhere in the app
- Control color opacity/alpha channel
- Provide a color palette for quick color selection
- Handle color-changed and brush-changed events
- Apply built-in or custom themes to match application design

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly dependencies and NuGet package setup
- Adding ColorPicker via Designer, XAML, and C#
- Control structure overview
- Basic configuration

### Solid and Gradient Colors
📄 **Read:** [references/solid-gradient-colors.md](references/solid-gradient-colors.md)
- Selecting solid colors with Color property
- Creating linear gradients with StartPoint, EndPoint, and GradientStops
- Creating radial gradients with GradientOrigin, Center, RadiusX, RadiusY
- Runtime color editing and selection
- Managing opacity with alpha channel
- Switching between solid, linear, and gradient modes

### Customization Options
📄 **Read:** [references/customization.md](references/customization.md)
- Enabling/disabling color palette visibility
- Gradient brush display modes (Default vs Extended)
- Inverted color calculation for contrast
- ScRGB color support
- Hiding or showing alpha value editors
- Restricting brush mode transitions

### Appearance and Theming
📄 **Read:** [references/appearance-theming.md](references/appearance-theming.md)
- Built-in theme support
- SfSkinManager integration for theme application
- ThemeStudio custom theme creation
- Theme-based appearance customization

### Events and Integration
📄 **Read:** [references/events-integration.md](references/events-integration.md)
- ColorChanged event handling
- SelectedBrushChanged event handling
- Runtime color change notifications
- Integration patterns with other controls

## Quick Start

### Basic XAML Setup
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:ColorPicker Name="colorPicker" 
                                Width="280" 
                                Height="100"/>
    </Grid>
</Window>
```

### Basic C# Setup
```csharp
using Syncfusion.Windows.Shared;

ColorPicker colorPicker = new ColorPicker();
colorPicker.Width = 300;
colorPicker.Height = 100;
grid.Children.Add(colorPicker);
```

### Set Initial Color
```xaml
<syncfusion:ColorPicker Color="Yellow" />
```

## Common Patterns

### Pattern 1: Solid Color Selection
Use when you need users to pick a single solid color for a brush or property.
```xaml
<syncfusion:ColorPicker x:Name="solidColorPicker"
                        Color="Blue"
                        IsColorPaletteVisible="True"/>
```

### Pattern 2: Gradient Selection
Use when you need users to create linear or radial gradients for advanced styling.
```xaml
<syncfusion:ColorPicker x:Name="gradientPicker" Width="200">
    <syncfusion:ColorPicker.Brush>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" />
            <GradientStop Color="Red" Offset="0.5" />
            <GradientStop Color="Blue" Offset="1.0" />
        </LinearGradientBrush>
    </syncfusion:ColorPicker.Brush>
</syncfusion:ColorPicker>
```

### Pattern 3: Respond to Color Changes
Use when you need to react to user color selections in real-time.
```csharp
ColorPicker colorPicker = new ColorPicker();
colorPicker.ColorChanged += ColorPicker_ColorChanged;
colorPicker.SelectedBrushChanged += ColorPicker_SelectedBrushChanged;

private void ColorPicker_ColorChanged(DependencyObject d, 
                                      DependencyPropertyChangedEventArgs e)
{
    // Handle color change
    Color selectedColor = (Color)e.NewValue;
}

private void ColorPicker_SelectedBrushChanged(DependencyObject d, 
                                              DependencyPropertyChangedEventArgs e)
{
    // Handle brush change
    Brush selectedBrush = (Brush)e.NewValue;
}
```

### Pattern 4: Hide Alpha Channel
Use when you only need opaque colors without transparency options.
```xaml
<syncfusion:ColorPicker IsAlphaVisible="False" />
```

### Pattern 5: Lock to Solid Colors Only
Use when you want to prevent users from switching to gradient modes.
```xaml
<syncfusion:ColorPicker EnableSolidToGradientSwitch="false" />
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Color` | Color | Gets or sets the selected solid color |
| `Brush` | Brush | Gets or sets the selected gradient or solid brush |
| `IsAlphaVisible` | bool | Shows/hides alpha (opacity) controls |
| `IsColorPaletteVisible` | bool | Shows/hides built-in color palette |
| `EnableSolidToGradientSwitch` | bool | Allows/prevents switching between modes |
| `GradientBrushDisplayMode` | enum | Sets display mode (Default or Extended) |

## Key Events

| Event | Triggered When |
|-------|----------------|
| `ColorChanged` | Selected solid color changes |
| `SelectedBrushChanged` | Selected brush (gradient) changes |

## Dependencies

- **Assembly:** Syncfusion.Shared.WPF
- **Framework:** .NET Framework 4.5+
- **Dependencies:** Syncfusion WPF shared components
