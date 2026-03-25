# Drag and Drop in WPF TreeView (SfTreeView)

## Table of Contents
- [Overview](#overview)
- [Enabling Drag and Drop](#enabling-drag-and-drop)
- [Dragging Multiple Items](#dragging-multiple-items)
- [Drag and Drop Events](#drag-and-drop-events)
- [Customizing Drag Behavior](#customizing-drag-behavior)
- [Drop Position Control](#drop-position-control)
- [Drag Between Controls](#drag-between-controls)
- [Custom Drag Preview](#custom-drag-preview)

## Overview

The WPF TreeView supports dragging and dropping nodes within the same TreeView or between different controls (TreeView, ListView, DataGrid). Users can reorder nodes, reorganize hierarchies, and move items between parent nodes with visual feedback through drop indicators.

## Enabling Drag and Drop

Enable drag and drop by setting `AllowDragging` to `true`:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       AllowDragging="True"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders">
```

```csharp
treeView.AllowDragging = true;
```

**Basic Behavior:**
- Drag a node by clicking and holding
- Drop indicator shows where item will be placed
- Drop above/below target or as child based on indicator position

## Dragging Multiple Items

Enable multi-selection to drag multiple nodes at once:

```xml
<syncfusion:SfTreeView AllowDragging="True"
                       SelectionMode="Multiple"
                       ItemsSource="{Binding Folders}"/>
```

**User Experience:**
- Select multiple items (Ctrl+Click or Shift+Click)
- Drag any selected item
- All selected items move together

```csharp
// Programmatically select multiple items for dragging
treeView.SelectionMode = SelectionMode.Multiple;
treeView.SelectedItems.Add(folder1);
treeView.SelectedItems.Add(folder2);
treeView.SelectedItems.Add(folder3);
// User can now drag all three items
```

## Drag and Drop Events

TreeView provides comprehensive events to control drag and drop operations:

### ItemDragStarting Event

Fires before drag operation begins. **Can be canceled.**

```csharp
treeView.ItemDragStarting += TreeView_ItemDragStarting;

private void TreeView_ItemDragStarting(object sender, TreeViewItemDragStartingEventArgs e)
{
    // Get dragged nodes
    var draggingNodes = e.DraggingNodes;
    
    // Prevent dragging certain items
    foreach (var node in draggingNodes)
    {
        var folder = node.Content as FolderModel;
        if (folder != null && folder.IsLocked)
        {
            e.Cancel = true;
            MessageBox.Show("Cannot drag locked folders");
            return;
        }
    }
    
    // Set custom drag data
    e.Data = new DataObject("Folders", draggingNodes.Select(n => n.Content).ToList());
}
```

**Event Arguments:**
- `DraggingNodes` - Collection of nodes being dragged
- `Data` - DataObject for drag operation
- `Cancel` - Set `true` to prevent drag

### ItemDragStarted Event

Fires after drag operation has started. **Cannot be canceled.**

```csharp
treeView.ItemDragStarted += TreeView_ItemDragStarted;

private void TreeView_ItemDragStarted(object sender, TreeViewItemDragStartedEventArgs e)
{
    var count = e.DraggingNodes.Count;
    statusTextBlock.Text = $"Dragging {count} item(s)...";
    
    // Log drag operation
    Debug.WriteLine($"Started dragging {count} nodes");
}
```

### ItemDragOver Event

Fires continuously while dragging over tree items.

```csharp
treeView.ItemDragOver += TreeView_ItemDragOver;

private void TreeView_ItemDragOver(object sender, TreeViewItemDragOverEventArgs e)
{
    var targetNode = e.TargetNode;
    var targetFolder = targetNode?.Content as FolderModel;
    
    // Provide visual feedback
    if (targetFolder != null)
    {
        statusTextBlock.Text = $"Drop on: {targetFolder.FolderName}";
    }
    
    // Customize drop position
    if (targetFolder != null && targetFolder.IsReadOnly)
    {
        e.DropPosition = DropPosition.None; // Disallow drop
    }
}
```

**Event Arguments:**
- `DraggingNodes` - Nodes being dragged
- `TargetNode` - Node being hovered over
- `DropPosition` - Where items will be dropped
- `DragSource` - Source control of drag operation

### ItemDropping Event

Fires when items are about to be dropped. **Can be canceled.**

```csharp
treeView.ItemDropping += TreeView_ItemDropping;

private void TreeView_ItemDropping(object sender, TreeViewItemDroppingEventArgs e)
{
    var targetNode = e.TargetNode;
    var targetFolder = targetNode?.Content as FolderModel;
    var draggingNodes = e.DraggingNodes;
    
    // Prevent dropping on certain targets
    if (targetFolder != null && targetFolder.IsProtected)
    {
        e.Handled = true; // Cancel drop
        MessageBox.Show($"Cannot drop items on protected folder: {targetFolder.FolderName}");
        return;
    }
    
    // Prevent dropping parent into its own child
    foreach (var draggingNode in draggingNodes)
    {
        if (IsAncestor(draggingNode, targetNode))
        {
            e.Handled = true;
            MessageBox.Show("Cannot move folder into its own subfolder");
            return;
        }
    }
    
    // Customize drop position
    if (e.DropPosition == DropPosition.DropAbove)
    {
        Debug.WriteLine("Dropping above target");
    }
}

private bool IsAncestor(TreeViewNode potentialAncestor, TreeViewNode node)
{
    var parent = node?.ParentNode;
    while (parent != null)
    {
        if (parent == potentialAncestor)
            return true;
        parent = parent.ParentNode;
    }
    return false;
}
```

**Event Arguments:**
- `DraggingNodes` - Nodes being dropped
- `TargetNode` - Drop target node
- `DropPosition` - Drop position (Above, Below, AsChild)
- `Handled` - Set `true` to cancel drop
- `DragSource` - Source control

### ItemDropped Event

Fires after items have been successfully dropped. **Cannot be canceled.**

```csharp
treeView.ItemDropped += TreeView_ItemDropped;

private void TreeView_ItemDropped(object sender, TreeViewItemDroppedEventArgs e)
{
    var targetNode = e.TargetNode;
    var droppedCount = e.DraggingNodes.Count;
    
    // Update UI
    statusTextBlock.Text = $"Dropped {droppedCount} item(s)";
    
    // Perform post-drop actions
    foreach (var node in e.DraggingNodes)
    {
        var folder = node.Content as FolderModel;
        if (folder != null)
        {
            // Update database, sync files, etc.
            UpdateFolderLocation(folder);
        }
    }
    
    // Log operation
    Debug.WriteLine($"Dropped {droppedCount} nodes on {targetNode.Content}");
}
```

## Customizing Drag Behavior

### Disable Dragging Specific Items

```csharp
treeView.ItemDragStarting += (s, e) =>
{
    foreach (var node in e.DraggingNodes)
    {
        var folder = node.Content as FolderModel;
        
        // Prevent dragging system folders
        if (folder != null && folder.IsSystem)
        {
            e.Cancel = true;
            MessageBox.Show("System folders cannot be moved");
            return;
        }
        
        // Prevent dragging root nodes
        if (node.Level == 0)
        {
            e.Cancel = true;
            MessageBox.Show("Root folders cannot be moved");
            return;
        }
    }
};
```

### Disable Dropping on Specific Items

```csharp
treeView.ItemDropping += (s, e) =>
{
    var targetFolder = e.TargetNode?.Content as FolderModel;
    
    // Disallow drop on files (only folders)
    if (targetFolder != null && !targetFolder.IsFolder)
    {
        e.Handled = true;
        return;
    }
    
    // Disallow drop on read-only folders
    if (targetFolder != null && targetFolder.IsReadOnly)
    {
        e.Handled = true;
        MessageBox.Show("Cannot drop items in read-only folder");
    }
};
```

### Validate Drop Based on Item Types

```csharp
treeView.ItemDropping += (s, e) =>
{
    var targetFolder = e.TargetNode?.Content as FolderModel;
    
    foreach (var node in e.DraggingNodes)
    {
        var draggedFolder = node.Content as FolderModel;
        
        // Only allow dropping folders with same type
        if (draggedFolder != null && targetFolder != null)
        {
            if (draggedFolder.Type != targetFolder.Type)
            {
                e.Handled = true;
                MessageBox.Show("Cannot mix different folder types");
                return;
            }
        }
    }
};
```

## Drop Position Control

Control where items are dropped using the `DropPosition` property:

### DropPosition Enum Values

- **DropAbove** - Drop before the target node (sibling)
- **DropBelow** - Drop after the target node (sibling)
- **DropAsChild** - Drop as child of target node
- **None** - No drop allowed

### Force Drop as Child

```csharp
treeView.ItemDropping += (s, e) =>
{
    var targetFolder = e.TargetNode?.Content as FolderModel;
    
    // Always drop as child for folder targets
    if (targetFolder != null && targetFolder.IsFolder)
    {
        e.DropPosition = DropPosition.DropAsChild;
    }
};
```

### Prevent Dropping as Child

```csharp
treeView.ItemDropping += (s, e) =>
{
    // Only allow sibling drops (above/below)
    if (e.DropPosition == DropPosition.DropAsChild)
    {
        e.DropPosition = DropPosition.DropBelow;
    }
};
```

### Dynamic Drop Position Based on Content

```csharp
treeView.ItemDragOver += (s, e) =>
{
    var targetFolder = e.TargetNode?.Content as FolderModel;
    
    if (targetFolder != null)
    {
        // Files can only be siblings, not children
        if (!targetFolder.IsFolder)
        {
            e.DropPosition = DropPosition.DropBelow;
        }
        // Folders can have children
        else
        {
            e.DropPosition = DropPosition.DropAsChild;
        }
    }
};
```

## Drag Between Controls

TreeView supports dragging between different controls:

### Drag from TreeView to ListView

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    
    <syncfusion:SfTreeView x:Name="sourceTreeView"
                           Grid.Column="0"
                           AllowDragging="True"
                           ItemsSource="{Binding SourceFolders}"/>
    
    <ListView x:Name="targetListView"
              Grid.Column="1"
              AllowDrop="True"
              Drop="TargetListView_Drop"/>
</Grid>
```

```csharp
private void TargetListView_Drop(object sender, DragEventArgs e)
{
    if (e.Data.GetDataPresent(typeof(TreeViewDragInfo)))
    {
        var dragInfo = e.Data.GetData(typeof(TreeViewDragInfo)) as TreeViewDragInfo;
        if (dragInfo != null)
        {
            foreach (var node in dragInfo.DraggingNodes)
            {
                var folder = node.Content as FolderModel;
                if (folder != null)
                {
                    // Add to ListView
                    viewModel.TargetItems.Add(folder);
                }
            }
        }
    }
}
```

### Drag Between Two TreeViews

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    
    <syncfusion:SfTreeView x:Name="sourceTree"
                           Grid.Column="0"
                           AllowDragging="True"
                           ItemDropping="SourceTree_ItemDropping"/>
    
    <syncfusion:SfTreeView x:Name="targetTree"
                           Grid.Column="1"
                           AllowDragging="True"
                           AllowDrop="True"
                           ItemDropping="TargetTree_ItemDropping"/>
</Grid>
```

```csharp
private void TargetTree_ItemDropping(object sender, TreeViewItemDroppingEventArgs e)
{
    // Handle drop from external TreeView
    if (e.DragSource != targetTree)
    {
        // Validate and accept drop
        var targetFolder = e.TargetNode?.Content as FolderModel;
        if (targetFolder != null)
        {
            foreach (var node in e.DraggingNodes)
            {
                var folder = node.Content as FolderModel;
                // Add to target tree data source
                viewModel.TargetFolders.Add(folder);
            }
        }
        
        // Prevent default TreeView handling
        e.Handled = true;
    }
}
```

## Custom Drag Preview

Customize the drag preview template:

```xml
<syncfusion:SfTreeView AllowDragging="True">
    <syncfusion:SfTreeView.DragPreviewTemplate>
        <DataTemplate>
            <Border Background="#FFE6E6E6" 
                    BorderBrush="#FF999999" 
                    BorderThickness="1"
                    CornerRadius="3"
                    Padding="8,4">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="📁" Margin="0,0,5,0"/>
                    <TextBlock Text="{Binding DraggingNodes.Count, StringFormat='Moving {0} item(s)'}"
                               FontWeight="SemiBold"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeView.DragPreviewTemplate>
</syncfusion:SfTreeView>
```

**DataContext:** The template's DataContext is `TreeViewDragInfo` with properties:
- `DraggingNodes` - Collection of nodes being dragged
- `DropPosition` - Current drop position

### Advanced Custom Preview

```xml
<syncfusion:SfTreeView.DragPreviewTemplate>
    <DataTemplate>
        <Border Background="White" 
                BorderBrush="DodgerBlue" 
                BorderThickness="2"
                CornerRadius="5"
                Padding="10"
                Effect="{StaticResource DropShadowEffect}">
            <StackPanel>
                <TextBlock Text="{Binding DraggingNodes.Count, StringFormat='Dragging {0} items'}"
                           FontSize="14"
                           FontWeight="Bold"
                           Foreground="DodgerBlue"/>
                <ItemsControl ItemsSource="{Binding DraggingNodes}"
                              MaxHeight="100">
                    <ItemsControl.ItemTemplate>
                        <DataTemplate>
                            <TextBlock Text="{Binding Content.FolderName}"
                                       FontSize="12"
                                       Margin="0,2"/>
                        </DataTemplate>
                    </ItemsControl.ItemTemplate>
                </ItemsControl>
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:SfTreeView.DragPreviewTemplate>
```

## Common Patterns

### File Explorer Drag and Drop

```csharp
// Only allow dragging non-root items
treeView.ItemDragStarting += (s, e) =>
{
    if (e.DraggingNodes.Any(n => n.Level == 0))
    {
        e.Cancel = true;
    }
};

// Only drop into folders, not files
treeView.ItemDropping += (s, e) =>
{
    var targetFolder = e.TargetNode?.Content as FileSystemItem;
    if (targetFolder != null && !targetFolder.IsDirectory)
    {
        e.Handled = true;
    }
};
```

### Move Confirmation Dialog

```csharp
treeView.ItemDropping += (s, e) =>
{
    var count = e.DraggingNodes.Count;
    var target = e.TargetNode?.Content as FolderModel;
    
    var result = MessageBox.Show(
        $"Move {count} item(s) to {target?.FolderName}?",
        "Confirm Move",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question);
    
    if (result != MessageBoxResult.Yes)
    {
        e.Handled = true;
    }
};
```

### Copy Instead of Move (with Ctrl Key)

```csharp
treeView.ItemDropping += (s, e) =>
{
    // Check if Ctrl key is pressed
    bool isCopyOperation = Keyboard.IsKeyDown(Key.LeftCtrl) || 
                          Keyboard.IsKeyDown(Key.RightCtrl);
    
    if (isCopyOperation)
    {
        // Perform copy instead of move
        e.Handled = true; // Prevent default move
        
        foreach (var node in e.DraggingNodes)
        {
            var folder = node.Content as FolderModel;
            var copy = folder.Clone();
            // Add copy to target location
        }
    }
};
```

## Best Practices

1. **Validate Drop Targets:** Use `ItemDropping` to prevent invalid drops
2. **Prevent Circular References:** Check if dragging parent into own child
3. **Provide Visual Feedback:** Use custom drag preview for clarity
4. **Handle Multi-Selection:** Enable `SelectionMode="Multiple"` for batch moves
5. **Log Operations:** Track drag-drop for undo/redo functionality
6. **Update Backend:** Persist changes in `ItemDropped` event
7. **Consider Performance:** Minimize work in `ItemDragOver` (fires continuously)

## Troubleshooting

**Drag not working:**
- Verify `AllowDragging="True"`
- Check if items are selectable
- Ensure data is bound properly

**Drop not working:**
- Verify target allows drop (not canceled in events)
- Check `DropPosition` is not `None`
- Ensure `Handled` is not set to `true` incorrectly

**Items disappearing after drag:**
- Check if source collection is being modified
- Verify data binding is working
- Review `ItemDropped` event logic

**Drag between controls not working:**
- Ensure target has `AllowDrop="True"`
- Handle `Drop` event on target control
- Check `DataObject` format compatibility
