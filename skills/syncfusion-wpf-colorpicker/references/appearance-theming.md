# Appearance and Theming

This reference covers applying themes and customizing the visual appearance of the WPF ColorPicker control.

## Built-in Theme Support

Syncfusion WPF ColorPicker supports multiple built-in themes that automatically adjust the control's colors, fonts, and visual style to match your application design.

### Available Themes

Common built-in themes include:
- **Office2016Colorful** - Colorful Office 2016 theme
- **Office2016White** - Clean white Office 2016 theme
- **Office2016DarkGray** - Dark Office 2016 theme
- **Office2016Black** - Black Office 2016 theme
- **VisualStudio2015** - Visual Studio 2015 theme
- **VisualStudio2015Dark** - Dark Visual Studio 2015 theme
- **MaterialLight** - Material Design light theme
- **MaterialDark** - Material Design dark theme

Refer to [Syncfusion WPF Themes documentation](https://help.syncfusion.com/wpf/themes/skin-manager) for a complete list of available themes.

## Applying Themes with SfSkinManager

The easiest method to apply themes across your entire application (including ColorPicker) is using `SfSkinManager`.

### Step 1: Add SkinManager Reference

Add the Syncfusion.Shared.WPF namespace to your XAML:
```xaml
xmlns:local="clr-namespace:YourNamespace"
```

### Step 2: Apply Theme to Window

```xaml
<Window x:Class="ColorPickerThemingSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:syncfusionskin ="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.VisualStyle="Office2016Colorful"
        Title="ColorPicker Theming Sample" 
        Height="450" 
        Width="800">
    <Grid>
        <syncfusion:ColorPicker Name="colorPicker"/>
    </Grid>
</Window>
```

### Step 3: Apply Theme in C#

```csharp
using Syncfusion.Windows.Shared;

// Apply theme to entire window
SfSkinManager.SetVisualStyle(this, VisualStyles.Office2016Colorful);

// Or apply to specific control
SfSkinManager.SetVisualStyle(colorPicker, VisualStyles.Office2016White);
```

## Theme Examples

Set the `SfSkinManager.VisualStyle` attribute on the Window to automatically apply the theme to all child controls, including ColorPicker. Light themes use light colors, dark themes use dark colors, and Material themes use Material Design styling.

## Creating Custom Themes with ThemeStudio

Syncfusion ThemeStudio allows you to create custom themes tailored to your brand colors and design requirements.

### Step 1: Open ThemeStudio

1. Download Syncfusion ThemeStudio (Web-based tool)
2. Open in your browser or install locally
3. Select "WPF" as the platform
4. Choose "ColorPicker" from the component list

### Step 2: Customize Colors

In ThemeStudio, you can customize:
- **Primary Colors** - Main theme color
- **Background Colors** - Control background
- **Foreground Colors** - Text and control foreground
- **Border Colors** - Control borders
- **Accent Colors** - Highlight and interaction colors

### Step 3: Generate Theme Files

1. Export the custom theme as a XAML ResourceDictionary file
2. Add the file to your project's Resources folder
3. Merge the ResourceDictionary in your App.xaml

### Step 4: Apply Custom Theme

In `App.xaml`:
```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Your custom theme file -->
            <ResourceDictionary Source="Resources/CustomColorPickerTheme.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Theme Reference Documentation

For comprehensive theming guides, refer to:

- **[Syncfusion WPF Themes - SfSkinManager](https://help.syncfusion.com/wpf/themes/skin-manager)** - Complete theme application guide
- **[ThemeStudio Documentation](https://help.syncfusion.com/wpf/themes/theme-studio)** - Custom theme creation
- **[VisualStyles Enumeration](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Shared.VisualStyles.html)** - Complete list of available themes

## Programmatic Theme Selection

Switch themes at runtime using `SfSkinManager.SetVisualStyle()` and a theme selector (ComboBox or buttons). Map theme names to `VisualStyles` enum values to apply the selected theme. All child controls including ColorPicker automatically inherit the applied theme.

## Color Appearance Customization

While themes apply global styling, you can customize specific properties: `Color` (initial selected color), `Brush` (initial gradient), `IsColorPaletteVisible`, `IsAlphaVisible`, `EnableSolidToGradientSwitch` (UI element visibility), and `GradientBrushDisplayMode` (Default or Extended for detail level). These properties affect visual appearance while respecting the applied theme.

## Theme Integration Best Practices

**Apply Theme Globally** - Set the default theme in App.xaml or apply via `SfSkinManager.SetVisualStyle(window, theme)` to ensure all controls use consistent styling.

**Maintain Theme Consistency** - Apply the same theme to the Window or parent container so all child Syncfusion controls automatically inherit it.

**Test Theme Compatibility** - Verify ColorPicker appearance with different themes (Light, Dark, Material).

**High Contrast Support** - Check `System.Windows.SystemParameters.HighContrast` and apply a high-contrast theme (Office2016Black) for accessibility.

## Theme vs CustomColors

| Aspect | Theme | Color Properties |
|--------|-------|------------------|
| Scope | Entire control appearance | Specific colors only |
| Consistency | Maintains design language | May break theme uniformity |
| Change | Affects all themed elements | Individual element changes |
| Best For | App-wide styling | Fine-tuning specific colors |

Use **themes** for consistent application appearance, and individual **color properties** for fine-tuning specific instances when needed.
