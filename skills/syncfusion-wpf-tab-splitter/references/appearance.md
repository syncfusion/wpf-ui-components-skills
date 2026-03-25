# Appearance and Themes

The TabSplitter provides brush properties for selected and hover states, as well as full Syncfusion theme support via `SfSkinManager` and ThemeStudio.

---

## Brush Customization

Set these properties directly on the `TabSplitter` control to customize tab item colors:

| Property | State | Description |
|----------|-------|-------------|
| `SelectedBackground` | Selected tab | Background brush of the active tab header |
| `SelectedForeground` | Selected tab | Foreground (text) brush of the active tab header |
| `MouseOverBackground` | Hovered tab | Background brush when the mouse hovers over a tab |
| `MouseOverForeground` | Hovered tab | Foreground brush when the mouse hovers over a tab |

**XAML:**
```xml
<syncfusion:TabSplitter
    Name="tabSplitter"
    SelectedBackground="Red"
    SelectedForeground="YellowGreen"
    MouseOverBackground="Green"
    MouseOverForeground="Yellow">

    <syncfusion:TabSplitterItem Header="Window1.xml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

**C#:**
```csharp
tabSplitter.SelectedBackground = Brushes.Red;
tabSplitter.SelectedForeground = Brushes.YellowGreen;
tabSplitter.MouseOverBackground = Brushes.Green;
tabSplitter.MouseOverForeground = Brushes.Yellow;
```

**Using resource brushes (recommended for consistent theming):**
```xml
<Window.Resources>
    <SolidColorBrush x:Key="BrandBlue" Color="#0078D4" />
    <SolidColorBrush x:Key="BrandWhite" Color="White" />
</Window.Resources>

<syncfusion:TabSplitter
    SelectedBackground="{StaticResource BrandBlue}"
    SelectedForeground="{StaticResource BrandWhite}"
    MouseOverBackground="#E0E0E0"
    MouseOverForeground="#000000">
    ...
</syncfusion:TabSplitter>
```

---

## Applying Syncfusion Themes via SfSkinManager

Use `SfSkinManager` to apply a consistent Syncfusion theme to the entire window or a specific control.

**Prerequisites — NuGet package:**
```
Syncfusion.SfSkinManager.WPF
```

**Apply theme at window level (recommended):**
```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        SfSkinManager.SetTheme(this, new Theme("FluentLight"));
        InitializeComponent();
    }
}
```

**Apply to a specific control:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"

<syncfusion:TabSplitter
    syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=FluentDark}">
    ...
</syncfusion:TabSplitter>
```

---

## Available Themes

| Theme Name | Style |
|------------|-------|
| `FluentLight` | Microsoft Fluent Design (light) |
| `FluentDark` | Microsoft Fluent Design (dark) |
| `Material3Light` | Material Design 3 (light) |
| `Material3Dark` | Material Design 3 (dark) |
| `Office2019White` | Office 2019 white |
| `Office2019Black` | Office 2019 dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `SystemTheme` | Matches Windows system accent color |

**Switch theme at runtime:**
```csharp
SfSkinManager.SetTheme(this, new Theme("Material3Dark"));
```

---

## ThemeStudio Custom Themes

Generate a fully custom theme with your brand colors using Syncfusion Theme Studio:

1. Visit [Syncfusion Theme Studio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)
2. Customize primary color, fonts, border radius, and other tokens
3. Export and add the generated XAML resource dictionary to your project

**Integrate the exported theme:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Themes/CustomBrandTheme.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> **Note:** When using a custom ControlTemplate (via Expression Blend), `SfSkinManager` theme overrides will not apply to the customized template parts.

---

## Related References

- [Getting Started](getting-started.md) — SfSkinManager basic setup
- [Items and Panels](items-and-panels.md) — HideHeaderOnSingleChild visual effect
- [Orientation and Layout](orientation-and-layout.md) — Layout configuration
