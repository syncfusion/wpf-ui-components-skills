# Exploring and Selecting Built-in Themes

## Table of Contents
- [Overview](#overview)
- [Complete Themes List](#complete-themes-list)
- [Theme Categories](#theme-categories)
- [NuGet Package References](#nuget-package-references)
- [Choosing the Right Theme](#choosing-the-right-theme)
- [Theme Characteristics](#theme-characteristics)

## Overview

Syncfusion WPF provides 17 professionally designed built-in themes across multiple design systems. Each theme is available in Light and Dark variants (where applicable) and can be applied using the theme name as a string.

All built-in themes are available via NuGet packages and assemblies, making it easy to reference and apply them in your applications.

## Complete Themes List

### Windows 11 Themes (2 themes)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `Windows11Light` | `Syncfusion.Themes.Windows11Light.Wpf.dll` | `Syncfusion.Themes.Windows11Light.WPF` | Modern Windows 11 design |
| `Windows11Dark` | `Syncfusion.Themes.Windows11Dark.Wpf.dll` | `Syncfusion.Themes.Windows11Dark.WPF` | Modern Windows 11 design |

**Characteristics:**
- Native Windows 11 appearance
- Clean, minimal design language
- Strong contrast for accessibility
- Recommended for applications targeting Windows 11

### Fluent Themes (2 themes)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `FluentLight` | `Syncfusion.Themes.FluentLight.Wpf.dll` | `Syncfusion.Themes.FluentLight.WPF` | Microsoft Fluent Design |
| `FluentDark` | `Syncfusion.Themes.FluentDark.Wpf.dll` | `Syncfusion.Themes.FluentDark.WPF` | Microsoft Fluent Design |

**Characteristics:**
- Reveal animations on hover and press
- Acrylic background effects
- Rounded corners and soft shadows
- Modern, polished appearance

### Material 3 Themes (2 themes)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `Material3Light` | `Syncfusion.Themes.Material3Light.Wpf.dll` | `Syncfusion.Themes.Material3Light.WPF` | Google Material Design 3 |
| `Material3Dark` | `Syncfusion.Themes.Material3Dark.Wpf.dll` | `Syncfusion.Themes.Material3Dark.WPF` | Google Material Design 3 |

**Characteristics:**
- Latest Material Design 3 guidelines
- Vibrant, expressive colors
- Dynamic color based on branding
- Great for cross-platform consistency

### Material Themes (4 themes)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `MaterialLight` | `Syncfusion.Themes.MaterialLight.Wpf.dll` | `Syncfusion.Themes.MaterialLight.WPF` | Google Material Design |
| `MaterialDark` | `Syncfusion.Themes.MaterialDark.Wpf.dll` | `Syncfusion.Themes.MaterialDark.WPF` | Google Material Design |
| `MaterialLightBlue` | `Syncfusion.Themes.MaterialLightBlue.Wpf.dll` | `Syncfusion.Themes.MaterialLightBlue.WPF` | Google Material Design (Blue variant) |
| `MaterialDarkBlue` | `Syncfusion.Themes.MaterialDarkBlue.Wpf.dll` | `Syncfusion.Themes.MaterialDarkBlue.WPF` | Google Material Design (Blue variant) |

**Characteristics:**
- Material Design System (earlier version)
- Flat design with subtle shadows
- 4 color variants for flexibility
- Great for modern, clean UIs

### Office 2019 Themes (5 themes)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `Office2019Colorful` | `Syncfusion.Themes.Office2019Colorful.Wpf.dll` | `Syncfusion.Themes.Office2019Colorful.WPF` | Microsoft Office 2019 style |
| `Office2019Black` | `Syncfusion.Themes.Office2019Black.Wpf.dll` | `Syncfusion.Themes.Office2019Black.WPF` | Microsoft Office 2019 style |
| `Office2019White` | `Syncfusion.Themes.Office2019White.Wpf.dll` | `Syncfusion.Themes.Office2019White.WPF` | Microsoft Office 2019 style |
| `Office2019DarkGray` | `Syncfusion.Themes.Office2019DarkGray.Wpf.dll` | `Syncfusion.Themes.Office2019DarkGray.WPF` | Microsoft Office 2019 style |
| `Office2019HighContrast` | `Syncfusion.Themes.Office2019HighContrast.Wpf.dll` | `Syncfusion.Themes.Office2019HighContrast.WPF` | Microsoft Office 2019 High Contrast |

**Characteristics:**
- Classic Office application look
- Professional, business appearance
- High visibility for accessibility
- Familiar to enterprise users

### High Contrast Themes (1 theme)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `Office2019HighContrastWhite` | `Syncfusion.Themes.Office2019HighContrastWhite.Wpf.dll` | `Syncfusion.Themes.Office2019HighContrastWhite.WPF` | Microsoft Office High Contrast |

**Characteristics:**
- White background with high contrast
- Maximum accessibility for low-vision users
- WCAG AA/AAA compliance
- Critical controls highly visible

### System Theme (1 theme)

| Theme Name | Assembly | NuGet Package | Design System |
|-----------|----------|---------------|----------------|
| `SystemTheme` | `Syncfusion.Themes.SystemTheme.Wpf.dll` | `Syncfusion.Themes.SystemTheme.WPF` | Windows OS settings |

**Characteristics:**
- Automatically follows OS theme (Light/Dark)
- Respects Windows accessibility settings
- Dynamic theme switching with OS
- Best for applications that should match OS preferences

## Theme Categories

### By Visual Appearance

**Modern & Contemporary:**
- Windows11Light, Windows11Dark
- FluentLight, FluentDark
- Material3Light, Material3Dark

**Traditional & Professional:**
- Office2019Colorful, Office2019Black, Office2019White, Office2019DarkGray
- MaterialLight, MaterialDark, MaterialLightBlue, MaterialDarkBlue

**Accessibility-Focused:**
- Office2019HighContrast, Office2019HighContrastWhite
- SystemTheme (respects OS accessibility settings)

### By Light/Dark Preference

**Light Themes (for bright environments):**
- Windows11Light
- FluentLight
- Material3Light
- MaterialLight
- MaterialLightBlue
- Office2019Colorful
- Office2019White
- Office2019HighContrastWhite

**Dark Themes (for low-light environments):**
- Windows11Dark
- FluentDark
- Material3Dark
- MaterialDark
- MaterialDarkBlue
- Office2019Black
- Office2019DarkGray
- Office2019HighContrast

**Dynamic (Follows OS):**
- SystemTheme

## NuGet Package References

### Quick Installation Commands

Install a single theme:
```powershell
Install-Package Syncfusion.Themes.Windows11Light.WPF
Install-Package Syncfusion.Themes.FluentDark.WPF
Install-Package Syncfusion.Themes.Material3Light.WPF
```

Install multiple themes (for theme switching):
```powershell
Install-Package Syncfusion.Themes.Windows11Light.WPF
Install-Package Syncfusion.Themes.Windows11Dark.WPF
Install-Package Syncfusion.Themes.FluentLight.WPF
Install-Package Syncfusion.Themes.FluentDark.WPF
Install-Package Syncfusion.Themes.Material3Light.WPF
Install-Package Syncfusion.Themes.Material3Dark.WPF
```

## Choosing the Right Theme

### Decision Tree

```
Do you need the application to follow OS settings?
├─ YES → Use: SystemTheme
└─ NO  → Continue...

What is your design preference?
├─ Modern/Contemporary → Windows 11 or Fluent or Material 3
├─ Traditional/Professional → Office 2019 or Material
├─ Maximum Accessibility → Office 2019 HighContrast variants
└─ Other → Continue...

What environment/lighting?
├─ Bright environments → Use Light variant
├─ Low-light environments → Use Dark variant
└─ User choice → Support both (Light + Dark variants)
```

### Common Scenarios

**Enterprise Application:**
- Primary: `Office2019Colorful` or `Office2019Black`
- Reason: Familiar to business users, professional appearance

**Modern SaaS/Web-like Application:**
- Primary: `Windows11Light` + `Windows11Dark` or `FluentLight` + `FluentDark`
- Reason: Contemporary design, smooth animations

**Cross-Platform/Consistent Branding:**
- Primary: `Material3Light` + `Material3Dark`
- Reason: Material Design 3 is platform-agnostic and widely recognized

**Accessibility-Critical Application:**
- Primary: `Office2019HighContrast` + `Office2019HighContrastWhite`
- Reason: Maximum contrast and visibility for low-vision users

**OS-Integrated Application:**
- Primary: `SystemTheme`
- Reason: Automatically matches user's Windows theme preference

**Dark Mode Popular App:**
- Primary: `FluentDark`, `Windows11Dark`, or `Material3Dark`
- Reason: Built for low-light environments, popular user preference

## Theme Characteristics

### Visual Features by Theme

| Theme | Animations | Rounded Corners | Shadows | Acrylic Effect | Accessibility |
|-------|-----------|-----------------|---------|----------------|---------------|
| Windows 11 | Smooth | High | Subtle | No | Good |
| Fluent | Reveal effect | High | Soft | Yes | Good |
| Material 3 | Subtle | Medium | Medium | No | Excellent |
| Material | Subtle | Medium | Medium | No | Good |
| Office 2019 | Minimal | Low | Minimal | No | Good |
| HighContrast | None | None | None | No | Excellent |
| System | Varies | Varies | Varies | No | Follows OS |

### Color Schemes

**Warm Colors:**
- Office2019HighContrast (Blue-based)
- Material3 variants

**Cool Colors:**
- Windows 11 themes
- Fluent themes

**Neutral/Professional:**
- Office 2019 themes
- Material themes

## Using Themes in Code

### Apply Theme by Name (String)

```csharp
// Simple theme application
SfSkinManager.SetTheme(this, new Theme("Windows11Light"));
SfSkinManager.SetTheme(this, new Theme("FluentDark"));
SfSkinManager.SetTheme(this, new Theme("Material3Light"));
```

### Create Dropdown for Theme Selection

```csharp
// Create dropdown with all built-in themes
private List<string> GetAvailableThemes()
{
    return new List<string>
    {
        "Windows11Light",
        "Windows11Dark",
        "FluentLight",
        "FluentDark",
        "Material3Light",
        "Material3Dark",
        "MaterialLight",
        "MaterialDark",
        "MaterialLightBlue",
        "MaterialDarkBlue",
        "Office2019Colorful",
        "Office2019Black",
        "Office2019White",
        "Office2019DarkGray",
        "Office2019HighContrast",
        "Office2019HighContrastWhite",
        "SystemTheme"
    };
}

// Handle theme selection
private void OnThemeSelectionChanged(string themeName)
{
    SfSkinManager.SetTheme(this, new Theme(themeName));
}
```
