# Automatic Layouts in WPF Diagram (SfDiagram)

## Table of Contents
- [Overview](#overview)
- [Common Setup](#common-setup)
- [Hierarchical Tree Layout](#hierarchical-tree-layout)
- [Organization Chart Layout](#organization-chart-layout)
- [Mind Map Layout](#mind-map-layout)
- [Flowchart Layout](#flowchart-layout)
- [Radial Tree Layout](#radial-tree-layout)
- [Force Directed Layout](#force-directed-layout)
- [Layout Configuration Options](#layout-configuration-options)

## Overview

SfDiagram provides built-in automatic layout algorithms that arrange nodes based on their relationships. Layouts work with either:
- **NodeCollection + ConnectorCollection** — define nodes/connectors manually
- **DataSourceSettings** — generate nodes from business data automatically

Supported layouts:
| Layout | Class | Use Case |
|--------|-------|----------|
| Hierarchical Tree | `DirectedTreeLayout` (Type=Hierarchical) | Multi-parent hierarchies |
| Organization Chart | `DirectedTreeLayout` (Type=Organization) | Org charts, reporting trees |
| Mind Map | `MindMapTreeLayout` | Concept maps, brainstorming |
| Flowchart | `FlowchartLayout` | Process flows with decision branches |
| Radial Tree | `RadialTreeLayout` | Radial/circular hierarchies |
| Force Directed | `ForceDirectedLayout` | Network graphs, relationship diagrams |

## Common Setup

Every layout requires a `LayoutManager` assigned to the diagram:

```xaml
<!--1. Define DataSourceSettings-->
<syncfusion:DataSourceSettings x:Key="DataSourceSettings"
                               Id="EmpId"
                               ParentId="ParentId"
                               DataSource="{StaticResource employees}"/>

<!--2. Define the Layout-->
<syncfusion:DirectedTreeLayout x:Key="treeLayout"
                               Type="Hierarchical"
                               Orientation="TopToBottom"
                               HorizontalSpacing="30"
                               VerticalSpacing="50"/>

<!--3. Wrap in LayoutManager-->
<syncfusion:LayoutManager x:Key="layoutManager"
                          Layout="{StaticResource treeLayout}"/>

<!--4. Assign to SfDiagram-->
<syncfusion:SfDiagram x:Name="diagram"
                      DataSourceSettings="{StaticResource DataSourceSettings}"
                      LayoutManager="{StaticResource layoutManager}"/>
```

## Hierarchical Tree Layout

Arranges nodes in a tree structure that supports multiple parents per node.

```csharp
diagram.DataSourceSettings = new DataSourceSettings()
{
    Id = "EmpId",
    ParentId = "ParentId",
    DataSource = employeeList,
};

diagram.LayoutManager = new LayoutManager()
{
    Layout = new DirectedTreeLayout()
    {
        Type = LayoutType.Hierarchical,
        Orientation = TreeOrientation.TopToBottom,
        HorizontalSpacing = 30,
        VerticalSpacing = 50,
        SpaceBetweenSubTrees = 20,
    }
};
```

**Sample data model:**
```csharp
public class Employee
{
    public string EmpId { get; set; }
    public string ParentId { get; set; }
    public string Name { get; set; }
}
```

## Organization Chart Layout

Arranges nodes in a classic org chart tree (single parent per node) with additional alignment options.

```csharp
diagram.DataSourceSettings = new DataSourceSettings()
{
    Id = "Name",
    ParentId = "ReportingPerson",
    DataSource = employeeList,
};

diagram.LayoutManager = new LayoutManager()
{
    Layout = new DirectedTreeLayout()
    {
        Type = LayoutType.Organization,
        Orientation = TreeOrientation.TopToBottom,
        HorizontalSpacing = 50,
        VerticalSpacing = 60,
    }
};
```

**Customize node appearance via ItemAddedEvent:**
```csharp
(diagram.Info as IGraphInfo).ItemAdded += (s, e) =>
{
    if (e.ItemType == ItemType.Node && e.Item is NodeViewModel node)
    {
        // Apply custom style based on data
        var employee = node.Content as Employee;
        node.ContentTemplate = App.Current.Resources["EmpTemplate"] as DataTemplate;
    }
};
```

## Mind Map Layout

Organizes nodes radially around a central root concept.

```csharp
diagram.DataSourceSettings = new DataSourceSettings()
{
    Id = "Label",
    ParentId = "ParentId",
    DataSource = mindmapItems,
    Root = "Creativity", // root node ID
};

diagram.LayoutManager = new LayoutManager()
{
    Layout = new MindMapTreeLayout()
    {
        Orientation = Orientation.Horizontal,
        HorizontalSpacing = 50,
        VerticalSpacing = 30,
    }
};
```

## Flowchart Layout

Arranges nodes based on flow direction, handling decision branches (diamond nodes) automatically.

```csharp
diagram.DataSourceSettings = new DataSourceSettings()
{
    Id = "Id",
    ParentId = "ParentId",
    DataSource = flowItems,
};

diagram.LayoutManager = new LayoutManager()
{
    Layout = new FlowchartLayout()
    {
        Orientation = FlowchartOrientation.TopToBottom,
        HorizontalSpacing = 30,
        VerticalSpacing = 50,
        YesBranchDirection = BranchDirection.LeftInFlow,
        NoBranchDirection  = BranchDirection.RightInFlow,
    }
};
```

**Flowchart-specific properties:**

| Property | Description |
|----------|-------------|
| `YesBranchValues` | Connector label values that indicate "yes" branch (e.g., `new List<string>{"Yes","True"}`) |
| `NoBranchValues` | Connector label values that indicate "no" branch |
| `YesBranchDirection` | Layout direction for yes branch (Left, Right, SameAsFlow) |
| `NoBranchDirection` | Layout direction for no branch |

## Radial Tree Layout

Arranges nodes concentrically around a root node, spreading outward in rings.

```csharp
diagram.DataSourceSettings = new DataSourceSettings()
{
    Id = "EmpId",
    ParentId = "ParentId",
    DataSource = employeeList,
    Root = "1",
};

diagram.LayoutManager = new LayoutManager()
{
    Layout = new RadialTreeLayout()
    {
        HorizontalSpacing = 30,
        VerticalSpacing   = 30,
    }
};
```

## Force Directed Layout

Simulates physics forces to produce a natural-looking network graph layout. No parent-child data required — use connectors to define relationships.

```csharp
// Build nodes and connectors manually
(diagram.Nodes as NodeCollection).Add(new NodeViewModel { ID = "A", Content = "A" });
(diagram.Nodes as NodeCollection).Add(new NodeViewModel { ID = "B", Content = "B" });
(diagram.Connectors as ConnectorCollection).Add(new ConnectorViewModel { SourceNodeID = "A", TargetNodeID = "B" });

diagram.LayoutManager = new LayoutManager()
{
    Layout = new ForceDirectedLayout()
    {
        HorizontalSpacing = 100,
        VerticalSpacing   = 100,
    }
};
```

## Layout Configuration Options

These properties are available on `DirectedTreeLayout` and most other layouts:

| Property | Description | Example |
|----------|-------------|---------|
| `HorizontalSpacing` | Space between nodes horizontally | `30` |
| `VerticalSpacing` | Space between nodes vertically | `50` |
| `SpaceBetweenSubTrees` | Extra gap between subtrees | `20` |
| `Orientation` | Overall layout direction | `TopToBottom`, `LeftToRight`, `BottomToTop`, `RightToLeft` |
| `Margin` | Padding around the layout | `new Thickness(50)` |

### Refresh Layout Programmatically
```csharp
// Force a layout refresh after data changes
diagram.LayoutManager.Layout.UpdateLayout();
```

### Layout with Custom Node Template
```xaml
<!--Apply DataTemplate to all auto-generated nodes-->
<syncfusion:SfDiagram.NodeTemplate>
    <DataTemplate>
        <Border Background="LightBlue" BorderBrush="Navy" BorderThickness="1"
                Padding="8" CornerRadius="4">
            <TextBlock Text="{Binding Name}" FontWeight="Bold"/>
        </Border>
    </DataTemplate>
</syncfusion:SfDiagram.NodeTemplate>
```

## Common Gotchas

- **Layout not rendering:** Make sure `DataSourceSettings.Id` and `ParentId` match your data model property names exactly.
- **Root node not found:** Set `DataSourceSettings.Root` to the ID of the root item (empty `ParentId` for root is also valid).
- **Nodes overlap:** Increase `HorizontalSpacing` and `VerticalSpacing`.
- **Flowchart branches wrong:** Set `YesBranchValues` and `NoBranchValues` to match your connector annotation text.

## Related
- Data source binding → `references/data-source.md`
- Node appearance customization → `references/nodes.md`
