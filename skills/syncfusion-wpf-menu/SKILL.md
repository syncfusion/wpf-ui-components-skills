---
name: syncfusion-wpf-menu
description: Implement Syncfusion WPF MenuAdv for hierarchical menus, context menus, and application menu bars. Use this when building nested menus, menus with icons, keyboard shortcuts, checkbox/radio button menu items, or command-bound menus in WPF. Covers ItemsSource data binding with HierarchicalDataTemplate, MenuItemSeparator, orientation, expand mode, animation, and theming.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing MenuAdv (WPF)

## When to Use This Skill

Use this skill when the user needs to:
- Build an application menu bar (File, Edit, View…) with nested submenus
- Add icons, keyboard shortcut labels, or separators to menu items
- Create checkable menu items (checkbox or radio button style)
- Bind a hierarchical data collection to a menu via ItemsSource
- Wire menu items to ICommand implementations (MVVM pattern)
- Switch between horizontal and vertical menu orientation
- Control how submenus open (click vs mouse-hover)

---

## Component Overview

```
MenuAdv                               (root menu bar container)
├── MenuItemAdv  (×N)                 (top-level or nested item)
│   ├── Header                        — display text
│   ├── Icon                          — image/UIElement on left
│   ├── InputGestureText              — shortcut label (e.g. "Ctrl+N")
│   ├── IsCheckable / CheckIconType   — checkbox or radio button mode
│   ├── Command / CommandParameter    — ICommand binding
│   └── MenuItemAdv  (×N)            (nested submenu items)
└── MenuItemSeparator                 (horizontal divider line)
```

**Assembly:** `Syncfusion.Shared.WPF`  
**Namespace:** `Syncfusion.Windows.Shared`  
**XAML schema:** `http://schemas.syncfusion.com/wpf`

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly reference and NuGet
- XAML namespace declaration
- Declarative nested MenuItemAdv in XAML
- C# code-behind creation
- Setting icons on items
- Themes with SfSkinManager

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding ObservableCollection via ItemsSource
- Model + ViewModel with SubItems hierarchy
- HierarchicalDataTemplate setup
- Binding from XML via XmlDataProvider + XPath

### Menu Item Features
📄 **Read:** [references/menu-item-features.md](references/menu-item-features.md)
- Icons on MenuItemAdv
- InputGestureText (keyboard shortcut labels)
- MenuItemSeparator (visual divider)
- CheckBox menu items (IsCheckable, CheckIconType)
- RadioButton menu items (GroupName for mutual exclusion)

### Commands
📄 **Read:** [references/commands.md](references/commands.md)
- Command property — binding to ICommand
- CommandParameter — value passed to handler
- CommandTarget — routing to a specific element
- Built-in ApplicationCommands (Copy, Cut, Paste)
- DelegateCommand implementation pattern for MVVM

### Orientation and Expand Mode
📄 **Read:** [references/orientation-and-expand-mode.md](references/orientation-and-expand-mode.md)
- Orientation — Horizontal (default) vs Vertical
- ExpandMode — ExpandOnClick vs ExpandOnMouseOver
- Boundary detection and scroll support overview
- Decision guide

### Appearance
📄 **Read:** [references/appearance.md](references/appearance.md)
- Animation support
- Keyboard navigation
- Blendability (Expression Blend customization)
- Themes via SfSkinManager and ThemeStudio

---

## Quick Start

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"

<syncfusion:MenuAdv Height="25" VerticalAlignment="Top">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="New"/>
        <syncfusion:MenuItemAdv Header="Open"/>
        <syncfusion:MenuItemSeparator/>
        <syncfusion:MenuItemAdv Header="Exit"/>
    </syncfusion:MenuItemAdv>
    <syncfusion:MenuItemAdv Header="Edit">
        <syncfusion:MenuItemAdv Header="Cut"/>
        <syncfusion:MenuItemAdv Header="Copy"/>
        <syncfusion:MenuItemAdv Header="Paste"/>
    </syncfusion:MenuItemAdv>
    <syncfusion:MenuItemAdv Header="View"/>
</syncfusion:MenuAdv>
```

```csharp
using Syncfusion.Windows.Shared;

MenuAdv menu = new MenuAdv();

MenuItemAdv file = new MenuItemAdv { Header = "File" };
file.Items.Add(new MenuItemAdv { Header = "New" });
file.Items.Add(new MenuItemAdv { Header = "Open" });
file.Items.Add(new MenuItemSeparator());
file.Items.Add(new MenuItemAdv { Header = "Exit" });

MenuItemAdv edit = new MenuItemAdv { Header = "Edit" };
edit.Items.Add(new MenuItemAdv { Header = "Cut" });
edit.Items.Add(new MenuItemAdv { Header = "Copy" });

menu.Items.Add(file);
menu.Items.Add(edit);
this.Content = menu;
```

---

## Common Patterns

| Scenario | Approach |
|---|---|
| Static app menu (File/Edit/View) | Declarative nested `MenuItemAdv` in XAML |
| Dynamic data-driven menu | `ItemsSource` + `HierarchicalDataTemplate` |
| Icon beside menu item | `MenuItemAdv.Icon` with `<Image Source="..."/>` |
| Keyboard shortcut label | `InputGestureText="Ctrl+N"` |
| Separator between groups | `<syncfusion:MenuItemSeparator/>` |
| Toggle option (checkable) | `IsCheckable="True"` + `CheckIconType="CheckBox"` |
| Mutually exclusive options | `CheckIconType="RadioButton"` + `GroupName="group1"` |
| MVVM command binding | `Command="{Binding SaveCommand}"` |
| Vertical sidebar menu | `Orientation="Vertical"` |
| Open submenu on hover | `ExpandMode="ExpandOnMouseOver"` |

---

## Key Properties

**MenuAdv:**

| Property | Description |
|---|---|
| `Orientation` | `Horizontal` (default) or `Vertical` item layout |
| `ExpandMode` | `ExpandOnClick` or `ExpandOnMouseOver` |
| `ItemsSource` | Data collection for data-bound population |
| `ItemTemplate` | `HierarchicalDataTemplate` for data-bound items |

**MenuItemAdv:**

| Property | Description |
|---|---|
| `Header` | Display text |
| `Icon` | Image/UIElement shown on left |
| `InputGestureText` | Shortcut key label (e.g., `"Ctrl+S"`) |
| `IsCheckable` | Enable checkbox/radio behavior |
| `CheckIconType` | `CheckBox` or `RadioButton` |
| `IsChecked` | Checked state (bool) |
| `GroupName` | Radio button group identifier |
| `Command` | `ICommand` binding |
| `CommandParameter` | Value passed to command |
| `CommandTarget` | Target element for routed commands |
