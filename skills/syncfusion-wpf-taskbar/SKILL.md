---
name: syncfusion-wpf-taskbar
description: Implement Syncfusion WPF TaskBar for Windows Explorer-style grouped task panels with collapsible TaskBarItem sections. Use this when building grouped collapsible panels, Windows XP-style sidebars, or expandable action panel sections in WPF. Covers TaskBarItem, GroupMargin, GroupPadding, GroupWidth, GroupOrientation, IsOpened, ButtonSize, and animation speed configuration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF TaskBar

The **TaskBar** control provides a Windows Explorer-style task panel UI — grouped collapsible sections (`TaskBarItem`) that can hold any WPF content. Ideal for action panels, property groups, navigation sidebars, and tool panels.

**Assembly required:**
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

| Scenario | Use TaskBar |
|----------|------------|
| Windows Explorer-style grouped action panel | ✅ Yes |
| Collapsible sidebar with grouped items | ✅ Yes |
| WPF tool panel with categorized sections | ✅ Yes |
| Simple tab control | ❌ Use TabControl |
| Navigation sidebar with drill-down | ❌ Use GroupBar or SfTreeNavigator |

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly and NuGet setup
- Adding TaskBar via XAML and C#
- Adding a TaskBarItem with a Header
- Adding content (text, panels) to TaskBarItem

### Items and Content
📄 **Read:** [references/items-and-content.md](references/items-and-content.md)
- TaskBarItem Items collection (any UIElement)
- Custom header with image (DockPanel + Image + TextBlock)
- Collapse/expand with `IsOpened` attached property
- Animation speed (`Speed` attached property)
- Expander button size (`ButtonSize` attached property)

### Layout and Orientation
📄 **Read:** [references/layout-and-orientation.md](references/layout-and-orientation.md)
- `GroupOrientation`: Horizontal vs Vertical
- `GroupOrientationChanged` event
- `GroupMargin`: spacing between items
- `GroupPadding`: internal content padding
- `GroupWidth`: uniform item width
- `FlowDirection`: LeftToRight / RightToLeft

### Appearance and Visual Styles
📄 **Read:** [references/appearance.md](references/appearance.md)
- `SkinStorage.SetVisualStyle()` — applying built-in visual styles
- All available style names (Office2007, Blend, Metro, VS2010, etc.)

---

## Quick Start Example

**Minimal TaskBar with two groups (XAML):**
```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="300">
    <Grid>
        <syncfusion:TaskBar Name="taskBar" GroupMargin="5">

            <syncfusion:TaskBarItem Header="File Tasks">
                <StackPanel Margin="10">
                    <TextBlock Text="Open a file" />
                    <TextBlock Text="Save the file" />
                </StackPanel>
            </syncfusion:TaskBarItem>

            <syncfusion:TaskBarItem Header="Edit Tasks">
                <StackPanel Margin="10">
                    <TextBlock Text="Copy" />
                    <TextBlock Text="Paste" />
                </StackPanel>
            </syncfusion:TaskBarItem>

        </syncfusion:TaskBar>
    </Grid>
</Window>
```

**C# equivalent:**
```csharp
TaskBar taskBar = new TaskBar();
taskBar.GroupMargin = new Thickness(5);

TaskBarItem item1 = new TaskBarItem() { Header = "File Tasks" };
item1.Items.Add(new TextBlock() { Text = "Open a file" });
taskBar.Items.Add(item1);

TaskBarItem item2 = new TaskBarItem() { Header = "Edit Tasks" };
item2.Items.Add(new TextBlock() { Text = "Copy" });
taskBar.Items.Add(item2);

this.Content = taskBar;
```

---

## Common Patterns

### Collapse all groups on load
```xml
<syncfusion:TaskBar Name="taskBar" syncfusion:TaskBar.IsOpened="False">
    <syncfusion:TaskBarItem Header="Tasks" />
</syncfusion:TaskBar>
```

### Horizontal layout (groups side by side)
```xml
<syncfusion:TaskBar GroupOrientation="Horizontal" GroupMargin="5">
    ...
</syncfusion:TaskBar>
```

### Custom header with icon
```xml
<syncfusion:TaskBarItem>
    <syncfusion:TaskBarItem.Header>
        <DockPanel>
            <Image Height="16" Width="16" Source="icon.png" />
            <TextBlock Foreground="White" Margin="5,0,0,0" Text="My Group" />
        </DockPanel>
    </syncfusion:TaskBarItem.Header>
    ...
</syncfusion:TaskBarItem>
```

### Apply visual style
```csharp
SkinStorage.SetVisualStyle(taskBar, "Office2007Blue");
```

---

## Key Properties

| Property | On | Type | Description |
|----------|----|------|-------------|
| `Header` | `TaskBarItem` | `object` | Group header label (supports templates) |
| `Items` | `TaskBarItem` | collection | Content items in the group |
| `GroupMargin` | `TaskBar` | `Thickness` | Margin between all TaskBarItems |
| `GroupWidth` | `TaskBar` | `double` | Uniform width for all TaskBarItems |
| `GroupOrientation` | `TaskBar` | `Orientation` | `Vertical` (default) or `Horizontal` |
| `FlowDirection` | `TaskBar` | `FlowDirection` | `LeftToRight` or `RightToLeft` |
| `IsOpened` | attached on `TaskBar` | `bool` | Expand (`true`) or collapse (`false`) all items |
| `TaskBar.Speed` | attached | `double` | Animation speed for expand/collapse |
| `TaskBar.ButtonSize` | attached | `double` | Height of the expander button |
| `TaskBar.GroupPadding` | attached | `Thickness` | Padding inside each TaskBarItem |
