# Getting Started with SfLinearProgressBar

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

The control can also be added by dragging `SfLinearProgressBar` from the Visual Studio Toolbox — the assembly reference is added automatically.

---

## Assembly References

If adding manually (without NuGet), reference:

```
Syncfusion.SfProgressBar.WPF.dll
```

---

## XAML Namespace Setup

**Option 1 — Syncfusion schema (recommended):**
```xaml
xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
```

**Option 2 — CLR namespace:**
```xaml
xmlns:Syncfusion="clr-namespace:Syncfusion.UI.Xaml.ProgressBar;assembly=Syncfusion.SfProgressBar.WPF"
```

---

## Adding via XAML

```xaml
<Window
    x:Class="MyApp.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
    Title="MainWindow" Height="300" Width="600">
    <Grid x:Name="grid">
        <Syncfusion:SfLinearProgressBar
            Progress="70"
            Width="500"
            Height="20" />
    </Grid>
</Window>
```

**Minimum required property:** `Progress` (double, 0–100).

Recommended sizing: `Height="4"` for a thin modern bar; `Height="20"` for a classic thick bar.

---

## Adding via Code-Behind

```csharp
using Syncfusion.UI.Xaml.ProgressBar;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SfLinearProgressBar linear = new SfLinearProgressBar();
        linear.Progress = 70;
        linear.Width = 500;
        linear.Height = 20;
        grid.Children.Add(linear);
    }
}
```

---

## Theme Integration

Apply a Syncfusion theme using `SfSkinManager`:

```xaml
<Window
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:skinManager="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">

    <Syncfusion:SfLinearProgressBar
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=FluentDark}"
        Progress="70"
        Width="500" Height="20" />
</Window>
```

Available themes: `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, and others.

---

## Gotchas

- **`Progress` range is 0–100** — values outside this range may cause unexpected rendering.
- **`Height` controls bar thickness** — set explicitly; the default may be very thin or system-dependent.
- **Same assembly as `SfCircularProgressBar`** — both controls share `Syncfusion.SfProgressBar.WPF`; no additional package needed if circular progressbar is already referenced.
