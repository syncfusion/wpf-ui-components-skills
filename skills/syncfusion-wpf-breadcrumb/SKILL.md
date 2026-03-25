---
name: syncfusion-wpf-breadcrumb
description: Implement the Syncfusion WPF HierarchyNavigator (Breadcrumb) control for hierarchical path navigation. Use this when building breadcrumb or address-bar UIs, binding hierarchical data to a navigation control, or enabling edit mode with AutoComplete path entry in WPF. Covers navigation history, refresh button, progress bar during navigation, and keyboard-driven hierarchical navigation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF HierarchyNavigator (Breadcrumb)

Guide for implementing the Syncfusion® WPF **HierarchyNavigator** control — a breadcrumb / address-bar component similar to the Windows Explorer address bar, enabling hierarchical path navigation with data binding, edit mode, history, and progress bar support.

## When to Use This Skill

Use this skill when you need to:
- Create a **breadcrumb / address-bar** navigation UI in WPF
- Populate `HierarchyNavigator` with **declarative items** (`HierarchyNavigatorItem`) or **data-bound collections**
- Use `HierarchicalDataTemplate` to display multi-level business objects, XML data, or WCF-sourced data
- Enable **edit mode** so users can type and autocomplete navigation paths
- Show **navigation history** via the back-arrow dropdown
- Add a **Refresh button** and handle `HierarchyNavigatorRefreshButtonClick`
- Display / cancel a **progress bar** during navigation operations
- Respond to item selection via **Command binding** (MVVM pattern)
- Apply **tooltips**, keyboard navigation, and built-in themes

## Component Overview

`HierarchyNavigator` renders a breadcrumb bar where each segment is a clickable `HierarchyNavigatorItem`. Clicking a segment opens a popup listing its children for drill-down navigation.

**Required assemblies:**
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

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly references and NuGet setup
- Adding via Designer, XAML, and C#
- Adding items declaratively with `HierarchyNavigatorItem`
- Basic `ItemsSource` + `HierarchicalDataTemplate` binding
- Applying built-in themes

### Populating Data
📄 **Read:** [references/populating-data.md](references/populating-data.md)
- Binding to `ObservableCollection` business objects
- `HierarchicalDataTemplate` for multi-level hierarchies
- Binding XML data via `XDocument` parsing
- Binding via WCF service
- On-demand / lazy item loading

### Edit Mode & AutoComplete
📄 **Read:** [references/edit-mode-and-autocomplete.md](references/edit-mode-and-autocomplete.md)
- `IsEnableEditMode` property to allow path typing
- AutoComplete filter dropdown showing matching paths
- Keyboard: Enter to confirm, Esc to cancel
- Behavior when an invalid path is entered

### Navigation Features
📄 **Read:** [references/navigation-features.md](references/navigation-features.md)
- `ShowToolTip` and per-item tooltips
- Navigation history (back-arrow dropdown)
- Refresh button and `HierarchyNavigatorRefreshButtonClick` event
- `ShowProgressBar()` / `CancelProgressBar()` with optional `TimeSpan`
- Keyboard navigation reference table
- Restricting level selection

### Command Binding (MVVM)
📄 **Read:** [references/command-binding.md](references/command-binding.md)
- `Command` property binding to `ICommand` / `DelegateCommand`
- `SelectedItemCommand` pattern in ViewModel
- Responding to selected item changes
- Passing `HierarchyNavigatorItem` as `CommandParameter`

### Appearance & Themes
📄 **Read:** [references/appearance-and-themes.md](references/appearance-and-themes.md)
- `ItemTemplate` with `HierarchicalDataTemplate` for custom item display
- Customizing with Microsoft Expression Blend
- Applying built-in themes via `SfSkinManager`
- Creating custom themes with ThemeStudio

## Quick Start

### Declarative items (XAML)
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="MyApp.MainWindow"
        Title="MainWindow" Height="350" Width="525">
  <Grid>
    <syncfusion:HierarchyNavigator x:Name="navigator"
                                   VerticalAlignment="Top"
                                   Height="30"
                                   Width="600">
      <syncfusion:HierarchyNavigator.Items>
        <syncfusion:HierarchyNavigatorItem Content="Syncfusion">
          <syncfusion:HierarchyNavigatorItem.Items>
            <syncfusion:HierarchyNavigatorItem Content="User Interface">
              <syncfusion:HierarchyNavigatorItem.Items>
                <syncfusion:HierarchyNavigatorItem Content="WPF"/>
                <syncfusion:HierarchyNavigatorItem Content="Silverlight"/>
              </syncfusion:HierarchyNavigatorItem.Items>
            </syncfusion:HierarchyNavigatorItem>
          </syncfusion:HierarchyNavigatorItem.Items>
        </syncfusion:HierarchyNavigatorItem>
      </syncfusion:HierarchyNavigator.Items>
    </syncfusion:HierarchyNavigator>
  </Grid>
</Window>
```

### Data-bound (XAML + MVVM)
```xml
<syncfusion:HierarchyNavigator ItemsSource="{Binding HierarchyItems}"
                               Height="30" Width="600">
  <syncfusion:HierarchyNavigator.ItemTemplate>
    <HierarchicalDataTemplate ItemsSource="{Binding Children}">
      <TextBlock Text="{Binding Name}" Margin="2,0"/>
    </HierarchicalDataTemplate>
  </syncfusion:HierarchyNavigator.ItemTemplate>
</syncfusion:HierarchyNavigator>
```

### C# code-behind
```csharp
using Syncfusion.Windows.Tools.Controls;

HierarchyNavigator navigator = new HierarchyNavigator { Height = 30 };
HierarchyNavigatorItem root = new HierarchyNavigatorItem { Content = "Syncfusion" };
HierarchyNavigatorItem child = new HierarchyNavigatorItem { Content = "WPF" };
root.Items.Add(child);
navigator.Items.Add(root);
this.Content = navigator;
```

## Common Patterns

### Choose the right population approach
- **Static/known hierarchy** → use declarative `HierarchyNavigatorItem` in XAML
- **Business object collection** → use `ItemsSource` + `HierarchicalDataTemplate`
- **XML or WCF data** → parse to `ObservableCollection`, then bind to `ItemsSource`
- **Large/lazy-loaded hierarchy** → load children on demand via item expanded event

### Enable edit mode with AutoComplete
```xml
<syncfusion:HierarchyNavigator IsEnableEditMode="True" .../>
```
User clicks the control to enter a path; a filtered dropdown suggests matching nodes. Press **Enter** to confirm, **Esc** to cancel.

### Show progress bar during navigation
```csharp
// Show for default 500ms
navigator.ShowProgressBar();

// Show for custom duration
navigator.ShowProgressBar(new TimeSpan(0, 0, 0, 0, 1000));

// Cancel immediately
navigator.CancelProgressBar();
```

### MVVM command binding
```xml
<syncfusion:HierarchyNavigator Command="{Binding SelectedItemCommand}"
                               ItemsSource="{Binding Items}" .../>
```
The command fires when the selected breadcrumb item changes, passing the `HierarchyNavigatorItem` as parameter.

## Key Properties & Methods

| Property / Method | Description |
|---|---|
| `Items` | Declarative `HierarchyNavigatorItem` children |
| `ItemsSource` | Bound data collection |
| `ItemTemplate` | `HierarchicalDataTemplate` for item display |
| `IsEnableEditMode` | Enables path editing with AutoComplete |
| `ShowToolTip` | Enables tooltips on all items |
| `Command` | `ICommand` fired on selected item change |
| `ShowProgressBar()` | Displays progress bar (default 500ms) |
| `ShowProgressBar(TimeSpan)` | Displays progress bar for specified duration |
| `CancelProgressBar()` | Cancels visible progress bar |
| `CancelProgressBar(TimeSpan)` | Cancels progress bar after specified delay |
| `HierarchyNavigatorRefreshButtonClick` | Event fired when Refresh button is clicked |
