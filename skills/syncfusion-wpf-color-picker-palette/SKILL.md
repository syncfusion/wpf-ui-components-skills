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

### Pattern 1: Color Panel Customization
**When:** You want specific color panels visible
```xaml
<syncfusion:ColorPickerPalette 
    Themes="Metro"
    GenerateThemeVariants="True" 
    GenerateStandardVariants="True"
    ThemePanelVisibility="Visible"
    StandardPanelVisibility="Visible"
    RecentlyUsedPanelVisibility="Visible"
    MoreColorOptionVisibility="Visible"
    NoColorVisibility="Visible" />
```

### Pattern 2: Custom Colors Collection
**When:** You want users to select from custom predefined colors
```csharp
var customColors = new ObservableCollection<CustomColor>
{
    new CustomColor { Color = Color.FromArgb(255, 17, 235, 248), ColorName = "Aqua" },
    new CustomColor { Color = Color.FromArgb(255, 248, 15, 166), ColorName = "Deep Pink" }
};

colorPickerPalette.CustomColorsCollection = customColors;
colorPickerPalette.SetCustomColors = true;
colorPickerPalette.CustomHeaderText = "Brand Colors";
```

### Pattern 3: Split Mode with Command
**When:** You want button and dropdown behavior with command execution
```xaml
<syncfusion:ColorPickerPalette 
    Mode="Split"
    SelectedCommand="{Binding ApplyColorCommand}"
    Color="{Binding SelectedColor, Mode=TwoWay}" />
```

### Pattern 4: Expanded Palette Mode
**When:** You want the palette always visible without dropdown
```xaml
<syncfusion:ColorPickerPalette 
    Mode="Palette"
    BorderWidth="30"
    BorderHeight="30" />
```

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

**Use Case 1: Text Color Picker**
User needs to apply text formatting colors. Use dropdown mode with standard and theme colors, handle SelectedBrushChanged to apply color to selected text.

**Use Case 2: Shape Fill Selector**
Designer needs to set fill colors for shapes. Use Split mode with custom brand colors, trigger SelectedCommand to apply color to selected shapes.

**Use Case 3: Theme Color Picker**
User needs to pick a color that matches application theme. Use Palette mode with GenerateThemeVariants enabled to show base and variant colors.

**Use Case 4: Color History Panel**
Application tracks recently used colors. Enable RecentlyUsedPanelVisibility and let users quickly reselect previously used colors.

---

## Next Steps

1. **Start with basics:** Read [getting-started.md](references/getting-started.md) to set up ColorPickerPalette
2. **Customize colors:** Use [color-management.md](references/color-management.md) to manage color sources
3. **Style the control:** Apply customizations from [appearance-customization.md](references/appearance-customization.md)
4. **Handle interactions:** Implement event handling from [events-interactions.md](references/events-interactions.md)
