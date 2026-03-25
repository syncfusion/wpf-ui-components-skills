# Data Binding for WPF TreeGrid

## Table of Contents
- [Overview](#overview)
- [Self-Relational Data Binding](#self-relational-data-binding)
  - [Properties](#properties)
  - [Basic Implementation](#basic-implementation)
  - [Code Examples](#code-examples)
- [Nested Collections Data Binding](#nested-collections-data-binding)
  - [Properties](#properties-1)
  - [Basic Implementation](#basic-implementation-1)
  - [Code Examples](#code-examples-1)
- [Load-On-Demand Binding](#load-on-demand-binding)
  - [Using RequestTreeItems Event](#using-requesttreeitems-event)
  - [Using LoadOnDemandCommand](#using-loadondemandcommand)
  - [Code Examples](#code-examples-2)
- [Node Expansion Control](#node-expansion-control)
  - [AutoExpandMode Property](#autoexpandmode-property)
  - [Programmatic Expansion](#programmatic-expansion)
- [View and Events](#view-and-events)
  - [View Property](#view-property)
  - [RequestTreeItems Event](#requesttreeitems-event)
  - [NodePopulating Event](#nodepopulating-event)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion WPF TreeGrid supports three types of data binding to display hierarchical data:

1. **Self-Relational Data Binding**: Uses a single collection where each item references its parent
2. **Nested Collections**: Uses hierarchical objects with child collections
3. **Load-On-Demand**: Loads child nodes dynamically when needed

## Self-Relational Data Binding

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | IEnumerable | Source collection for the TreeGrid |
| `ParentPropertyName` | string | Name of the property that identifies the parent item |
| `ChildPropertyName` | string | Name of the property containing the ID value |
| `SelfRelationRootValue` | object | Value that identifies root-level items |

### Basic Implementation

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       ChildPropertyName="ID"
                       ParentPropertyName="ReportsTo"
                       SelfRelationRootValue="-1"
                       AutoExpandMode="RootNodesExpanded" />
```

### Code Examples

**Data Model for Self-Relational Binding:**

```csharp
public class EmployeeInfo : INotifyPropertyChanged
{
    private int _id;
    private string _firstName;
    private string _lastName;
    private int _reportsTo;
    private string _title;

    public int ID
    {
        get { return _id; }
        set
        {
            _id = value;
            RaisePropertyChanged("ID");
        }
    }

    public string FirstName
    {
        get { return _firstName; }
        set
        {
            _firstName = value;
            RaisePropertyChanged("FirstName");
        }
    }

    public string LastName
    {
        get { return _lastName; }
        set
        {
            _lastName = value;
            RaisePropertyChanged("LastName");
        }
    }

    public int ReportsTo
    {
        get { return _reportsTo; }
        set
        {
            _reportsTo = value;
            RaisePropertyChanged("ReportsTo");
        }
    }

    public string Title
    {
        get { return _title; }
        set
        {
            _title = value;
            RaisePropertyChanged("Title");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**ViewModel with Self-Relational Data:**

```csharp
public class ViewModel
{
    public ObservableCollection<EmployeeInfo> EmployeeDetails { get; set; }

    public ViewModel()
    {
        this.EmployeeDetails = GetEmployeeData();
    }

    private ObservableCollection<EmployeeInfo> GetEmployeeData()
    {
        var employees = new ObservableCollection<EmployeeInfo>();

        // Root employee
        employees.Add(new EmployeeInfo
        {
            ID = 1,
            FirstName = "John",
            LastName = "Doe",
            Title = "CEO",
            ReportsTo = -1 // Root value
        });

        // Child employees
        employees.Add(new EmployeeInfo
        {
            ID = 2,
            FirstName = "Jane",
            LastName = "Smith",
            Title = "Manager",
            ReportsTo = 1 // Reports to John (ID=1)
        });

        employees.Add(new EmployeeInfo
        {
            ID = 3,
            FirstName = "Bob",
            LastName = "Johnson",
            Title = "Developer",
            ReportsTo = 2 // Reports to Jane (ID=2)
        });

        return employees;
    }
}
```

## Nested Collections Data Binding

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | IEnumerable | Source collection containing root items |
| `ChildPropertyName` | string | Property name containing child collection |

### Basic Implementation

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       ChildPropertyName="Children"
                       AutoExpandMode="RootNodesExpanded" />
```

### Code Examples

**Data Model for Nested Collections:**

```csharp
public class Employee : INotifyPropertyChanged
{
    private string _firstName;
    private string _lastName;
    private int _employeeID;
    private ObservableCollection<Employee> _children;

    public string FirstName
    {
        get { return _firstName; }
        set
        {
            _firstName = value;
            RaisePropertyChanged("FirstName");
        }
    }

    public string LastName
    {
        get { return _lastName; }
        set
        {
            _lastName = value;
            RaisePropertyChanged("LastName");
        }
    }

    public int EmployeeID
    {
        get { return _employeeID; }
        set
        {
            _employeeID = value;
            RaisePropertyChanged("EmployeeID");
        }
    }

    public ObservableCollection<Employee> Children
    {
        get { return _children; }
        set
        {
            _children = value;
            RaisePropertyChanged("Children");
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    private void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**ViewModel with Nested Collection Data:**

```csharp
public class ViewModel
{
    public ObservableCollection<Employee> EmployeeDetails { get; set; }

    public ViewModel()
    {
        this.EmployeeDetails = CreateEmployeeHierarchy();
    }

    private ObservableCollection<Employee> CreateEmployeeHierarchy()
    {
        var employees = new ObservableCollection<Employee>();

        var ceo = new Employee
        {
            FirstName = "John",
            LastName = "Doe",
            EmployeeID = 1,
            Children = new ObservableCollection<Employee>()
        };

        var manager = new Employee
        {
            FirstName = "Jane",
            LastName = "Smith",
            EmployeeID = 2,
            Children = new ObservableCollection<Employee>()
        };

        var developer = new Employee
        {
            FirstName = "Bob",
            LastName = "Johnson",
            EmployeeID = 3,
            Children = new ObservableCollection<Employee>()
        };

        // Build hierarchy
        manager.Children.Add(developer);
        ceo.Children.Add(manager);
        employees.Add(ceo);

        return employees;
    }
}
```

## Load-On-Demand Binding

### Using RequestTreeItems Event

Load child nodes when parent is expanded:

```csharp
this.treeGrid.RequestTreeItems += TreeGrid_RequestTreeItems;

private void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    if (e.ParentItem == null)
    {
        // Load root items
        e.ChildItems = GetRootEmployees();
    }
    else
    {
        // Load child items
        var employee = e.ParentItem as EmployeeInfo;
        e.ChildItems = GetSubordinates(employee.ID);
    }
}

private IEnumerable GetRootEmployees()
{
    return EmployeeRepository.GetEmployees()
        .Where(x => x.ReportsTo == -1);
}

private IEnumerable GetSubordinates(int managerId)
{
    return EmployeeRepository.GetEmployees()
        .Where(x => x.ReportsTo == managerId);
}
```

### Using LoadOnDemandCommand

Bind command to load data asynchronously:

```csharp
public class ViewModel
{
    public ICommand LoadOnDemandCommand { get; set; }

    public ViewModel()
    {
        LoadOnDemandCommand = new RelayCommand<TreeGridNodeEventArgs>(ExecuteLoadOnDemand);
    }

    private void ExecuteLoadOnDemand(TreeGridNodeEventArgs args)
    {
        if (args.Node.Item == null)
        {
            // Load root items
            args.Node.ChildNodes.AddRange(GetRootItems());
        }
        else
        {
            var parentItem = args.Node.Item as EmployeeInfo;
            var childItems = GetChildItems(parentItem.ID);
            args.Node.ChildNodes.AddRange(childItems);
        }
    }
}
```

**XAML Configuration:**

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       LoadOnDemandCommand="{Binding LoadOnDemandCommand}" />
```

### Code Examples

**Async Load-On-Demand:**

```csharp
private async void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    if (e.ParentItem == null)
    {
        e.ChildItems = await LoadRootItemsAsync();
    }
    else
    {
        var parent = e.ParentItem as EmployeeInfo;
        e.ChildItems = await LoadChildItemsAsync(parent.ID);
    }
}

private async Task<IEnumerable> LoadRootItemsAsync()
{
    await Task.Delay(100); // Simulate network delay
    return GetRootEmployees();
}

private async Task<IEnumerable> LoadChildItemsAsync(int parentId)
{
    await Task.Delay(100); // Simulate network delay
    return GetSubordinates(parentId);
}
```

## Node Expansion Control

### AutoExpandMode Property

Controls initial expansion state:

| Value | Description |
|-------|-------------|
| `None` | All nodes collapsed by default |
| `RootNodesExpanded` | Only root nodes expanded |
| `AllNodesExpanded` | All nodes expanded |

```xaml
<syncfusion:SfTreeGrid AutoExpandMode="RootNodesExpanded"
                       ItemsSource="{Binding EmployeeDetails}" />
```

### Programmatic Expansion

**Expand/Collapse Specific Nodes:**

```csharp
// Expand a specific node
var node = treeGrid.View.Nodes[0];
treeGrid.ExpandNode(node);

// Collapse a specific node
treeGrid.CollapseNode(node);

// Expand all nodes
treeGrid.ExpandAllNodes();

// Collapse all nodes
treeGrid.CollapseAllNodes();

// Expand to specific level
treeGrid.ExpandAllNodes(2); // Expand to level 2
```

**Expand Node by Data Object:**

```csharp
var employee = viewModel.EmployeeDetails[0];
var node = treeGrid.View.Nodes.GetNode(employee);
if (node != null)
{
    treeGrid.ExpandNode(node);
}
```

## View and Events

### View Property

Access the TreeGrid's data view:

```csharp
// Get all nodes
var allNodes = treeGrid.View.Nodes;

// Get filtered nodes
var filteredNodes = treeGrid.View.Nodes.Where(n => !n.IsFiltered);

// Get node at specific index
var nodeAtIndex = treeGrid.View.Nodes[0];

// Find node by data object
var employee = new EmployeeInfo { ID = 1 };
var node = treeGrid.View.Nodes.GetNode(employee);
```

### RequestTreeItems Event

Fired when child items are needed:

```csharp
public class TreeGridRequestTreeItemsEventArgs : EventArgs
{
    public object ParentItem { get; } // Parent data object
    public IEnumerable ChildItems { get; set; } // Set child items here
}
```

### NodePopulating Event

Fired before node is populated:

```csharp
this.treeGrid.NodePopulating += TreeGrid_NodePopulating;

private void TreeGrid_NodePopulating(object sender, TreeGridNodePopulatingEventArgs e)
{
    // Customize node before it's created
    if (e.Node.Level == 0)
    {
        // Root node customization
    }
}
```

## Best Practices

### 1. Choose Appropriate Binding Mode

- Use **Self-Relational** for flat data with parent-child relationships
- Use **Nested Collections** for naturally hierarchical data
- Use **Load-On-Demand** for large datasets or remote data

### 2. Optimize Load-On-Demand

```csharp
// Cache loaded data
private Dictionary<int, List<Employee>> _childCache = new Dictionary<int, List<Employee>>();

private void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    if (e.ParentItem == null)
    {
        e.ChildItems = GetRootEmployees();
    }
    else
    {
        var parent = e.ParentItem as EmployeeInfo;
        
        // Check cache first
        if (_childCache.ContainsKey(parent.ID))
        {
            e.ChildItems = _childCache[parent.ID];
        }
        else
        {
            var children = GetSubordinates(parent.ID).ToList();
            _childCache[parent.ID] = children;
            e.ChildItems = children;
        }
    }
}
```

### 3. Implement INotifyPropertyChanged

```csharp
public class EmployeeInfo : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### 4. Handle Null References

```csharp
private void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    if (e.ParentItem == null)
    {
        e.ChildItems = GetRootEmployees() ?? Enumerable.Empty<EmployeeInfo>();
    }
    else
    {
        var parent = e.ParentItem as EmployeeInfo;
        if (parent != null)
        {
            e.ChildItems = GetSubordinates(parent.ID) ?? Enumerable.Empty<EmployeeInfo>();
        }
    }
}
```

## Troubleshooting

### Issue: Nodes Not Appearing

**Problem**: TreeGrid shows no data or hierarchy

**Solutions**:
1. Verify `ItemsSource` is set correctly
2. Check property names in binding
3. Ensure data model implements INotifyPropertyChanged
4. Verify `ChildPropertyName` or `ParentPropertyName` is correct

```csharp
// Debug binding
Debug.WriteLine($"ItemsSource Count: {treeGrid.View.Nodes.Count}");
Debug.WriteLine($"ChildPropertyName: {treeGrid.ChildPropertyName}");
```

### Issue: LoadOnDemand Not Triggering

**Problem**: RequestTreeItems event not firing

**Solutions**:
1. Ensure HasChildNodes returns true
2. Check if node is already populated

```csharp
// Implement ITreeNode interface
public class EmployeeInfo : ITreeNode
{
    public bool HasChildNodes
    {
        get
        {
            // Return true if this employee might have subordinates
            return this.Title == "Manager" || this.Title == "CEO";
        }
    }
}
```

### Issue: Performance Degradation

**Problem**: Slow rendering with large datasets

**Solutions**:
1. Use Load-On-Demand instead of loading all data upfront
2. Implement virtualization
3. Limit initial expansion level

```xaml
<syncfusion:SfTreeGrid AutoExpandMode="None"
                       LoadOnDemandCommand="{Binding LoadCommand}"
                       AllowDataShaping="False" />
```

### Issue: Data Not Refreshing

**Problem**: Changes to ItemsSource not reflected

**Solutions**:
1. Use ObservableCollection
2. Call RefreshNode or Refresh methods
3. Ensure INotifyPropertyChanged is implemented

```csharp
// Refresh specific node
var node = treeGrid.View.Nodes[0];
treeGrid.View.RefreshNode(node);

// Refresh all nodes
treeGrid.View.Refresh();

// Refresh with reset
treeGrid.View.Refresh(true);
```

### Issue: Self-Relational Binding Shows Duplicates

**Problem**: Same item appears in multiple locations

**Solutions**:
1. Verify `SelfRelationRootValue` is correct
2. Check for circular references in data
3. Ensure ID and ParentID values are unique

```csharp
// Validate data before binding
private void ValidateHierarchy(ObservableCollection<EmployeeInfo> employees)
{
    var ids = new HashSet<int>();
    
    foreach (var employee in employees)
    {
        if (ids.Contains(employee.ID))
        {
            throw new InvalidOperationException($"Duplicate ID found: {employee.ID}");
        }
        ids.Add(employee.ID);
        
        // Check for circular reference
        if (employee.ID == employee.ReportsTo && employee.ReportsTo != -1)
        {
            throw new InvalidOperationException($"Circular reference detected: {employee.ID}");
        }
    }
}
```

### Issue: Expansion State Not Persisting

**Problem**: Expansion state lost on refresh

**Solutions**:
1. Save expansion state before refresh
2. Restore after refresh completes

```csharp
// Save expansion state
private List<int> SaveExpansionState()
{
    var expandedIds = new List<int>();
    foreach (var node in treeGrid.View.Nodes)
    {
        if (node.IsExpanded)
        {
            var employee = node.Item as EmployeeInfo;
            expandedIds.Add(employee.ID);
        }
    }
    return expandedIds;
}

// Restore expansion state
private void RestoreExpansionState(List<int> expandedIds)
{
    foreach (var node in treeGrid.View.Nodes)
    {
        var employee = node.Item as EmployeeInfo;
        if (expandedIds.Contains(employee.ID))
        {
            treeGrid.ExpandNode(node);
        }
    }
}
```
