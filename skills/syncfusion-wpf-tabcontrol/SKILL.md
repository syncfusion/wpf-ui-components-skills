---
name: syncfusion-wpf-tabcontrol
description: Create and manage WPF TabControl (TabControlExt) components for organizing content into tabs. Use this skill when users need to implement tabbed interfaces, manage tab selection, configure close buttons, handle tab interactions (clicking, keyboard navigation), customize tab appearance (orientation, placement), or bind tab data. Covers tab creation via XAML/C#, tab item management, event handling, context menus, drag-and-drop reordering, and theming.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF TabControl (TabControlExt)

The `TabControl` (TabControlExt) is a powerful component for organizing application content into multiple tabs. It provides features like tab orientation, editable headers, pin/unpin functionality, context menus, and customizable close buttons, enabling developers to create flexible, organized tabbed interfaces.

## When to Use This Skill

- **Creating tabbed interfaces:** When you need to organize multiple content areas into a tab-based layout
- **Tab management:** For handling tab item creation, deletion, selection, and manipulation
- **User interactions:** When you need to respond to tab selection changes, tab closing, or tab navigation
- **Customization:** For styling tabs (orientation, placement, themes), configuring close buttons, or adding icons
- **Data binding:** When binding tab items to data sources or observable collections
- **Advanced features:** For implementing context menus, drag-drop reordering, pin/unpin, or keyboard navigation

## Component Overview

**Key Capabilities:**
- **Tab Orientation** - Position tabs horizontally (top/bottom) or vertically (left/right)
- **Flexible Closing** - Configure close button display (Common, Individual, Both, Extended, Hidden)
- **Tab Selection** - Support for mouse, keyboard (Ctrl+Tab, Arrow keys), and programmatic selection
- **Editable Headers** - Allow users to edit tab headers interactively
- **Pin/Unpin** - Enable quick access to frequently used tabs
- **Context Menus** - Built-in and custom context menu support
- **Data Binding** - Bind tab items to data sources
- **Drag and Drop** - Reorder tabs by dragging
- **Navigation** - Tab list menu and scroll buttons for navigation
- **Theming** - Rich set of built-in themes and customization options

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Basic TabControl creation (XAML and C# approaches)
- Adding TabItems with headers and content
- Tab placement and orientation options
- Assembly dependencies

### Tab Interactions & Selection
📄 **Read:** [references/tab-interactions.md](references/tab-interactions.md)
- Tab selection (mouse, keyboard, programmatic)
- SelectedItemChangedEvent for monitoring changes
- Keyboard navigation (Ctrl+Tab, Arrow keys)
- Tab list context menu for quick navigation
- Getting selected item properties

### Tab Closing & Button Configuration
📄 **Read:** [references/closing-tabs.md](references/closing-tabs.md)
- CloseButtonType options and display modes
- Individual vs common close buttons
- Disabling close on specific tabs (CanClose)
- Hiding close buttons for particular tabs
- Close button visibility states

### Customization & Styling
📄 **Read:** [references/customization.md](references/customization.md)
- Editable tab headers
- Tab orientation and placement variations
- Layout modes (SingleLine, MultiLine, MultiLineWithFillWidth)
- Pin/unpin functionality
- Tab scrolling configuration
- Built-in themes and theming support

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding TabControl items to data sources
- Creating TabItems from data collections
- Header and content templates
- Observable collection patterns
- TabItem data binding

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Context menu customization (tab-level and item-level)
- Custom menu items and handlers
- Tab reordering via drag and drop
- New button feature (IsNewButtonEnabled)
- Tab scrolling with navigation buttons
- Localization support
- New tab button events

## Quick Start Example

### Basic XAML TabControl
```xml
<Window x:Class="TabControlApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="TabControl Example" Height="450" Width="800">

    <Grid>
        <syncfusion:TabControlExt Name="tabControl" Height="100" Width="280">
            <syncfusion:TabItemExt Header="Tab 1">
                <TextBlock Text="Content of Tab 1" />
            </syncfusion:TabItemExt>
            <syncfusion:TabItemExt Header="Tab 2">
                <TextBlock Text="Content of Tab 2" />
            </syncfusion:TabItemExt>
            <syncfusion:TabItemExt Header="Tab 3">
                <TextBlock Text="Content of Tab 3" />
            </syncfusion:TabItemExt>
        </syncfusion:TabControlExt>
    </Grid>
</Window>
```

### Basic C# Creation
```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();

        // Create TabControl instance
        TabControlExt tabControlExt = new TabControlExt();
        tabControlExt.Height = 100;
        tabControlExt.Width = 280;

        // Create and add TabItems
        TabItemExt tabItem1 = new TabItemExt() 
        {
            Header = "Tab 1",
            Content = new TextBlock() { Text = "Content of Tab 1" }
        };

        TabItemExt tabItem2 = new TabItemExt() 
        {
            Header = "Tab 2",
            Content = new TextBlock() { Text = "Content of Tab 2" }
        };

        tabControlExt.Items.Add(tabItem1);
        tabControlExt.Items.Add(tabItem2);

        this.Content = tabControlExt;
    }
}
```

## Common Patterns

### Pattern 1: Responding to Tab Selection Changes
```csharp
// Handle tab selection changes
tabControl.SelectedItemChangedEvent += TabControl_SelectedItemChangedEvent;

private void TabControl_SelectedItemChangedEvent(object sender, SelectedItemChangedEventArgs e)
{
    var newTabItem = e.NewSelectedItem.Header;
    var oldTabItem = e.OldSelectedItem?.Header ?? "None";
    MessageBox.Show($"Changed from {oldTabItem} to {newTabItem}");
}
```

### Pattern 2: Configuring Close Buttons
```xml
<!-- Enable close buttons on both TabControl and TabItems -->
<syncfusion:TabControlExt CloseButtonType="Both" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" CanClose="False" />
</syncfusion:TabControlExt>
```

### Pattern 3: Changing Tab Orientation
```xml
<!-- Position tabs at the bottom -->
<syncfusion:TabControlExt TabStripPlacement="Bottom" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" />
</syncfusion:TabControlExt>
```

### Pattern 4: Adding a New Tab Button
```xml
<syncfusion:TabControlExt IsNewButtonEnabled="True" 
                          NewButtonClick="TabControl_NewButtonClick"
                          Name="tabControl" />
```

```csharp
private void TabControl_NewButtonClick(object sender, EventArgs e)
{
    TabItemExt newItem = new TabItemExt() 
    {
        Header = $"Tab {tabControl.Items.Count + 1}",
        Content = new TextBlock() { Text = "New tab content" }
    };
    tabControl.Items.Add(newItem);
}
```

### Pattern 5: Enabling Navigation with Scroll Buttons
```xml
<syncfusion:TabControlExt TabScrollButtonVisibility="Visible"
                          TabScrollStyle="Extended"
                          Name="tabControl" />
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Items` | Collection | Collection of TabItemExt objects |
| `SelectedItem` | TabItemExt | Currently selected tab item |
| `TabStripPlacement` | Dock | Tab position (Top, Bottom, Left, Right) |
| `CloseButtonType` | CloseButtonType | Close button display mode |
| `IsNewButtonEnabled` | bool | Show/hide new tab button |
| `ShowTabListContextMenu` | bool | Enable tab list navigation menu |
| `ShowTabItemContextMenu` | bool | Enable built-in context menu |
| `TabScrollButtonVisibility` | Visibility | Show/hide navigation scroll buttons |

## Key Events

| Event | Purpose |
|-------|---------|
| `SelectedItemChangedEvent` | Fires when selected tab changes |
| `NewButtonClick` | Fires when new tab button is clicked |
| `TabItemClosing` | Fires before tab item closes |
| `TabItemClosed` | Fires after tab item closes |

## Common Use Cases

1. **Document Editor Interface** - Multiple open documents as tabs with close buttons
2. **Settings Dashboard** - Different settings categories organized as tabs
3. **Multi-View Application** - Switch between different data views or modules
4. **Tool Palettes** - Different tool options grouped in tabs
5. **Properties Panel** - Different property categories in collapsible tabs
