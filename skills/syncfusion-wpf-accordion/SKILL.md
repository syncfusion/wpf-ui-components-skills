---
name: syncfusion-wpf-accordion
description: Implement Syncfusion WPF SfAccordion for collapsible and expandable panel layouts. Use this when working with accordion controls, expandable panels, or collapsible section navigation in WPF. Covers populating items declaratively or via data binding, selection modes (One/OneOrMore/ZeroOrOne/ZeroOrMore), programmatic selection, appearance customization (AccentBrush, HeaderTemplate, ContentTemplate), and theming with SfSkinManager.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing SfAccordion (WPF)

## When to Use This Skill

Use this skill when the user needs to:
- Build collapsible/expandable section UI in WPF
- Display categorized content with expand-on-demand behavior
- Create Outlook-style, FAQ-style, or settings-panel accordion layouts
- Bind a business object collection to an accordion
- Control how many items can be expanded simultaneously
- Customize accordion item headers, content, expander buttons, or animation

---

## Component Overview

```
SfAccordion                          (ItemsControl)
└── SfAccordionItem  (×N)            (HeaderedContentControl)
    ├── Header      — always visible (collapsed & expanded)
    └── Content     — visible only when expanded (IsSelected=true)
```

**Assembly:** `Syncfusion.SfAccordion.WPF`  
**Namespace:** `Syncfusion.Windows.Controls.Layout`  
**XAML Schema:** `http://schemas.syncfusion.com/wpf`

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly reference and NuGet setup
- XAML and C# control creation
- Basic SfAccordionItem with Header and Content
- Themes with SfSkinManager

### Populating Items
📄 **Read:** [references/populating-items.md](references/populating-items.md)
- Declarative items via `Items` collection
- Data binding via `ItemsSource`
- Model + ViewModel + ObservableCollection
- `DisplayMemberPath` for quick headers
- `HeaderTemplate` / `HeaderTemplateSelector`
- `ContentTemplate` / `ContentTemplateSelector`

### Selecting Items
📄 **Read:** [references/selecting-items.md](references/selecting-items.md)
- `SelectedIndex` — select by index
- `SelectedItem` — select by instance (TwoWay binding)
- `SelectedItems` / `SelectedIndices` — read-only collections
- `IsSelected` on individual items
- `SelectAll()` / `UnselectAll()` methods
- `SelectedItemsChanged` / `SelectionChanged` events

### Selection Mode
📄 **Read:** [references/selection-mode.md](references/selection-mode.md)
- `One` — exactly one expanded (default)
- `OneOrMore` — multiple expanded, at least one required
- `ZeroOrOne` — at most one, all can collapse
- `ZeroOrMore` — multiple, all can collapse
- Decision guide: choosing the right mode

### Appearance and Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- `AccentBrush` — highlight color
- `HeaderTemplate` / `ContentTemplate`
- `AccordionButtonStyle` — expander arrow customization
- `ItemContainerStyle` — per-item styling
- Animation enable/disable
- SfSkinManager themes

---

## Quick Start

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"

<syncfusion:SfAccordion Width="300" Height="200">
    <syncfusion:SfAccordionItem Header="WPF"
                                Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight"
                                Content="Essential Studio for Silverlight"/>
    <syncfusion:SfAccordionItem Header="WinRT"
                                Content="Essential Studio for WinRT" IsSelected="True"/>
</syncfusion:SfAccordion>
```

```csharp
using Syncfusion.Windows.Controls.Layout;

SfAccordion accordion = new SfAccordion();
accordion.Items.Add(new SfAccordionItem { Header = "WPF",        Content = "Essential Studio for WPF" });
accordion.Items.Add(new SfAccordionItem { Header = "Silverlight", Content = "Essential Studio for Silverlight" });
accordion.Items.Add(new SfAccordionItem { Header = "WinRT",       Content = "Essential Studio for WinRT", IsSelected = true });
this.Content = accordion;
```

---

## Common Patterns

| Scenario | Approach |
|---|---|
| Multiple sections, one open at a time | `SelectionMode="One"` (default) |
| FAQ where multiple can be open | `SelectionMode="ZeroOrMore"` |
| Data-driven content | `ItemsSource` + `HeaderTemplate` + `ContentTemplate` |
| Highlight active item color | `AccentBrush` |
| Custom expander arrow | `AccordionButtonStyle` on `SfAccordionItem` |
| Slow expand/collapse animation | `ExpandableContentControl.Percentage` in `ItemContainerStyle` |
| Track which items are expanded | `SelectedItemsChanged` event |

---

## Key Properties

| Property | Type | Description |
|---|---|---|
| `SelectionMode` | `AccordionSelectionMode` | One / OneOrMore / ZeroOrOne / ZeroOrMore |
| `SelectedIndex` | `int` | Index of most recently selected item |
| `SelectedItem` | `object` | Instance of most recently selected item |
| `SelectedItems` | read-only | All currently selected item instances |
| `SelectedIndices` | read-only | All currently selected item indices |
| `ItemsSource` | `IEnumerable` | Data collection binding |
| `HeaderTemplate` | `DataTemplate` | Shared header template for all items |
| `ContentTemplate` | `DataTemplate` | Shared content template for all items |
| `HeaderTemplateSelector` | `DataTemplateSelector` | Per-item header template |
| `ContentTemplateSelector` | `DataTemplateSelector` | Per-item content template |
| `AccentBrush` | `Brush` | Accent color for item highlight |
| `ItemContainerStyle` | `Style` | Style applied to each `SfAccordionItem` |

**SfAccordionItem properties:**

| Property | Description |
|---|---|
| `Header` | Always-visible item header |
| `Content` | Visible when item is expanded |
| `IsSelected` | `true` = expanded, `false` = collapsed |
| `IsLocked` | Read-only; `true` when item cannot be collapsed |
| `AccordionButtonStyle` | Style for the expander toggle button |
