# Connectors in WPF Diagram (SfDiagram)

## Table of Contents
- [Overview](#overview)
- [Connector Types](#connector-types)
- [Creating Connectors](#creating-connectors)
- [Connection Targets](#connection-targets)
- [Connector Segments](#connector-segments)
- [Connector Appearance](#connector-appearance)
- [Decorators (Arrows)](#decorators-arrows)
- [Connector Constraints](#connector-constraints)
- [Connector Interaction](#connector-interaction)
- [Connector Splitting](#connector-splitting)
- [Free-Hand and Polyline Drawing](#free-hand-and-polyline-drawing)

## Overview

Connectors link nodes, ports, or arbitrary points and represent relationships or flow between diagram elements. They are added to `SfDiagram.Connectors` as `ConnectorViewModel` instances.

## Connector Types

SfDiagram supports five connector types, set via `DefaultConnectorType` on the diagram or `Type` on individual connectors:

| Type | Description |
|------|-------------|
| `Orthogonal` | Right-angle lines (default) |
| `Line` | Straight line between points |
| `CubicBezier` | Smooth curved line with control points |
| `QuadraticBezier` | Single curve with one control point |
| `PolyLine` | Multi-segment line drawn interactively |

```xaml
<!--Set default type for all new connectors-->
<syncfusion:SfDiagram x:Name="diagram" DefaultConnectorType="CubicBezier"/>
```

```csharp
// Set the diagram-wide default
diagram.DefaultConnectorType = ConnectorType.Orthogonal;
```

## Creating Connectors

### Between Two Nodes (using IDs)
```xaml
<syncfusion:ConnectorViewModel SourceNodeID="node1" TargetNodeID="node2"/>
```
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourceNodeID = "node1",
    TargetNodeID = "node2",
};
(diagram.Connectors as ConnectorCollection).Add(connector);
```

### Between Two Points
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourcePoint = new Point(100, 100),
    TargetPoint = new Point(300, 250),
};
```

### Between Ports
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourceNodeID = "node1",
    SourcePortID = "port1",
    TargetNodeID = "node2",
    TargetPortID = "port2",
};
```

### Point to Node
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourcePoint = new Point(50, 50),
    TargetNodeID = "node1",
};
```

## Connection Targets

| Property | Type | Description |
|----------|------|-------------|
| `SourceNodeID` | string | ID of the source node |
| `TargetNodeID` | string | ID of the target node |
| `SourcePortID` | string | ID of the source port |
| `TargetPortID` | string | ID of the target port |
| `SourcePoint` | Point | Absolute source coordinate |
| `TargetPoint` | Point | Absolute target coordinate |

## Connector Segments

Control the path shape by defining segments. Each connector type has its own segment class.

### Orthogonal Segments
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourceNodeID = "n1",
    TargetNodeID = "n2",
    Segments = new ObservableCollection<IConnectorSegment>()
    {
        new OrthogonalSegment() { Direction = OrthogonalDirection.Right, Length = 100 },
        new OrthogonalSegment() { Direction = OrthogonalDirection.Bottom, Length = 150 },
    }
};
```

### Straight Segments
```csharp
Segments = new ObservableCollection<IConnectorSegment>()
{
    new StraightSegment() { Point = new Point(200, 100) },
}
```

### Cubic Bezier Segments
```csharp
Segments = new ObservableCollection<IConnectorSegment>()
{
    new CubicCurveSegment()
    {
        Point1 = new Point(150, 50),   // control point 1
        Point2 = new Point(200, 250),  // control point 2
        Point  = new Point(300, 150),  // end point
    }
}
```

## Connector Appearance

### ConnectorGeometryStyle
Controls the line color, thickness, and dash pattern:
```xaml
<Style x:Key="ConnectorStyle" TargetType="Path">
    <Setter Property="Stroke" Value="#6BA5D7"/>
    <Setter Property="StrokeThickness" Value="2"/>
    <Setter Property="StrokeDashArray" Value="5,3"/>
</Style>
```

### Apply Globally to All Connectors
```xaml
<Style TargetType="syncfusion:Connector">
    <Setter Property="ConnectorGeometryStyle">
        <Setter.Value>
            <Style TargetType="Path">
                <Setter Property="Stroke" Value="Black"/>
                <Setter Property="StrokeThickness" Value="1"/>
            </Style>
        </Setter.Value>
    </Setter>
</Style>
```

### Per-Connector
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourceNodeID = "n1",
    TargetNodeID = "n2",
    ConnectorGeometryStyle = App.Current.Resources["ConnectorStyle"] as Style,
};
```

## Decorators (Arrows)

Control arrow heads at source and target:

```xaml
<Style x:Key="ArrowStyle" TargetType="Path">
    <Setter Property="Fill" Value="Black"/>
    <Setter Property="Stretch" Value="Fill"/>
    <Setter Property="Height" Value="10"/>
    <Setter Property="Width" Value="10"/>
</Style>
```

```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourceNodeID = "n1",
    TargetNodeID = "n2",
    // Arrow at target (default)
    TargetDecoratorStyle = App.Current.Resources["ArrowStyle"] as Style,
    // Arrow at source
    SourceDecoratorStyle = App.Current.Resources["ArrowStyle"] as Style,
    // Hide decorators
    TargetDecorator = null,
};
```

## Connector Constraints

Use `ConnectorConstraints` flags to control behaviors:

```csharp
// Make connector non-interactive (read-only appearance)
connector.Constraints = ConnectorConstraints.Default
    & ~ConnectorConstraints.Draggable
    & ~ConnectorConstraints.Selectable;

// Disable deletion
connector.Constraints = ConnectorConstraints.Default & ~ConnectorConstraints.Delete;
```

**Key ConnectorConstraints flags:**
- `Selectable` – can be selected
- `Draggable` – the connector line can be dragged
- `Deletable` / `Delete` – can be deleted
- `DragSourceEnd` – source end can be moved
- `DragTargetEnd` – target end can be moved
- `InheritBridging` – inherits line bridging from diagram

## Connector Interaction

- **Move connector:** Drag the connector line body (if `Draggable` is set)
- **Redirect endpoint:** Drag the source/target thumbs to reconnect
- **Select:** Click the connector line
- **Label:** Add `AnnotationEditorViewModel` to `connector.Annotations`

## Connector Splitting

When `EnableConnectorSplitting` is true, dropping a node on a connector automatically splits it and inserts the node into the flow:

```xaml
<syncfusion:SfDiagram x:Name="diagram" 
                      Constraints="Default,EnableConnectorSplitting"/>
```

```csharp
diagram.Constraints |= GraphConstraints.EnableConnectorSplitting;
```

## Free-Hand and Polyline Drawing

### Polyline (click to add segments, double-click to finish)
```csharp
diagram.DefaultConnectorType = ConnectorType.PolyLine;
diagram.Tool = Tool.ContinuesDraw;
diagram.DrawingTool = DrawingTool.Connector;
```

### Free-Hand Drawing
```csharp
diagram.Tool = Tool.ContinuesDraw;
diagram.DrawingTool = DrawingTool.FreeHand;
```

## Common Gotchas

- **Connector not appearing:** Ensure both `SourceNodeID` and `TargetNodeID` match the node `ID` values exactly.
- **Orthogonal routing conflicts:** Use `GraphConstraints.Routing` to enable automatic routing around obstacles.
- **Line bridging at intersections:** Enable with `diagram.Constraints |= GraphConstraints.Bridging`.

## Related
- Ports for fixed connection points → `references/ports.md`
- Labels on connectors → `references/annotations.md`
- Connector routing constraints → `references/performance-constraints.md`
