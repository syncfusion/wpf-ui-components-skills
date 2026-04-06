# Customization Options

This reference covers customizing the appearance and behavior of the WPF ColorPicker control.

## Table of Contents
- [Color Palette Visibility](#color-palette-visibility)
- [Gradient Brush Display Modes](#gradient-brush-display-modes)
- [Inverted Color Calculation](#inverted-color-calculation)
- [ScRGB Color Support](#scrgb-color-support)
- [Alpha Channel Control](#alpha-channel-control)
- [Restricting Brush Mode Transitions](#restricting-brush-mode-transitions)

## Color Palette Visibility

The built-in color palette provides users with quick access to predefined colors without using the picker region.

### Enable Color Palette (Default)
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="True"/>
```

When enabled, a dropdown button appears that displays a grid of standard colors for quick selection.

```csharp
colorPicker.IsColorPaletteVisible = true;
```

### Disable Color Palette
```xaml
<syncfusion:ColorPicker IsColorPaletteVisible="False"/>
```

Use this to force users to use the picker region or manually enter color values, keeping the UI cleaner.

```csharp
colorPicker.IsColorPaletteVisible = false;
```

### Palette Use Cases

**Enable palette when:**
- You want quick color selection for common colors
- Users are less familiar with color pickers
- You want to suggest a standard color set

**Disable palette when:**
- Space is limited
- You need complete control over color values
- Fine-grained color selection is required

## Gradient Brush Display Modes

Set `GradientBrushDisplayMode` to control gradient preview appearance:

| Mode | Size | Use Case |
|------|------|----------|
| Default | Compact | Space-constrained dialogs; minimal UI footprint |
| Extended | Large | Dedicated color panels; detailed gradient control |

Set in XAML: `GradientBrushDisplayMode="Default"` or `GradientBrushDisplayMode="Extended"`. In C#: `colorPicker.GradientBrushDisplayMode = GradientBrushDisplayMode.Default;`

## Inverted Color Calculation

**Note:** Inverted color and ScRGB color support are features of the `ColorEdit` control (related component), not `ColorPicker`. These advanced features are available in `ColorEdit`:
- **Inverted Color:** Access via `colorEdit.InvertColor` property for auto-contrasting text colors
- **ScRGB Support:** Set `colorEdit.IsScRGBColor = true` for extended color gamut (professional graphics applications)

## Alpha Channel Control

Use `IsAlphaVisible="True"` to show opacity controls or `IsAlphaVisible="False"` to hide them. Enable when users need transparency control (image editing); disable for solid colors only. See [solid-gradient-colors.md](solid-gradient-colors.md#managing-opacity-alpha-channel) for comprehensive examples.

## Restricting Brush Mode Transitions

Use `EnableSolidToGradientSwitch` to allow or prevent mode switching:
- `EnableSolidToGradientSwitch="True"` (default): Shows mode buttons; users can switch between solid and gradient
- `EnableSolidToGradientSwitch="False"`: Hides mode buttons; locks to current mode only

**Use cases:** Enable for full-featured pickers, disable for solid-color-only or gradient-only applications. In code, set based on user context or feature availability.

## Combining Customization Options

Combine multiple properties to achieve specific configurations:
- **Solid color only:** `EnableSolidToGradientSwitch="False"` + `IsAlphaVisible="False"`
- **Professional gradient editor:** `EnableSolidToGradientSwitch="True"` + `GradientBrushDisplayMode="Extended"` + `IsAlphaVisible="True"`
- **Compact picker:** Use Default display mode with `Width="180" Height="100"`
