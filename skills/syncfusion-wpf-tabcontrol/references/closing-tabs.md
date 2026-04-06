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

Configure close button display with `CloseButtonType`:
- **Common**: Single close button for entire TabControl
- **Individual**: Close button on each tab header (document-based)
- **Both**: Both control-level and individual buttons
- **Hide**: No close buttons (static tabs)
- **IndividualOnMouseOver**: Close button appears on hover (clean UI)
- **Extended**: Close button on selected tab only; hover for others

## Protecting Specific Tabs

Disable closing with `CanClose="False"` (close button grayed out; user cannot close). Hide close button entirely with `CloseButtonState="Collapsed"`. Programmatic removal still possible even with `CanClose="False"`. Toggle dynamically: `tabItem.CanClose = true/false` and `tabItem.CloseButtonState = Visibility.Collapsed/Visible`.

## Close Events

Handle tab closing events to perform cleanup, validation, or prompt users before closing.

### TabItemClosing Event

Fires before a tab is closed. Use this to validate or prevent closing:

```csharp
tabControl.TabItemClosing += (sender, e) => {
    TabItemExt item = e.Source as TabItemExt;
    if (NeedsConfirmation(item))
    {
        e.Cancel = true;  // Prevent closing
    }
};
```

### TabItemClosed Event

Fires after a tab has been successfully closed:

```csharp
tabControl.TabItemClosed += (sender, e) => {
    TabItemExt closedItem = e.Source as TabItemExt;
    // Perform cleanup or update UI after tab is closed
};
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

## Common Closing Patterns

- **Save Before Closing**: Check for unsaved changes in `TabItemClosing` event; show confirmation dialog (Yes/No/Cancel); call `SaveTabContent()` then `tabControl.Items.Remove(tabItem)`
- **Close All Except Current**: Iterate `tabControl.Items`, collect all tabs except `tabControl.SelectedItem`, then remove them
- **Prevent Last Tab Close**: Check `if (tabControl.Items.Count == 1)` in `TabItemClosing` event and set `e.Cancel = true`
- **Confirmation Dialog**: Show MessageBox in `TabItemClosing` event; set `e.Cancel = true` if user cancels

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

- Set `CanClose="False"` for protected tabs
- Show save dialogs before closing unsaved content
- Clean up resources when tabs close
- Test close button styling across different placements
