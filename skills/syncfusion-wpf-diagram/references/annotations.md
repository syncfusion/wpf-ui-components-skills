# Annotations in WPF Diagram (SfDiagram)

## Overview

Annotations are text labels displayed over nodes or connectors. They allow you to describe elements textually, and can be edited at runtime. Multiple annotations can be added to any node or connector.

## Defining Annotations

### On a Node (XAML)
```xaml
<syncfusion:NodeViewModel UnitWidth="120" UnitHeight="60"
                          OffsetX="200" OffsetY="150"
                          Shape="{StaticResource Rectangle}">
    <syncfusion:NodeViewModel.Annotations>
        <syncfusion:AnnotationCollection>
            <syncfusion:AnnotationEditorViewModel Content="Node Label"/>
        </syncfusion:AnnotationCollection>
    </syncfusion:NodeViewModel.Annotations>
</syncfusion:NodeViewModel>
```

### On a Connector (XAML)
```xaml
<syncfusion:ConnectorViewModel SourceNodeID="n1" TargetNodeID="n2">
    <syncfusion:ConnectorViewModel.Annotations>
        <syncfusion:AnnotationCollection>
            <syncfusion:AnnotationEditorViewModel Content="Flow Label"/>
        </syncfusion:AnnotationCollection>
    </syncfusion:ConnectorViewModel.Annotations>
</syncfusion:ConnectorViewModel>
```

### In C#
```csharp
NodeViewModel node = new NodeViewModel()
{
    UnitWidth = 120, UnitHeight = 60,
    OffsetX = 200, OffsetY = 150,
    Annotations = new ObservableCollection<IAnnotation>()
    {
        new AnnotationEditorViewModel() { Content = "Node Label" }
    }
};
```

### Multiple Annotations
You can add more than one annotation to a node or connector:
```csharp
Annotations = new ObservableCollection<IAnnotation>()
{
    new AnnotationEditorViewModel() { Content = "Title", Offset = new Point(0.5, 0) },
    new AnnotationEditorViewModel() { Content = "Detail", Offset = new Point(0.5, 1) },
}
```

## Positioning Annotations

### Offset (for Node Annotations)
The `Offset` property positions the annotation relative to the node using fractional coordinates:
- `(0, 0)` = top-left corner
- `(0.5, 0.5)` = center (default)
- `(1, 1)` = bottom-right corner

```csharp
new AnnotationEditorViewModel()
{
    Content = "Top Label",
    Offset = new Point(0.5, 0),     // top-center
    VerticalAlignment = VerticalAlignment.Bottom,  // anchor below the point
}
```

### HorizontalAlignment and VerticalAlignment
Fine-tune position relative to the offset point:
```csharp
new AnnotationEditorViewModel()
{
    Content = "Right Label",
    Offset = new Point(1, 0.5),
    HorizontalAlignment = HorizontalAlignment.Left,  // label extends to the right of the point
}
```

### Margin
Add spacing between the annotation and the node:
```csharp
new AnnotationEditorViewModel()
{
    Content = "Below Node",
    Offset = new Point(0.5, 1),
    VerticalAlignment = VerticalAlignment.Top,
    Margin = new Thickness(0, 5, 0, 0),
}
```

### Connector Annotation Positioning
For connectors, use `Length` (0–1, fraction along the connector) and `Displacement`:
```csharp
new AnnotationEditorViewModel()
{
    Content = "Midpoint Label",
    Length = 0.5,           // middle of the connector
    Displacement = new Point(0, -15), // offset above the line
}
```

## Annotation Appearance

### Custom Style via AnnotationStyle
```xaml
<Style x:Key="AnnotationStyle" TargetType="syncfusion:AnnotationEditor">
    <Setter Property="FontSize" Value="13"/>
    <Setter Property="FontWeight" Value="Bold"/>
    <Setter Property="Foreground" Value="DarkBlue"/>
    <Setter Property="Background" Value="LightYellow"/>
    <Setter Property="BorderBrush" Value="Gray"/>
    <Setter Property="BorderThickness" Value="1"/>
    <Setter Property="Padding" Value="4,2"/>
</Style>
```

```csharp
new AnnotationEditorViewModel()
{
    Content = "Styled Label",
    FontSize = 13,
    FontWeight = FontWeights.Bold,
    FontFamily = new System.Windows.Media.FontFamily("Segoe UI"),
    Foreground = System.Windows.Media.Brushes.DarkBlue,
    Background = System.Windows.Media.Brushes.LightYellow,
    BorderBrush = System.Windows.Media.Brushes.Gray,
    BorderThickness = new Thickness(1),
    Padding = new Thickness(4,2),
}
```

### Rotation
Rotate an annotation to match its container:
```csharp
new AnnotationEditorViewModel()
{
    Content = "Rotated",
    RotateAngle = 45,
}
```

### Text Wrapping
Long text wraps within a constrained width:
```csharp
new AnnotationEditorViewModel()
{
    Content = "This is a long description that should wrap",
    WrapText = TextWrapping.Wrap,
    UnitWidth = 100,
}
```

## Editing Annotations at Runtime

### Allow Inline Editing
By default, double-clicking a node enters annotation edit mode. To control this:
```csharp
// Disable editing on a specific annotation
new AnnotationEditorViewModel()
{
    Content = "Read Only",
    Constraints = AnnotationConstraints.Default & ~AnnotationConstraints.Editable,
}
```

### Programmatic Editing
```csharp
// Activate edit mode on a node's annotation
(diagram.Info as IGraphInfo).Commands.EditAnnotation.Execute(nodeViewModel);
```

## Annotation Constraints

| Constraint | Description |
|-----------|-------------|
| `Editable` | User can double-click to edit |
| `Draggable` | Annotation can be dragged |
| `Resizable` | Annotation text box can be resized |
| `Rotatable` | Annotation can be rotated |
| `InheritEditable` | Inherits editability from diagram |

```csharp
new AnnotationEditorViewModel()
{
    Content = "Draggable Label",
    Constraints = AnnotationConstraints.Default | AnnotationConstraints.Draggable,
}
```

## Annotation Events

```csharp
(diagram.Info as IGraphInfo).AnnotationChanged += (sender, args) =>
{
    var annotation = args.Item as AnnotationEditorViewModel;
    // Fired when annotation text, position, or style changes
};
```

## Common Gotchas

- **Annotations are null by default.** Always initialize with `new ObservableCollection<IAnnotation>()` or `new AnnotationCollection()`.
- **Text doesn't appear:** Check that `Content` is not empty and that `UnitWidth`/`UnitHeight` are large enough.
- **Connector label moves unexpectedly:** Use `Length` + `Displacement` rather than `Offset` for connectors.

## Related
- Node positioning → `references/nodes.md`
- Connector labels → `references/connectors.md`
