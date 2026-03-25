# Getting Started with TabSplitter

This guide covers assembly setup, adding the control, and building a basic split tab layout.

## Table of Contents
- [Assembly Setup](#assembly-setup)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C#](#adding-via-c)
- [Adding a TabSplitterItem with SplitterPages](#adding-a-tabsplitteritem-with-splitterpages)
- [Adding Content to SplitterPages](#adding-content-to-splitterpages)
- [Applying Themes](#applying-themes)

---

## Assembly Setup

**NuGet package (recommended):**
```
Syncfusion.Tools.WPF
```

**Or add assembly references manually:**
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

See [control dependencies](https://help.syncfusion.com/wpf/control-dependencies#tabsplitter) for the full list. See [NuGet packages guide](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages) for installation steps.

---

## Adding via XAML

1. Create a new WPF application in Visual Studio.
2. Add assembly references (`Syncfusion.Tools.WPF`, `Syncfusion.Shared.WPF`).
3. Declare the Syncfusion XAML namespace and add the control:

```xml
<Window x:Class="TabSplitter.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">

    <Grid Name="grid">
        <syncfusion:TabSplitter Name="tabSplitter" Height="280" Width="400" />
    </Grid>
</Window>
```

---

## Adding via C#

1. Add assembly references.
2. Import the namespace:

```csharp
using Syncfusion.Windows.Tools.Controls;
```

3. Create and add the control:

```csharp
// Create TabSplitter instance
TabSplitter tabSplitter = new TabSplitter();
tabSplitter.Height = 280;
tabSplitter.Width = 400;

// Add to the layout
grid.Children.Add(tabSplitter);
```

---

## Adding a TabSplitterItem with SplitterPages

Each `TabSplitterItem` represents one tab group. It holds a `TopPanelItems` collection and a `BottomPanelItems` collection, each containing `SplitterPage` instances.

**XAML:**
```xml
<syncfusion:TabSplitter Name="tabSplitter">
    <syncfusion:TabSplitterItem Header="Window1.xaml">
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
// Create splitter pages
SplitterPage splitterPage = new SplitterPage() { Header = "XAML" };
SplitterPage splitterPage1 = new SplitterPage() { Header = "Design" };

// Create tab splitter item
TabSplitterItem tabSplitterItem = new TabSplitterItem() { Header = "Window1.xaml" };

// Add pages to the item
tabSplitterItem.TopPanelItems.Add(splitterPage);
tabSplitterItem.BottomPanelItems.Add(splitterPage1);

// Add item to the control
tabSplitter.Items.Add(tabSplitterItem);
```

---

## Adding Content to SplitterPages

Place any WPF content inside a `SplitterPage`:

**XAML:**
```xml
<syncfusion:TabSplitterItem Header="Window1.xaml">
    <syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:SplitterPage Header="XAML">
            <Label Content="Xaml"
                   HorizontalContentAlignment="Center"
                   VerticalContentAlignment="Center" />
        </syncfusion:SplitterPage>
    </syncfusion:TabSplitterItem.TopPanelItems>
    <syncfusion:TabSplitterItem.BottomPanelItems>
        <syncfusion:SplitterPage Header="Design">
            <Label Content="Design"
                   HorizontalContentAlignment="Center"
                   VerticalContentAlignment="Center" />
        </syncfusion:SplitterPage>
    </syncfusion:TabSplitterItem.BottomPanelItems>
</syncfusion:TabSplitterItem>
```

**C#:**
```csharp
Label label = new Label()
{
    Content = "XAML",
    HorizontalContentAlignment = HorizontalAlignment.Center,
    VerticalContentAlignment = VerticalAlignment.Center
};

Label label1 = new Label()
{
    Content = "Design",
    HorizontalContentAlignment = HorizontalAlignment.Center,
    VerticalContentAlignment = VerticalAlignment.Center
};

splitterPage.Content = label;
splitterPage1.Content = label1;
```

**Content can be any UIElement** — UserControls, Grids, custom views, etc. The `SplitterPage` is a standard WPF `ContentControl`.

---

## Applying Themes

TabSplitter supports Syncfusion built-in themes via `SfSkinManager`.

**Apply at window level (affects all Syncfusion controls):**
```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("FluentLight"));
InitializeComponent();
```

**Or reference the theme docs:**
- [Apply theme using SfSkinManager](https://help.syncfusion.com/wpf/themes/skin-manager)
- [Create a custom theme using ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)

For detailed theme options, see [appearance.md](appearance.md).

---

## Related References

- [Items and Panels](items-and-panels.md) — Panel structure, collapse, selected page
- [Orientation and Layout](orientation-and-layout.md) — Horizontal/Vertical split
- [Appearance](appearance.md) — Colors and themes
