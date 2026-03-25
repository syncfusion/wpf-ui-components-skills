# Edit Mode and AutoComplete

The HierarchyNavigator supports an edit mode where users can type a path directly into the control, with AutoComplete suggestions filtering as they type — similar to the Windows Explorer address bar.

## Table of Contents
- [Enabling Edit Mode](#enabling-edit-mode)
- [How Edit Mode Works](#how-edit-mode-works)
- [AutoComplete Behavior](#autocomplete-behavior)
- [Keyboard Interaction in Edit Mode](#keyboard-interaction-in-edit-mode)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

---

## Enabling Edit Mode

Set `IsEnableEditMode="True"` to allow users to click the control and type a path.

**XAML:**
```xml
<syncfusion:HierarchyNavigator
    x:Name="hierarchyNavigator"
    IsEnableEditMode="True"
    ItemsSource="{Binding Items}"
    ItemTemplate="{StaticResource NavigatorTemplate}" />
```

**C# (code-behind):**
```csharp
hierarchyNavigator.IsEnableEditMode = true;
```

**Default:** `IsEnableEditMode` is `false` — users cannot type in the control without this enabled.

---

## How Edit Mode Works

When edit mode is enabled, the user workflow is:

1. **Click** on the `HierarchyNavigatorItemsControl` (the breadcrumb display area)
2. The control switches to an **editable text box** showing the current path
3. The user types or modifies the path
4. The control shows an **AutoComplete dropdown** with matching nodes
5. The user selects a match or presses Enter/Esc

**Visual state transition:**
```
[Breadcrumb View]  →  [Click]  →  [Text Box with path]  →  [AutoComplete dropdown]
   Root > Folder                    Root\Folder\_              matching items list
```

---

## AutoComplete Behavior

As the user types, the AutoComplete dropdown filters available nodes based on the typed text.

**Behavior details:**
- Dropdown populates with **all nodes matching** the current typed path segment
- Matching is typically **case-insensitive**
- If the typed text matches a path exactly, the dropdown may show sub-items of that level
- **No match found:** The dropdown closes; no item is auto-selected

**Example scenario:**
```
User types: "Root\Syncf"
Dropdown shows:
  ├── Root\Syncfusion
  └── Root\SyncfusionTools

User types: "Root\SyncfusionX" (no match)
Dropdown: closes, no selection made
```

**When data is bound:** AutoComplete works with both declarative `HierarchyNavigatorItem` and data-bound `ItemsSource` scenarios. The control introspects the item hierarchy to build suggestions.

---

## Keyboard Interaction in Edit Mode

| Key | Action |
|-----|--------|
| `Enter` | Confirms the current AutoComplete selection; exits edit mode and navigates to the selected path |
| `Esc` | Cancels edit mode; reverts to the previously displayed breadcrumb path (no navigation) |
| `↓` Arrow | Moves focus down in the AutoComplete dropdown |
| `↑` Arrow | Moves focus up in the AutoComplete dropdown |
| `Backspace` | Deletes the last character; AutoComplete updates accordingly |

**Enter confirmation flow:**
```csharp
// After Enter is pressed, the control fires selection-changed with the chosen item
// This is equivalent to clicking a node in the breadcrumb normally
hierarchyNavigator.Command = selectItemCommand; // Command fires with new item
```

**Esc cancellation flow:**
- Control reverts to displaying the last valid selected path
- No command or event fires for the cancelled edit
- The `HierarchyNavigatorRefreshButtonClick` event does NOT fire on Esc

---

## Edge Cases and Troubleshooting

**Edit mode not activating:**
- Ensure `IsEnableEditMode="True"` is set (default is `false`)
- Click specifically on the items control area, not on a dropdown arrow or separator

**AutoComplete not showing:**
- Verify the control has a valid `ItemsSource` or declarative items
- With `HierarchicalDataTemplate`, ensure `ItemsSource` binding inside the template is correct — AutoComplete traverses these bindings

**Invalid path typed → no navigation:**
- If the user types a path with no matching node and presses Enter, the control does **not** navigate
- The breadcrumb reverts to the last valid selection
- No error is thrown; handle this gracefully by validating in your ViewModel command

**Edit mode with read-only scenarios:**
```xml
<!-- Disable edit mode for read-only navigation (e.g., view-only breadcrumb) -->
<syncfusion:HierarchyNavigator
    IsEnableEditMode="False"
    ItemsSource="{Binding Items}" />
```

**Programmatically entering edit mode:**
The control does not expose a direct method to programmatically trigger edit mode. It is user-initiated via click interaction.

---

## Related References

- [Getting Started](getting-started.md) — Initial setup and declarative items
- [Populating Data](populating-data.md) — Data binding patterns that affect AutoComplete
- [Navigation Features](navigation-features.md) — Keyboard navigation table, history, and refresh
- [Command Binding](command-binding.md) — MVVM command pattern fired after edit mode confirms
