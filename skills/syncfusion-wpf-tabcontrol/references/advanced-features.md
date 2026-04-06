# Advanced Features and Interactions

## Table of Contents
- [Context Menus](#context-menus)
- [Custom Context Menu Items](#custom-context-menu-items)
- [Tab Reordering](#tab-reordering)
- [New Tab Button](#new-tab-button)
- [Tab Navigation and Scrolling](#tab-navigation-and-scrolling)

## Context Menus

The TabControl provides built-in context menu support at two levels: tab control level and tab item level.

### Tab Context Menus

Enable tab list menu (top-right dropdown) with `ShowTabListContextMenu="True"`. Customize with `TabListContextMenuOptions` (Default, ShowHiddenItems, ShowDisabledItems).

Enable tab item context menu (right-click) with `ShowTabItemContextMenu="True"`. Built-in options: Close, Close All But This, Close All.

## Custom Context Menu Items

Enable custom menus with `IsCustomTabItemContextMenuEnabled="True"`. Add `CustomMenuItem` elements to `TabItemExt.ContextMenuItems`. Bind to commands via `Command="{Binding SaveCommand}"` or handle via `Click` event. Add icons via `CustomMenuItem.Icon` with Image element.

## Tab Reordering

Enable users to reorder tabs by dragging and dropping:

### Enabling Drag and Drop

The TabControl supports drag-drop reordering by default. Users can click and drag tab headers to reorder them.

### Programmatic Reordering

Move tabs programmatically:

```csharp
public void MoveTabToPosition(TabItemExt tab, int newIndex)
{
    int currentIndex = tabControl.Items.IndexOf(tab);
    
    if (currentIndex >= 0 && newIndex >= 0 && newIndex < tabControl.Items.Count)
    {
        tabControl.Items.RemoveAt(currentIndex);
        tabControl.Items.Insert(newIndex, tab);
    }
}

// Usage
MoveTabToPosition(tabItem, 0);  // Move to first position
```

### Reordering with Constraints

Maintain a `HashSet<TabItemExt>` of locked tabs. Override drag-drop event handlers to check if tab is locked and prevent the operation. Use `tabControl.Items.RemoveAt(currentIndex); tabControl.Items.Insert(newIndex, tab)` for programmatic reordering.

## New Tab Button

Add a button to create new tabs:

### Enabling New Tab Button

```xml
<syncfusion:TabControlExt IsNewButtonEnabled="True" 
                          NewButtonClick="TabControl_NewButtonClick"
                          Name="tabControl" />
```

```csharp
tabControl.IsNewButtonEnabled = true;
tabControl.NewButtonClick += TabControl_NewButtonClick;
```

### Handling New Tab Button Click

```csharp
private void TabControl_NewButtonClick(object sender, EventArgs e)
{
    // Create new tab
    TabItemExt newTab = new TabItemExt()
    {
        Header = $"Tab {tabControl.Items.Count + 1}",
        Content = new TextBlock() { Text = "New tab content" }
    };
    
    // Add to control
    tabControl.Items.Add(newTab);
    
    // Select the new tab
    tabControl.SelectedItem = newTab;
}
```



## Tab Navigation and Scrolling

### Navigation Buttons

Show navigation buttons for scrolling through tabs:

```xml
<syncfusion:TabControlExt TabScrollButtonVisibility="Visible"
                          TabScrollStyle="Extended"
                          Name="tabControl" />
```

**TabScrollStyle Options:**
- `Standard` - Previous and Next buttons
- `Extended` - First, Previous, Next, and Last buttons

### Programmatic Navigation

Cycle through tabs: `(tabControl.SelectedIndex + 1) % tabControl.Items.Count` for next, decrement with wraparound for previous. Scroll tab into view: `tab.BringIntoView()` after setting selection.

**Tab Search/Filter**: Iterate `tabControl.Items`, set `tab.Visibility = Visibility.Collapsed` for non-matching tabs, `Visibility.Visible` for matches.

## Best Practices for Advanced Features

1. **Context Menus**: Provide meaningful, context-specific options
2. **Drag and Drop**: Give clear visual feedback during reordering
3. **New Tab Button**: Provide sensible defaults for new tabs
4. **Localization**: Test thoroughly with different language strings
5. **Performance**: Monitor performance with many tabs and custom menus
6. **Accessibility**: Ensure all features are keyboard-accessible
7. **State Management**: Persist important tab states for users

## Troubleshooting Advanced Features

| Issue | Solution |
|-------|----------|
| Custom menu items not appearing | Verify `IsCustomTabItemContextMenuEnabled=True` |
| Drag-drop not working | Check for event handlers preventing drag operations |
| Localization strings not updating | Force UI refresh after culture change |
| New tab button disabled | Verify `IsNewButtonEnabled=True` |
| Navigation buttons hidden | Check `TabScrollButtonVisibility` property |
