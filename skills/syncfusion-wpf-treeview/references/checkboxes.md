# Checkboxes in WPF TreeView (SfTreeView)

## Table of Contents
- [Overview](#overview)
- [Enabling Checkboxes](#enabling-checkboxes)
- [Checkbox Modes](#checkbox-modes)
- [Working with Checked Items](#working-with-checked-items)
- [Checkbox in Bound Mode](#checkbox-in-bound-mode)
- [Checkbox in Unbound Mode](#checkbox-in-unbound-mode)
- [Programmatic Check/Uncheck](#programmatic-checkuncheck)
- [Checkbox Events](#checkbox-events)

## Overview

The WPF TreeView supports adding checkboxes to tree nodes, allowing users to select multiple items for batch operations. Checkboxes can be configured with different behaviors including recursive state management where parent-child checkbox states are automatically synchronized.

## Enabling Checkboxes

To add checkboxes to TreeView nodes:

1. Add a CheckBox to the `ItemTemplate`
2. Bind `IsChecked` property to `TreeViewNode.IsChecked`
3. Set `ItemTemplateDataContextType` to `Node`
4. Configure `CheckBoxMode` for desired behavior

**Basic Setup:**

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Items}"
                       ChildPropertyName="Children"
                       CheckBoxMode="Recursive"
                       ItemTemplateDataContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <CheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"
                          FocusVisualStyle="{x:Null}"/>
                <TextBlock Text="{Binding Content.Name}" 
                           Margin="5,0,0,0"
                           VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

**⚠️ Critical:** Set `ItemTemplateDataContextType="Node"` to access `TreeViewNode` properties like `IsChecked` in the template.

## Checkbox Modes

The `CheckBoxMode` property controls checkbox behavior:

### None (Default)

Checkboxes are not managed by TreeView. Manual handling required.

```xml
<syncfusion:SfTreeView CheckBoxMode="None"/>
```

### Default

Independent checkbox states. No parent-child relationship.

```xml
<syncfusion:SfTreeView CheckBoxMode="Default"/>
```

**Behavior:**
- Each checkbox operates independently
- Checking parent doesn't affect children
- Checking children doesn't affect parent
- Use when items are unrelated

### Recursive

Parent-child checkbox states are synchronized automatically.

```xml
<syncfusion:SfTreeView CheckBoxMode="Recursive"/>
```

**Behavior:**
- Check parent → all children checked
- Uncheck parent → all children unchecked
- Check all children → parent automatically checked
- Uncheck any child → parent shows indeterminate state
- Check some children → parent shows indeterminate state

**Use Cases:**
- Folder/file selection (select folder = select all files)
- Category hierarchies
- Permission trees
- Any parent-child dependent selection

## Working with Checked Items

### CheckedItems Collection

The `CheckedItems` property manages checked items in bound mode:

```xml
<syncfusion:SfTreeView CheckedItems="{Binding CheckedItems}"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders"
                       CheckBoxMode="Recursive"
                       ItemTemplateDataContextType="Node">
```

**ViewModel:**

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> checkedItems;

    public ObservableCollection<FolderModel> Folders { get; set; }
    
    public ObservableCollection<object> CheckedItems
    {
        get { return checkedItems; }
        set
        {
            checkedItems = value;
            OnPropertyChanged(nameof(CheckedItems));
        }
    }

    public ViewModel()
    {
        Folders = new ObservableCollection<FolderModel>();
        CheckedItems = new ObservableCollection<object>();
        
        LoadFolders();
        
        // Pre-check some items
        CheckedItems.Add(Folders[0]); // Check first folder
        CheckedItems.Add(Folders[1].SubFolders[0]); // Check specific sub-folder
    }
}
```

**Important Notes:**
- `CheckedItems` type must be `ObservableCollection<object>`
- Only works in **Bound Mode** (not Unbound Mode)
- TreeView automatically manages `TreeViewNode.IsChecked` based on `CheckedItems`

## Checkbox in Bound Mode

Complete implementation with data binding:

**Step 1:** Define your model:

```csharp
public class CategoryModel : INotifyPropertyChanged
{
    private string categoryName;
    private ObservableCollection<CategoryModel> subCategories;

    public string CategoryName
    {
        get { return categoryName; }
        set
        {
            categoryName = value;
            OnPropertyChanged(nameof(CategoryName));
        }
    }

    public ObservableCollection<CategoryModel> SubCategories
    {
        get { return subCategories; }
        set
        {
            subCategories = value;
            OnPropertyChanged(nameof(SubCategories));
        }
    }

    public CategoryModel()
    {
        SubCategories = new ObservableCollection<CategoryModel>();
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Step 2:** Create ViewModel:

```csharp
public class CategoryViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CategoryModel> Categories { get; set; }
    public ObservableCollection<object> CheckedCategories { get; set; }

    public CategoryViewModel()
    {
        Categories = new ObservableCollection<CategoryModel>();
        CheckedCategories = new ObservableCollection<object>();
        
        LoadCategories();
    }

    private void LoadCategories()
    {
        var electronics = new CategoryModel { CategoryName = "Electronics" };
        electronics.SubCategories.Add(new CategoryModel { CategoryName = "Computers" });
        electronics.SubCategories.Add(new CategoryModel { CategoryName = "Phones" });
        electronics.SubCategories.Add(new CategoryModel { CategoryName = "Tablets" });

        var clothing = new CategoryModel { CategoryName = "Clothing" };
        clothing.SubCategories.Add(new CategoryModel { CategoryName = "Men" });
        clothing.SubCategories.Add(new CategoryModel { CategoryName = "Women" });
        clothing.SubCategories.Add(new CategoryModel { CategoryName = "Kids" });

        Categories.Add(electronics);
        Categories.Add(clothing);
        
        // Pre-check Electronics category
        CheckedCategories.Add(electronics);
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Step 3:** XAML Configuration:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace">
    
    <Window.DataContext>
        <local:CategoryViewModel/>
    </Window.DataContext>

    <Grid>
        <syncfusion:SfTreeView x:Name="treeView"
                               ItemsSource="{Binding Categories}"
                               ChildPropertyName="SubCategories"
                               CheckBoxMode="Recursive"
                               CheckedItems="{Binding CheckedCategories}"
                               ItemTemplateDataContextType="Node"
                               SelectionMode="Multiple"
                               AutoExpandMode="AllNodes">
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal">
                        <CheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"
                                  VerticalAlignment="Center"
                                  FocusVisualStyle="{x:Null}"/>
                        <TextBlock Text="{Binding Content.CategoryName}"
                                   Margin="8,0,0,0"
                                   VerticalAlignment="Center"
                                   FontSize="14"/>
                    </StackPanel>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
        
        <!-- Display checked count -->
        <TextBlock Text="{Binding CheckedCategories.Count, StringFormat='Checked: {0} items'}"
                   VerticalAlignment="Bottom"
                   Margin="10"/>
    </Grid>
</Window>
```

## Checkbox in Unbound Mode

For manually created `TreeViewNode` objects:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:Engine="clr-namespace:Syncfusion.UI.Xaml.TreeView.Engine;assembly=Syncfusion.SfTreeView.WPF">
    <Grid>
        <syncfusion:SfTreeView x:Name="treeView"
                               CheckBoxMode="Recursive"
                               ItemTemplateDataContextType="Node">
            <syncfusion:SfTreeView.Nodes>
                <Engine:TreeViewNode Content="All Features" 
                                     IsExpanded="True"
                                     IsChecked="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Feature A" IsChecked="True"/>
                        <Engine:TreeViewNode Content="Feature B" IsChecked="False"/>
                        <Engine:TreeViewNode Content="Feature C" IsChecked="True"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
            </syncfusion:SfTreeView.Nodes>
            
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal">
                        <CheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
                        <TextBlock Text="{Binding Content}" Margin="5,0,0,0"/>
                    </StackPanel>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

**Programmatically in Code-Behind:**

```csharp
// Create nodes with checkbox state
var parentNode = new TreeViewNode
{
    Content = "Parent Item",
    IsExpanded = true,
    IsChecked = true // Pre-checked
};

parentNode.ChildNodes.Add(new TreeViewNode 
{ 
    Content = "Child 1", 
    IsChecked = true 
});

parentNode.ChildNodes.Add(new TreeViewNode 
{ 
    Content = "Child 2", 
    IsChecked = false 
});

treeView.Nodes.Add(parentNode);
```

## Programmatic Check/Uncheck

### In Bound Mode

Use the `CheckedItems` collection:

```csharp
// Check items
treeView.CheckedItems.Add(viewModel.Categories[0]);
treeView.CheckedItems.Add(viewModel.Categories[1].SubCategories[0]);

// Uncheck items
treeView.CheckedItems.Remove(specificCategory);

// Check all items
foreach (var category in viewModel.Categories)
{
    treeView.CheckedItems.Add(category);
}

// Clear all checked items
treeView.CheckedItems.Clear();

// Check items matching criteria
var electronicsCategories = viewModel.Categories
    .Where(c => c.CategoryName.Contains("Electronics"));
foreach (var cat in electronicsCategories)
{
    treeView.CheckedItems.Add(cat);
}
```

### In Unbound Mode

Directly set `TreeViewNode.IsChecked`:

```csharp
// Check specific node
treeView.Nodes[0].IsChecked = true;

// Check all root nodes
foreach (var node in treeView.Nodes)
{
    node.IsChecked = true;
}

// Recursively check all nodes
void CheckAllNodes(TreeViewNode node)
{
    node.IsChecked = true;
    foreach (var child in node.ChildNodes)
    {
        CheckAllNodes(child);
    }
}

foreach (var rootNode in treeView.Nodes)
{
    CheckAllNodes(rootNode);
}

// Toggle checkbox
var node = treeView.Nodes[0];
node.IsChecked = !node.IsChecked;
```

## Checkbox Events

### ItemChecked Event

Fires when an item is checked:

```csharp
treeView.ItemChecked += TreeView_ItemChecked;

private void TreeView_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    var checkedNode = e.Node;
    var checkedItem = checkedNode.Content;
    
    Console.WriteLine($"Checked: {checkedItem}");
    
    // Perform action on checked item
    if (checkedItem is CategoryModel category)
    {
        EnableCategory(category);
    }
}
```

### ItemUnchecked Event

Fires when an item is unchecked:

```csharp
treeView.ItemUnchecked += TreeView_ItemUnchecked;

private void TreeView_ItemUnchecked(object sender, ItemUncheckedEventArgs e)
{
    var uncheckedNode = e.Node;
    var uncheckedItem = uncheckedNode.Content;
    
    Console.WriteLine($"Unchecked: {uncheckedItem}");
    
    // Perform action on unchecked item
    if (uncheckedItem is CategoryModel category)
    {
        DisableCategory(category);
    }
}
```

## Common Patterns

### Select All / Clear All

```xml
<StackPanel>
    <Button Content="Select All" Click="SelectAll_Click"/>
    <Button Content="Clear All" Click="ClearAll_Click"/>
    
    <syncfusion:SfTreeView x:Name="treeView"
                           CheckBoxMode="Recursive"
                           CheckedItems="{Binding CheckedItems}"/>
</StackPanel>
```

```csharp
private void SelectAll_Click(object sender, RoutedEventArgs e)
{
    // Add all root items (Recursive mode will check children)
    foreach (var category in viewModel.Categories)
    {
        if (!treeView.CheckedItems.Contains(category))
        {
            treeView.CheckedItems.Add(category);
        }
    }
}

private void ClearAll_Click(object sender, RoutedEventArgs e)
{
    treeView.CheckedItems.Clear();
}
```

### Batch Operations on Checked Items

```csharp
private void DeleteCheckedItems_Click(object sender, RoutedEventArgs e)
{
    if (treeView.CheckedItems.Count == 0)
    {
        MessageBox.Show("No items selected");
        return;
    }

    var itemsToDelete = treeView.CheckedItems.Cast<CategoryModel>().ToList();
    
    var result = MessageBox.Show(
        $"Delete {itemsToDelete.Count} items?",
        "Confirm Delete",
        MessageBoxButton.YesNo);
    
    if (result == MessageBoxResult.Yes)
    {
        foreach (var item in itemsToDelete)
        {
            viewModel.Categories.Remove(item);
        }
        
        treeView.CheckedItems.Clear();
    }
}
```

### Export Checked Items

```csharp
private void ExportChecked_Click(object sender, RoutedEventArgs e)
{
    var checkedItems = treeView.CheckedItems.Cast<CategoryModel>().ToList();
    
    var exportData = checkedItems.Select(c => new
    {
        Name = c.CategoryName,
        HasChildren = c.SubCategories.Count > 0
    });
    
    // Export to CSV, JSON, etc.
    SaveToFile(exportData);
}
```

### Three-State Checkbox (Indeterminate)

When using `Recursive` mode, parent checkboxes automatically show indeterminate state when some (but not all) children are checked.

```xml
<CheckBox IsChecked="{Binding IsChecked, Mode=TwoWay}"
          IsThreeState="True"
          VerticalAlignment="Center"/>
```

**States:**
- **Checked (true):** All children checked
- **Unchecked (false):** No children checked
- **Indeterminate (null):** Some children checked

## Best Practices

1. **Always Set ItemTemplateDataContextType:** Use `ItemTemplateDataContextType="Node"` to access `IsChecked`
2. **Choose Appropriate CheckBoxMode:** Use `Recursive` for hierarchical dependencies
3. **Use CheckedItems in ViewModel:** Bind to ViewModel for MVVM pattern
4. **Clear CheckedItems After Operations:** Prevent stale selections
5. **Handle Recursive Cascades:** Be aware that checking parent checks all children
6. **Consider Performance:** For large trees (1000+ nodes), minimize checkbox event handlers
7. **Provide Visual Feedback:** Show checked count or selection summary

## Troubleshooting

**Checkboxes not working:**
- Verify `ItemTemplateDataContextType="Node"` is set
- Check binding path: `{Binding IsChecked}` not `{Binding Content.IsChecked}`
- Ensure `CheckBoxMode` is not `None`

**Recursive mode not working:**
- Confirm `CheckBoxMode="Recursive"`
- Verify parent-child relationships in data model
- Check `ChildPropertyName` is correct

**CheckedItems not updating:**
- Use `ObservableCollection<object>` for `CheckedItems`
- Implement `INotifyPropertyChanged` in ViewModel
- Verify two-way binding: `Mode=TwoWay`

**Performance issues with checkboxes:**
- Use `NodePopulationMode="OnDemand"` for large trees
- Minimize work in checkbox event handlers
- Consider virtualization for very large datasets
