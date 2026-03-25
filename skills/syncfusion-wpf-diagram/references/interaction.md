# Interaction in WPF Diagram (SfDiagram)

## Table of Contents
- [Selection](#selection)
- [Drag and Drop](#drag-and-drop)
- [Resize and Rotate](#resize-and-rotate)
- [Zoom and Pan](#zoom-and-pan)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Tooltips](#tooltips)
- [User Handles](#user-handles)
- [Scroll Settings and Limits](#scroll-settings-and-limits)
- [Snapping](#snapping)

## Selection

### Single Selection
Click a node or connector to select it. Previously selected items are deselected.

### Multiple Selection
- **Shift+Click** – select a range
- **Ctrl+Click** – add/toggle individual items
- **Rubber band (drag)** – draw a rectangle to select all enclosed elements

### Selection Mode
```csharp
// Always keep the node selected (no toggle)
diagram.SingleSelectionMode = SingleSelectionMode.Select;

// Toggle on re-click
diagram.SingleSelectionMode = SingleSelectionMode.ToggleSelection;

// Rubber band must fully enclose elements to select
diagram.MultipleSelectionMode = MultipleSelectionMode.RubberBandCompleteIntersect;
```

### Programmatic Selection
```csharp
// Select a node
nodeViewModel.IsSelected = true;

// Clear all selections
(diagram.Info as IGraphInfo).Commands.SelectAll.Execute(null);
```

### Select By Type
```csharp
// Select all nodes only
(diagram.Info as IGraphInfo).Commands.SelectObjects.Execute(
    new SelectObjectParameter { Type = typeof(NodeViewModel) });
```

### SelectorChangedEvent
```csharp
(diagram.Info as IGraphInfo).SelectorChangedEvent += (sender, args) =>
{
    // args contains OldValue and NewValue (OffsetX, OffsetY, Width, Height, RotateAngle)
};
```

## Drag and Drop

Nodes and connectors can be dragged by clicking and holding. Drag events are available:
```csharp
(diagram.Info as IGraphInfo).NodeChangedEvent += (s, e) =>
{
    if (e.NewValue.InteractionState == NodeChangedInteractionState.Dragging)
    {
        // Node is being dragged
    }
};
```

### Drag Limit
Prevent nodes from being dragged outside a region:
```xaml
<syncfusion:SfDiagram.ScrollSettings>
    <syncfusion:ScrollSettings ScrollLimit="Diagram"
                               ViewportWidth="800" ViewportHeight="600"/>
</syncfusion:SfDiagram.ScrollSettings>
```

```csharp
// Restrict node dragging to positive axis
diagram.Constraints |= GraphConstraints.AllowDragOnlyPositiveAxis;
```

### Collision State
Detect when nodes overlap:
```csharp
nodeViewModel.AllowOverlap = false; // prevent overlapping
```

## Resize and Rotate

Selected nodes show resize handles at corners/edges and a rotation handle above. Drag to resize or rotate.

### Resize Constraints
```csharp
// Lock width but allow height resize
node.Constraints = NodeConstraints.Default
    & ~NodeConstraints.ResizeWidth;
```

### Rotate
```csharp
node.RotateAngle = 45; // degrees
```

### Flip
```csharp
// Horizontal flip
(diagram.Info as IGraphInfo).Commands.Flip.Execute(
    new FlipParameter { Flip = FlipDirection.Horizontal });
```

## Zoom and Pan

### Mouse Gestures
- **Ctrl + Mouse Scroll** – zoom in/out
- **Mouse Scroll** – scroll vertically
- **Shift + Mouse Scroll** – scroll horizontally
- **Hold Middle Mouse Button + Move** – IntelliMouse pan

### Programmatic Zoom
```csharp
IGraphInfo graphInfo = diagram.Info as IGraphInfo;
// Zoom in
graphInfo.Commands.Zoom.Execute(new ZoomPositionParameter
{
    ZoomFactor = 1.2,
    ZoomCommand = ZoomCommand.ZoomIn,
});

// Reset zoom to 100%
graphInfo.Commands.Reset.Execute(
    new ResetParameter { Reset = Reset.ZoomAndPan });
```

### Fit to Page
```csharp
graphInfo.Commands.FitToPage.Execute(
    new FitToPageParameter { FitToPage = FitToPage.FitToPage });
```

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Ctrl+A | Select all |
| Ctrl+C / Ctrl+X / Ctrl+V | Copy / Cut / Paste |
| Ctrl+Z / Ctrl+Y | Undo / Redo |
| Ctrl+D | Duplicate selected |
| Delete | Delete selected |
| Arrow keys | Move selected 1px |
| Shift+Arrow | Move selected 10px |
| Ctrl+Mouse Scroll | Zoom in/out |
| Ctrl+G / Ctrl+Shift+U | Group / Ungroup |
| Ctrl+Shift+B / Ctrl+Shift+F | Send to back / Bring to front |
| Ctrl+] / Ctrl+[ | Bring forward / Send backward |
| Ctrl+Shift+W | Fit to page |
| F2 | Edit annotation |
| Ctrl+R / Ctrl+L | Rotate right/left 90° |
| Ctrl+H / Ctrl+J | Flip horizontal / vertical |

## Tooltips

Display a tooltip when hovering over a node or connector:
```xaml
<syncfusion:NodeViewModel UnitHeight="60" UnitWidth="120" OffsetX="200" OffsetY="150">
    <syncfusion:NodeViewModel.Tooltip>
        <syncfusion:ToolTipViewModel Content="Node description"/>
    </syncfusion:NodeViewModel.Tooltip>
</syncfusion:NodeViewModel>
```

```csharp
node.Tooltip = new ToolTipViewModel() { Content = "Node description" };
```

### Custom Tooltip Template
```xaml
<DataTemplate x:Key="TooltipTemplate">
    <Border Background="LightYellow" Padding="6" CornerRadius="3">
        <TextBlock Text="{Binding}" FontWeight="Bold"/>
    </Border>
</DataTemplate>

<syncfusion:NodeViewModel>
    <syncfusion:NodeViewModel.Tooltip>
        <syncfusion:ToolTipViewModel Content="Custom Tooltip"
                                     ContentTemplate="{StaticResource TooltipTemplate}"/>
    </syncfusion:NodeViewModel.Tooltip>
</syncfusion:NodeViewModel>
```

## User Handles

User handles are interactive buttons displayed around a selected node for quick actions:
```csharp
// Define a handle
UserHandleViewModel handle = new UserHandleViewModel()
{
    Name = "delete",
    Visible = true,
    HorizontalAlignment = HorizontalAlignment.Right,
    VerticalAlignment = VerticalAlignment.Top,
    Content = new Path() { Data = Geometry.Parse("M..."), Fill = Brushes.Red },
};

// Add to diagram
diagram.UserHandles = new ObservableCollection<IUserHandle>() { handle };

// React to click
(diagram.Info as IGraphInfo).UserHandleClickEvent += (s, e) =>
{
    if (e.Item.Name == "delete")
        (diagram.Info as IGraphInfo).Commands.Delete.Execute(null);
};
```

## Scroll Settings and Limits

```xaml
<syncfusion:SfDiagram.ScrollSettings>
    <syncfusion:ScrollSettings ScrollLimit="Infinity"
                               AutoScrollBorder="20,20,20,20"/>
</syncfusion:SfDiagram.ScrollSettings>
```

### Auto-Scroll
When dragging near the viewport edges, the diagram auto-scrolls:
```csharp
diagram.ScrollSettings = new ScrollSettings()
{
    ScrollLimit = ScrollLimit.Diagram, // restrict scroll to diagram bounds
    AutoScrollBorder = new Thickness(20),
};
```

### DragLimit (Restrict Node Movement)
```csharp
node.DragLimit = new Thickness(0, 0, 500, 500); // left, top, right, bottom bounds
```

### AutoScrollLimit
```csharp
diagram.ScrollSettings.AutoScrollLimit = new Rect(0, 0, 2000, 2000);
```

## Snapping

Snapping helps align elements precisely during drag, resize, or draw operations.

### Snap to Grid
```xaml
<syncfusion:SfDiagram.SnapSettings>
    <syncfusion:SnapSettings SnapConstraints="SnapToLines" SnapAngle="5"/>
</syncfusion:SfDiagram.SnapSettings>
```

### Snap to Objects
Snap to the bounds and centers of neighboring elements:
```csharp
diagram.SnapSettings = new SnapSettings()
{
    SnapToObject = SnapToObject.All,
};
```

**SnapToObject values:**
- `All` – snap to all neighbor edges and centers
- `HorizontalCenter` / `VerticalCenter` – center alignment
- `Left`, `Right`, `Top`, `Bottom` – edge alignment
- `Width`, `Height`, `Size` – same size snapping
- `HorizontalSpacing` / `VerticalSpacing` – equal spacing
- `Port` – snap to ports

### Custom Snap Interval
```csharp
diagram.SnapSettings = new SnapSettings()
{
    SnapConstraints = SnapConstraints.SnapToLines,
    SnapObjectDistance = 5,   // snap distance in pixels
    SnapAngle = 15,           // snap rotation to 15° increments
};
```

## Related
- Commands (alignment, clipboard, undo/redo) → `references/commands.md`
- Node constraints (lock/unlock) → `references/nodes.md`
- Tools (drawing mode) → `references/diagram-customization.md`
