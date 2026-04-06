# Merge Tabs Between Windows in WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Tear-Off Windows](#tear-off-windows)
- [Floating Window Lifecycle](#floating-window-lifecycle)
- [Cross-Window Tab Merging](#cross-window-tab-merging)
- [PreviewTabMerge Event](#previewtabmerge-event)
- [Merge Validation](#merge-validation)
- [Common Patterns](#common-patterns)
- [Edge Cases and Gotchas](#edge-cases-and-gotchas)

## Overview

The Tabbed Window supports advanced multi-window scenarios through tear-off and tab merging functionality. Users can:

- **Tear off tabs** to create floating windows
- **Merge tabs** between windows via drag-drop
- **Validate merges** before they occur
- **Transform items** during the merge process
- **Organize workspaces** flexibly across multiple windows

This enables professional multi-monitor workflows similar to Visual Studio, Chrome, and other modern applications.

**Key Features:**
- Drag tabs outside window to create floating windows
- Drag tabs between windows to merge
- Event-driven merge validation
- Automatic window cleanup when empty
- Full control over merge operations

## Tear-Off Windows

Enable tear-off by setting `AllowDragDrop="True"` on SfTabControl. Drag tabs outside window boundary to create floating windows. System automatically creates new SfChromelessWindow with the torn-off tab and provides drag-drop visual feedback.

## Floating Window Lifecycle

When a tab is torn off, system automatically creates new SfChromelessWindow with same WindowType. Tab removed from source window and added to floating window. Floating window auto-closes when last tab is removed.
// - Similar size and styling
// - The torn-off tab as content
// - AllowDragDrop enabled
```

**Properties Inherited:**

The floating window inherits certain properties from the source:
- `WindowType` (Tabbed or Normal)
- Theme and styling
- Tab control configuration (close buttons, drag-drop)

### Positioning

Floating windows are positioned:
- Near the drop point (cursor location)
- Adjusted to stay within screen bounds
- With reasonable default size
- Can be moved/resized normally

### Reattaching Floating Tabs

To reattach a tab from a floating window back to the main window:

**User Actions:**

1. **Click and drag** tab from floating window
2. **Drag into** main window's tab area
3. **Release mouse** over tab strip

**System Behavior:**

1. Tab is removed from floating window
2. Tab is added to target window at drop position
3. Floating window closes if empty (no tabs remain)
4. Tab becomes active in target window

**Example:**

```
┌─────────────────────┐
│ Main Window         │
│ [Tab1] [Tab2] [Tab3]│ ← Drop here
└─────────────────────┘

┌──────────────┐
│ Floating     │
│ [Document] ×─┤ ← Drag from here
└──────────────┘
```

### Empty Window Cleanup

When the last tab is removed from a floating window:

**Automatic Behavior:**
- Floating window automatically closes
- No empty windows remain
- Focus returns to previous window

**Preventing Auto-Close:**

To keep a window open even when empty:

```csharp
// In floating window
private void OnTabRemoving(object sender, EventArgs e)
{
    if (MainTabControl.Items.Count == 1)
    {
        // Add a placeholder tab
        var placeholder = new SfTabItem
        {
            Header = "Empty",
            Content = new TextBlock { Text = "No documents open" }
        };
        MainTabControl.Items.Add(placeholder);
    }
}
```

## Cross-Window Tab Merging

Tabs can be dragged between windows with `AllowDragDrop="True"`. Tab is removed from source window and added to target window at drop position. Merge behavior: tab becomes active in target, source window adjusts remaining tabs, floating windows auto-close when empty.

## PreviewTabMerge Event

`PreviewTabMerge` event fires before tab merge. Access DraggedItem, SourceControl, TargetControl properties. Set `e.Allow = false` to cancel merge, or set `e.ResultingItem` to transform item during merge.

## Merge Validation

Use PreviewTabMerge to validate merges:

- **Deny all merges:** Set `e.Allow = false`
- **Validate content:** Check tab Tag/content for locked/read-only status
- **Validate window types:** Ensure compatible source/target windows
- **Enforce limits:** Check `TargetControl.Items.Count` against maximum
- **Show confirmations:** Display MessageBox before allowing merge

## Common Patterns

**Unrestricted:** Enable `AllowDragDrop="True"` with no validation.

**Validated:** Implement PreviewTabMerge with permission checks (locked status, window type, tab count limits).
