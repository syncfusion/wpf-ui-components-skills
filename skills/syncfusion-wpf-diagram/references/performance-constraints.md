# Performance and Constraints in WPF Diagram (SfDiagram)

## Virtualization

Virtualization renders only the diagram elements currently visible in the viewport, dramatically improving performance for large diagrams.

```xaml
<syncfusion:SfDiagram x:Name="diagram" Constraints="Default,Virtualize"/>
```

```csharp
diagram.Constraints = diagram.Constraints | GraphConstraints.Virtualize;
```

### Deferred Scrolling (Outline)

While scrolling, show element outlines instead of full rendering until the viewport stabilizes:

```xaml
<syncfusion:SfDiagram Constraints="Default,Virtualize,Outline"/>
```

```csharp
diagram.Constraints |= GraphConstraints.Virtualize | GraphConstraints.Outline;
```

> **Note:** `Outline` requires `Virtualize` to be enabled.

### Outline Customization

```csharp
diagram.OutlineSettings = new OutlineSettings()
{
    OutlineStyle = new Style(typeof(Path))
    {
        Setters =
        {
            new Setter(Shape.StrokeProperty, new SolidColorBrush(Colors.Gray)),
            new Setter(Shape.StrokeThicknessProperty, 1.5),
        }
    },
    RenderInterval = new TimeSpan(0, 0, 0, 0, 200),  // milliseconds
};
```

---

## Bitwise Operations on Constraints

All constraint enums are flags — use bitwise operators to combine/remove them.

```csharp
// Add a constraint
diagram.Constraints = diagram.Constraints | GraphConstraints.Virtualize;
node.Constraints    = node.Constraints    | NodeConstraints.Rotatable;

// Remove a constraint
node.Constraints = node.Constraints & ~NodeConstraints.Rotatable;

// Combine add and remove in one expression
node.Constraints = NodeConstraints.Default & ~(NodeConstraints.Rotatable | NodeConstraints.InheritRotatable);

// Check if constraint is set
bool canRotate = (node.Constraints & NodeConstraints.Rotatable) == NodeConstraints.Rotatable;
```

Extension method equivalents:

```csharp
node.Constraints = node.Constraints.Add(NodeConstraints.Rotatable);
node.Constraints = node.Constraints.Remove(NodeConstraints.Rotatable);
```

---

## GraphConstraints (Diagram-level)

Apply to `diagram.Constraints`. Default enabled: `Zoomable`, `Pannable`, `PanRails`, `Relationship`, `Events`, `AutoScroll`, `PageEditing`.

| Constraint | Description |
|-----------|-------------|
| `Default` | All default behaviors |
| `None` | Disable everything |
| `Virtualize` | Viewport-only rendering |
| `Outline` | Deferred scrolling (requires Virtualize) |
| `Undoable` | Enable Undo/Redo |
| `Zoomable` | Allow zooming |
| `Pannable` | Allow panning (X and Y) |
| `PannableX` / `PannableY` | Limit panning to one axis |
| `AutoScroll` | Auto-scroll when dragging near edges |
| `Selectable` | Allow selecting elements |
| `Draggable` | Allow dragging between diagrams |
| `Resizable` | Allow resizing |
| `Rotatable` | Allow rotation |
| `Connectable` | Allow connector creation |
| `Routing` | Enable connector auto-routing |
| `Bridging` | Enable line bridging at intersections |
| `ContextMenu` | Show right-click context menu |
| `Commands` | Enable default keyboard commands |
| `DrawingTool` | Allow runtime drawing |
| `PageEditing` | Combines Selectable+Draggable+Connectable+Drop+Resizable+Rotatable+ContextMenu+DrawingTool+Commands |
| `Events` | Fire all diagram events |
| `AutomaticPortCreation` | Create ports automatically on connection |
| `FloatElements` | Drag elements between multiple diagrams |

**Common patterns:**

```csharp
// Read-only diagram
diagram.Constraints = GraphConstraints.Default & ~GraphConstraints.PageEditing;

// Disable undo
diagram.Constraints = GraphConstraints.Default & ~GraphConstraints.Undoable;

// Enable virtualization + undo
diagram.Constraints = GraphConstraints.Default | GraphConstraints.Virtualize | GraphConstraints.Undoable;

// Enable line bridging
diagram.Constraints = GraphConstraints.Default | GraphConstraints.Bridging;

// Enable connector routing
diagram.Constraints = GraphConstraints.Default | GraphConstraints.Routing;
```

---

## NodeConstraints

Apply to `NodeViewModel.Constraints`. Default enabled: `Selectable`, `Connectable`, `Inherit`, `RoutingObstacle`, `PivotDraggable`, `Delete`, `ThemeStyle`.

| Constraint | Description |
|-----------|-------------|
| `Default` | All default node behaviors |
| `None` | Disable all |
| `Selectable` | Can be selected |
| `Draggable` | Can be dragged |
| `Resizable` | Can be resized |
| `Rotatable` | Can be rotated |
| `Connectable` | Can have connectors attached |
| `Delete` | Can be deleted |
| `AspectRatio` | Resize proportionally |
| `InConnect` / `OutConnect` | Restrict connection direction |
| `DragAnnotation` | Allow dragging the node's annotation |
| `ExcludeFromLayout` | Skip this node in auto-layout |
| `AutomaticPortCreation` | Auto-create port on connection |
| `RoutingObstacle` | Treat as obstacle for connector routing |
| `ThemeStyle` | Apply diagram theme style |
| `Inherit` | Inherit Drag/Resize/Rotate/Snap from diagram |
| `InheritDraggable` | Inherit drag from diagram |
| `InheritResizable` | Inherit resize from diagram |
| `InheritRotatable` | Inherit rotate from diagram |
| `Menu` | Show custom context menu |

**Common patterns:**

```csharp
// Lock a node (no move/resize/rotate)
node.Constraints = NodeConstraints.Default
    & ~NodeConstraints.Draggable
    & ~NodeConstraints.Resizable
    & ~NodeConstraints.Rotatable
    & ~(NodeConstraints.InheritDraggable | NodeConstraints.InheritResizable | NodeConstraints.InheritRotatable);

// No rotation
node.Constraints = NodeConstraints.Default & ~(NodeConstraints.Rotatable | NodeConstraints.InheritRotatable);

// Aspect ratio resize
node.Constraints = NodeConstraints.Default | NodeConstraints.AspectRatio;

// Exclude from layout
node.Constraints = NodeConstraints.Default | NodeConstraints.ExcludeFromLayout;
```

---

## ConnectorConstraints

Apply to `ConnectorViewModel.Constraints`. Default enabled: `Selectable`, `EndDraggable`, `Inherit`, `Thumbs`, `Connectable`, `Delete`, `BridgeObstacle`, `ThemeStyle`.

| Constraint | Description |
|-----------|-------------|
| `Default` | All default connector behaviors |
| `None` | Disable all |
| `Selectable` | Can be selected |
| `Draggable` | Can be moved freely |
| `EndDraggable` | Source or target can be dragged |
| `SourceDraggable` | Source end can be dragged |
| `TargetDraggable` | Target end can be dragged |
| `Delete` | Can be deleted |
| `Connectable` | Node/port can connect to this connector |
| `Bridging` | Show bridge arcs at intersections |
| `BridgeObstacle` | Other connectors bridge over this one |
| `Routing` | Apply auto-routing |
| `InheritRouting` | Inherit routing from diagram |
| `InheritBridging` | Inherit bridging from diagram |
| `SegmentThumbs` | Show segment edit thumbs |
| `Thumbs` | Show all thumbs |
| `ThemeStyle` | Apply diagram theme |
| `DragAnnotation` | Allow dragging the connector's annotation |

**Common patterns:**

```csharp
// Disable selection
connector.Constraints = ConnectorConstraints.Default & ~ConnectorConstraints.Selectable;

// Fixed connector (no endpoint dragging)
connector.Constraints = ConnectorConstraints.Default
    & ~ConnectorConstraints.EndDraggable
    & ~ConnectorConstraints.SourceDraggable
    & ~ConnectorConstraints.TargetDraggable;

// Inherit bridging from diagram-level setting
connector.Constraints = ConnectorConstraints.Default | ConnectorConstraints.InheritBridging;
```

---

## PortConstraints

Apply to `NodePortViewModel.Constraints`. Default enabled: `Inherit`.

| Constraint | Description |
|-----------|-------------|
| `Default` | Inherit constraints |
| `None` | Disable all |
| `Connectable` | Allow connectors to start/end here |
| `InConnect` | Allow incoming connections |
| `OutConnect` | Allow outgoing connections |
| `Draggable` | Port can be dragged along node boundary |
| `Dynamic` | Port moves to closest boundary point when connecting |
| `Snapping` | Snap connectors to this port |
| `Inherit` | Inherit from parent node |
| `InheritConnectable` | Inherit `Connectable` from node |
| `InheritPortVisibility` | Inherit visibility from diagram/node |
| `ConnectionDirection` | Use `PortBase.ConnectionDirection` |

```csharp
// Disable connection to this port
port.Constraints = PortConstraints.Default & ~PortConstraints.Connectable;

// Outgoing only
port.Constraints = PortConstraints.Default | PortConstraints.OutConnect & ~PortConstraints.InConnect;
```

---

## AnnotationConstraints

Apply to `AnnotationEditorViewModel.Constraints`. Default enabled: `Inherit`, `Editable`.

| Constraint | Description |
|-----------|-------------|
| `Default` | All default annotation behaviors |
| `None` | Disable all |
| `Editable` | Can be double-click edited |
| `Draggable` | Can be dragged |
| `Selectable` | Can be selected |
| `Resizable` | Can be resized |
| `Rotatable` | Can be rotated |
| `Inherit` | Inherit from parent node/connector |
| `InheritEditable` | Inherit editable |
| `InheritDraggable` | Inherit draggable |
| `DragLimit` | Constrain drag within bounds |

```csharp
// Read-only annotation
annotation.Constraints = AnnotationConstraints.Default & ~AnnotationConstraints.Editable;

// Draggable annotation
annotation.Constraints = AnnotationConstraints.Draggable;
```

---

## SelectorConstraints

Apply to `(diagram.SelectedItems as SelectorViewModel).SelectorConstraints`.

| Constraint | Description |
|-----------|-------------|
| `Default` | All selector behaviors |
| `Rotator` | Show rotation handle |
| `Resizer` | Show resize handles |
| `Pivot` | Show pivot point |
| `Tooltip` | Show tooltip on hover |
| `TooltipPosition` | Show position tooltip while dragging |
| `TooltipSize` | Show size tooltip while resizing |
| `TooltipAngle` | Show angle tooltip while rotating |
| `QuickCommands` | Show quick command buttons |
| `HideDisabledResizer` | Hide resize handles when disabled |

```csharp
// Hide rotation handle
(diagram.SelectedItems as SelectorViewModel).SelectorConstraints =
    (diagram.SelectedItems as SelectorViewModel).SelectorConstraints & ~SelectorConstraints.Rotator;
```

---

## Common Gotchas

- **Inherit constraints must also be removed:** When disabling rotation on a node, remove both `Rotatable` AND `InheritRotatable`, otherwise the node inherits the diagram-level setting.
- **Virtualization + export:** Virtualization affects what is rendered in the viewport — use `ExportMode.Content` to export all elements regardless of virtualization.
- **Outline without Virtualize:** `GraphConstraints.Outline` has no effect without `GraphConstraints.Virtualize`.
- **PageEditing shortcut:** `GraphConstraints.PageEditing` enables a bundle of behaviors — to make a fully read-only diagram, use `~GraphConstraints.PageEditing` rather than removing each one individually.

## Related
- Undo/Redo constraints → `references/serialization-undo-redo.md`
- Snapping constraints → `references/interaction.md`
- Port constraints → `references/ports.md`
