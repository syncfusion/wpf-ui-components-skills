---
name: syncfusion-wpf-color-picker-palette
description: Implements the Syncfusion WPF ColorPickerPalette control for color selection from themed and standard color palettes. Use this when adding color pickers with predefined palettes, customizing color options, or handling color selection events in WPF applications. Covers setup, color management, appearance customization, and interaction patterns.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF ColorPickerPalette

The [ColorPickerPalette](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Tools.Controls.ColorPickerPalette.html) is a rich color selection control providing a visual palette interface with theme colors, standard colors, custom colors, and extended color options. Use this skill when you need to implement color selection functionality with flexible customization options.

## When to Use This Skill

Use ColorPickerPalette when you need to:
- Allow users to select colors from predefined palettes (theme, standard, custom)
- Display recently used colors
- Provide "More Colors" options for extended selection
- Customize color panels and visibility
- Handle color selection events
- Support RTL flow direction
- Enable transparent or "No Color" options

## Component Overview

ColorPickerPalette provides:
- **Multiple color sources:** Theme colors, standard colors, custom colors, recently used
- **Flexible modes:** Dropdown (default), Palette (expanded), Split (button + dropdown)
- **Rich customization:** Panel visibility, sizing, icons, headers, themes
- **Event support:** SelectedBrushChanged event and SelectedCommand binding
- **Accessibility:** Tooltip support, keyboard navigation, RTL support

---

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Adding ColorPickerPalette via designer, XAML, and C#
- Control structure and key properties
- Setting initial colors programmatically
- Theme and localization support

### Color Management
📄 **Read:** [references/color-management.md](references/color-management.md)
- Accessing and changing colors programmatically
- Working with color brushes
- Automatic color defaults
- Transparent color selection
- Theme color variants and standard colors
- Custom colors collection
- Recently used color panel

### Appearance & Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)
- Flow direction (RTL) support
- Panel visibility options (theme, standard, custom, recently used)
- Display modes (Dropdown, Palette, Split)
- Resizing color items and palette
- Icon customization and sizing
- Header template customization

### Events & Interactions
📄 **Read:** [references/events-interactions.md](references/events-interactions.md)
- SelectedBrushChanged event handling
- SelectedCommand binding patterns
- "No Color" button functionality
- Tooltip support for color names
- More Colors window interaction
- Header template customization

---

## Quick Start Example

```xaml
<!-- Basic ColorPickerPalette setup -->
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        Title="ColorPickerPalette Sample">
    <Grid>
        <!-- Default dropdown mode with theme and standard colors -->
        <syncfusion:ColorPickerPalette x:Name="colorPickerPalette" 
                                       Color="Red"
                                       GenerateThemeVariants="True"
                                       GenerateStandardVariants="True"
                                       RecentlyUsedPanelVisibility="Visible"
                                       Width="60" 
                                       Height="40" />
    </Grid>
</Window>
```

**C# Code-Behind:**
```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();

        // Handle color selection
        colorPickerPalette.SelectedBrushChanged += ColorPickerPalette_SelectedBrushChanged;
    }

    private void ColorPickerPalette_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e) {
        var newColor = e.NewColor;
        var newBrush = e.NewBrush;
        // Apply color to UI elements
    }
}
```

---

## Common Patterns

- **Color Panel Customization:** Set `ThemePanelVisibility`, `StandardPanelVisibility`, `RecentlyUsedPanelVisibility`, `MoreColorOptionVisibility` to control which panels appear
- **Custom Colors Collection:** Bind `CustomColorsCollection` property to `ObservableCollection<CustomColor>` and set `SetCustomColors="True"`
- **Split Mode with Command:** Set `Mode="Split"` and bind `SelectedCommand` for button behavior with dropdown access
- **Expanded Palette Mode:** Set `Mode="Palette"` to show palette always visible; adjust `BorderWidth`/`BorderHeight` for color item sizing

See [appearance-customization.md](references/appearance-customization.md) and [events-interactions.md](references/events-interactions.md) for complete examples.

---

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Color` | `Color` | Gets/sets the selected color (default: Black) |
| `SelectedBrush` | `Brush` | Gets/sets the selected brush |
| `Mode` | `PickerMode` | Dropdown, Palette, or Split mode |
| `Themes` | `PaletteTheme` | Office, Apex, Metro, etc. |
| `GenerateThemeVariants` | `bool` | Show/hide theme color variants |
| `GenerateStandardVariants` | `bool` | Show/hide standard color variants |
| `CustomColorsCollection` | `ObservableCollection<CustomColor>` | Custom color items |
| `SetCustomColors` | `bool` | Enable custom colors display |
| `BorderWidth` / `BorderHeight` | `double` | Color item dimensions |
| `PopupWidth` / `PopupHeight` | `double` | Palette popup size |
| `RecentlyUsedPanelVisibility` | `Visibility` | Show recently selected colors |
| `ThemePanelVisibility` | `Visibility` | Show theme colors |
| `StandardPanelVisibility` | `Visibility` | Show standard colors |
| `MoreColorOptionVisibility` | `Visibility` | Show "More Colors" button |
| `NoColorVisibility` | `Visibility` | Show transparent/no color button |
| `AutomaticColor` | `Color` | Default reset color |

---

## Common Use Cases

- **Text Color Picker:** Dropdown mode with standard/theme colors + `SelectedBrushChanged` to apply formatting
- **Shape Fill Selector:** Split mode with custom brand colors + `SelectedCommand` binding
- **Theme Color Picker:** Palette mode with `GenerateThemeVariants="True"` to show color variants
- **Color History Panel:** Enable `RecentlyUsedPanelVisibility="Visible"` for quick access to recent colors

---

## Next Steps

1. **Start with basics:** Read [getting-started.md](references/getting-started.md) to set up ColorPickerPalette
2. **Customize colors:** Use [color-management.md](references/color-management.md) to manage color sources
3. **Style the control:** Apply customizations from [appearance-customization.md](references/appearance-customization.md)
4. **Handle interactions:** Implement event handling from [events-interactions.md](references/events-interactions.md)
