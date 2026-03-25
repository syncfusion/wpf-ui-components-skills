---
name: syncfusion-wpf-tab-splitter
description: Implement Syncfusion WPF TabSplitter for VS 2008-style split tab views with top and bottom panel sections. Use this when building split tab layouts, dual-pane views, or side-by-side tabbed views in WPF. Covers SplitterPage, TopPanelItems, BottomPanelItems, TabSplitterItem, and collapsible split panel configuration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF TabSplitter

The **TabSplitter** control provides a Visual Studio 2008-style split view of tabbed groups. It divides content into top and bottom panels within each tab item — ideal for split editor layouts (e.g., XAML + Design views), dual-pane content viewers, and side-by-side tabbed panels.

**Assemblies required:**
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

**XAML namespace:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**C# namespace:**
```csharp
using Syncfusion.Windows.Tools.Controls;
```

---

## When to Use This Skill

| Scenario | Use TabSplitter |
|----------|----------------|
| Split editor with XAML + Design views | ✅ Yes |
| Dual-pane content within a tab | ✅ Yes |
| Multiple tabbed groups each with split panels | ✅ Yes |
| Simple tab control (no split) | ❌ Use standard TabControl |
| Horizontal navigation sidebar | ❌ Use GroupBar or TreeNavigator |

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly and NuGet setup
- Adding TabSplitter via XAML and C#
- Adding TabSplitterItem with content
- Adding SplitterPages to TopPanelItems and BottomPanelItems
- Applying themes via SfSkinManager

### Items and Panel Structure
📄 **Read:** [references/items-and-panels.md](references/items-and-panels.md)
- TabSplitterItem: Header, TopPanelItems, BottomPanelItems
- SplitterPage: Header, content, IsSelectedPage
- Collapsing/expanding the bottom panel (IsCollapsedBottomPanel)
- Setting BottomPanelHeight
- Hiding the header tab when only one child (HideHeaderOnSingleChild)
- Multiple TabSplitterItems in one TabSplitter

### Orientation and Layout
📄 **Read:** [references/orientation-and-layout.md](references/orientation-and-layout.md)
- Horizontal vs Vertical orientation per item
- Swap, collapse, and expand behaviors overview

### Appearance and Themes
📄 **Read:** [references/appearance.md](references/appearance.md)
- MouseOverBackground, MouseOverForeground
- SelectedBackground, SelectedForeground
- SfSkinManager built-in themes
- ThemeStudio custom theme creation

---

## Quick Start Example

**Minimal TabSplitter with one split tab (XAML):**
```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:TabSplitter Name="tabSplitter" Height="300" Width="500">
            <syncfusion:TabSplitterItem Header="Window1.xaml">
                <syncfusion:TabSplitterItem.TopPanelItems>
                    <syncfusion:SplitterPage Header="XAML">
                        <TextBlock Text="XAML View" HorizontalAlignment="Center"
                                   VerticalAlignment="Center" />
                    </syncfusion:SplitterPage>
                </syncfusion:TabSplitterItem.TopPanelItems>
                <syncfusion:TabSplitterItem.BottomPanelItems>
                    <syncfusion:SplitterPage Header="Design">
                        <TextBlock Text="Design View" HorizontalAlignment="Center"
                                   VerticalAlignment="Center" />
                    </syncfusion:SplitterPage>
                </syncfusion:TabSplitterItem.BottomPanelItems>
            </syncfusion:TabSplitterItem>
        </syncfusion:TabSplitter>
    </Grid>
</Window>
```

**C# equivalent:**
```csharp
TabSplitter tabSplitter = new TabSplitter();

SplitterPage topPage = new SplitterPage() { Header = "XAML" };
SplitterPage bottomPage = new SplitterPage() { Header = "Design" };

TabSplitterItem item = new TabSplitterItem() { Header = "Window1.xaml" };
item.TopPanelItems.Add(topPage);
item.BottomPanelItems.Add(bottomPage);

tabSplitter.Items.Add(item);
grid.Children.Add(tabSplitter);
```

---

## Common Patterns

### Multiple tab groups
```xml
<syncfusion:TabSplitter>
    <syncfusion:TabSplitterItem Header="Window1.xaml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
    <syncfusion:TabSplitterItem Header="Window2.xaml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

### Start with bottom panel collapsed
```xml
<syncfusion:TabSplitterItem Header="Window1.xaml" IsCollapsedBottomPanel="True">
    ...
</syncfusion:TabSplitterItem>
```

### Vertical split layout
```xml
<syncfusion:TabSplitterItem Header="Window1.xaml" Orientation="Vertical">
    ...
</syncfusion:TabSplitterItem>
```

---

## Key Properties

| Property | On | Type | Description |
|----------|----|------|-------------|
| `Header` | `TabSplitterItem` | `object` | Tab header label |
| `TopPanelItems` | `TabSplitterItem` | collection | Pages in the top panel |
| `BottomPanelItems` | `TabSplitterItem` | collection | Pages in the bottom panel |
| `Orientation` | `TabSplitterItem` | `Orientation` | `Horizontal` (default) or `Vertical` |
| `IsCollapsedBottomPanel` | `TabSplitterItem` | `bool` | Collapse the bottom panel (default: `false`) |
| `Header` | `SplitterPage` | `object` | Tab label for the page |
| `IsSelectedPage` | `SplitterPage` | `bool` | Set this page as selected |
| `BottomPanelHeight` | `TabSplitter` | `double` | Height of the bottom panel |
| `HideHeaderOnSingleChild` | `TabSplitter` | `bool` | Hide tab header if only one item |
| `SelectedBackground` | `TabSplitter` | `Brush` | Background of selected tab |
| `SelectedForeground` | `TabSplitter` | `Brush` | Foreground of selected tab |
| `MouseOverBackground` | `TabSplitter` | `Brush` | Background on hover |
| `MouseOverForeground` | `TabSplitter` | `Brush` | Foreground on hover |
