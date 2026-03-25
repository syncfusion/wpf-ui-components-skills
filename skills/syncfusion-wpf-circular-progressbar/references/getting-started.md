# Getting Started with SfCircularProgressBar

## Table of Contents
- [Installation](#installation)
- [Assembly References](#assembly-references)
- [XAML Namespace Setup](#xaml-namespace-setup)
- [Adding via XAML](#adding-via-xaml)
- [Adding via Code-Behind](#adding-via-code-behind)
- [Theme Integration](#theme-integration)

---

## Installation

Install the NuGet package in your WPF project:

```
Syncfusion.SfProgressBar.WPF
```

Via Package Manager Console:
```powershell
Install-Package Syncfusion.SfProgressBar.WPF
```

The package can also be added by dragging `SfCircularProgressBar` from the Visual Studio Toolbox — the assembly reference is added automatically.

---

## Assembly References

If adding manually (without NuGet), reference:

```
Syncfusion.SfProgressBar.WPF.dll
```

---

## XAML Namespace Setup

Two options for the XAML namespace:

**Option 1 — Syncfusion schema (recommended):**
```xaml
xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
```

**Option 2 — CLR namespace:**
```xaml
xmlns:Syncfusion="clr-namespace:Syncfusion.UI.Xaml.ProgressBar;assembly=Syncfusion.SfProgressBar.WPF"
```

Both work identically. The schema form is shorter and commonly used in Syncfusion docs.

---

## Adding via XAML

Full minimal window example:

```xaml
<Window
    x:Class="MyApp.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
    Title="MainWindow" Height="300" Width="400">
    <Grid x:Name="grid">
        <Syncfusion:SfCircularProgressBar
            Progress="50"
            ShowProgressValue="True"
            Width="150"
            Height="150" />
    </Grid>
</Window>
```

**Minimum required property:** `Progress` (double, 0–100).

---

## Adding via Code-Behind

```csharp
using Syncfusion.UI.Xaml.ProgressBar;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SfCircularProgressBar circular = new SfCircularProgressBar();
        circular.Progress = 50;
        circular.ShowProgressValue = true;
        circular.Width = 150;
        circular.Height = 150;
        grid.Children.Add(circular);
    }
}
```

---

## Theme Integration

Apply a Syncfusion theme using `SfSkinManager`:

```xaml
<!-- Add reference: Syncfusion.SfSkinManager.WPF -->
<Window
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:syncfusion="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">

    <Syncfusion:SfCircularProgressBar
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=FluentDark}"
        Progress="70"
        Width="150" Height="150" />
</Window>
```

Available themes: `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, and others.

Alternatively, refer to [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio) for creating a custom theme.

---

## Gotchas

- **`Progress` range is 0–100** — values outside this range may produce unexpected visual results.
- **`Width` and `Height` should be equal** for a perfect circle; unequal values produce an ellipse.
- **`ShowProgressValue`** shows a numeric percentage in the center of the ring. Set to `false` if using `ProgressContent` for custom center UI to avoid overlapping content.
- **Designer toolbox** — dragging from toolbox automatically adds only `Syncfusion.SfProgressBar.WPF`; other Syncfusion assemblies must be added manually if needed.
