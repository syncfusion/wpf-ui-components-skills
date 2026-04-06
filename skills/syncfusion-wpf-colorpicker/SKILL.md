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

**Pattern 1: Solid Color Selection** - Set `Color` property and `IsColorPaletteVisible="True"` for user solid color picking.

**Pattern 2: Gradient Selection** - Set `Brush` property with `LinearGradientBrush` or `RadialGradientBrush` with `GradientStops` for gradient selection.

**Pattern 3: Respond to Color Changes** - Subscribe to `ColorChanged` and `SelectedBrushChanged` events to handle real-time color/brush updates.

**Pattern 4: Hide Alpha Channel** - Set `IsAlphaVisible="False"` for opaque colors only.

**Pattern 5: Lock to Solid Colors** - Set `EnableSolidToGradientSwitch="False"` to prevent gradient mode switching.

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
