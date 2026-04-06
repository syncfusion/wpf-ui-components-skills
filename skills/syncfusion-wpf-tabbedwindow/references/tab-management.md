# Tab Management in WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Close Button Support](#close-button-support)
- [New Tab Button](#new-tab-button)
- [Tab Selection](#tab-selection)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Mouse Interactions](#mouse-interactions)
- [Programmatic Tab Operations](#programmatic-tab-operations)
- [Common Patterns](#common-patterns)

## Overview

The Tabbed Window provides comprehensive tab management capabilities for dynamic, interactive tab-based interfaces. This guide covers:

- **Close buttons** on individual tabs
- **New tab button** for user-initiated tab creation
- **Tab selection** programmatic and user-driven
- **Keyboard shortcuts** for power users
- **Mouse interactions** including middle-click close
- **Programmatic operations** for add, remove, and select

These features enable flexible, responsive tab management similar to modern web browsers and IDEs.

## Close Button Support

Display close buttons on tabs to allow users to close individual tabs. Each tab can independently control whether its close button is visible.

### Basic Close Button Configuration

Set `CloseButtonVisibility="Visible"` on SfTabItem. Apply to all tabs via ItemContainerStyle.

### Close Button Behavior

- Tab automatically removed when close button clicked
- Next tab selected if available, otherwise previous
- Use custom close button in HeaderTemplate for confirmation dialogs

### Preventing Last Tab Close

Check Items.Count before allowing close.

## New Tab Button

Enable new tab (+) button:

```xml
<syncfusion:SfTabControl 
    EnableNewTabButton="True"
    NewTabRequested="OnNewTabRequested"/>
```

Handle NewTabRequested event to create new tabs. Customize appearance with NewTabButtonStyle.

## Tab Selection

Set active tab with `SelectedItem` (by item) or `SelectedIndex` (by position). Handle `SelectionChanged` event to respond to user interactions.

```csharp
tabControl.SelectionChanged += OnTabSelectionChanged;

private void OnTabSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var newTab = e.AddedItems[0] as SfTabItem;
        Console.WriteLine($"Switched to: {newTab?.Header}");
    }
    
    if (e.RemovedItems.Count > 0)
    {
        var oldTab = e.RemovedItems[0] as SfTabItem;
        Console.WriteLine($"Left: {oldTab?.Header}");
    }
}
```

### Cycling Through Tabs

Navigate tabs programmatically:

```csharp
// Next tab
private void SelectNextTab()
{
    int current = tabControl.SelectedIndex;
    int next = (current + 1) % tabControl.Items.Count;
    tabControl.SelectedIndex = next;
}

// Previous tab
private void SelectPreviousTab()
{
    int current = tabControl.SelectedIndex;
    int previous = (current - 1 + tabControl.Items.Count) % tabControl.Items.Count;
    tabControl.SelectedIndex = previous;
}
```

## Keyboard Shortcuts

The Tabbed Window supports built-in keyboard shortcuts for efficient tab navigation.

### Built-In Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Ctrl + Tab` | Next tab | Move to the next tab |
| `Ctrl + Shift + Tab` | Previous tab | Move to the previous tab |
| `Ctrl + T` | New tab | Create new tab (requires implementation) |
| Middle-click | Close tab | Close the clicked tab |

### Implementing Ctrl+T for New Tab

```csharp
// In window constructor or initialization
this.PreviewKeyDown += OnPreviewKeyDown;

private void OnPreviewKeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+T for new tab
    if (e.Key == Key.T && Keyboard.Modifiers == ModifierKeys.Control)
    {
        CreateNewTab();
        e.Handled = true;
    }
    
    // Ctrl+W for close current tab
    if (e.Key == Key.W && Keyboard.Modifiers == ModifierKeys.Control)
    {
        CloseCurrentTab();
        e.Handled = true;
    }
}

private void CreateNewTab()
{
    var args = new NewTabRequestedEventArgs();
    OnNewTabRequested(tabControl, args);
    
    if (args.Item != null)
    {
        tabControl.Items.Add(args.Item);
        tabControl.SelectedItem = args.Item;
    }
}

private void CloseCurrentTab()
{
    var currentTab = tabControl.SelectedItem as SfTabItem;
    if (currentTab != null && currentTab.CloseButtonVisibility == Visibility.Visible)
    {
        tabControl.Items.Remove(currentTab);
    }
}
```

## Mouse Interactions

### Middle-Click to Close

The control supports middle-click (mouse wheel click) to close tabs by default.

**Behavior:**
- Middle-click on tab header closes the tab
- Works only if `CloseButtonVisibility` is `Visible`
- Automatically selects next/previous tab

**No additional code required** - this is built-in functionality.

## Programmatic Tab Operations

### Adding Tabs

Create SfTabItem instances with Header, Content, and CloseButtonVisibility. Use tabControl.Items.Add() or Items.Insert() to add tabs.

### Removing Tabs

Call tabControl.Items.Remove(tab) or Items.RemoveAt(index) to remove tabs. Clear all with Items.Clear().

## Common Patterns

### Pattern 1: Document Editor

Configure SfTabControl with AllowDragDrop, EnableNewTabButton, and ItemContainerStyle with CloseButtonVisibility.

### Pattern 2: Session Management

Save open tabs to JSON file with header, content, and selection state. Restore on application startup.

### Pattern 3: Modified Tab Indicator

Append asterisk to tab header when content is modified. Remove when saved.
