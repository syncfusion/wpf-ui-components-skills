# CRUD Operations in WPF TreeView (SfTreeView)

TreeView supports Create, Read, Update, Delete (CRUD) operations at runtime for both Bound and Unbound modes. Changes to the underlying collection automatically reflect in the TreeView UI.

## Adding Nodes

Add nodes dynamically by manipulating the underlying data source.

### Add to Bound Mode

Add data objects to the bound collection:

```csharp
// ViewModel with ObservableCollection
public class FolderViewModel
{
    public ObservableCollection<FolderModel> Folders { get; set; }

    public void AddNewFolder()
    {
        var newFolder = new FolderModel 
        { 
            FolderName = "New Folder",
            SubFolders = new ObservableCollection<FolderModel>()
        };
        Folders.Add(newFolder);
    }
}

// Usage
viewModel.Folders.Add(new FolderModel { FolderName = "Documents" });
```

**XAML:**

```xml
<syncfusion:SfTreeView ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders"/>
```

### Add Child Node

Add to child collection:

```csharp
public void AddChildFolder(FolderModel parent)
{
    var child = new FolderModel 
    { 
        FolderName = "Subfolder",
        SubFolders = new ObservableCollection<FolderModel>()
    };
    parent.SubFolders.Add(child);
}

// Usage
var parentFolder = viewModel.Folders[0];
parentFolder.SubFolders.Add(new FolderModel { FolderName = "Images" });
```

### Add to Unbound Mode

Add `TreeViewNode` objects directly:

```csharp
// Create and add node
var newNode = new TreeViewNode 
{ 
    Content = new FolderModel { FolderName = "Downloads" }
};
treeView.Nodes.Add(newNode);

// Add child node
var childNode = new TreeViewNode { Content = "Music" };
treeView.Nodes[0].ChildNodes.Add(childNode);
```

### Add at Specific Position

```csharp
// Insert at index (Bound Mode)
viewModel.Folders.Insert(1, new FolderModel { FolderName = "Projects" });

// Insert in Unbound Mode
var node = new TreeViewNode { Content = "Videos" };
treeView.Nodes.Insert(2, node);
```

## Deleting Nodes

Remove nodes using keyboard shortcut or programmatically.

### Enable Delete with Keyboard

Allow deletion by pressing `Delete` key:

```xml
<syncfusion:SfTreeView AllowDeleting="True"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders"/>
```

```csharp
treeView.AllowDeleting = true;
```

**User Action:** Select node(s) → Press `Delete` key

### Programmatic Deletion (Bound Mode)

Remove from bound collection:

```csharp
// Remove specific item
var folderToRemove = viewModel.Folders.FirstOrDefault(f => f.FolderName == "Temp");
if (folderToRemove != null)
{
    viewModel.Folders.Remove(folderToRemove);
}

// Remove selected item
var selectedFolder = treeView.SelectedItem as FolderModel;
if (selectedFolder != null)
{
    viewModel.Folders.Remove(selectedFolder);
}

// Remove by index
viewModel.Folders.RemoveAt(2);
```

### Programmatic Deletion (Unbound Mode)

Remove from `Nodes` collection:

```csharp
// Remove specific node
var nodeToRemove = treeView.Nodes[0];
treeView.Nodes.Remove(nodeToRemove);

// Remove selected node
if (treeView.SelectedItem != null)
{
    var selectedNode = treeView.GetNode(treeView.SelectedItem);
    if (selectedNode != null)
    {
        if (selectedNode.ParentNode != null)
            selectedNode.ParentNode.ChildNodes.Remove(selectedNode);
        else
            treeView.Nodes.Remove(selectedNode);
    }
}

// Remove by index
treeView.Nodes.RemoveAt(1);
```

### Delete Multiple Nodes

```csharp
// Get all selected items
var selectedItems = treeView.SelectedItems.Cast<FolderModel>().ToList();

// Remove each
foreach (var item in selectedItems)
{
    viewModel.Folders.Remove(item);
}
```

### ItemDeleting Event

Cancel or customize deletion:

```xml
<syncfusion:SfTreeView AllowDeleting="True"
                       ItemDeleting="TreeView_ItemDeleting"/>
```

```csharp
private void TreeView_ItemDeleting(object sender, ItemDeletingEventArgs e)
{
    var nodesToDelete = e.Nodes.ToList();
    
    foreach (var node in nodesToDelete)
    {
        var folder = node.Content as FolderModel;
        if (folder != null)
        {
            // Cancel deletion for protected folders
            if (folder.IsProtected)
            {
                e.Cancel = true;
                MessageBox.Show($"Cannot delete protected folder: {folder.FolderName}");
                return;
            }
            
            // Skip system folders from batch delete
            if (folder.IsSystemFolder)
            {
                e.Nodes.Remove(node);
            }
        }
    }
}
```

**Event Arguments:**
- `Nodes` - Collection of nodes being deleted (can modify)
- `Cancel` - Set to `true` to cancel deletion

### ItemDeleted Event

Handle post-deletion:

```xml
<syncfusion:SfTreeView ItemDeleted="TreeView_ItemDeleted"/>
```

```csharp
private void TreeView_ItemDeleted(object sender, ItemDeletedEventArgs e)
{
    // Reset selection after deletion
    if (treeView.Nodes.Count > 0)
    {
        treeView.SelectedItem = treeView.Nodes[0].Content;
    }
    
    // Log deletion
    foreach (var node in e.Nodes)
    {
        var folder = node.Content as FolderModel;
        Log($"Deleted: {folder?.FolderName}");
    }
    
    // Update UI status
    StatusText = $"{e.Nodes.Count} item(s) deleted";
}
```

## Updating Nodes

Modify node content using property changes or editing.

### Update Property (Bound Mode)

With `INotifyPropertyChanged`:

```csharp
public class FolderModel : INotifyPropertyChanged
{
    private string folderName;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged(nameof(FolderName));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}

// Update property
var folder = viewModel.Folders[0];
folder.FolderName = "Updated Name"; // UI auto-updates
```

### Update Property (Unbound Mode)

Update `Content` and refresh:

```csharp
var node = treeView.Nodes[0];
var folder = node.Content as FolderModel;
if (folder != null)
{
    folder.FolderName = "New Name";
    treeView.RefreshView(); // Manual refresh required
}
```

### Inline Editing

See `editing.md` for comprehensive editing documentation.

**Quick Example:**

```xml
<syncfusion:SfTreeView AllowEditing="True"
                       EditTrigger="DoubleTap"/>
```

### Replace Entire Node

```csharp
// Bound Mode: Replace in collection
var index = viewModel.Folders.IndexOf(oldFolder);
if (index >= 0)
{
    viewModel.Folders[index] = newFolder;
}

// Unbound Mode: Replace content
var node = treeView.Nodes[0];
node.Content = newFolderModel;
treeView.RefreshView();
```

## Refreshing View

### Auto-Refresh (Bound Mode)

Use `ObservableCollection` for automatic UI updates:

```csharp
public ObservableCollection<FolderModel> Folders { get; set; }
```

**Changes trigger auto-refresh:**
- Add/Remove items
- Property changes (with `INotifyPropertyChanged`)

### Manual Refresh (Unbound Mode)

Call `RefreshView()` after changes:

```csharp
// Update content
treeView.Nodes[0].Content = newData;

// Refresh UI
treeView.RefreshView();
```

### Refresh Specific Node

```csharp
void RefreshNode(TreeViewNode node)
{
    node.IsExpanded = false;
    node.IsExpanded = true;
}
```

## Common Patterns

### Add Node with Confirmation

```csharp
private void AddFolder()
{
    var dialog = new InputDialog("Enter folder name:");
    if (dialog.ShowDialog() == true)
    {
        var newFolder = new FolderModel
        {
            FolderName = dialog.InputText,
            SubFolders = new ObservableCollection<FolderModel>()
        };
        viewModel.Folders.Add(newFolder);
        
        // Select newly added
        treeView.SelectedItem = newFolder;
    }
}
```

### Delete with Confirmation

```csharp
private void DeleteSelected()
{
    var selected = treeView.SelectedItem as FolderModel;
    if (selected != null)
    {
        var result = MessageBox.Show(
            $"Delete '{selected.FolderName}'?",
            "Confirm Delete",
            MessageBoxButton.YesNo);
            
        if (result == MessageBoxResult.Yes)
        {
            viewModel.Folders.Remove(selected);
        }
    }
}
```

### Undo Delete

```csharp
private Stack<FolderModel> deletedFolders = new Stack<FolderModel>();

private void TreeView_ItemDeleted(object sender, ItemDeletedEventArgs e)
{
    foreach (var node in e.Nodes)
    {
        var folder = node.Content as FolderModel;
        if (folder != null)
        {
            deletedFolders.Push(folder);
        }
    }
}

private void UndoDelete()
{
    if (deletedFolders.Count > 0)
    {
        var folder = deletedFolders.Pop();
        viewModel.Folders.Add(folder);
    }
}
```

### Bulk Operations

```csharp
// Add multiple
var newFolders = new List<FolderModel>
{
    new FolderModel { FolderName = "Folder1" },
    new FolderModel { FolderName = "Folder2" },
    new FolderModel { FolderName = "Folder3" }
};

foreach (var folder in newFolders)
{
    viewModel.Folders.Add(folder);
}

// Delete multiple
var toDelete = viewModel.Folders.Where(f => f.IsTemporary).ToList();
foreach (var folder in toDelete)
{
    viewModel.Folders.Remove(folder);
}
```

### Move Node

```csharp
void MoveNode(FolderModel source, FolderModel newParent)
{
    // Remove from current location
    var currentParent = FindParent(source);
    if (currentParent != null)
    {
        currentParent.SubFolders.Remove(source);
    }
    else
    {
        viewModel.Folders.Remove(source);
    }
    
    // Add to new location
    newParent.SubFolders.Add(source);
}
```

## Best Practices

1. **Use ObservableCollection:** For automatic UI updates in Bound Mode
2. **Implement INotifyPropertyChanged:** For property-level updates
3. **Validate before delete:** Use `ItemDeleting` event
4. **Handle empty collections:** Check `Count` before accessing items
5. **Select intelligently:** Update selection after CRUD operations
6. **Batch operations:** Suspend notifications for multiple changes
7. **Provide undo:** Stack deleted items for recovery

## Troubleshooting

**Changes not reflecting:**
- Check if using `ObservableCollection`
- Verify `INotifyPropertyChanged` implementation
- Call `RefreshView()` in Unbound Mode

**Delete key not working:**
- Ensure `AllowDeleting="True"`
- Check if TreeView has focus
- Verify node is selected

**Cannot delete nodes:**
- Check `ItemDeleting` event handlers
- Verify collection is not read-only
- Ensure proper parent-child relationship

**Selection lost after delete:**
- Handle `ItemDeleted` event
- Manually set `SelectedItem` to valid node
- Use index-based selection for consistency
