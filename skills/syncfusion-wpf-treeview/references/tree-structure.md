# Tree Structure Features in WPF TreeView (SfTreeView)

## Table of Contents
- [Expand and Collapse](#expand-and-collapse)
- [Expand Action Trigger](#expand-action-trigger)
- [Auto Expand Mode](#auto-expand-mode)
- [Binding Expand State](#binding-expand-state)
- [Programmatic Expand/Collapse](#programmatic-expandcollapse)
- [Load on Demand](#load-on-demand)
- [Tree Lines](#tree-lines)
- [Expander Template](#expander-template)

## Expand and Collapse

TreeView nodes can be expanded or collapsed through user interaction or programmatically, revealing or hiding child nodes in the hierarchy.

### Basic Expand/Collapse

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders"/>
```

**User Actions:**
- Click expander icon to expand/collapse
- Use keyboard: → (expand), ← (collapse)

## Expand Action Trigger

Control what triggers expand/collapse using `ExpandActionTrigger`:

### Expander (Default)

Only clicking the expander icon expands/collapses:

```xml
<syncfusion:SfTreeView ExpandActionTrigger="Expander"/>
```

### Node

Clicking anywhere on the node expands/collapses:

```xml
<syncfusion:SfTreeView ExpandActionTrigger="Node"/>
```

```csharp
treeView.ExpandActionTrigger = ExpandActionTrigger.Node;
```

**Use Cases:**
- **Expander:** Traditional tree behavior (Windows Explorer)
- **Node:** Touch-friendly, mobile applications

## Auto Expand Mode

Control initial expansion state using `AutoExpandMode`:

### None (Default)

All nodes collapsed on load:

```xml
<syncfusion:SfTreeView AutoExpandMode="None"/>
```

### RootNodes

Only root level nodes expanded:

```xml
<syncfusion:SfTreeView AutoExpandMode="RootNodes"/>
```

**Best for:** Showing top-level categories with collapsed children.

### AllNodes

All nodes expanded on load:

```xml
<syncfusion:SfTreeView AutoExpandMode="AllNodes"/>
```

```csharp
treeView.AutoExpandMode = AutoExpandMode.AllNodes;
```

**Best for:** Small trees, search results, file previews.

**⚠️ Note:** `AutoExpandMode` only works in **Bound Mode**. For Unbound Mode, set `IsExpanded` on `TreeViewNode`.

## Binding Expand State

Bind expansion state to your data model for persistence:

```csharp
public class FolderModel : INotifyPropertyChanged
{
    private bool isExpanded;

    public string FolderName { get; set; }
    public ObservableCollection<FolderModel> SubFolders { get; set; }

    public bool IsExpanded
    {
        get { return isExpanded; }
        set
        {
            isExpanded = value;
            OnPropertyChanged(nameof(IsExpanded));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Configure in XAML:**

```xml
<syncfusion:SfTreeView ItemsSource="{Binding Folders}">
    <syncfusion:SfTreeView.HierarchyPropertyDescriptors>
        <treeViewEngine:HierarchyPropertyDescriptor 
            TargetType="{x:Type local:FolderModel}"
            ChildPropertyName="SubFolders"
            IsExpandedPropertyName="IsExpanded"/>
    </syncfusion:SfTreeView.HierarchyPropertyDescriptors>
</syncfusion:SfTreeView>
```

**Benefits:**
- Expansion state persists with data
- Restore user's tree state across sessions
- MVVM-friendly pattern

## Programmatic Expand/Collapse

### Expand Nodes

```csharp
// Expand specific node
var node = treeView.Nodes[0];
node.IsExpanded = true;

// Expand node for data item
var folder = viewModel.Folders[0];
folder.IsExpanded = true; // If bound to model

// Expand all root nodes
foreach (var node in treeView.Nodes)
{
    node.IsExpanded = true;
}

// Recursively expand all nodes
void ExpandAllNodes(TreeViewNode node)
{
    node.IsExpanded = true;
    foreach (var child in node.ChildNodes)
    {
        ExpandAllNodes(child);
    }
}

foreach (var root in treeView.Nodes)
{
    ExpandAllNodes(root);
}
```

### Collapse Nodes

```csharp
// Collapse specific node
treeView.Nodes[0].IsExpanded = false;

// Collapse all root nodes
foreach (var node in treeView.Nodes)
{
    node.IsExpanded = false;
}

// Recursively collapse all
void CollapseAllNodes(TreeViewNode node)
{
    node.IsExpanded = false;
    foreach (var child in node.ChildNodes)
    {
        CollapseAllNodes(child);
    }
}
```

### Expand/Collapse with Animation

```xml
<syncfusion:SfTreeView IsAnimationEnabled="True"/>
```

```csharp
treeView.IsAnimationEnabled = true;
node.IsExpanded = true; // Animates expansion
```

## Load on Demand

Lazy load child nodes only when parent expands, improving performance for large datasets.

### Setup Load on Demand

**Step 1:** Add `HasChildNodes` property to model:

```csharp
public class FolderModel : INotifyPropertyChanged
{
    public string FolderName { get; set; }
    public ObservableCollection<FolderModel> SubFolders { get; set; }
    public bool HasChildNodes { get; set; } // Indicates children exist

    public FolderModel()
    {
        SubFolders = new ObservableCollection<FolderModel>();
    }
}
```

**Step 2:** Handle `LoadOnDemand` event:

```xml
<syncfusion:SfTreeView ItemsSource="{Binding RootFolders}"
                       ChildPropertyName="SubFolders"
                       LoadOnDemand="TreeView_LoadOnDemand"/>
```

```csharp
private void TreeView_LoadOnDemand(object sender, LoadOnDemandEventArgs e)
{
    var node = e.Node;
    var folder = node.Content as FolderModel;
    
    if (folder != null && folder.SubFolders.Count == 0)
    {
        // Load child items from database or service
        var childFolders = LoadChildFoldersFromDatabase(folder.FolderId);
        
        // Add to collection
        foreach (var child in childFolders)
        {
            folder.SubFolders.Add(child);
        }
        
        // Notify TreeView
        e.HasChildNodes = childFolders.Count > 0;
    }
}

private List<FolderModel> LoadChildFoldersFromDatabase(int parentId)
{
    // Query database for child folders
    return database.Folders.Where(f => f.ParentId == parentId).ToList();
}
```

### Using ICommand for Load on Demand

```csharp
public class FolderViewModel
{
    public ObservableCollection<FolderModel> RootFolders { get; set; }
    public ICommand LoadOnDemandCommand { get; private set; }

    public FolderViewModel()
    {
        LoadOnDemandCommand = new RelayCommand<TreeViewNode>(ExecuteLoadOnDemand);
        RootFolders = LoadRootFolders();
    }

    private void ExecuteLoadOnDemand(TreeViewNode node)
    {
        var folder = node.Content as FolderModel;
        if (folder != null)
        {
            // Async load
            var children = LoadChildFolders(folder.FolderId);
            node.PopulateChildNodes(children);
            node.IsExpanded = children.Count > 0;
        }
    }
}
```

```xml
<syncfusion:SfTreeView LoadOnDemandCommand="{Binding LoadOnDemandCommand}"/>
```

### Async Load on Demand

```csharp
private async void TreeView_LoadOnDemand(object sender, LoadOnDemandEventArgs e)
{
    var node = e.Node;
    var folder = node.Content as FolderModel;
    
    // Show loading indicator
    node.ShowExpanderAnimation = true;
    
    try
    {
        // Async load from service
        var children = await LoadChildFoldersAsync(folder.FolderId);
        
        // Add to UI thread
        Application.Current.Dispatcher.Invoke(() =>
        {
            foreach (var child in children)
            {
                folder.SubFolders.Add(child);
            }
            e.HasChildNodes = children.Count > 0;
        });
    }
    finally
    {
        // Stop loading animation
        node.ShowExpanderAnimation = false;
    }
}
```

## Tree Lines

Display lines connecting parent and child nodes:

### Enable Tree Lines

```xml
<syncfusion:SfTreeView ShowLines="True"/>
```

```csharp
treeView.ShowLines = true;
```

### Show Root Lines

Include lines for root nodes:

```xml
<syncfusion:SfTreeView ShowLines="True"
                       ShowRootLines="True"/>
```

```csharp
treeView.ShowLines = true;
treeView.ShowRootLines = true;
```

### Customize Line Appearance

```xml
<syncfusion:SfTreeView ShowLines="True"
                       ShowRootLines="True"
                       LineStroke="DodgerBlue"
                       LineStrokeThickness="1.5"/>
```

```csharp
treeView.ShowLines = true;
treeView.ShowRootLines = true;
treeView.LineStroke = new SolidColorBrush(Colors.DodgerBlue);
treeView.LineStrokeThickness = 1.5;
```

**Properties:**
- `LineStroke` - Line color (default: LightSlateGray)
- `LineStrokeThickness` - Line thickness (default: 1)

## Expander Template

Customize the expand/collapse icon:

### Basic Custom Expander

```xml
<syncfusion:SfTreeView ItemsSource="{Binding Folders}">
    <syncfusion:SfTreeView.ExpanderTemplate>
        <DataTemplate>
            <Grid>
                <TextBlock Text="▶" 
                           FontSize="12"
                           Visibility="{Binding IsExpanded, Converter={StaticResource BoolToVisibilityConverter}, ConverterParameter=Inverse}"/>
                <TextBlock Text="▼" 
                           FontSize="12"
                           Visibility="{Binding IsExpanded, Converter={StaticResource BoolToVisibilityConverter}}"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ExpanderTemplate>
</syncfusion:SfTreeView>
```

### Icon-Based Expander

```xml
<syncfusion:SfTreeView.ExpanderTemplate>
    <DataTemplate>
        <Border Background="Transparent" 
                Padding="4">
            <Path Data="M 0 0 L 4 4 L 8 0 Z"
                  Fill="Gray"
                  RenderTransformOrigin="0.5,0.5">
                <Path.RenderTransform>
                    <RotateTransform Angle="{Binding IsExpanded, Converter={StaticResource BoolToAngleConverter}}"/>
                </Path.RenderTransform>
            </Path>
        </Border>
    </DataTemplate>
</syncfusion:SfTreeView.ExpanderTemplate>
```

### Animated Expander

```xml
<syncfusion:SfTreeView.ExpanderTemplate>
    <DataTemplate>
        <Grid Width="16" Height="16">
            <Path x:Name="expanderPath"
                  Data="M 4 2 L 8 6 L 4 10"
                  Stroke="Black"
                  StrokeThickness="2"
                  RenderTransformOrigin="0.5,0.5">
                <Path.RenderTransform>
                    <RotateTransform x:Name="rotation" Angle="0"/>
                </Path.RenderTransform>
            </Path>
            <DataTemplate.Triggers>
                <DataTrigger Binding="{Binding IsExpanded}" Value="True">
                    <DataTrigger.EnterActions>
                        <BeginStoryboard>
                            <Storyboard>
                                <DoubleAnimation Storyboard.TargetName="rotation"
                                               Storyboard.TargetProperty="Angle"
                                               To="90"
                                               Duration="0:0:0.2"/>
                            </Storyboard>
                        </BeginStoryboard>
                    </DataTrigger.EnterActions>
                </DataTrigger>
            </DataTemplate.Triggers>
        </Grid>
    </DataTemplate>
</syncfusion:SfTreeView.ExpanderTemplate>
```

## Common Patterns

### Expand on Selection

```csharp
treeView.SelectionChanged += (s, e) =>
{
    if (e.AddedItems.Count > 0)
    {
        var selectedNode = treeView.GetNode(e.AddedItems[0]);
        if (selectedNode != null && selectedNode.HasChildNodes)
        {
            selectedNode.IsExpanded = true;
        }
    }
};
```

### Expand Path to Selected Item

```csharp
void ExpandPathToNode(TreeViewNode node)
{
    var parent = node.ParentNode;
    while (parent != null)
    {
        parent.IsExpanded = true;
        parent = parent.ParentNode;
    }
}
```

### Search and Expand Results

```csharp
void SearchAndExpand(string searchText)
{
    var matchingNodes = FindNodes(treeView.Nodes, searchText);
    foreach (var node in matchingNodes)
    {
        ExpandPathToNode(node);
        treeView.BringIntoView(node);
    }
}
```

## Best Practices

1. **Use AutoExpandMode Wisely:** `RootNodes` for medium trees, `None` for large
2. **Enable Load on Demand:** For large datasets (>1000 nodes)
3. **Bind Expand State:** Persist user's tree state
4. **Animate Expand/Collapse:** Improves UX with smooth transitions
5. **Show Tree Lines:** For complex hierarchies needing visual clarity
6. **Async Load on Demand:** Don't block UI during data loading

## Troubleshooting

**Nodes won't expand:**
- Check if `HasChildNodes` is true
- Verify child collection has items
- Ensure `IsExpanded` binding is working

**AutoExpandMode not working:**
- Only works in Bound Mode
- Check `ItemsSource` is bound
- Verify `ChildPropertyName` is correct

**Load on Demand not firing:**
- Ensure initial items have `HasChildNodes = true`
- Check event handler is attached
- Verify `e.HasChildNodes` is set correctly

**Tree lines not showing:**
- Verify `ShowLines="True"`
- Check line color isn't matching background
- Ensure `LineStrokeThickness` > 0
