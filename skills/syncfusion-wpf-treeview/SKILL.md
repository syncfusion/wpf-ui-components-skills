---
name: syncfusion-wpf-treeview
description: Comprehensive guide for implementing Syncfusion WPF TreeView (SfTreeView) control to display hierarchical data in Windows Presentation Foundation applications. Use this when working with tree structures, folder hierarchies, organizational charts, or parent-child data relationships. Supports drag-and-drop reordering, checkbox selection, load-on-demand for large datasets, and inline editing of tree nodes.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF TreeView (SfTreeView)

The Syncfusion WPF TreeView (SfTreeView) is a powerful data-oriented control for displaying hierarchical data in a tree structure with expanding and collapsing nodes. It's commonly used to display folder structures, organizational hierarchies, category trees, and nested relationships in WPF applications.

## When to Use This Skill

Use this skill when you need to:
- **Display hierarchical data** in a tree structure (folders, categories, org charts)
- **Implement expandable/collapsible nodes** with parent-child relationships
- **Enable selection** in tree structures (single or multiple items)
- **Add checkbox functionality** to tree nodes with recursive state management
- **Support drag-and-drop** for reordering or reorganizing tree items
- **Enable inline editing** of tree node content
- **Load data on demand** for large hierarchical datasets
- **Bind data** to TreeView from collections (bound or unbound modes)
- **Customize appearance** with templates, themes, and visual styles
- **Display tree lines** to visualize hierarchical relationships
- **Perform CRUD operations** on tree data dynamically

This skill provides comprehensive guidance for all TreeView features, from basic setup to advanced customization.

## Component Overview

**Key Capabilities:**
- **Data Binding Modes:** Bound (data source) and Unbound (manual nodes)
- **Selection:** Single, Multiple, Extended with keyboard navigation
- **Checkboxes:** Individual or recursive state management
- **Drag & Drop:** Within TreeView or to other controls
- **Editing:** Inline editing with templates and validation
- **Load on Demand:** Lazy loading for performance with large datasets
- **Customization:** Full template support for items, expanders, and editing
- **Tree Lines:** Visual indicators for hierarchical structure
- **Performance:** Optimized view reuse and flat rendering architecture

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly deployment
- NuGet package setup (Syncfusion.SfTreeView.WPF)
- Required assembly references
- Namespace imports (XAML and C#)
- Creating basic TreeView in XAML
- Creating TreeView programmatically
- License configuration

### Data Population
📄 **Read:** [references/data-population.md](references/data-population.md)
- Unbound mode (TreeViewNode objects)
- Bound mode (hierarchical data source binding)
- ChildPropertyName for nested collections
- HierarchyPropertyDescriptors for complex hierarchies
- ItemTemplate configuration
- ItemsSource binding patterns
- Data binding with MVVM

### Selection
📄 **Read:** [references/selection.md](references/selection.md)
- Selection modes (None, Single, SingleDeselect, Multiple, Extended)
- UI selection with mouse and keyboard
- Programmatic selection (SelectedItem, SelectedItems)
- Binding selection state to data model
- Selection events (SelectionChanging, SelectionChanged)
- CurrentItem property
- Keyboard navigation

### Checkboxes
📄 **Read:** [references/checkboxes.md](references/checkboxes.md)
- Adding checkboxes to tree items
- CheckBoxMode (None, Default, Recursive)
- CheckedItems collection binding
- Recursive checkbox state propagation
- Programmatic check/uncheck
- ItemTemplateDataContextType configuration
- Checkbox in bound and unbound modes

### Editing
📄 **Read:** [references/editing.md](references/editing.md)
- Enabling editing (AllowEditing property)
- Edit triggers (F2 key, single click, double click)
- EditTemplate and EditTemplateSelector
- Programmatic editing (BeginEdit, EndEdit)
- Editing events and validation
- Committing and reverting changes
- Edit mode in bound and unbound modes

### Drag and Drop
📄 **Read:** [references/drag-and-drop.md](references/drag-and-drop.md)
- Enabling drag and drop (AllowDragging)
- Dragging multiple selected items
- Drop position indicators
- Drag and drop events (ItemDragStarting, ItemDragOver, ItemDropping, ItemDropped)
- Dragging between TreeViews
- Drag to other controls (ListView, DataGrid)
- Customizing drag behavior

### Tree Structure Features
📄 **Read:** [references/tree-structure.md](references/tree-structure.md)
- Expanding and collapsing nodes
- AutoExpandMode property
- ExpandActionTrigger (Node, Expander)
- Load on demand for lazy loading
- Tree lines visualization (ShowLines property)
- Expander template customization
- Programmatic expand/collapse
- ExpanderTemplate

### Appearance and Customization
📄 **Read:** [references/appearance.md](references/appearance.md)
- Item height customization
- ItemTemplate and ItemTemplateSelector
- Visual states and styling
- Theming with SfSkinManager
- Custom expander icons
- Indentation and spacing control
- Border and background customization
- FullRowSelect property

### CRUD Operations
📄 **Read:** [references/crud-operations.md](references/crud-operations.md)
- Adding nodes programmatically
- Removing nodes from TreeView
- Updating node content
- CRUD in bound vs unbound mode
- Refreshing data and UI updates
- Collection change handling

## Quick Start Example

### Basic Unbound TreeView

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:Engine="clr-namespace:Syncfusion.UI.Xaml.TreeView.Engine;assembly=Syncfusion.SfTreeView.WPF">
    <Grid>
        <syncfusion:SfTreeView x:Name="treeView" Width="300">
            <syncfusion:SfTreeView.Nodes>
                <Engine:TreeViewNode Content="Documents" IsExpanded="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Reports"/>
                        <Engine:TreeViewNode Content="Invoices"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
                <Engine:TreeViewNode Content="Pictures" IsExpanded="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Vacation"/>
                        <Engine:TreeViewNode Content="Family"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
            </syncfusion:SfTreeView.Nodes>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

### Basic Bound TreeView

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders"
                       AutoExpandMode="RootNodes">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FolderName}" 
                       VerticalAlignment="Center"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

```csharp
// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Folder> Folders { get; set; }
    
    public ViewModel()
    {
        Folders = new ObservableCollection<Folder>
        {
            new Folder 
            { 
                FolderName = "Documents",
                SubFolders = new ObservableCollection<Folder>
                {
                    new Folder { FolderName = "Reports" },
                    new Folder { FolderName = "Invoices" }
                }
            }
        };
    }
}

public class Folder : INotifyPropertyChanged
{
    public string FolderName { get; set; }
    public ObservableCollection<Folder> SubFolders { get; set; }
}
```

## Common Patterns

### TreeView with Selection and Checkboxes

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="Children"
                       SelectionMode="Multiple"
                       CheckBoxMode="Recursive"
                       CheckedItems="{Binding CheckedItems}"
                       ItemTemplateDataContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <CheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}" 
                          FocusVisualStyle="{x:Null}"/>
                <TextBlock Text="{Binding Content.Name}" 
                           Margin="25,0,0,0" 
                           VerticalAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### TreeView with Load on Demand

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="Children"
                       LoadOnDemand="TreeView_LoadOnDemand"/>
```

```csharp
private void TreeView_LoadOnDemand(object sender, LoadOnDemandEventArgs e)
{
    var node = e.Node;
    var model = node.Content as MyModel;
    
    // Load child items asynchronously
    var childItems = LoadChildItemsFromDatabase(model.Id);
    node.PopulateChildNodes(childItems);
    
    e.HasChildNodes = childItems.Count > 0;
}
```

### TreeView with Drag and Drop

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="Children"
                       AllowDragging="True"
                       SelectionMode="Multiple"
                       ItemDropping="TreeView_ItemDropping"/>
```

```csharp
private void TreeView_ItemDropping(object sender, TreeViewItemDroppingEventArgs e)
{
    // Validate drop operation
    if (!IsValidDrop(e.TargetNode, e.DraggingNodes))
    {
        e.Handled = true; // Cancel drop
    }
}
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | IEnumerable | Data source for bound mode |
| `ChildPropertyName` | string | Property name for child collection |
| `SelectionMode` | SelectionMode | Single, Multiple, Extended, etc. |
| `SelectedItem` | object | Currently selected item |
| `SelectedItems` | ObservableCollection | Multiple selected items |
| `CheckBoxMode` | CheckBoxMode | None, Default, Recursive |
| `CheckedItems` | ObservableCollection | Checked items collection |
| `AllowDragging` | bool | Enable drag and drop |
| `AllowEditing` | bool | Enable inline editing |
| `AutoExpandMode` | AutoExpandMode | AllNodes, RootNodes, None |
| `ExpandActionTrigger` | ExpandActionTrigger | Node or Expander click |
| `ShowLines` | bool | Display tree lines |
| `ItemTemplate` | DataTemplate | Template for item display |
| `EditTemplate` | DataTemplate | Template for editing mode |
| `ExpanderTemplate` | DataTemplate | Template for expander icon |

## Common Use Cases

### File Explorer
Display folder and file hierarchies with icons, allowing users to navigate, select, and manage files with drag-and-drop.

### Organizational Chart
Show company hierarchy with employee nodes, supporting selection and expansion to view team structures.

### Category Management
Display product categories with checkboxes for multi-selection, enabling bulk operations on category trees.

### Project Structure
Show project files and folders with load-on-demand for large projects, supporting inline editing of file names.

### Settings/Preferences Tree
Display nested configuration options with checkboxes for enabling/disabling features hierarchically.

### Menu/Navigation Tree
Create hierarchical navigation menus with expandable sections and visual tree lines.

---

**Next Steps:** Navigate to the specific reference documentation above based on the feature you need to implement. Start with [getting-started.md](references/getting-started.md) for initial setup, then explore other features as needed.
