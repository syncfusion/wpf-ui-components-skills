# Getting Started

## Installation

Add the NuGet package to your WPF project:

```
Syncfusion.SfProgressBar.WPF
```

Or add the assembly reference manually:
- `Syncfusion.SfProgressBar.WPF`

> **Note:** This is the same assembly used by `SfLinearProgressBar` and `SfCircularProgressBar`. You only need one NuGet package for all three.

## XAML Namespace

Import the Syncfusion WPF schema in your XAML page:

```xaml
xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
```

## Basic Step Bar (XAML)

A 4-step order tracking bar with step 3 active:

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:Syncfusion="http://schemas.syncfusion.com/wpf"
    x:Class="MyApp.MainWindow">
    <Grid x:Name="grid">
        <Syncfusion:SfStepProgressBar SelectedIndex="3">
            <Syncfusion:StepViewItem Content="Ordered" />
            <Syncfusion:StepViewItem Content="Shipped" />
            <Syncfusion:StepViewItem Content="Packed" />
            <Syncfusion:StepViewItem Content="Delivered" />
        </Syncfusion:SfStepProgressBar>
    </Grid>
</Window>
```

- `SelectedIndex="3"` — zero-based; all steps at indices 0, 1, 2 become Active; index 3 uses `SelectedItemStatus` (default `Inactive`)
- Add `StepViewItem` elements as direct children — each `Content` becomes the step label

## Code-Behind Creation

```csharp
using Syncfusion.UI.Xaml.ProgressBar;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SfStepProgressBar stepProgressBar = new SfStepProgressBar();

        stepProgressBar.Items.Add(new StepViewItem { Content = "Ordered" });
        stepProgressBar.Items.Add(new StepViewItem { Content = "Shipped" });
        stepProgressBar.Items.Add(new StepViewItem { Content = "Packed" });
        stepProgressBar.Items.Add(new StepViewItem { Content = "Delivered" });

        stepProgressBar.SelectedIndex = 3;

        grid.Children.Add(stepProgressBar);
    }
}
```

## Applying Themes

Use `SfSkinManager` to apply a built-in theme:

```xaml
<Window
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    xmlns:theme="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
    theme:SfSkinManager.Theme="{theme:SkinManagerExtension ThemeName=FluentLight}">
```

Or in code-behind:

```csharp
SfSkinManager.SetTheme(stepProgressBar, new Theme("FluentLight"));
```

Available themes: `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, and more. See [Apply theme using SfSkinManager](https://help.syncfusion.com/wpf/themes/skin-manager).

## Gotchas

- **`SelectedIndex` is zero-based** — `SelectedIndex="3"` marks the 4th item; steps 0–2 become Active automatically
- **Same assembly as linear/circular** — `Syncfusion.SfProgressBar.WPF` covers all three progress controls
- **`Content` vs `ContentTemplate`** — use `Content="Text"` for simple labels; use `ContentTemplate` for custom XAML inside the step label area
- **Minimum 2 items required** — single-item step bars render but have no connectors
