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

## Related
- Connector source/target ports → `references/connectors.md`
- Node definition → `references/nodes.md`
