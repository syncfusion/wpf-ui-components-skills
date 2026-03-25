---
name: syncfusion-wpf-theming
description: Implement and customize themes for Syncfusion WPF controls using SfSkinManager. Apply built-in themes (Windows 11, Fluent, Material, Office 2019), create custom themes with Theme Studio, and control visual effects like reveal animations and acrylic backgrounds. Use this skill whenever user needs to theme WPF applications, apply Syncfusion themes, customize colors/fonts, or configure theme-based visual effects and accessibility features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Theming

Apply professional, customizable themes to Syncfusion WPF controls and framework controls using **SfSkinManager**. The theming system provides built-in themes following modern design standards (Windows 11, Fluent, Material 3, Office 2019) and supports complete customization through the **Theme Studio** utility.

## When to Use This Skill

- **Applying built-in themes:** Choose and apply pre-designed themes to your entire application or specific controls
- **Customizing theme colors:** Modify theme colors and fonts programmatically
- **Creating branded themes:** Build custom themes that match your application's branding
- **Configuring visual effects:** Enable/disable reveal animations, acrylic effects, and hover effects
- **Multi-window theming:** Apply themes globally across all windows or to individual controls
- **Accessibility:** Configure keyboard focus visuals and high-contrast themes
- **Theme switching:** Support dynamic theme switching at runtime

## Quick Overview

**Key Components:**
- **SfSkinManager:** Applies themes to Syncfusion and Framework controls (XAML attached property or C# code)
- **Built-in Themes:** 17+ pre-designed themes including Windows 11, Fluent, Material, and Office styles
- **Custom Themes:** After exporting from Theme Studio, register custom themes and integrate into applications

## Documentation and Navigation Guide

### Getting Started with SfSkinManager
📄 **Read:** [references/skin-manager-setup.md](references/skin-manager-setup.md)
- Adding SfSkinManager references and assemblies
- Applying themes to individual controls
- Applying themes globally to entire applications
- Understanding theme inheritance (window → child elements)
- Basic implementation in XAML and C#
- ApplyThemeAsDefaultStyle property usage

### Exploring Built-in Themes
📄 **Read:** [references/built-in-themes.md](references/built-in-themes.md)
- Complete list of 17 available themes
- Light/Dark theme variants
- Theme categories: Windows 11, Fluent, Material 3, Material, Office 2019, High Contrast, System
- Theme assembly and NuGet package references
- Choosing the right theme for your application
- Theme compatibility with Syncfusion and Framework controls

### Creating and Customizing Themes
📄 **Read:** [references/theme-customization.md](references/theme-customization.md)
- After exporting custom theme from Theme Studio, follow the steps in this document
- Customizing colors and fonts programmatically
- Working with exported theme projects
- Generating theme assemblies
- Registering and integrating custom themes
- Multi-framework SDK-style theme projects (.NET 4.6.2, .NET 8.0, .NET 9.0, .NET 10.0)

### Advanced Theme Features
📄 **Read:** [references/theme-features.md](references/theme-features.md)
- Fluent theme reveal animations (hover and pressed effects)
- Configuring HoverEffectMode and PressedEffectMode
- Acrylic background effects for windows
- Keyboard focus visual indicators
- ScrollBar modes (Native, Compact)
- Touch support configuration

## Quick Start Example

### Apply a Built-in Theme in XAML
```xaml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=Windows11Light}">
    <Grid>
        <!-- Your controls here - theme automatically applied -->
    </Grid>
</Window>
```

### Apply a Theme in C#
```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        // Enable theme as default style
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        
        // Apply theme to this window and all child controls
        SfSkinManager.SetTheme(this, new Theme("Windows11Light"));
        
        InitializeComponent();
    }
}
```

### Apply Theme Globally to Application
```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        SfSkinManager.ApplyThemeAsDefaultStyle = true;
        SfSkinManager.ApplicationTheme = new Theme("Fluent
Dark");
        
        base.OnStartup(e);
    }
}
```

## Common Patterns

### Pattern 1: Theme Switching at Runtime
Allow users to switch themes dynamically in your application:

```csharp
// Example: Switch theme when user selects from dropdown
private void OnThemeSelectionChanged(string themeName)
{
    SfSkinManager.SetTheme(this, new Theme(themeName));
}
```

### Pattern 2: Combine Custom Theme with Built-in Theme
Register a custom theme derived from a built-in theme:

```csharp
string customThemeName = "MyCustomTheme";
string baseThemeName = "Windows11Light";

// Register custom theme (assuming custom theme assembly is referenced)
SkinHelper customThemeInstance = new MyCustomThemeSkinHelper();
SfSkinManager.RegisterTheme(customThemeName, customThemeInstance);

// Apply the custom theme
SfSkinManager.SetTheme(this, new Theme($"{customThemeName};{baseThemeName}"));
```

### Pattern 3: Customize Theme Colors Programmatically
Modify specific theme colors at runtime:

```csharp
// Example: Customize Windows 11 Light theme colors
var themeSettings = new Windows11LightThemeSettings();
themeSettings.Palette = new Windows11Palette 
{ 
    PrimaryColor = Colors.Blue,
    SecondaryColor = Colors.LightBlue
};

SfSkinManager.RegisterThemeSettings("Windows11Light", themeSettings);
SfSkinManager.SetTheme(this, new Theme("Windows11Light"));
```

## Theme Selection Guide

**Choose by Application Style:**
- **Modern/Windows 11 Native:** Windows11Light, Windows11Dark
- **Microsoft Fluent Design:** FluentLight, FluentDark
- **Material Design:** Material3Light, Material3Dark, MaterialLight, MaterialDark
- **Office-like:** Office2019Colorful, Office2019Black, Office2019White, Office2019DarkGray
- **High Contrast/Accessibility:** Office2019HighContrast, Office2019HighContrastWhite
- **Follow OS:** SystemTheme

**Light vs. Dark:**
- Use Light themes for bright environments or daytime use
- Use Dark themes for low-light environments or as a user preference
- Pair with SystemTheme to automatically match OS settings

## Key Properties

| Property | Purpose | Type | Default |
|----------|---------|------|---------|
| `SfSkinManager.Theme` | Applied theme name (XAML attached property) | string | None |
| `SfSkinManager.ApplicationTheme` | Global theme for entire application | Theme object | None |
| `HoverEffectMode` | Visual effect on hover (Fluent theme) | HoverEffect enum | BackgroundAndBorder |
| `PressedEffectMode` | Visual effect on press (Fluent theme) | PressedEffect enum | Reveal |
| `ShowAcrylicBackground` | Enable acrylic blur effect (Fluent theme) | bool | false |
| `ApplyThemeAsDefaultStyle` | Apply theme as default style resource | bool | false |

## Related Skills

- Custom theme creation and export (Theme Studio guide)
- Keyboard accessibility and focus indicators
- Responsive design for themed applications
