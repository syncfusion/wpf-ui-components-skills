---
name: syncfusion-wpf-tree-navigator
description: Implement Syncfusion WPF SfTreeNavigator for in-place hierarchical tree navigation and drill-down panels. Use this when building hierarchical navigation panels, drill-down navigation, or breadcrumb-style tree navigation in WPF. Covers declarative item creation, ItemsSource data binding with HierarchicalDataTemplate, Default and Extended navigation modes, HeaderTemplate customization, and MVVM-based SelectedItem selection.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing SfTreeNavigator (WPF)

## When to Use This Skill

Use this skill when the user needs to:
- Build in-place hierarchical drill-down navigation in WPF (no popup, no separate panel)
- Display a multi-level tree structure that expands within its own footprint
- Show product/category/department hierarchy navigation
- Choose between breadcrumb-style (Default) or stacked-header (Extended) navigation
- Bind a nested business object collection to tree navigator items
- Programmatically set or read the currently selected item

---

## Component Overview

```
SfTreeNavigator                        (root container)
├── Header                             — root label text (always visible)
├── HeaderTemplate                     — customize root header appearance
└── SfTreeNavigatorItem  (×N)          (each level item)
    ├── Header                         — item display text
    └── SfTreeNavigatorItem  (×N)      (nested child items, unlimited depth)
```

**Assembly:** `Syncfusion.SfTreeNavigator.WPF`  
**Namespace:** `Syncfusion.Windows.Controls.Navigation`  
**XAML prefix:** `navigation:` (typical alias)

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly reference and namespace
- Declarative XAML with SfTreeNavigatorItem
- C# code-behind creation
- Header on SfTreeNavigator
- Themes with SfSkinManager

### Populating Items
📄 **Read:** [references/populating-items.md](references/populating-items.md)
- Declarative items via Items collection
- Data binding via ItemsSource
- Nested Model + ViewModel with ObservableCollection
- ItemTemplate with HierarchicalDataTemplate
- Connecting child levels via ItemsSource binding in template

### Navigation Mode
📄 **Read:** [references/navigation-mode.md](references/navigation-mode.md)
- `Default` — single header + back button at top
- `Extended` — stacked breadcrumb headers from root to current level
- When to use each mode
- TreeNavigatorHeaderItem styling in Extended mode
- Decision guide

### Header and Selection
📄 **Read:** [references/header-and-selection.md](references/header-and-selection.md)
- `HeaderTemplate` — customize root header appearance
- `Header` — root title string
- `SelectedItem` — get/set the selected item
- TwoWay MVVM binding for SelectedItem
- Programmatic selection

---

## Quick Start

```xaml
xmlns:navigation="clr-namespace:Syncfusion.Windows.Controls.Navigation;assembly=Syncfusion.SfTreeNavigator.WPF"

<navigation:SfTreeNavigator Header="Enterprise Toolkit"
                             Width="300" Height="400">
    <navigation:SfTreeNavigatorItem Header="WinRT (XAML)">
        <navigation:SfTreeNavigatorItem Header="Chart"/>
        <navigation:SfTreeNavigatorItem Header="Tools"/>
    </navigation:SfTreeNavigatorItem>
    <navigation:SfTreeNavigatorItem Header="Metro Studio"/>
</navigation:SfTreeNavigator>
```

```csharp
using Syncfusion.Windows.Controls.Navigation;

SfTreeNavigator navigator = new SfTreeNavigator { Header = "Enterprise Toolkit" };

SfTreeNavigatorItem winrt  = new SfTreeNavigatorItem { Header = "WinRT (XAML)" };
SfTreeNavigatorItem chart  = new SfTreeNavigatorItem { Header = "Chart" };
SfTreeNavigatorItem tools  = new SfTreeNavigatorItem { Header = "Tools" };
SfTreeNavigatorItem metro  = new SfTreeNavigatorItem { Header = "Metro Studio" };

winrt.Items.Add(chart);
winrt.Items.Add(tools);
navigator.Items.Add(winrt);
navigator.Items.Add(metro);
this.Content = navigator;
```

---

## Common Patterns

| Scenario | Approach |
|---|---|
| Static hierarchy | Declarative `SfTreeNavigatorItem` nesting in XAML |
| Dynamic data-driven hierarchy | `ItemsSource` + `HierarchicalDataTemplate` |
| Show full breadcrumb path | `NavigationMode="Extended"` |
| Single back button at top | `NavigationMode="Default"` |
| Custom root title styling | `HeaderTemplate` |
| Pre-select an item on load | `SelectedItem` binding in ViewModel constructor |
| Respond to selection | `SelectedItem` TwoWay binding + ViewModel property |

---

## Key Properties

| Property | Description |
|---|---|
| `Header` | Root label text shown at top of navigator |
| `HeaderTemplate` | DataTemplate to customize root header appearance |
| `NavigationMode` | `Default` (back button) or `Extended` (stacked headers) |
| `ItemsSource` | Data collection for data-bound population |
| `ItemTemplate` | `HierarchicalDataTemplate` for data-bound items |
| `SelectedItem` | Gets or sets the currently selected item |

**SfTreeNavigatorItem properties:**

| Property | Description |
|---|---|
| `Header` | Display text for the item |
| `Items` | Child item collection |
