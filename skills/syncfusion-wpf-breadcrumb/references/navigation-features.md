# Navigation Features

The HierarchyNavigator provides a rich set of navigation features: tooltips, navigation history, a refresh button, progress bar feedback, keyboard support, and level restrictions. This reference covers configuration and usage of each.

## Table of Contents
- [Tooltips](#tooltips)
- [Navigation History](#navigation-history)
- [Refresh Button](#refresh-button)
- [Progress Bar](#progress-bar)
- [Keyboard Support](#keyboard-support)
- [Mouse Support](#mouse-support)
- [Restricting Level Selection](#restricting-level-selection)
- [Handling Navigation Popup Events](#handling-navigation-popup-events)

---

## Tooltips

Enable tooltips to display the full path text when hovering over breadcrumb items — useful when items are truncated.

**Enable tooltip:**
```xml
<syncfusion:HierarchyNavigator
    x:Name="hierarchyNavigator"
    ShowToolTip="True"
    ItemsSource="{Binding Items}" />
```

**C# equivalent:**
```csharp
hierarchyNavigator.ShowToolTip = true;
```

**Behavior:**
- Tooltip appears on hover over each `HierarchyNavigatorItem`
- Tooltip text shows the item's `Content` value (the display text)
- `ShowToolTip` is `false` by default

---

## Navigation History

The HierarchyNavigator automatically maintains a **navigation history**. Clicking the back-arrow button (left of the breadcrumb) reveals a dropdown of previously visited paths.

**No configuration required** — history tracking is built-in and always active.

**How it works:**
- Each navigation (clicking a node or confirming edit mode) adds the previous path to history
- The back-arrow dropdown lists recent paths in reverse chronological order
- Clicking a history entry navigates back to that path

**Use case:** Ideal for file browser UIs where users frequently navigate back to recently visited locations.

---

## Refresh Button

The refresh button appears at the right end of the control. Wire up the `HierarchyNavigatorRefreshButtonClick` event to reload data or re-query the current path.

**XAML (using EventToCommand or code-behind):**
```xml
<syncfusion:HierarchyNavigator
    x:Name="hierarchyNavigator"
    HierarchyNavigatorRefreshButtonClick="OnRefreshClicked"
    ItemsSource="{Binding Items}" />
```

**C# event handler:**
```csharp
private void OnRefreshClicked(object sender, RoutedEventArgs e)
{
    // Reload data for the current path
    var currentItem = hierarchyNavigator.SelectedItem as HierarchyNavigatorItem;
    if (currentItem != null)
    {
        // Re-query or reload children of the current item
        LoadChildren(currentItem.Content?.ToString());
    }
}
```

**Typical refresh pattern:**
```csharp
private void OnRefreshClicked(object sender, RoutedEventArgs e)
{
    // Show progress while reloading
    hierarchyNavigator.ShowProgressBar();
    
    Task.Run(() => ReloadData())
        .ContinueWith(_ =>
        {
            Dispatcher.Invoke(() => hierarchyNavigator.CancelProgressBar());
        });
}
```

---

## Progress Bar

Display a progress bar inside the HierarchyNavigator during long-running operations (data loading, network requests, etc.).

### Show Progress Bar

**Default duration (500ms):**
```csharp
hierarchyNavigator.ShowProgressBar();
```

**Custom duration:**
```csharp
// Show for 1000 milliseconds
hierarchyNavigator.ShowProgressBar(new TimeSpan(0, 0, 0, 0, 1000));
```

### Cancel Progress Bar

**Cancel immediately:**
```csharp
hierarchyNavigator.CancelProgressBar();
```

**Cancel with delay:**
```csharp
// Cancel after 500ms delay
hierarchyNavigator.CancelProgressBar(new TimeSpan(0, 0, 0, 0, 500));
```

### Full usage pattern

```csharp
private async void LoadDataForPath(string path)
{
    // Show progress
    hierarchyNavigator.ShowProgressBar(new TimeSpan(0, 0, 0, 0, 5000));
    
    try
    {
        var items = await FetchItemsAsync(path);
        UpdateNavigatorItems(items);
    }
    finally
    {
        // Cancel regardless of success or error
        hierarchyNavigator.CancelProgressBar();
    }
}
```

**When to use:**
- After `HierarchyNavigatorRefreshButtonClick` fires
- During async data loading on initial bind
- When navigating triggers a network/database request for children

---

## Keyboard Support

The HierarchyNavigator supports full keyboard navigation:

| Key | Action |
|-----|--------|
| `←` Left Arrow | Navigate to the previous (parent) level |
| `→` Right Arrow | Navigate to the next (child) level — opens dropdown if children exist |
| `↑` Up Arrow | Move focus to the previous item in the dropdown popup |
| `↓` Down Arrow | Move focus to the next item in the dropdown popup |
| `Home` | Move focus to the first item in the dropdown popup |
| `End` | Move focus to the last item in the dropdown popup |
| `Enter` | Select the focused dropdown item and close the popup |
| `Esc` | Close the dropdown popup without selecting / cancel edit mode |
| `Page Up` | Scroll up in the dropdown popup list |
| `Page Down` | Scroll down in the dropdown popup list |

**No configuration required** — keyboard navigation is built-in.

---

## Mouse Support

Standard mouse interactions supported out of the box:

| Interaction | Action |
|-------------|--------|
| Left-click on breadcrumb item | Navigate to that level |
| Left-click on dropdown arrow (▶) | Open the child items popup |
| Left-click on item in popup | Navigate to the selected child |
| Left-click on back arrow | Open navigation history dropdown |
| Left-click on refresh icon | Fire `HierarchyNavigatorRefreshButtonClick` |
| Left-click on breadcrumb text (edit mode) | Enter edit mode (if `IsEnableEditMode="True"`) |
| Scroll wheel in popup | Scroll through items in the popup list |

---

## Restricting Level Selection

To prevent users from navigating to certain levels (e.g., prevent selecting the root), handle the navigation selection event and conditionally cancel the selection.

**Pattern — prevent root-level navigation:**
```csharp
hierarchyNavigator.SelectionChanged += OnSelectionChanged;

private void OnSelectionChanged(object sender, 
    System.Windows.Controls.SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var item = e.AddedItems[0] as HierarchyNavigatorItem;
        if (item != null && IsRestrictedLevel(item))
        {
            // Cancel by reverting to previous item
            // Note: HierarchyNavigator doesn't have a built-in cancel;
            // implement by tracking last valid selection
            hierarchyNavigator.SelectedItem = _lastValidItem;
            return;
        }
        _lastValidItem = item;
    }
}
```

**MVVM approach — restrict via CanExecute:**
```csharp
public ICommand SelectItemCommand => new DelegateCommand<HierarchyNavigatorItem>(
    execute: item => NavigateTo(item),
    canExecute: item => item != null && item.Level > 0  // Restrict root (level 0)
);
```

---

## Handling Navigation Popup Events

The popup that appears when clicking a breadcrumb item's dropdown arrow fires events you can handle to customize behavior.

**Popup opened/closed:**
```xml
<syncfusion:HierarchyNavigator
    x:Name="hierarchyNavigator"
    ItemsSource="{Binding Items}" />
```

```csharp
// Access the popup host via loaded event if needed
hierarchyNavigator.Loaded += (s, e) =>
{
    // Customize popup behavior after control loads
};
```

**Load children on popup open (on-demand pattern):**
```csharp
// In your data model, populate children lazily when the popup opens:
public class FileSystemItem
{
    private ObservableCollection<FileSystemItem> _children;
    
    public ObservableCollection<FileSystemItem> Children
    {
        get
        {
            if (_children == null)
            {
                _children = new ObservableCollection<FileSystemItem>();
                // Load children now (triggered by HierarchicalDataTemplate binding)
                foreach (var dir in Directory.GetDirectories(FullPath))
                    _children.Add(new FileSystemItem(dir));
            }
            return _children;
        }
    }
}
```

---

## Related References

- [Getting Started](getting-started.md) — Initial setup
- [Edit Mode and AutoComplete](edit-mode-and-autocomplete.md) — Edit mode keyboard interaction
- [Command Binding](command-binding.md) — MVVM commands on navigation
- [Appearance and Themes](appearance-and-themes.md) — Visual customization
