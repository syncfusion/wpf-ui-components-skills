# Ports in WPF Diagram (SfDiagram)

## Overview

Ports are fixed connection points on a node or connector. When a connector is glued to a port (rather than the node body), it stays attached to that exact point even when the node moves. SfDiagram supports three port types: **NodePort**, **ConnectorPort**, and **DockPort**.

**Dynamic connection** (node body): Connector moves freely around the node perimeter — shortest path.  
**Port connection**: Connector stays fixed to a specific point, regardless of node movement.

## Node Ports

### Adding a Port to a Node (XAML)
```xaml
<syncfusion:SfDiagram x:Name="diagram" PortVisibility="Visible">
    <syncfusion:SfDiagram.Nodes>
        <syncfusion:NodeCollection>
            <syncfusion:NodeViewModel UnitHeight="100" UnitWidth="100"
                                      OffsetX="200" OffsetY="200"
                                      Shape="{StaticResource Rectangle}">
                <syncfusion:NodeViewModel.Ports>
                    <syncfusion:PortCollection>
                        <!--Port at center-right (NodeOffsetX=1, NodeOffsetY=0.5)-->
                        <syncfusion:NodePortViewModel x:Name="rightPort"
                                                      NodeOffsetX="1"
                                                      NodeOffsetY="0.5"/>
                        <!--Port at bottom-center-->
                        <syncfusion:NodePortViewModel NodeOffsetX="0.5"
                                                      NodeOffsetY="1"/>
                    </syncfusion:PortCollection>
                </syncfusion:NodeViewModel.Ports>
            </syncfusion:NodeViewModel>
        </syncfusion:NodeCollection>
    </syncfusion:SfDiagram.Nodes>
</syncfusion:SfDiagram>
```

### In C#
```csharp
NodeViewModel node = new NodeViewModel()
{
    ID = "node1",
    UnitWidth = 100, UnitHeight = 100,
    OffsetX = 200, OffsetY = 200,
    Shape = App.Current.Resources["Rectangle"],
};

// Add port at center-right
(node.Ports as PortCollection).Add(new NodePortViewModel()
{
    ID = "rightPort",
    NodeOffsetX = 1.0,
    NodeOffsetY = 0.5,
});

(diagram.Nodes as NodeCollection).Add(node);
```

### NodeOffsetX / NodeOffsetY

Position the port as a fraction of the node's width/height:

| NodeOffsetX | NodeOffsetY | Position |
|-------------|-------------|----------|
| 0 | 0 | Top-left |
| 0.5 | 0 | Top-center |
| 1 | 0 | Top-right |
| 0 | 0.5 | Center-left |
| 0.5 | 0.5 | Center |
| 1 | 0.5 | Center-right |
| 0.5 | 1 | Bottom-center |

## Port Appearance

Style the port shape and fill:
```xaml
<Style TargetType="syncfusion:NodePort">
    <Setter Property="Shape">
        <Setter.Value>
            <EllipseGeometry RadiusX="5" RadiusY="5"/>
        </Setter.Value>
    </Setter>
    <Setter Property="ShapeStyle">
        <Setter.Value>
            <Style TargetType="Path">
                <Setter Property="Stretch" Value="Fill"/>
                <Setter Property="Fill" Value="#FF6BA5D7"/>
                <Setter Property="Stroke" Value="White"/>
                <Setter Property="StrokeThickness" Value="1"/>
            </Style>
        </Setter.Value>
    </Setter>
</Style>
```

## Port Visibility

Control when ports are visible:

```xaml
<!--Always show ports-->
<syncfusion:SfDiagram PortVisibility="Visible"/>

<!--Show ports only on hover (default)-->
<syncfusion:SfDiagram PortVisibility="MouseOver"/>

<!--Hide ports entirely-->
<syncfusion:SfDiagram PortVisibility="Collapse"/>
```

```csharp
diagram.PortVisibility = PortVisibility.Visible;
```

## Port Hover Effect

Add a visual hover effect when the user hovers over a port:
```csharp
NodePortViewModel port = new NodePortViewModel()
{
    NodeOffsetX = 1,
    NodeOffsetY = 0.5,
    // PortHoverEffect highlights the port on mouse hover
};
```

See docs at `docs/Port/PortHoverEffect.md` for style customization.

## Automatic Port Creation

When `AutomaticPortCreation` is enabled, ports are created automatically when a user starts dragging a connector from or to a node:

```xaml
<syncfusion:SfDiagram Constraints="Default,AutomaticPortCreation"/>
```

```csharp
diagram.Constraints |= GraphConstraints.AutomaticPortCreation;
```

Or per-node:
```csharp
node.Constraints = NodeConstraints.Default | NodeConstraints.AutomaticPortCreation;
```

## Port-to-Port Connections

Connect two specific ports explicitly:
```csharp
ConnectorViewModel connector = new ConnectorViewModel()
{
    SourceNodeID = "node1",
    SourcePortID = "rightPort",
    TargetNodeID = "node2",
    TargetPortID = "leftPort",
};
(diagram.Connectors as ConnectorCollection).Add(connector);
```

## Connector Ports

A ConnectorPort is a point on a connector's path. Connectors can be connected to other connectors via connector ports:
```csharp
ConnectorPortViewModel connectorPort = new ConnectorPortViewModel()
{
    Length = 0.5,  // midpoint of the connector
};
(connector.Ports as PortCollection).Add(connectorPort);
```

## Dock Port

A `DockPort` defines a **line segment** on a node boundary — connectors snap to the nearest point along that line. Unlike `NodePort` (fixed point), multiple connectors can attach at different positions on the same dock line.  
[`SourcePoint`](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Diagram.DockPortViewModel.html#Syncfusion_UI_Xaml_Diagram_DockPortViewModel_SourcePoint) and [`TargetPoint`](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Diagram.DockPortViewModel.html#Syncfusion_UI_Xaml_Diagram_DockPortViewModel_TargetPoint) are fractions `(X, Y)` of the node width/height — same coordinate system as `NodeOffsetX`/`NodeOffsetY`.

> **Important:** `SourcePoint`, `TargetPoint`, and `ConnectorGeometryStyle` are all **required**. Omitting `ConnectorGeometryStyle` makes the dock line invisible.

### Common DockPort Positions

| SourcePoint | TargetPoint | Dock line |
|-------------|-------------|-----------|
| `0,1` | `1,1` | Full bottom edge |
| `0,0` | `1,0` | Full top edge |
| `0,0` | `0,1` | Full left edge |
| `1,0` | `1,1` | Full right edge |
| `0,0.5` | `1,0.5` | Horizontal center line |

### XAML
```xaml
<syncfusion:NodeViewModel OffsetX="100" OffsetY="100"
                          UnitHeight="100" UnitWidth="100"
                          Shape="{StaticResource Rectangle}">
    <syncfusion:NodeViewModel.Ports>
        <syncfusion:PortCollection>
            <syncfusion:DockPortViewModel ID="bottomDock"
                                          SourcePoint="0,1"
                                          TargetPoint="1,1">
                <syncfusion:DockPortViewModel.ConnectorGeometryStyle>
                    <Style TargetType="Path">
                        <Setter Property="Stroke"          Value="Black"/>
                        <Setter Property="StrokeThickness" Value="3"/>
                    </Style>
                </syncfusion:DockPortViewModel.ConnectorGeometryStyle>
            </syncfusion:DockPortViewModel>
        </syncfusion:PortCollection>
    </syncfusion:NodeViewModel.Ports>
</syncfusion:NodeViewModel>
```

### C#
```csharp
// ConnectorGeometryStyle is required — omitting it makes the dock line invisible
var dockLineStyle = new Style(typeof(Path));
dockLineStyle.Setters.Add(new Setter(Path.StrokeProperty, new SolidColorBrush(Colors.Black)));
dockLineStyle.Setters.Add(new Setter(Path.StrokeThicknessProperty, 3.0));

var node = new NodeViewModel
{
    ID        = "node1",
    UnitWidth = 100, UnitHeight = 100,
    OffsetX   = 100, OffsetY    = 100,
    Shape     = App.Current.Resources["Rectangle"],
    Ports     = new PortCollection()   // must initialise — Ports is null by default
};

(node.Ports as PortCollection).Add(new DockPortViewModel()
{
    ID                     = "bottomDock",
    SourcePoint            = new Point(0, 1),
    TargetPoint            = new Point(1, 1),
    ConnectorGeometryStyle = dockLineStyle
});

(diagram.Nodes as NodeCollection).Add(node);
```

### Connecting a Connector to a Dock Port

Reference the dock port `ID` via `SourcePortID` or `TargetPortID`. The connector snaps to the nearest point on the dock line automatically.

```csharp
var connector = new ConnectorViewModel()
{
    SourceNodeID = "node1",
    SourcePortID = "bottomDock",  // DockPort ID
    TargetNodeID = "node2",
    TargetPortID = "topPort"      // NodePort ID on target node
};
(diagram.Connectors as ConnectorCollection).Add(connector);
```

## Port Constraints

```csharp
NodePortViewModel port = new NodePortViewModel()
{
    NodeOffsetX = 0.5,
    NodeOffsetY = 0,
    // Only allow incoming connections on this port
    Constraints = PortConstraints.Default & ~PortConstraints.OutConnect,
};
```

**Key PortConstraints flags:**
- `InConnect` – allows connectors to terminate at this port
- `OutConnect` – allows connectors to originate from this port
- `Drag` – allows the port to be dragged

## Common Gotchas

- **Ports not visible:** Set `diagram.PortVisibility = PortVisibility.Visible` to always show, or `PortVisibility.MouseOver` to show on hover.
- **Connector not snapping to port:** Ensure `SourcePortID`/`TargetPortID` matches the port's `ID` exactly.
- **Port ID not set:** Always assign an `ID` to ports you want to reference in connectors.
- **DockPort not rendering:** `DockPort` requires `SourcePoint`, `TargetPoint`, **and** `ConnectorGeometryStyle` all set. Omitting `ConnectorGeometryStyle` makes the dock line completely invisible even though the port is functional.
- **NullReferenceException when adding ports in C#:** `NodeViewModel.Ports` is `null` by default. Always initialise it before adding: `node.Ports = new PortCollection();`

## Related
- Connector source/target ports → `references/connectors.md`
- Node definition → `references/nodes.md`
