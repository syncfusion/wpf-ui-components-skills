# BPMN Shapes in WPF Diagram (SfDiagram)

## Table of Contents
- [Overview](#overview)
- [BPMN Node Setup](#bpmn-node-setup)
- [Activities](#activities)
- [Events](#events)
- [Gateways](#gateways)
- [Data Objects and Data Stores](#data-objects-and-data-stores)
- [BPMN Connectors](#bpmn-connectors)
- [Text Annotations](#text-annotations)
- [BPMN Groups and Expanded Sub-Process](#bpmn-groups-and-expanded-sub-process)
- [BPMN Symbol Palette](#bpmn-symbol-palette)

## Overview

BPMN (Business Process Model and Notation) shapes represent standardized process elements. SfDiagram uses `BpmnNodeViewModel` and `BpmnConnectorViewModel` for BPMN diagrams instead of the standard `NodeViewModel`/`ConnectorViewModel`.

**Available BPMN shape types:**

| Shape | BpmnShapeType | Description |
|-------|---------------|-------------|
| Activity | `Activity` | Tasks and sub-processes |
| Event | `Event` | Start, intermediate, end events |
| Gateway | `Gateway` | Flow control decisions |
| Message | `Message` | Communication content |
| DataStore | `DataStore` | Persistent data storage |
| DataObject | `DataObject` | Data flowing in the process |
| TextAnnotation | `TextAnnotation` | Notes attached to shapes |
| Group | `Group` | Logical grouping |
| ExpandedSubProcess | `Expandedsubprocess` | Collapsed/expanded subprocess |

## BPMN Node Setup

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.Nodes>
        <syncfusion:NodeCollection>
            <!--Basic BPMN Activity node-->
            <syncfusion:BpmnNodeViewModel UnitHeight="70" UnitWidth="100"
                                          OffsetX="200" OffsetY="150"
                                          Type="Activity"/>
        </syncfusion:NodeCollection>
    </syncfusion:SfDiagram.Nodes>
</syncfusion:SfDiagram>
```

```csharp
BpmnNodeViewModel node = new BpmnNodeViewModel()
{
    OffsetX = 200, OffsetY = 150,
    UnitHeight = 70, UnitWidth = 100,
    Type = BpmnShapeType.Activity,
};
(diagram.Nodes as NodeCollection).Add(node);
```

## Activities

Activities represent work being done. Use the `ActivityType` property:

```csharp
// Simple Task
BpmnNodeViewModel task = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Activity,
    ActivityType = ActivityType.Task,
    TaskType = BpmnTaskType.None,     // None, Send, Receive, User, Manual, BusinessRule, Service, Script, Instantiating
    OffsetX = 200, OffsetY = 150,
    UnitHeight = 70, UnitWidth = 100,
};

// Call Activity (thick border)
task.ActivityType = ActivityType.CallActivity;

// Sub-Process (collapsed)
BpmnNodeViewModel subProc = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Activity,
    ActivityType = ActivityType.SubProcess,
    IsExpanded = false,  // collapsed
    OffsetX = 400, OffsetY = 150,
    UnitHeight = 70, UnitWidth = 100,
};
```

## Events

Events mark things that happen during a process:

```csharp
// Start Event
BpmnNodeViewModel startEvent = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Event,
    EventType = EventType.Start,
    EventTrigger = EventTriggers.None,   // None, Message, Timer, Conditional, Signal, etc.
    OffsetX = 100, OffsetY = 200,
    UnitHeight = 40, UnitWidth = 40,
};

// Intermediate Event
BpmnNodeViewModel interEvent = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Event,
    EventType = EventType.Intermediate,
    EventTrigger = EventTriggers.Timer,
    OffsetX = 300, OffsetY = 200,
    UnitHeight = 40, UnitWidth = 40,
};

// End Event
BpmnNodeViewModel endEvent = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Event,
    EventType = EventType.End,
    EventTrigger = EventTriggers.Terminate,
    OffsetX = 500, OffsetY = 200,
    UnitHeight = 40, UnitWidth = 40,
};
```

**EventType values:** `Start`, `Intermediate`, `End`, `NonInterrupting`  
**EventTriggers values:** `None`, `Message`, `Timer`, `Conditional`, `Link`, `Signal`, `Error`, `Escalation`, `Terminate`, `Compensation`, `Cancel`, `Multiple`, `Parallel`

## Gateways

Gateways control process flow:

```csharp
BpmnNodeViewModel gateway = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Gateway,
    GatewayType = GatewayType.Exclusive,  // Exclusive, Inclusive, Parallel, Complex, EventBased, etc.
    OffsetX = 300, OffsetY = 150,
    UnitHeight = 50, UnitWidth = 50,
};
```

**GatewayType values:** `None`, `Exclusive`, `Inclusive`, `Parallel`, `Complex`, `EventBased`, `ExclusiveEventBased`, `ParallelEventBased`

## Data Objects and Data Stores

```csharp
// Data Object
BpmnNodeViewModel dataObject = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.DataObject,
    DataObjectType = DataObjectType.Input, // Input, Output, None
    IsCollection = false,
    OffsetX = 400, OffsetY = 100,
    UnitHeight = 70, UnitWidth = 50,
};

// Data Store
BpmnNodeViewModel dataStore = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.DataStore,
    OffsetX = 500, OffsetY = 100,
    UnitHeight = 70, UnitWidth = 100,
};
```

## BPMN Connectors

Use `BpmnConnectorViewModel` for standard BPMN flow connections:

```csharp
// Sequence Flow (default)
BpmnConnectorViewModel seqFlow = new BpmnConnectorViewModel()
{
    SourceNodeID = "startEvent",
    TargetNodeID = "task1",
    FlowType = BpmnFlowType.SequenceFlow,
    SequenceFlowType = SequenceFlowType.Normal, // Normal, Default, Conditional
};

// Message Flow
BpmnConnectorViewModel msgFlow = new BpmnConnectorViewModel()
{
    SourceNodeID = "task1",
    TargetNodeID = "pool2",
    FlowType = BpmnFlowType.MessageFlow,
};

// Association
BpmnConnectorViewModel assoc = new BpmnConnectorViewModel()
{
    SourceNodeID = "task1",
    TargetNodeID = "dataObject1",
    FlowType = BpmnFlowType.Association,
    AssociationFlowType = AssociationFlowType.Directional,
};

(diagram.Connectors as ConnectorCollection).Add(seqFlow);
```

## Text Annotations

Attach descriptive notes to BPMN shapes:

```csharp
BpmnNodeViewModel annotation = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.TextAnnotation,
    TextAnnotationDirection = TextAnnotationDirection.Left,
    TextAnnotationTarget = "task1", // ID of the node being annotated
    OffsetX = 100, OffsetY = 200,
    UnitHeight = 40, UnitWidth = 100,
};
node.Annotations = new ObservableCollection<IAnnotation>()
{
    new AnnotationEditorViewModel() { Content = "This is a note" }
};
```

## BPMN Groups and Expanded Sub-Process

### Group
Groups related activities without affecting flow:
```csharp
BpmnNodeViewModel group = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Group,
    OffsetX = 300, OffsetY = 250,
    UnitHeight = 200, UnitWidth = 300,
};
```

### Expanded Sub-Process
A subprocess that shows its internal flow:
```csharp
BpmnNodeViewModel expandedSub = new BpmnNodeViewModel()
{
    Type = BpmnShapeType.Expandedsubprocess,
    IsExpanded = true,
    OffsetX = 300, OffsetY = 300,
    UnitHeight = 200, UnitWidth = 400,
};
```

## BPMN Symbol Palette

Add BPMN shapes to the stencil for drag-and-drop:

```csharp
// Add BPMN symbols to stencil SymbolSource
var bpmnSymbols = new SymbolCollection()
{
    new BpmnNodeViewModel { Type = BpmnShapeType.Event, EventType = EventType.Start,
                            UnitHeight = 40, UnitWidth = 40, Key = "BPMN" },
    new BpmnNodeViewModel { Type = BpmnShapeType.Activity, ActivityType = ActivityType.Task,
                            UnitHeight = 60, UnitWidth = 100, Key = "BPMN" },
    new BpmnNodeViewModel { Type = BpmnShapeType.Gateway, GatewayType = GatewayType.Exclusive,
                            UnitHeight = 50, UnitWidth = 50, Key = "BPMN" },
    new BpmnNodeViewModel { Type = BpmnShapeType.Event, EventType = EventType.End,
                            UnitHeight = 40, UnitWidth = 40, Key = "BPMN" },
};
stencil.SymbolSource = bpmnSymbols;
```

For full stencil setup → `references/stencil.md`

## Common Gotchas

- **Use `BpmnNodeViewModel`**, not plain `NodeViewModel`, for BPMN shapes — it has the `Type`, `ActivityType`, `EventType` etc. properties.
- **Use `BpmnConnectorViewModel`** for sequence flows and message flows to get proper BPMN arrow heads.
- **TextAnnotation target:** Set `TextAnnotationTarget` to the `ID` of the node it annotates.

## Related

- Non-BPMN node shapes → `references/nodes.md`
- Connectors between BPMN shapes → `references/connectors.md`
- Symbol palette (stencil) for BPMN → `references/stencil.md`
