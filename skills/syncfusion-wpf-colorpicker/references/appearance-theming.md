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

### Light Theme (Office 2016 White)
```xaml
<Window shared:SfSkinManager.VisualStyle="Office2016White">
    <!-- ColorPicker will use light colors -->
    <syncfusion:ColorPicker/>
</Window>
```

### Dark Theme (Visual Studio 2015 Dark)
```xaml
<Window shared:SfSkinManager.VisualStyle="VisualStudio2015Dark">
    <!-- ColorPicker will use dark colors -->
    <syncfusion:ColorPicker/>
</Window>
```

### Material Design Theme
```xaml
<Window shared:SfSkinManager.VisualStyle="MaterialLight">
    <!-- ColorPicker will use Material Design colors -->
    <syncfusion:ColorPicker/>
</Window>
```

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

### Switching Themes at Runtime

```csharp
public class ThemeSwitcher
{
    private ColorPicker colorPicker;

    public void ApplyTheme(string themeName)
    {
        VisualStyles selectedStyle = GetThemeStyle(themeName);
        SfSkinManager.SetVisualStyle(colorPicker, selectedStyle);
    }

    private VisualStyles GetThemeStyle(string themeName)
    {
        return themeName switch
        {
            "Light" => VisualStyles.Office2016White,
            "Dark" => VisualStyles.VisualStudio2015Dark,
            "Material" => VisualStyles.MaterialLight,
            "Office" => VisualStyles.Office2016Colorful,
            _ => VisualStyles.Office2016Colorful
        };
    }
}
```

### Theme Selection UI Pattern

```csharp
// XAML: ComboBox to select theme
<ComboBox SelectionChanged="ThemeComboBox_SelectionChanged">
    <ComboBoxItem Content="Office 2016 Colorful"/>
    <ComboBoxItem Content="Office 2016 White"/>
    <ComboBoxItem Content="Visual Studio 2015 Dark"/>
    <ComboBoxItem Content="Material Light"/>
</ComboBox>

// C# Code-behind
private void ThemeComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string selectedTheme = ((ComboBoxItem)e.AddedItems[0]).Content.ToString();
    
    VisualStyles style = selectedTheme switch
    {
        "Office 2016 Colorful" => VisualStyles.Office2016Colorful,
        "Office 2016 White" => VisualStyles.Office2016White,
        "Visual Studio 2015 Dark" => VisualStyles.VisualStudio2015Dark,
        "Material Light" => VisualStyles.MaterialLight,
        _ => VisualStyles.Office2016Colorful
    };
    
    SfSkinManager.SetVisualStyle(colorPicker, style);
}
```

## Color Appearance Customization

### Individual Color Property Customization

While SfSkinManager applies themes, you can also customize specific appearance properties:

```csharp
// Customize ColorPicker appearance
ColorPicker colorPicker = new ColorPicker();

// Set initial color (affects visual appearance)
colorPicker.Color = Colors.Blue;

// Set initial brush
colorPicker.Brush = new LinearGradientBrush(Colors.Blue, Colors.Cyan, 
                                            new Point(0, 0), new Point(1, 0));

// Control visibility of UI elements (affects appearance)
colorPicker.IsColorPaletteVisible = true;
colorPicker.IsAlphaVisible = true;
colorPicker.EnableSolidToGradientSwitch = true;
```

### Gradient Brush Display for Theme Consistency

```csharp
// Extended display mode shows more detail (can use more theme colors)
colorPicker.GradientBrushDisplayMode = 
    Syncfusion.Windows.Tools.GradientBrushDisplayMode.Extended;
```

## Theme Integration Best Practices

### 1. Apply Theme Globally
```xaml
<!-- In App.xaml root Window -->
<Application.Resources>
    <ResourceDictionary>
        <!-- Set default theme -->
        <shared:SfSkinManager.VisualStyle>Office2016Colorful</shared:SfSkinManager.VisualStyle>
    </ResourceDictionary>
</Application.Resources>
```

### 2. Maintain Theme Consistency
Ensure all Syncfusion controls in your application use the same theme:
```csharp
// Apply to multiple controls
SfSkinManager.SetVisualStyle(window, VisualStyles.MaterialDark);
// All child controls automatically inherit the theme
```

### 3. Test Theme Compatibility
```csharp
// Test ColorPicker with different themes
var themes = new[]
{
    VisualStyles.Office2016Colorful,
    VisualStyles.Office2016White,
    VisualStyles.VisualStudio2015Dark,
    VisualStyles.MaterialLight
};

foreach (var theme in themes)
{
    SfSkinManager.SetVisualStyle(colorPicker, theme);
    // Verify appearance is correct
}
```

### 4. High Contrast Support
```csharp
// For accessibility, consider high-contrast themes
if (System.Windows.SystemParameters.HighContrast)
{
    SfSkinManager.SetVisualStyle(colorPicker, VisualStyles.Office2016Black);
}
```

## Theme vs CustomColors

| Aspect | Theme | Color Properties |
|--------|-------|------------------|
| Scope | Entire control appearance | Specific colors only |
| Consistency | Maintains design language | May break theme uniformity |
| Change | Affects all themed elements | Individual element changes |
| Best For | App-wide styling | Fine-tuning specific colors |

Use **themes** for consistent application appearance, and individual **color properties** for fine-tuning specific instances when needed.
