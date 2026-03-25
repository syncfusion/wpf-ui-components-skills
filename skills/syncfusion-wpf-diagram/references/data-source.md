# Data Source in WPF Diagram (SfDiagram)

## Overview

`DataSourceSettings` connects an external data collection to SfDiagram, automatically generating nodes and connectors based on data relationships (ID/ParentID). Typically used together with automatic layouts.

## Business Object Data Source

### 1. Define a Business Object Class

```csharp
public class Employee
{
    public string Name       { get; set; }
    public string EmpId      { get; set; }
    public string ParentId   { get; set; }
    public string Designation{ get; set; }
}

public class Employees : ObservableCollection<Employee> { }
```

### 2. Configure DataSourceSettings

```csharp
diagram.DataSourceSettings = new DataSourceSettings()
{
    Id       = "EmpId",      // property that uniquely identifies each item
    ParentId = "ParentId",   // property that specifies the parent item's ID
    Root     = "1",          // ID of the root node (optional)
    DataSource = new Employees()
    {
        new Employee { Name="Steve", EmpId="1", ParentId="",  Designation="CEO"     },
        new Employee { Name="Kevin", EmpId="2", ParentId="1", Designation="Manager" },
        new Employee { Name="John",  EmpId="3", ParentId="1", Designation="Manager" },
        new Employee { Name="Raj",   EmpId="4", ParentId="2", Designation="Team Lead" },
        new Employee { Name="Will",  EmpId="5", ParentId="2", Designation="S/w Developer" },
    }
};
```

### 3. XAML Equivalent

```xaml
<local:Employees x:Key="employees">
    <local:Employee Name="Steve" EmpId="1" ParentId=""  Designation="CEO"/>
    <local:Employee Name="Kevin" EmpId="2" ParentId="1" Designation="Manager"/>
    <local:Employee Name="John"  EmpId="3" ParentId="1" Designation="Manager"/>
</local:Employees>

<syncfusion:DataSourceSettings x:Key="DataSourceSettings"
                               Id="EmpId" ParentId="ParentId"
                               Root="1"
                               DataSource="{StaticResource employees}"/>

<syncfusion:SfDiagram x:Name="diagram"
                      DataSourceSettings="{StaticResource DataSourceSettings}"/>
```

## DataSourceSettings Properties

| Property | Description |
|----------|-------------|
| `DataSource` | The collection of business objects |
| `Id` | Name of the property that uniquely identifies each object |
| `ParentId` | Name of the property that points to the parent object's ID |
| `Root` | ID of the root node; nodes without parents are treated as roots by default |

> **Important:** `Id` and `ParentId` must be the same data type (e.g., both `string`).

## Combining with Automatic Layout

DataSource populates nodes, but they are positioned at (0,0) by default. Use a `LayoutManager` to arrange them:

```csharp
diagram.LayoutManager = new LayoutManager()
{
    Layout = new DirectedTreeLayout()
    {
        Type              = LayoutType.Hierarchical,
        Orientation       = TreeOrientation.TopToBottom,
        HorizontalSpacing = 80,
        VerticalSpacing   = 50,
    }
};
```

```xaml
<syncfusion:DirectedTreeLayout x:Key="treeLayout"
                               Type="Hierarchical"
                               Orientation="TopToBottom"
                               HorizontalSpacing="80"
                               VerticalSpacing="50"/>
<syncfusion:LayoutManager x:Key="layoutManager"
                          Layout="{StaticResource treeLayout}"/>

<syncfusion:SfDiagram DataSourceSettings="{StaticResource DataSourceSettings}"
                      LayoutManager="{StaticResource layoutManager}"/>
```

## Dynamic Updates

Because `DataSource` is an `ObservableCollection`, changes automatically update the diagram:

```csharp
var ds = diagram.DataSourceSettings.DataSource as Employees;

// Add new employee
ds.Add(new Employee { Name="Steven", EmpId="8", ParentId="2", Designation="Dev" });

// Insert at position
ds.Insert(3, new Employee { Name="William", EmpId="9", ParentId="2", Designation="Dev" });

// Remove
var emp = ds.ElementAt(5);
ds.Remove(emp);
ds.RemoveAt(5);

// Reset
diagram.DataSourceSettings.DataSource = null;
diagram.DataSourceSettings.DataSource = new Employees();

// Move item
ds.Move(2, 1);
```

## Multiple Parents

A child node can reference multiple parents by setting `ParentId` to a `List<string>`:

```csharp
public class ItemInfo
{
    public string Name               { get; set; }
    public List<string> ReportingTo  { get; set; }
}

// Usage
var data = new ObservableCollection<ItemInfo>()
{
    new ItemInfo { Name = "Manager A" },
    new ItemInfo { Name = "Manager B" },
    new ItemInfo { Name = "Shared Employee", ReportingTo = new List<string> { "Manager A", "Manager B" } },
};

diagram.DataSourceSettings = new DataSourceSettings()
{
    Id       = "Name",
    ParentId = "ReportingTo",
    DataSource = data,
};
```

The child node is placed at the center between its parents.

## FlowchartDataSourceSettings

`FlowchartDataSourceSettings` (derives from `DataSourceSettings`) adds shape and connector-text mapping for flowcharts:

```csharp
diagram.DataSourceSettings = new FlowchartDataSourceSettings()
{
    Id                   = "Id",
    ParentId             = "ParentId",
    DataSource           = GetFlowchartData(),
    ContentMapping       = "Name",          // display text on node
    ShapeMapping         = "NodeShape",     // shape geometry
    WidthMapping         = "Width",
    HeightMapping        = "Height",
    ConnectorTextMapping = "Label",         // label on connector
};

diagram.LayoutManager = new LayoutManager()
{
    Layout = new FlowchartLayout()
    {
        Orientation        = FlowchartOrientation.TopToBottom,
        YesBranchDirection = BranchDirection.LeftInFlow,
        NoBranchDirection  = BranchDirection.RightInFlow,
        HorizontalSpacing  = 50,
        VerticalSpacing    = 30,
    }
};
```

```csharp
// Data class for flowchart
public class FlowItem
{
    public string Id         { get; set; }
    public List<string> ParentId  { get; set; }
    public string NodeShape  { get; set; }  // shape key from ResourceDictionary
    public List<string> Label     { get; set; }  // connector label(s)
    public string Name       { get; set; }
    public double Width      { get; set; }
    public double Height     { get; set; }
}
```

## Customizing Node Appearance from DataSource

Use `NodeCreatingEvent` to customize how nodes are rendered when generated from data:

```csharp
diagram.NodeCreatingEvent += (s, e) =>
{
    if (e.Item is NodeViewModel node &&
        e.Data is Employee emp)
    {
        node.UnitWidth  = 150;
        node.UnitHeight = 60;
        node.Content    = emp.Designation;
        node.ContentTemplate = App.Current.Resources["EmployeeTemplate"] as DataTemplate;
    }
};
```

## Common Gotchas

- **Types must match:** `Id` and `ParentId` property types must match (both string, or both int, etc.).
- **Nodes not positioned:** DataSource alone only creates nodes at (0,0) — always pair with a `LayoutManager`.
- **Root node:** If multiple nodes have empty `ParentId`, they all become roots. Use the `Root` property to designate a single root.
- **No NodeCollection needed:** When using `DataSourceSettings`, SfDiagram auto-creates nodes — you don't need to manually populate `diagram.Nodes`.

## Related
- Layout algorithms → `references/automatic-layouts.md`
- Flowchart layout details → `references/automatic-layouts.md`
