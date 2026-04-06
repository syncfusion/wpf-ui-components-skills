# Getting Started with WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Installation and Setup](#installation-and-setup)
- [Assembly References](#assembly-references)
- [Namespace Imports](#namespace-imports)
- [Creating Your First Tabbed Window](#creating-your-first-tabbed-window)
- [Window Type Modes](#window-type-modes)
- [Basic Tab Creation](#basic-tab-creation)
- [Key Properties Overview](#key-properties-overview)
- [Common Setup Patterns](#common-setup-patterns)

## Overview

The Syncfusion WPF Tabbed Window combines `SfChromelessWindow` with `SfTabControl` for document-based applications. Learn installation, setup, tab creation, and core features.

## Installation and Setup

Install required NuGet packages:

```powershell
Install-Package Syncfusion.SfChromelessWindow.WPF
Install-Package Syncfusion.Shared.WPF
```

Or use Visual Studio's **Manage NuGet Packages** UI.

## Assembly References

Add these required assemblies to your project:
- `Syncfusion.SfChromelessWindow.WPF` - Chromeless window functionality
- `Syncfusion.Shared.WPF` - Shared utilities and controls

## Namespace Imports

Add Syncfusion namespace to XAML:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### C# Namespace

Import required namespaces:

```csharp
using Syncfusion.Windows.Controls;
using System.Windows.Controls;
```

## Creating Your First Tabbed Window

Inherit from `SfChromelessWindow` and add `SfTabControl`:

```xml
<syncfusion:SfChromelessWindow 
    x:Class="MyApp.MainWindow"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    WindowType="Tabbed" Title="App" Height="450" Width="800">
    <syncfusion:SfTabControl>
        <syncfusion:SfTabItem Header="Tab 1" Content="Content 1"/>
        <syncfusion:SfTabItem Header="Tab 2" Content="Content 2"/>
    </syncfusion:SfTabControl>
</syncfusion:SfChromelessWindow>
```

Code-behind:

```csharp
public partial class MainWindow : SfChromelessWindow
{
    public MainWindow() => InitializeComponent();
}
```

## Window Type Modes

`WindowType` controls tab integration:

- **Tabbed Mode** (`WindowType="Tabbed"`): Tabs in window chrome, browser-like layout, document-centric apps
- **Normal Mode** (`WindowType="Normal"`): Tabs in content area with flexible layout for complex UIs

## Basic Tab Creation

Create tabs in XAML:

```xml
<syncfusion:SfTabControl>
    <syncfusion:SfTabItem Header="Tab 1" Content="Content 1"/>
    <syncfusion:SfTabItem Header="Tab 2" Content="Content 2"/>
</syncfusion:SfTabControl>
```

Or add programmatically:

```csharp
var tab = new SfTabItem { Header = "New", Content = "Content" };
MainTabControl.Items.Add(tab);
```

For icons in headers, wrap header content in StackPanel with Image and TextBlock.

## Key Properties Overview

**Window:** `WindowType` (Tabbed/Normal), `Title`, `Height`, `Width`

**Tab Control:** `AllowDragDrop`, `EnableNewTabButton`, `SelectedItem`, `SelectedIndex`, `ItemsSource` (for MVVM)

**Tab Item:** `Header`, `Content`, `CloseButtonVisibility`, `IsSelected`

## Next Steps

Now that you have a basic tabbed window running:

1. **Add tab management features** - Read `tab-management.md` to implement close buttons, new tab button, and keyboard shortcuts
2. **Implement MVVM pattern** - Read `data-binding.md` for data-driven tab scenarios
3. **Enable floating windows** - Read `merge-tabs.md` for tear-off and tab merging functionality
4. **Customize appearance** - Read `customization.md` for styling and theming

Your foundation is ready - explore these advanced features to build powerful document-based applications!
