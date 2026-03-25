# Tab Closing and Close Button Configuration

## Table of Contents
- [Overview](#overview)
- [Close Button Types](#close-button-types)
- [Controlling Close Visibility](#controlling-close-visibility)
- [Disabling Close on Specific Tabs](#disabling-close-on-specific-tabs)
- [Close Events](#close-events)
- [Advanced Closing Scenarios](#advanced-closing-scenarios)

## Overview

The TabControl provides flexible options for controlling how and when users can close tabs. You can configure close button display modes, disable closing for specific tabs, and handle close events to perform cleanup or validation.

## Close Button Types

Configure how close buttons appear using the `CloseButtonType` property. Different modes suit different application needs.

### CloseButtonType.Common
Shows a single close button for the entire TabControl (not per tab):

```xml
<syncfusion:TabControlExt CloseButtonType="Common" Name="tabControl" />
```

```csharp
tabControl.CloseButtonType = CloseButtonType.Common;
```

**When to use:** When you want to close all or multiple tabs at once from a single button.

### CloseButtonType.Individual
Shows close buttons only in individual tab headers:

```xml
<syncfusion:TabControlExt CloseButtonType="Individual" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" />
</syncfusion:TabControlExt>
```

```csharp
tabControl.CloseButtonType = CloseButtonType.Individual;
```

**When to use:** For document-based interfaces where each tab can be independently closed.

### CloseButtonType.Both
Shows close buttons on both the TabControl and individual tab headers:

```xml
<syncfusion:TabControlExt CloseButtonType="Both" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" />
</syncfusion:TabControlExt>
```

```csharp
tabControl.CloseButtonType = CloseButtonType.Both;
```

**When to use:** When you want both individual tab closing and a TabControl-level option.

### CloseButtonType.Hide
Hides all close buttons:

```xml
<syncfusion:TabControlExt CloseButtonType="Hide" Name="tabControl" />
```

```csharp
tabControl.CloseButtonType = CloseButtonType.Hide;
```

**When to use:** When tabs should not be closable by users (static tab layout).

### CloseButtonType.IndividualOnMouseOver
Shows close button only when hovering over a tab header:

```xml
<syncfusion:TabControlExt CloseButtonType="IndividualOnMouseOver" Name="tabControl" />
```

```csharp
tabControl.CloseButtonType = CloseButtonType.IndividualOnMouseOver;
```

**When to use:** For cleaner UI that reveals close buttons on demand.

### CloseButtonType.Extended
Shows close button only for the selected tab; other tabs show it on hover:

```xml
<syncfusion:TabControlExt CloseButtonType="Extended" Name="tabControl" />
```

```csharp
tabControl.CloseButtonType = CloseButtonType.Extended;
```

**When to use:** For applications with many tabs where you want to emphasize the active tab's close button.

## Controlling Close Visibility

### Basic Close Button Display

The simplest configuration for enabling close buttons on individual tabs:

```xml
<syncfusion:TabControlExt CloseButtonType="Individual" Name="tabControl">
    <syncfusion:TabItemExt Header="File 1">
        <TextBlock Text="File 1 Content" />
    </syncfusion:TabItemExt>
    <syncfusion:TabItemExt Header="File 2">
        <TextBlock Text="File 2 Content" />
    </syncfusion:TabItemExt>
</syncfusion:TabControlExt>
```

## Disabling Close on Specific Tabs

### Using CanClose Property

Prevent specific tabs from being closed by setting `CanClose` to `false`:

```xml
<syncfusion:TabControlExt CloseButtonType="Both" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2 (Cannot Close)" CanClose="False" />
    <syncfusion:TabItemExt Header="Tab 3" />
</syncfusion:TabControlExt>
```

When `CanClose="False"`:
- The close button is disabled (grayed out)
- User cannot click the close button
- The tab cannot be closed through any user action
- Programmatic removal is still possible

### C# Approach
```csharp
tabItem2.CanClose = false;
```

### Disabling Close Button Visibility

Hide the close button entirely for specific tabs while keeping the button visible for others:

```xml
<syncfusion:TabControlExt CloseButtonType="Both" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2 (No Close Button)" 
                           CloseButtonState="Collapsed" 
                           CanClose="False" />
    <syncfusion:TabItemExt Header="Tab 3" />
</syncfusion:TabControlExt>
```

```csharp
tabItem2.CloseButtonState = Visibility.Collapsed;
tabItem2.CanClose = false;
```

**Result:** Tab 2 will have no close button visible, while Tab 1 and Tab 3 will have visible close buttons.

### Toggling Close Ability Dynamically
```csharp
public void LockTab(TabItemExt tab)
{
    tab.CanClose = false;
    tab.CloseButtonState = Visibility.Collapsed;
}

public void UnlockTab(TabItemExt tab)
{
    tab.CanClose = true;
    tab.CloseButtonState = Visibility.Visible;
}
```

## Close Events

Handle tab closing events to perform cleanup, validation, or prompt users before closing.

### TabItemClosing Event

Fires before a tab is closed. Use this to validate or prevent closing:

```csharp
tabControl.PreviewMouseLeftButtonDown += TabControl_PreviewMouseLeftButtonDown;

private void TabControl_PreviewMouseLeftButtonDown(object sender, MouseButtonEventArgs e)
{
    // Custom close event handling
}
```

### TabItemClosed Event

Fires after a tab has been successfully closed:

```csharp
// Note: TabControl doesn't have a built-in TabItemClosed event
// Use SelectedItemChangedEvent to detect when tabs are removed
```

### Preventing Tab Closure

Cancel the close operation by handling events and setting appropriate flags:

```csharp
private bool _allowClose = true;

private void HandleTabClosing()
{
    if (!_allowClose)
    {
        // Prevent close by not removing the item
        return;
    }
    
    // Allow close to proceed
}
```

## Advanced Closing Scenarios

### Scenario 1: Save Changes Before Closing
```csharp
public void CloseTabWithValidation(TabItemExt tabItem)
{
    bool hasUnsavedChanges = CheckForUnsavedChanges(tabItem);
    
    if (hasUnsavedChanges)
    {
        MessageBoxResult result = MessageBox.Show(
            $"Save changes to '{tabItem.Header}'?",
            "Unsaved Changes",
            MessageBoxButton.YesNoCancel
        );
        
        switch (result)
        {
            case MessageBoxResult.Yes:
                SaveTabContent(tabItem);
                RemoveTab(tabItem);
                break;
            case MessageBoxResult.No:
                RemoveTab(tabItem);
                break;
            case MessageBoxResult.Cancel:
                // Don't close
                break;
        }
    }
    else
    {
        RemoveTab(tabItem);
    }
}

private void RemoveTab(TabItemExt tabItem)
{
    tabControl.Items.Remove(tabItem);
}

private bool CheckForUnsavedChanges(TabItemExt tabItem)
{
    // Implement your validation logic
    return false;
}

private void SaveTabContent(TabItemExt tabItem)
{
    // Implement save logic
}
```

### Scenario 2: Close All Tabs Except Current
```csharp
private void CloseAllButCurrent()
{
    TabItemExt currentTab = tabControl.SelectedItem as TabItemExt;
    List<TabItemExt> tabsToRemove = new List<TabItemExt>();
    
    foreach (TabItemExt item in tabControl.Items)
    {
        if (item != currentTab)
        {
            tabsToRemove.Add(item);
        }
    }
    
    foreach (TabItemExt tab in tabsToRemove)
    {
        tabControl.Items.Remove(tab);
    }
}
```

### Scenario 3: Prevent Closing Last Tab
```csharp
public override void OnTabClose(TabItemExt tabItem)
{
    if (tabControl.Items.Count == 1)
    {
        MessageBox.Show("Cannot close the last tab");
        return;
    }
    
    tabControl.Items.Remove(tabItem);
}
```

### Scenario 4: Close with Animation/Feedback
```csharp
private async Task CloseTabAsync(TabItemExt tabItem)
{
    // Fade out animation
    tabItem.Opacity = 0.5;
    await Task.Delay(300);
    
    // Remove from collection
    tabControl.Items.Remove(tabItem);
    
    MessageBox.Show($"Tab '{tabItem.Header}' closed");
}
```

## Close Button State Matrix

| Configuration | Individual Close | Common Close | Behavior |
|---------------|------------------|--------------|----------|
| `CloseButtonType.Individual` | ✓ Visible | ✗ None | Close individual tabs |
| `CloseButtonType.Common` | ✗ None | ✓ Visible | Close from control button |
| `CloseButtonType.Both` | ✓ Visible | ✓ Visible | Both options available |
| `CloseButtonType.Hide` | ✗ None | ✗ None | No closing via UI |
| `CloseButtonType.IndividualOnMouseOver` | ✓ On Hover | ✗ None | Close on hover |
| `CloseButtonType.Extended` | ✓ Selected Only | ✗ None | Only selected tab shows close |

## Best Practices

1. **Meaningful Defaults**: Set `CanClose="False"` for tabs that shouldn't be closed
2. **User Feedback**: Show save dialogs before closing tabs with unsaved content
3. **Consistency**: Use the same close behavior across your application
4. **Accessibility**: Ensure close buttons are keyboard accessible
5. **Performance**: Clean up resources when tabs are closed
6. **Visual Feedback**: Consider animations when tabs are closed

## Common Issues

**Issue: Close button appears but clicking doesn't close the tab**
- Check `CanClose` property is set to `true`
- Verify no event handlers are preventing the close operation
- Ensure the tab item is not locked or protected

**Issue: Cannot close all tabs**
- Consider whether you need at least one tab open
- Implement validation to prevent closing the last tab if required
- Check application logic for required open tabs

**Issue: Close button position or styling looks wrong**
- Check `CloseButtonType` setting
- Verify theme/styling hasn't overridden button appearance
- Test with different `TabStripPlacement` values
