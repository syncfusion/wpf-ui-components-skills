# Appearance and Themes

The HierarchyNavigator supports visual customization through `HierarchicalDataTemplate` for custom item layouts, built-in Syncfusion themes via `SfSkinManager`, and ThemeStudio for creating custom themes.

## Table of Contents
- [Custom Item Templates](#custom-item-templates)
- [Inline vs Resource Templates](#inline-vs-resource-templates)
- [Expression Blend Customization](#expression-blend-customization)
- [Applying Syncfusion Themes](#applying-syncfusion-themes)
- [Available Themes](#available-themes)
- [ThemeStudio Custom Themes](#themestudio-custom-themes)

---

## Custom Item Templates

Use `HierarchicalDataTemplate` as the `ItemTemplate` to control how each breadcrumb item renders. This template defines both the **item display** and the **source of child items**.

**Basic icon + text layout:**
```xml
<Window.Resources>
    <HierarchicalDataTemplate x:Key="NavItemTemplate"
                              ItemsSource="{Binding Children}">
        <StackPanel Orientation="Horizontal">
            <Image Source="{Binding IconPath}"
                   Width="16" Height="16"
                   Margin="0,0,4,0" />
            <TextBlock Text="{Binding Name}"
                       VerticalAlignment="Center" />
        </StackPanel>
    </HierarchicalDataTemplate>
</Window.Resources>

<syncfusion:HierarchyNavigator
    ItemsSource="{Binding Items}"
    ItemTemplate="{StaticResource NavItemTemplate}" />
```

**Bold current level:**
```xml
<HierarchicalDataTemplate x:Key="NavItemTemplate"
                          ItemsSource="{Binding Children}">
    <TextBlock Text="{Binding Name}"
               FontWeight="{Binding IsCurrentLevel,
                   Converter={StaticResource BoolToFontWeightConverter}}" />
</HierarchicalDataTemplate>
```

**Key rule:** Always use `HierarchicalDataTemplate` (not `DataTemplate`) — the `ItemsSource` binding in the template is what allows the dropdown popup to show child items at each level.

---

## Inline vs Resource Templates

**Inline template** (single-use, defined directly on the control):
```xml
<syncfusion:HierarchyNavigator ItemsSource="{Binding Items}">
    <syncfusion:HierarchyNavigator.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding Children}">
            <TextBlock Text="{Binding Name}" Foreground="DarkBlue" />
        </HierarchicalDataTemplate>
    </syncfusion:HierarchyNavigator.ItemTemplate>
</syncfusion:HierarchyNavigator>
```

**Resource template** (reusable, defined in `Window.Resources` or `App.Resources`):
```xml
<Window.Resources>
    <HierarchicalDataTemplate x:Key="SharedNavTemplate"
                              ItemsSource="{Binding SubItems}">
        <Border Padding="3,1" CornerRadius="2">
            <TextBlock Text="{Binding DisplayName}" />
        </Border>
    </HierarchicalDataTemplate>
</Window.Resources>

<!-- Reference from multiple controls -->
<syncfusion:HierarchyNavigator
    ItemTemplate="{StaticResource SharedNavTemplate}" />
```

**When to use resource templates:**
- Multiple `HierarchyNavigator` instances sharing the same layout
- Complex templates that benefit from being defined once
- Templates referenced by other controls (TreeView, etc.)

---

## Expression Blend Customization

For pixel-level control style customization:

1. Open the project in **Microsoft Expression Blend** (or use VS WPF Designer's "Edit Template" option)
2. Right-click the `HierarchyNavigator` → **Edit Template** → **Edit a Copy**
3. Blend generates a full `ControlTemplate` copy in your resources
4. Modify brushes, sizes, animations in the generated template

**Key editable parts in the default template:**
- `PART_EditableTextBox` — the edit mode text input
- `PART_ItemsPresenter` — breadcrumb items display area
- Separator elements between items
- Back-arrow and refresh button visual states

> **Note:** After editing the template, apply theme changes carefully — `SfSkinManager` overrides will not apply to a custom `ControlTemplate`.

---

## Applying Syncfusion Themes

Use `SfSkinManager` to apply a consistent Syncfusion theme to the `HierarchyNavigator`.

**Prerequisites — add NuGet package:**
```
Syncfusion.SfSkinManager.WPF
```

**Apply theme to the window (affects all Syncfusion controls):**
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

**Apply theme to a single control:**
```xml
<syncfusion:HierarchyNavigator
    syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=FluentDark}"
    ItemsSource="{Binding Items}" />
```

**XAML namespace for SfSkinManager:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Available Themes

Syncfusion WPF themes available for `HierarchyNavigator`:

| Theme Name | Style |
|------------|-------|
| `FluentLight` | Microsoft Fluent Design (light) |
| `FluentDark` | Microsoft Fluent Design (dark) |
| `Material3Light` | Material Design 3 (light) |
| `Material3Dark` | Material Design 3 (dark) |
| `Office2019White` | Office 2019 white |
| `Office2019Black` | Office 2019 dark |
| `Office2019Colorful` | Office 2019 colorful |
| `SystemTheme` | Matches Windows system accent color |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |

**Apply theme in code:**
```csharp
// Change theme at runtime
SfSkinManager.SetTheme(this, new Theme("Material3Dark"));
```

**Apply theme in XAML (App.xaml for global):**
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <syncfusion:SkinManagerExtension ThemeName="FluentLight" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

---

## ThemeStudio Custom Themes

Use **Syncfusion Theme Studio** to generate a custom theme with your brand colors.

**Steps:**
1. Go to [Syncfusion Theme Studio](https://ej2.syncfusion.com/themestudio/)
2. Select the WPF platform
3. Customize primary colors, fonts, border radius, etc.
4. Export the generated theme package
5. Add the exported resource dictionary to your project

**Integrate exported theme:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- Generated custom theme -->
            <ResourceDictionary Source="Themes/CustomTheme.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

**Or apply via SfSkinManager with a custom theme name:**
```csharp
SfSkinManager.RegisterTheme("MyBrandTheme", typeof(MyBrandTheme));
SfSkinManager.SetTheme(this, new Theme("MyBrandTheme"));
```

---

## Related References

- [Getting Started](getting-started.md) — Initial assembly setup and theme import
- [Populating Data](populating-data.md) — HierarchicalDataTemplate with data binding
- [Navigation Features](navigation-features.md) — Control configuration
