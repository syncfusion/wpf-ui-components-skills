---
name: syncfusion-wpf-chromeless-window
description: Implements the Syncfusion WPF ChromelessWindow for custom window chrome and title bar customization. Use this when creating custom window designs, customizing title bars, or implementing borderless windows in WPF applications. Covers title bar customization, background styling, border radius, custom buttons, and native chrome integration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Chromeless Window

This skill explains how to implement and customize the Syncfusion WPF `ChromelessWindow`.

## When to Use This Skill
- Use when the user needs a frameless/chromeless WPF window with a customizable title bar.
- Use when customizing titlebar background, height, font, icon, or alignment.
- Use when adding controls or custom buttons to the title bar (left/right).
- Use when changing borders, corner radius, resize behavior, or theme integration.
- Use when the user needs `UseNativeChrome` behavior for window docking.

## Navigation Guide
Follow the links below to open focused reference pages for each task.

### Getting started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Add `ChromelessWindow` to a project, basic properties and quick examples.

### Overview
📄 **Read:** [references/overview.md](references/overview.md)
- High-level features and supported scenarios.

### Title bar customization
📄 **Read:** [references/titlebar-customization.md](references/titlebar-customization.md)
- Background, height, font, visibility, maximize/minimize buttons.

### Title bar custom buttons & controls
📄 **Read:** [references/titlebar-custom-buttons.md](references/titlebar-custom-buttons.md)
📄 **Read:** [references/adding-controls.md](references/adding-controls.md)
- Add custom buttons or adaptive controls (Left/Right header items).

### Styling & Templates
📄 **Read:** [references/styling.md](references/styling.md)
- Templates for TitleBar, TitleButton, overriding default styles.

### Border & Corner Radius
📄 **Read:** [references/border-customization.md](references/border-customization.md)
📄 **Read:** [references/corner-radius.md](references/corner-radius.md)
- Resize border color, thickness, corner radius examples.

### Visual structure & themes
📄 **Read:** [references/visual-structure.md](references/visual-structure.md)
📄 **Read:** [references/set-visual-styles.md](references/set-visual-styles.md)
- Visual element layout and theme application guidance.

### Native chrome / docking
📄 **Read:** [references/use-native-chrome.md](references/use-native-chrome.md)
- `UseNativeChrome` behavior and notes for docking.

### End-user capabilities
📄 **Read:** [references/end-user-capabilities.md](references/end-user-capabilities.md)
- What end users can do (maximize, minimize, restore, resize).

## Quick Start
Minimal `ChromelessWindow` example (XAML):

```xml
<syncfusion:ChromelessWindow x:Class="Chromelesswindow.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Shared;assembly=Syncfusion.Shared.WPF"
    Title="ChromelessWindow Sample" Height="350" Width="525"
    TitleBarBackground="#FF2D2D2D" TitleFontSize="14" TitleBarHeight="36">
    <Grid>
        <!-- App content here -->
    </Grid>
</syncfusion:ChromelessWindow>
```

## Common Patterns
- Add custom left/right titlebar items with `LeftHeaderItemsSource` / `RightHeaderItemsSource`.
- Use `TitleBarTemplate` / `TitleButtonTemplate` to fully customize visuals.
- Override default style in `App.xaml` when modifying many template parts.

## Troubleshooting
- If maximize/minimize buttons don't appear, check `ResizeMode` and `ShowMaximizeButton`/`ShowMinimizeButton`.
- For drag-to-move to work, ensure a `TitleBar` named `PART_TitleBar` is present in the template.
- If resizing is hard, increase `ResizeBorderThickness`.
- When custom templates are not applied, verify resources are included in `App.xaml` and keys match.