# Commands in WPF Diagram (SfDiagram)

## Table of Contents
- [Overview](#overview)
- [Alignment Commands](#alignment-commands)
- [Clipboard Commands](#clipboard-commands)
- [Delete Command](#delete-command)
- [Undo / Redo Commands](#undo--redo-commands)
- [Nudge Commands](#nudge-commands)
- [Rotate and Flip Commands](#rotate-and-flip-commands)
- [Spacing and Sizing Commands](#spacing-and-sizing-commands)
- [Z-Order Commands](#z-order-commands)
- [Fit to Page and Zoom](#fit-to-page-and-zoom)
- [Expand and Collapse](#expand-and-collapse)
- [Grouping Commands](#grouping-commands)
- [Custom Command Manager](#custom-command-manager)

## Overview

Commands are accessed via `IGraphInfo.Commands` and can be executed programmatically or bound to UI buttons via `DiagramCommands` static class. All commands work on the currently selected elements.

```csharp
IGraphInfo graphInfo = diagram.Info as IGraphInfo;

// Execute a command
graphInfo.Commands.AlignLeft.Execute(null);
```

**XAML Button Binding:**
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"

<Button Content="Align Left"
        Command="syncfusion:DiagramCommands.AlignLeft"/>
```

## Alignment Commands

Align selected nodes relative to the reference element (first in selection list):

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

g.Commands.AlignLeft.Execute(null);      // left edge
g.Commands.AlignRight.Execute(null);     // right edge
g.Commands.AlignTop.Execute(null);       // top edge
g.Commands.AlignBottom.Execute(null);    // bottom edge
g.Commands.AlignCenter.Execute(null);    // horizontal center
g.Commands.AlignMiddle.Execute(null);    // vertical middle
```

**XAML:**
```xaml
<Button Command="syncfusion:DiagramCommands.AlignLeft"/>
<Button Command="syncfusion:DiagramCommands.AlignRight"/>
<Button Command="syncfusion:DiagramCommands.AlignTop"/>
<Button Command="syncfusion:DiagramCommands.AlignBottom"/>
<Button Command="syncfusion:DiagramCommands.AlignCenter"/>
<Button Command="syncfusion:DiagramCommands.AlignMiddle"/>
```

## Clipboard Commands

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

g.Commands.Cut.Execute(null);
g.Commands.Copy.Execute(null);
g.Commands.Paste.Execute(null);

// Duplicate (copy + paste in one step)
g.Commands.Duplicate.Execute(null);
```

**XAML:**
```xaml
<Button Command="syncfusion:DiagramCommands.Cut"/>
<Button Command="syncfusion:DiagramCommands.Copy"/>
<Button Command="syncfusion:DiagramCommands.Paste"/>
<Button Command="syncfusion:DiagramCommands.Duplicate"/>
```

## Delete Command

```csharp
(diagram.Info as IGraphInfo).Commands.Delete.Execute(null);
```

```xaml
<Button Command="syncfusion:DiagramCommands.Delete"/>
```

## Undo / Redo Commands

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

g.Commands.Undo.Execute(null);
g.Commands.Redo.Execute(null);
```

Check availability:
```csharp
bool canUndo = g.Commands.Undo.CanExecute(null);
bool canRedo = g.Commands.Redo.CanExecute(null);
```

**Configure history depth:**
```csharp
diagram.HistoryManager = new SfDiagramHistoryManager()
{
    HistoryLimit = 50, // max undo steps
};
```

## Nudge Commands

Move selected elements by 1px or 10px (Shift):

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

g.Commands.Nudge.Execute(new NudgeCommandParameter { Direction = NudgeDirection.Left });
g.Commands.Nudge.Execute(new NudgeCommandParameter { Direction = NudgeDirection.Right });
g.Commands.Nudge.Execute(new NudgeCommandParameter { Direction = NudgeDirection.Up });
g.Commands.Nudge.Execute(new NudgeCommandParameter { Direction = NudgeDirection.Down });
```

These are also bound to arrow keys by default. See keyboard shortcuts in `references/interaction.md`.

## Rotate and Flip Commands

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

// Rotate 90° clockwise
g.Commands.RotateRight.Execute(null);

// Rotate 90° counter-clockwise
g.Commands.RotateLeft.Execute(null);

// Flip
g.Commands.Flip.Execute(new FlipParameter
{
    Flip = FlipDirection.Horizontal,
    FlipMode = FlipMode.FlipBoth, // FlipBoth, FlipX, FlipY
});
```

## Spacing and Sizing Commands

### Spacing
Distribute selected nodes at equal intervals:
```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

g.Commands.SpaceAcross.Execute(null);   // equal horizontal spacing
g.Commands.SpaceDown.Execute(null);     // equal vertical spacing
```

### Sizing
Make selected nodes the same size:
```csharp
g.Commands.SameSize.Execute(new SizingCommandParameter
{
    Sizing = Sizing.SameSize,   // SameWidth, SameHeight, SameSize
});
```

**XAML:**
```xaml
<Button Command="syncfusion:DiagramCommands.SpaceAcross"/>
<Button Command="syncfusion:DiagramCommands.SpaceDown"/>
<Button Command="syncfusion:DiagramCommands.SameSize"/>
```

## Z-Order Commands

Control the stacking order of overlapping elements:
```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

g.Commands.BringToFront.Execute(null);    // top of all elements
g.Commands.BringForward.Execute(null);    // one step up
g.Commands.SendToBack.Execute(null);      // behind all elements
g.Commands.SendBackward.Execute(null);    // one step down
```

**XAML:**
```xaml
<Button Command="syncfusion:DiagramCommands.BringToFront"/>
<Button Command="syncfusion:DiagramCommands.SendToBack"/>
```

## Fit to Page and Zoom

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

// Fit all content in view
g.Commands.FitToPage.Execute(new FitToPageParameter
{
    FitToPage = FitToPage.FitToPage,   // or FitToWidth, FitToHeight
    Region = Region.PageSettings,       // or Region.Content
});

// Zoom to specific level
g.Commands.Zoom.Execute(new ZoomPositionParameter
{
    ZoomFactor = 1.5,
    ZoomCommand = ZoomCommand.ZoomIn,
});

// Reset zoom and pan to default
g.Commands.Reset.Execute(new ResetParameter { Reset = Reset.ZoomAndPan });
```

## Expand and Collapse

For nodes that have child nodes (group hierarchies):
```csharp
// Collapse a node's children
g.Commands.ExpandCollapse.Execute(new ExpandCollapseParameter
{
    Nodes = new List<object>() { nodeViewModel },
    IsExpanded = false,
});

// Expand
g.Commands.ExpandCollapse.Execute(new ExpandCollapseParameter
{
    Nodes = new List<object>() { nodeViewModel },
    IsExpanded = true,
});
```

## Grouping Commands

```csharp
IGraphInfo g = diagram.Info as IGraphInfo;

// Group selected elements
g.Commands.Group.Execute(null);

// Ungroup selected group
g.Commands.UnGroup.Execute(null);
```

**XAML:**
```xaml
<Button Command="syncfusion:DiagramCommands.Group"/>
<Button Command="syncfusion:DiagramCommands.UnGroup"/>
```

## Custom Command Manager

Override or add new keyboard shortcuts using the `CommandManager`:

```csharp
// Override Ctrl+A to do custom selection
(diagram.Info as IGraphInfo).Commands.SelectAll.GestureKey = Key.A;
(diagram.Info as IGraphInfo).Commands.SelectAll.GestureModifiers = ModifierKeys.Control;

// Add a new custom command
ICommand customCommand = new DelegateCommand(ExecuteCustom, CanExecuteCustom);
diagram.CommandManager.Commands.Add(new CommandViewModel()
{
    Name = "CustomCommand",
    Command = customCommand,
    GestureKey = Key.F5,
    GestureModifiers = ModifierKeys.None,
});

private void ExecuteCustom(object param)
{
    // Your custom action
}
```

## Related
- Undo/Redo history manager details → `references/serialization-undo-redo.md`
- Keyboard shortcut reference → `references/interaction.md`
