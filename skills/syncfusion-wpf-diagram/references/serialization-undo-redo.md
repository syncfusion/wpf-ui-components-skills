# Serialization and Undo/Redo in WPF Diagram (SfDiagram)

## Serialization Overview

SfDiagram uses `DataContractSerializer` to serialize the diagram state (nodes, connectors, positions, styles) to a stream, and deserialize it back.

## Save Diagram

```csharp
// Save to file (with dialog)
SaveFileDialog dialog = new SaveFileDialog();
dialog.Title  = "Save Diagram";
dialog.Filter = "XAML File (*.xaml)|*.xaml";
if (dialog.ShowDialog() == true)
{
    File.Delete(dialog.FileName);  // Remove if exists
    using (Stream s = File.Open(dialog.FileName, FileMode.OpenOrCreate))
    {
        sfDiagram.Save(s);
    }
}

// Save to memory stream
MemoryStream memStream = new MemoryStream();
sfDiagram.Save(memStream);
```

## Load Diagram

```csharp
// Load from file (with dialog)
OpenFileDialog dialog = new OpenFileDialog();
if (dialog.ShowDialog() == true)
{
    using (Stream s = dialog.OpenFile())
    {
        sfDiagram.Load(s);
    }
}

// Load from memory stream
memStream.Position = 0;
sfDiagram.Load(memStream);
```

## Check for Unsaved Changes

`HasChanges` tracks whether the diagram was modified since the last save:

```csharp
private void SaveButton_Click(object sender, RoutedEventArgs e)
{
    if (diagram.HasChanges)
    {
        var result = MessageBox.Show("Save changes?", "Diagram",
                                     MessageBoxButton.YesNo);
        if (result == MessageBoxResult.Yes)
            SaveDiagram();
    }
}
```

## Serializing Custom Properties

### Properties on SfDiagram View Model Classes

Use `[DataMember]` on properties of classes derived from SfDiagram's view model or interface classes:

```csharp
// Derived from INode — no [DataContract] needed
public class CustomNode : INode
{
    [DataMember]
    public string Category { get; set; }

    [DataMember]
    public int Priority { get; set; }
}
```

### Standalone Custom Classes

For classes NOT derived from SfDiagram base classes, add `[DataContract]` + `[DataMember]`, and register with `KnownTypes`:

```csharp
[DataContract]
public class NodeMetadata
{
    [DataMember]
    public string NodeType { get; set; }

    [DataMember]
    public DateTime CreatedAt { get; set; }
}

// Register with the diagram
diagram.KnownTypes = () => new List<Type>()
{
    typeof(NodeMetadata)
};
```

> **Note:** `Content` and `ContentTemplate` properties of nodes/connectors are **not** serialized. Store templates in `ResourceDictionary` and re-apply them after loading.

## Load Old Version Files

To load a diagram file saved with an older version of SfDiagram:

```csharp
using (Stream stream = dialog.OpenFile())
{
    sfDiagram.Upgrade(stream);  // upgrade first
    sfDiagram.Load(stream);
}
```

## Mermaid Syntax Export/Import

SfDiagram supports Mermaid syntax for Flowchart, MindMap, and Sequence Diagram layouts:

```csharp
// Export to Mermaid string
string mermaidText = diagram.SaveDiagramAsMermaid();

// Import from Mermaid string
diagram.LoadDiagramFromMermaid(mermaidText);
```

> Mermaid serialization only works with Flowchart, MindMap, and Sequence Diagram layouts.

---

## Undo / Redo

### Enable Undo/Redo

```xaml
<syncfusion:SfDiagram x:Name="diagram" Constraints="Default,Undoable"/>
```

```csharp
diagram.Constraints = GraphConstraints.Default | GraphConstraints.Undoable;
```

### Execute Undo/Redo via Commands

```csharp
IGraphInfo graphInfo = diagram.Info as IGraphInfo;

// Undo
graphInfo.Commands.Undo.Execute(null);

// Redo
graphInfo.Commands.Redo.Execute(null);
```

**Keyboard shortcuts:**
- Undo: `Ctrl+Z`
- Redo: `Ctrl+Y`

### History Manager

`HistoryManager` gives access to the undo/redo stack and allows custom history entries.

```csharp
// Access history
var historyManager = diagram.HistoryManager;

// Listen for history changes
diagram.HistoryChangedEvent += (s, e) =>
{
    // e.Action (HistoryAction) tells what changed
    // PositionChanged, SizeChanged, CollectionChanged, etc.
};
```

### HistoryEntry Properties

| Property | Description |
|----------|-------------|
| `Action` | The type of change recorded |
| `Mode` | Whether the entry came from `Internal` (SfDiagram) or `External` (your code) |

### HistoryAction Values

| Value | Description |
|-------|-------------|
| `None` | Default |
| `PositionChanged` | Node/connector was moved |
| `RotationChanged` | Element was rotated |
| `SizeChanged` | Element was resized |
| `CollectionChanged` | Element added or deleted |
| `ConnectorSourceChanged` | Connector source point changed |
| `ConnectorTargetChanged` | Connector target point changed |
| `CustomAction` | User-defined action |

### Custom Undo/Redo Entries

Add custom actions to the undo stack so they participate in Ctrl+Z/Y:

```csharp
// Begin custom action group
diagram.HistoryManager.BeginComposite(diagram.HistoryManager, new PropertyChangedAction());

// Make your changes here...
myNode.ShapeStyle = newStyle;

// End the group — all changes since BeginComposite are undone together
diagram.HistoryManager.EndComposite(diagram.HistoryManager, new PropertyChangedAction());
```

### Batch Undo (Group Multiple Changes)

Use `BeginUpdate`/`EndUpdate` to group multiple changes into a single undo step:

```csharp
diagram.BeginUpdate();

// Multiple operations — all count as one undo step
(diagram.Nodes as NodeCollection).Add(node1);
(diagram.Nodes as NodeCollection).Add(node2);
(diagram.Connectors as ConnectorCollection).Add(connector1);

diagram.EndUpdate();
```

## Common Gotchas

- **ContentTemplate not restored after load:** Store DataTemplates in a ResourceDictionary and apply them in `NodeCreatingEvent` after loading.
- **Custom class not serialized:** Add `[DataContract]`/`[DataMember]` and register with `diagram.KnownTypes`.
- **Undo not working:** Check that `GraphConstraints.Undoable` is set in `diagram.Constraints`.
- **Loading old file fails:** Use `diagram.Upgrade(stream)` before `diagram.Load(stream)`.
- **Memory leak with MemoryStream:** Always reset `stream.Position = 0` before calling `Load()` on a stream you have already written to.

## Related
- Commands for undo/redo → `references/commands.md`
- Diagram constraints → `references/performance-constraints.md`
