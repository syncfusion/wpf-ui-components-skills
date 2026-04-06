# Getting Started with ColorPickerPalette

## Table of Contents
- [Installation & Assembly References](#installation--assembly-references)
- [Adding via Designer](#adding-via-designer)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C#](#adding-via-c)
- [Control Structure](#control-structure)
- [Accessing Colors Programmatically](#accessing-colors-programmatically)
- [Applying Themes](#applying-themes)
- [Localization Support](#localization-support)

## Installation & Assembly References

### Required Assemblies
To use the ColorPickerPalette control, add the following assembly reference to your WPF project:

**Assembly:**
- `Syncfusion.Shared.WPF`

### NuGet Package Installation
If you're using NuGet Package Manager, install the Syncfusion WPF package:
```
Install-Package Syncfusion.Shared.WPF
```

### Creating a New WPF Project
1. Create a new WPF project in Visual Studio
2. Add the `Syncfusion.Shared.WPF` assembly reference
3. Import the Syncfusion WPF schema in your XAML

## Adding ColorPickerPalette

You can add ColorPickerPalette using the Visual Studio designer or XAML. Here's the XAML approach:

**1. Add the Syncfusion namespace:**
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
</Window>
```

**2. Add the control:**
```xaml
<StackPanel Margin="20">
    <TextBlock Text="Select a Color:" FontSize="14" Margin="0,0,0,10"/>
    <syncfusion:ColorPickerPalette x:Name="colorPickerPalette" 
                                   Width="60" 
                                   Height="40" />
</StackPanel>
```

**Via Designer:** Use Visual Studio designer to drag ColorPickerPalette from the Syncfusion Controls toolbox.

**Via C#:** Use `ColorPickerPalette colorPickerPalette = new ColorPickerPalette();` and set properties programmatically. See [color-management.md](color-management.md) for code examples.

## Control Structure

The ColorPickerPalette consists of these key visual components:

- **Selected Color**: Displays the currently selected color at the top
- **Dropdown Button**: Opens the color palette popup (in Dropdown/Split mode)
- **Automatic Color**: Shows the default/reset color (clicking resets to this)
- **Theme Colors Panel**: Displays theme-based color variants
- **Standard Colors Panel**: Shows predefined standard colors
- **Custom Colors Panel**: User-defined or application-provided colors
- **Recently Used Panel**: Shows previously selected colors
- **More Colors Button**: Opens extended color selection window
- **No Color Button**: Resets selection to transparent

## Working with Colors

To set initial colors, get selected values, or work with brushes and transparency, see [color-management.md](color-management.md) for comprehensive examples including:
- Setting initial color and getting selected values
- Working with brushes vs. colors
- Setting automatic (default) and transparent colors

## Applying Themes

ColorPickerPalette supports various built-in themes using the SfSkinManager:

### Apply Theme via SfSkinManager
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:ColorPickerPalette x:Name="colorPickerPalette" />
    </Grid>
</Window>
```

```csharp
// Apply theme programmatically
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Apply a specific theme
        SfSkinManager.SetTheme(this, new FluentDarkTheme());
    }
}
```

### Available Themes
- Fluent Light / Fluent Dark
- Material Light / Material Dark
- Office2019 Colorful
- High Contrast

## Localization Support

To provide localized UI for ColorPickerPalette, create resource files for each language:

**Step 1: Create resource file**
- Add a resx file for each language in your project
- Example: `ColorPickerResources.en-US.resx`, `ColorPickerResources.de-DE.resx`

**Step 2: Add translations**
Add these key-value pairs to your resource files:
- `AutomaticColorHeader` = "Automatic Color"
- `StandardColorsHeader` = "Standard Colors"
- `ThemeColorsHeader` = "Theme Colors"
- `CustomColorsHeader` = "Custom Colors"
- `RecentlyUsedHeader` = "Recently Used"
- `MoreColorsButtonText` = "More Colors"
- `NoColorButtonText` = "No Color"

**Step 3: Apply culture**
```csharp
// Set culture before creating controls
CultureInfo.CurrentUICulture = new CultureInfo("de-DE");
```

For additional patterns and troubleshooting, refer to the other reference guides.
