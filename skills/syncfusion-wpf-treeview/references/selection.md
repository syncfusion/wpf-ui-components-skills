# Selection in WPF TreeView (SfTreeView)

## Table of Contents
- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [UI Selection](#ui-selection)
- [Programmatic Selection](#programmatic-selection)
- [Binding Selection State](#binding-selection-state)
- [Selection Events](#selection-events)
- [Keyboard Navigation](#keyboard-navigation)
- [Working with Selected Items](#working-with-selected-items)

## Overview

The WPF TreeView supports flexible selection mechanisms allowing users to select items through mouse/touch interactions or programmatically. Selection behavior is controlled through the `SelectionMode` property, which offers multiple modes for different user interaction patterns.

## Selection Modes

The TreeView provides five selection modes through the `SelectionMode` property:

### None

Disables selection completely. No items can be selected by user interaction or programmatically.

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="None"
                       ItemsSource="{Binding Items}"/>
```

**Use Cases:**
- Read-only tree displays
- When selection is not required
- Navigation-only scenarios

### Single (Default)

Allows selecting only one item at a time. Clicking a selected item does **not** clear the selection.

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Single"
                       ItemsSource="{Binding Items}"/>
```

```csharp
treeView.SelectionMode = SelectionMode.Single;
```

**Behavior:**
- Click on item → selects it
- Click on different item → deselects previous, selects new
- Click on selected item → remains selected
- Default mode if not specified

**Use Cases:**
- Single item operations
- Detail view scenarios
- Master-detail patterns

### SingleDeselect

Allows selecting only one item. Clicking the selected item **clears** the selection.

```xml
<syncfusion:SfTreeView SelectionMode="SingleDeselect"/>
```

**Behavior:**
- Click on item → selects it
- Click on different item → deselects previous, selects new
- Click on selected item → **deselects** it

**Use Cases:**
- Optional single selection
- Toggle selection scenarios
- When empty selection state is valid

### Multiple

Allows selecting multiple items. Clicking on a selected item **deselects** it.

```xml
<syncfusion:SfTreeView SelectionMode="Multiple"/>
```

**Behavior:**
- Click on unselected item → adds to selection
- Click on selected item → removes from selection
- No keyboard modifiers required

**Use Cases:**
- Multi-item operations (delete, move, copy)
- Batch processing
- Touch-friendly multi-selection

### Extended

Allows multiple selection using keyboard modifiers (Ctrl, Shift).

```xml
<syncfusion:SfTreeView SelectionMode="Extended"/>
```

**Keyboard Modifiers:**
- **Click** → Single selection (clears previous)
- **Ctrl + Click** → Toggle individual item
- **Shift + Click** → Range selection
- **Ctrl + A** → Select all

**Use Cases:**
- Desktop applications with keyboard support
- Traditional Windows UI patterns
- Advanced users familiar with modifier keys

## UI Selection

Enable UI-based selection by setting `SelectionMode` to any value except `None`:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       SelectionMode="Multiple"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FolderName}" 
                       VerticalAlignment="Center"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

**User Interaction:**
- Mouse click on item
- Touch tap on item
- Keyboard navigation (Arrow keys + Enter/Space)

## Programmatic Selection

Select items programmatically using `SelectedItem` and `SelectedItems` properties.

### Single Item Selection

For `Single` or `SingleDeselect` modes, use the `SelectedItem` property:

```csharp
// Select the first item
treeView.SelectedItem = viewModel.Folders[0];

// Select a specific item
var folderToSelect = viewModel.Folders.FirstOrDefault(f => f.FolderName == "Documents");
treeView.SelectedItem = folderToSelect;

// Clear selection
treeView.SelectedItem = null;
```

**Important:** Set the underlying data object, not the TreeViewNode.

### Multiple Item Selection

For `Multiple` or `Extended` modes, use the `SelectedItems` collection:

```csharp
// Select multiple items
treeView.SelectedItems.Add(viewModel.Folders[0]);
treeView.SelectedItems.Add(viewModel.Folders[2]);
treeView.SelectedItems.Add(viewModel.Folders[4]);

// Select items matching criteria
var documentsFolder = viewModel.Folders.Where(f => f.Type == "Documents");
foreach (var folder in documentsFolder)
{
    treeView.SelectedItems.Add(folder);
}

// Clear all selections
treeView.SelectedItems.Clear();

// Replace entire selection
treeView.SelectedItems.Clear();
treeView.SelectedItems.Add(newSelectedItem);
```

### Selection Validation

```csharp
// Check if item is selected
bool isSelected = treeView.SelectedItems.Contains(specificFolder);

// Get selection count
int selectionCount = treeView.SelectedItems.Count;

// Validate selection before operation
if (treeView.SelectedItem != null)
{
    var selected = treeView.SelectedItem as FolderModel;
    // Perform operation
}
```

### Exception Handling

**⚠️ Important:** Invalid selections throw exceptions:

```csharp
// ❌ Will throw exception: SelectionMode is None
treeView.SelectionMode = SelectionMode.None;
treeView.SelectedItem = viewModel.Folders[0]; // Exception!

// ❌ Will throw exception: Multiple selection in Single mode
treeView.SelectionMode = SelectionMode.Single;
treeView.SelectedItems.Add(viewModel.Folders[0]);
treeView.SelectedItems.Add(viewModel.Folders[1]); // Exception!

// ✅ Correct approach
if (treeView.SelectionMode != SelectionMode.None)
{
    treeView.SelectedItem = viewModel.Folders[0];
}
```

## Binding Selection State

Bind the selection state directly to your data model using `IsSelectedPropertyName` in `HierarchyPropertyDescriptors`.

**Step 1:** Add `IsSelected` property to your model:

```csharp
public class FileManager : INotifyPropertyChanged
{
    private string itemName;
    private bool isSelected;
    private ObservableCollection<FileManager> subFiles;

    public string ItemName
    {
        get { return itemName; }
        set
        {
            itemName = value;
            OnPropertyChanged(nameof(ItemName));
        }
    }

    public bool IsSelected
    {
        get { return isSelected; }
        set
        {
            isSelected = value;
            OnPropertyChanged(nameof(IsSelected));
        }
    }

    public ObservableCollection<FileManager> SubFiles
    {
        get { return subFiles; }
        set
        {
            subFiles = value;
            OnPropertyChanged(nameof(SubFiles));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Step 2:** Configure `IsSelectedPropertyName` in XAML:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}">
    <syncfusion:SfTreeView.HierarchyPropertyDescriptors>
        <treeViewEngine:HierarchyPropertyDescriptor 
            TargetType="{x:Type local:FileManager}"
            ChildPropertyName="SubFiles"
            IsSelectedPropertyName="IsSelected"/>
    </syncfusion:SfTreeView.HierarchyPropertyDescriptors>
</syncfusion:SfTreeView>
```

**Step 3:** Set initial selection in ViewModel:

```csharp
public class FileManagerViewModel
{
    public ObservableCollection<FileManager> Folders { get; set; }

    public FileManagerViewModel()
    {
        Folders = new ObservableCollection<FileManager>();
        
        var documents = new FileManager 
        { 
            ItemName = "Documents",
            IsSelected = true  // Pre-selected
        };
        
        var pictures = new FileManager 
        { 
            ItemName = "Pictures",
            IsSelected = false
        };
        
        Folders.Add(documents);
        Folders.Add(pictures);
    }
}
```

**Benefits:**
- Two-way synchronization between UI and data model
- Selection state persists with data
- MVVM-friendly pattern
- Automatic UI updates when model changes

**⚠️ Important Notes:**
- Only supported in **Bound Mode** (not Unbound Mode)
- Property must be of type `bool`
- Requires `INotifyPropertyChanged` implementation

## Selection Events

Handle selection changes through events:

### SelectionChanging Event

Fires before selection changes. Can be canceled.

```csharp
treeView.SelectionChanging += TreeView_SelectionChanging;

private void TreeView_SelectionChanging(object sender, ItemSelectionChangingEventArgs e)
{
    // Access items being added/removed
    var addedItems = e.AddedItems;
    var removedItems = e.RemovedItems;
    
    // Prevent selection of specific items
    foreach (var item in e.AddedItems)
    {
        var folder = item as FolderModel;
        if (folder != null && folder.IsProtected)
        {
            e.Cancel = true; // Cancel selection
            MessageBox.Show("Cannot select protected folders");
            return;
        }
    }
}
```

### SelectionChanged Event

Fires after selection changes. Cannot be canceled.

```csharp
treeView.SelectionChanged += TreeView_SelectionChanged;

private void TreeView_SelectionChanged(object sender, ItemSelectionChangedEventArgs e)
{
    // Get newly selected items
    var addedItems = e.AddedItems;
    
    // Get deselected items
    var removedItems = e.RemovedItems;
    
    // Update UI or perform operations
    if (addedItems.Count > 0)
    {
        var selectedFolder = addedItems[0] as FolderModel;
        LoadFolderDetails(selectedFolder);
    }
}
```

**Event Arguments:**
- `AddedItems` - Items added to selection
- `RemovedItems` - Items removed from selection

## Keyboard Navigation

TreeView supports keyboard navigation when `SelectionMode` is not `None`:

### Navigation Keys

| Key | Action |
|-----|--------|
| **↑ (Up Arrow)** | Move to previous item |
| **↓ (Down Arrow)** | Move to next item |
| **← (Left Arrow)** | Collapse current node |
| **→ (Right Arrow)** | Expand current node |
| **Home** | Move to first item |
| **End** | Move to last item |
| **Enter/Space** | Select/Deselect current item |
| **Tab** | Move focus to next control |

### Extended Mode Shortcuts

Additional shortcuts in `Extended` selection mode:

| Key Combination | Action |
|----------------|--------|
| **Ctrl + A** | Select all items |
| **Ctrl + Click** | Toggle individual selection |
| **Shift + Click** | Range selection |
| **Shift + ↑/↓** | Extend selection up/down |

### CurrentItem Property

Track keyboard navigation focus:

```csharp
// Get currently focused item (may not be selected)
var focusedItem = treeView.CurrentItem;

// Set keyboard focus
treeView.CurrentItem = specificFolder;

// Listen to focus changes
treeView.PropertyChanged += (s, e) =>
{
    if (e.PropertyName == nameof(SfTreeView.CurrentItem))
    {
        var current = treeView.CurrentItem as FolderModel;
        // Update preview pane, etc.
    }
};
```

**CurrentItem vs SelectedItem:**
- `CurrentItem` - Has keyboard focus (highlighted)
- `SelectedItem` - Actually selected (for operations)
- In `Single` mode, usually the same
- In `Extended` mode with Ctrl, can differ

## Working with Selected Items

### Get Selected Items

```csharp
// Single selection mode
var selectedItem = treeView.SelectedItem as FolderModel;
if (selectedItem != null)
{
    Console.WriteLine($"Selected: {selectedItem.FolderName}");
}

// Multiple selection mode
var selectedItems = treeView.SelectedItems.Cast<FolderModel>().ToList();
Console.WriteLine($"Selected {selectedItems.Count} items");
```

### Clear Selection

```csharp
// Clear all selections
treeView.SelectedItems.Clear();

// Or set SelectedItem to null (Single mode)
treeView.SelectedItem = null;
```

### Select All

```csharp
// Select all root items
foreach (var folder in viewModel.Folders)
{
    treeView.SelectedItems.Add(folder);
}

// Recursively select all items (including children)
private void SelectAllNodes(ObservableCollection<FolderModel> folders)
{
    foreach (var folder in folders)
    {
        treeView.SelectedItems.Add(folder);
        if (folder.SubFolders != null && folder.SubFolders.Count > 0)
        {
            SelectAllNodes(folder.SubFolders);
        }
    }
}
```

### Perform Operations on Selection

```csharp
private void DeleteSelectedItems()
{
    if (treeView.SelectedItems.Count == 0)
    {
        MessageBox.Show("Please select items to delete");
        return;
    }

    var itemsToDelete = treeView.SelectedItems.Cast<FolderModel>().ToList();
    
    foreach (var item in itemsToDelete)
    {
        // Remove from data source
        viewModel.Folders.Remove(item);
    }
    
    // Clear selection after operation
    treeView.SelectedItems.Clear();
}
```

## Common Patterns

### Master-Detail View

```csharp
private void TreeView_SelectionChanged(object sender, ItemSelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var selected = e.AddedItems[0] as FolderModel;
        if (selected != null)
        {
            // Update detail view
            detailsPanel.DataContext = selected;
            LoadFolderContents(selected);
        }
    }
}
```

### Context Menu on Selection

```csharp
<syncfusion:SfTreeView SelectionMode="Single">
    <syncfusion:SfTreeView.ContextMenu>
        <ContextMenu>
            <MenuItem Header="Open" Click="OpenItem_Click"/>
            <MenuItem Header="Delete" Click="DeleteItem_Click"/>
        </ContextMenu>
    </syncfusion:SfTreeView.ContextMenu>
</syncfusion:SfTreeView>

private void DeleteItem_Click(object sender, RoutedEventArgs e)
{
    var selected = treeView.SelectedItem as FolderModel;
    if (selected != null)
    {
        viewModel.Folders.Remove(selected);
    }
}
```

### Conditional Selection

```csharp
private void TreeView_SelectionChanging(object sender, ItemSelectionChangingEventArgs e)
{
    foreach (var item in e.AddedItems)
    {
        var folder = item as FolderModel;
        
        // Prevent selection based on criteria
        if (folder.IsLocked)
        {
            e.Cancel = true;
            MessageBox.Show("Cannot select locked items");
            return;
        }
        
        if (!folder.HasPermission)
        {
            e.Cancel = true;
            MessageBox.Show("Insufficient permissions");
            return;
        }
    }
}
```

## Best Practices

1. **Choose Appropriate Mode:** Use `Single` for simple scenarios, `Multiple` for touch, `Extended` for desktop
2. **Handle Null Checks:** Always check if `SelectedItem` is null before operations
3. **Clear Selection After Operations:** Prevent stale selections
4. **Use Events for Updates:** React to selection changes to update UI
5. **Validate in SelectionChanging:** Use `SelectionChanging` event to prevent invalid selections
6. **Bind to ViewModel:** Use `IsSelectedPropertyName` for MVVM pattern
7. **Consider CurrentItem:** Use for keyboard focus tracking separate from selection

## Troubleshooting

**Selection not working:**
- Verify `SelectionMode` is not `None`
- Check if item exists in data source
- Ensure proper data binding

**Multiple selection throwing exception:**
- Confirm `SelectionMode` is `Multiple` or `Extended`
- Don't use `SelectedItem` property for multiple selection

**Selection not visible:**
- Check TreeView has focus
- Verify selection colors/styles are not overridden
- Ensure item is scrolled into view
