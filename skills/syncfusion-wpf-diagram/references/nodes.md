# Nodes in WPF Diagram (SfDiagram)

## Table of Contents
- [Overview](#overview)
- [Creating Nodes](#creating-nodes)
- [Node Visualization Options](#node-visualization-options)
- [Built-In Shapes](#built-in-shapes)
- [Custom Shapes](#custom-shapes)
- [Node Appearance](#node-appearance)
- [Node Position and Size](#node-position-and-size)
- [Groups and Containers](#groups-and-containers)
- [Node Constraints](#node-constraints)
- [Node Events](#node-events)

## Overview

Nodes are graphical objects used to represent entities, processes, or data in a diagram. They are added to the `SfDiagram.Nodes` collection as `NodeViewModel` instances. Nodes stack in the diagram from bottom to top in the order they are added.

## Creating Nodes

### XAML
```xaml
<syncfusion:SfDiagram.Nodes>
    <syncfusion:NodeCollection>
        <syncfusion:NodeViewModel ID="node1"
                                  OffsetX="200" OffsetY="150"
                                  UnitWidth="120" UnitHeight="60"
                                  Shape="{StaticResource Rectangle}"
                                  ShapeStyle="{StaticResource ShapeStyle}"/>
    </syncfusion:NodeCollection>
</syncfusion:SfDiagram.Nodes>
```

### C#
```csharp
NodeViewModel node = new NodeViewModel()
{
    ID = "node1",
    UnitWidth = 120,
    UnitHeight = 60,
    OffsetX = 200,
    OffsetY = 150,
    Shape = App.Current.Resources["Rectangle"],
    ShapeStyle = App.Current.Resources["ShapeStyle"] as Style,
};
(diagram.Nodes as NodeCollection).Add(node);
```

## Node Visualization Options

SfDiagram supports five ways to visualize a node:

| Option | Property | Use When |
|--------|----------|----------|
| Built-in shape | `Shape` (from ResourceDictionary) | Standard flowchart/diagram shapes |
| Custom path | `Shape` (path data string) | Custom SVG-like geometry |
| Data template | `ContentTemplate` | Complex visual with XAML layout |
| Content | `Content` | Image, text, or any UIElement |
| Geometry | `Shape` (Geometry object) | Programmatic geometry shapes |

## Built-In Shapes

Merge the built-in resource dictionary first:
```xaml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/BasicShapes.xaml"/>
</ResourceDictionary.MergedDictionaries>
```

Then reference shapes by key:
```xaml
<syncfusion:NodeViewModel Shape="{StaticResource Ellipse}"/>
<syncfusion:NodeViewModel Shape="{StaticResource Rectangle}"/>
<syncfusion:NodeViewModel Shape="{StaticResource Diamond}"/>
<syncfusion:NodeViewModel Shape="{StaticResource PredefinedProcess}"/>
```

**Available Basic Shapes:** Rectangle, Triangle, Plus, Pentagon, Hexagon, Heptagon, Octagon, Trapezoid, Decagon, RightTriangle, Cylinder, Diamond, RoundedRectangle, Cloud, Paralleogram, Donut, and more.

**Flow Chart Shapes (in FlowShapes.xaml):** Process, Decision, Terminator, PredefinedProcess, DirectData, StoredData, ManualInput, and more.

## Custom Shapes

### Using Path Data String
```csharp
// Define a custom path in Resources
// <sys:String x:Key="CustomShape">M0,0 L100,0 L100,100 L0,100z</sys:String>
NodeViewModel node = new NodeViewModel()
{
    Shape = App.Current.Resources["CustomShape"],
    ShapeStyle = App.Current.Resources["ShapeStyle"] as Style,
};
```

### Using DataTemplate
```xaml
<DataTemplate x:Key="NodeTemplate">
    <Border BorderThickness="2" BorderBrush="DarkBlue" CornerRadius="5">
        <StackPanel>
            <Image Source="Images/icon.png" Height="30"/>
            <TextBlock Text="Label" HorizontalAlignment="Center"/>
        </StackPanel>
    </Border>
</DataTemplate>

<syncfusion:NodeViewModel UnitHeight="80" UnitWidth="100"
                          OffsetX="200" OffsetY="150"
                          ContentTemplate="{StaticResource NodeTemplate}"/>
```

### Using Image as Content
```xaml
<DataTemplate x:Key="UserTemplate">
    <Image Stretch="Uniform" Source="Images/user.png"/>
</DataTemplate>

<syncfusion:NodeViewModel ContentTemplate="{StaticResource UserTemplate}"
                          UnitHeight="80" UnitWidth="80"/>
```

## Node Appearance

### ShapeStyle
Controls fill, stroke, and stroke thickness of the shape:
```xaml
<Style x:Key="ShapeStyle" TargetType="Path">
    <Setter Property="Fill" Value="#FF5B9BD5"/>
    <Setter Property="Stroke" Value="#FFEDF1F6"/>
    <Setter Property="StrokeThickness" Value="1"/>
    <Setter Property="Stretch" Value="Fill"/>
</Style>
```

### Apply Globally to All Nodes
```xaml
<Style TargetType="syncfusion:Node">
    <Setter Property="ShapeStyle">
        <Setter.Value>
            <Style TargetType="Path">
                <Setter Property="Fill" Value="CornflowerBlue"/>
                <Setter Property="Stretch" Value="Fill"/>
                <Setter Property="Stroke" Value="Black"/>
            </Style>
        </Setter.Value>
    </Setter>
</Style>
```

## Node Position and Size

| Property | Type | Description |
|----------|------|-------------|
| `OffsetX` | double | X position of node center |
| `OffsetY` | double | Y position of node center |
| `UnitWidth` | double | Width of the node |
| `UnitHeight` | double | Height of the node |
| `RotateAngle` | double | Rotation in degrees |
| `Pivot` | Point | Pivot point for rotation (default: 0.5, 0.5) |

```csharp
NodeViewModel node = new NodeViewModel()
{
    OffsetX = 300, OffsetY = 200,
    UnitWidth = 150, UnitHeight = 80,
    RotateAngle = 45,
};
```

## Groups and Containers

### Grouping Nodes
Group multiple nodes so they move and resize together:
```csharp
GroupViewModel group = new GroupViewModel()
{
    Nodes = new NodeCollection()
    {
        new NodeViewModel { ID = "n1", OffsetX = 100, OffsetY = 100, UnitWidth = 80, UnitHeight = 50 },
        new NodeViewModel { ID = "n2", OffsetX = 250, OffsetY = 100, UnitWidth = 80, UnitHeight = 50 },
    }
};
(diagram.Nodes as NodeCollection).Add(group);
```

### Container
A container visually encloses child nodes with a labeled border:
```csharp
ContainerViewModel container = new ContainerViewModel()
{
    ID = "container1",
    UnitWidth = 300,
    UnitHeight = 200,
    OffsetX = 200,
    OffsetY = 150,
    Header = new ContainerHeaderViewModel() { Content = "Container Title" },
};
(diagram.Nodes as NodeCollection).Add(container);
```

## Node Constraints

Use `NodeConstraints` flags to enable/disable behaviors:

```csharp
// Lock a node (no interaction)
node.Constraints = NodeConstraints.Default & ~NodeConstraints.Draggable
                                           & ~NodeConstraints.Resizable
                                           & ~NodeConstraints.Rotatable;

// Allow only selection, no drag
node.Constraints = NodeConstraints.Selectable;

// Exclude node from automatic layout
node.Constraints = NodeConstraints.Default | NodeConstraints.ExcludeFromLayout;
```

**Key NodeConstraints flags:**
- `Selectable` – can be selected
- `Draggable` – can be dragged
- `Resizable` – can be resized
- `Rotatable` – can be rotated
- `Connectable` – can be connected with a connector
- `Delete` – can be deleted
- `ExcludeFromLayout` – skipped during automatic layout

## Node Events

Listen for node changes via `IGraphInfo`:
```csharp
(diagram.Info as IGraphInfo).ItemAdded += OnItemAdded;
(diagram.Info as IGraphInfo).ItemDeleted += OnItemDeleted;
(diagram.Info as IGraphInfo).NodeChangedEvent += OnNodeChanged;

private void OnNodeChanged(object sender, ChangeEventArgs<object, NodeChangedEventArgs> args)
{
    if (args.Item is NodeViewModel node)
    {
        // React to position, size, or style changes
    }
}
```

## Related
- Annotations on nodes → `references/annotations.md`
- Ports on nodes → `references/ports.md`
- Interaction (select, drag, resize) → `references/interaction.md`
- Drawing nodes interactively → `references/diagram-customization.md` (Tools section)
