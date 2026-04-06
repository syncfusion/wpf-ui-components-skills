# Tab Interactions and Selection

Learn how to handle tab selection, keyboard navigation, and user interactions with TabControl items.

## Tab Selection Basics

Get the selected item with `tabControl.SelectedItem as TabItemExt` or access by `tabControl.SelectedIndex` (0-based). Set selection with `tabControl.SelectedItem = tabItem` or `tabControl.SelectedIndex = 1`.

## Selection Changed Event

Handle `SelectedItemChangedEvent` to respond when selection changes. Access `e.NewSelectedItem` and `e.OldSelectedItem` (may be null on first selection) to perform cleanup/initialization. Use for validation, state saving, or content loading.

## Keyboard Navigation

Built-in shortcuts: **Ctrl+Tab** (next), **Ctrl+Shift+Tab** (previous), **Arrow keys** (Left/Right for horizontal, Up/Down for vertical when focused). For custom handling, subscribe to `KeyDown` event and check `Keyboard.Modifiers` and `e.Key`.

## Tab List Context Menu

Enable/disable with `ShowTabListContextMenu="True|False"` (default true). Use `TabListContextMenuOptions` to show hidden or disabled tabs (e.g., `TabListContextMenuOptions.Default | TabListContextMenuOptions.ShowHiddenItems`). Selection is automatic via `SelectedItemChangedEvent`.

## Tab Navigation & Visibility

Navigate programmatically: Check `tabControl.SelectedIndex` against `tabControl.Items.Count`, then increment/decrement. Use `tabItem.Visibility = Visibility.Collapsed` to hide tabs or `tabItem.IsEnabled = false` to disable. Hidden/disabled tabs can appear in context menu via `TabListContextMenuOptions`.

## Common Patterns

- **MVVM Navigation**: Bind `SelectedIndex="{Binding SelectedTabIndex}"` with `INotifyPropertyChanged` in ViewModel
- **Validation Before Switch**: Check `e.OldSelectedItem` in `SelectedItemChangedEvent`; revert with `tabControl.SelectedItem = e.OldSelectedItem` if invalid
- **Tab Lookup by Name**: Iterate `tabControl.Items` comparing `item.Header?.ToString()` to find and select

## Best Practices

1. **Always check for null**: `OldSelectedItem` can be null on initial selection
2. **Keyboard Support**: Test keyboard navigation in your application
3. **Visual Feedback**: Provide clear indication of the active tab
4. **Validation**: Validate tab content before allowing switches if needed
5. **Performance**: Avoid heavy operations in `SelectedItemChangedEvent` to maintain responsiveness
6. **Accessibility**: Ensure tab headers are descriptive for screen readers


