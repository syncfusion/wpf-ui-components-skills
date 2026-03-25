# UML Diagrams in WPF Diagram (SfDiagram)

## Overview

SfDiagram supports two main UML approaches:
1. **UML Shape Diagrams** — Use built-in UML shape resource dictionaries to build activity, use case, state, class, and relationship diagrams using standard `NodeViewModel`.
2. **UML Sequence Diagrams** — Use the dedicated `UMLSequenceDiagramModel` with participants, messages, and fragments for interaction diagrams.

## UML Shape Diagrams

### Merging UML Resource Dictionaries

Each UML diagram type has its own resource dictionary:

```xaml
<ResourceDictionary.MergedDictionaries>
    <!--Activity diagrams-->
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/UMLActivity.xaml"/>
    <!--Use case diagrams-->
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/UMLUseCase.xaml"/>
    <!--State diagrams-->
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/UMLStateDiagram.xaml"/>
    <!--ER / Relationship diagrams-->
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/UMLRelationship.xaml"/>
</ResourceDictionary.MergedDictionaries>
```

### Available UML Shapes

**UMLActivity shapes** (use `Source="...UMLActivity.xaml"`):

| Key | Shape |
|-----|-------|
| `Action` | Activity action state |
| `Initial` | Initial pseudo-state (filled circle) |
| `Final` | Activity final (circle with dot) |
| `FlowFinal` | Flow final (circle with X) |
| `ForkNode` | Fork/join bar |
| `JoinNode` | Join bar |
| `MergeNode` | Merge diamond |
| `ObjectNode` | Object node rectangle |
| `SendSignal` | Send signal shape |
| `AcceptEvent` | Accept event shape |
| `Note` | Note shape |

**UMLUseCase shapes** (use `Source="...UMLUseCase.xaml"`):

| Key | Shape |
|-----|-------|
| `User` | Actor (stick figure) |
| `UseCase` | Use case (ellipse) |
| `SubSystem` | Sub-system rectangle |
| `AssociationLink` | Association line |
| `DependencyLink` | Dependency dashed line |
| `GeneralizationLink` | Generalization arrow |
| `IncludeLink` | Include dashed arrow |
| `ExtendLink` | Extend dashed arrow |

**UMLStateDiagram shapes** (use `Source="...UMLStateDiagram.xaml"`):

| Key | Shape |
|-----|-------|
| `SimpleState` | Simple state rounded rect |
| `StateWithInternalBehaviour` | State with compartments |
| `CompositeState` | Composite state |
| `Initial` | Initial state (filled circle) |
| `Final` | Final state |
| `Choice` | Choice diamond |

### Using UML Shapes

```xaml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/UMLActivity.xaml"/>
</ResourceDictionary.MergedDictionaries>

<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.Nodes>
        <syncfusion:NodeCollection>
            <!--Initial state-->
            <syncfusion:NodeViewModel ID="start" OffsetX="200" OffsetY="80"
                                      UnitWidth="40" UnitHeight="40"
                                      Shape="{StaticResource Initial}"/>
            <!--Action-->
            <syncfusion:NodeViewModel ID="action1" OffsetX="200" OffsetY="180"
                                      UnitWidth="120" UnitHeight="60"
                                      Shape="{StaticResource Action}">
                <syncfusion:NodeViewModel.Annotations>
                    <syncfusion:AnnotationCollection>
                        <syncfusion:AnnotationEditorViewModel Content="Do Something"/>
                    </syncfusion:AnnotationCollection>
                </syncfusion:NodeViewModel.Annotations>
            </syncfusion:NodeViewModel>
            <!--Final state-->
            <syncfusion:NodeViewModel ID="end" OffsetX="200" OffsetY="300"
                                      UnitWidth="40" UnitHeight="40"
                                      Shape="{StaticResource Final}"/>
        </syncfusion:NodeCollection>
    </syncfusion:SfDiagram.Nodes>
    <syncfusion:SfDiagram.Connectors>
        <syncfusion:ConnectorCollection>
            <syncfusion:ConnectorViewModel SourceNodeID="start" TargetNodeID="action1"/>
            <syncfusion:ConnectorViewModel SourceNodeID="action1" TargetNodeID="end"/>
        </syncfusion:ConnectorCollection>
    </syncfusion:SfDiagram.Connectors>
</syncfusion:SfDiagram>
```

```csharp
// C# equivalent
NodeViewModel startNode = new NodeViewModel()
{
    ID = "start",
    OffsetX = 200, OffsetY = 80,
    UnitWidth = 40, UnitHeight = 40,
    Shape = App.Current.Resources["Initial"],
};
(diagram.Nodes as NodeCollection).Add(startNode);
```

## UML Sequence Diagrams

Sequence diagrams use a dedicated model: `UMLSequenceDiagramModel` assigned to `SfDiagram.Model`.

### Basic Setup

```csharp
SfDiagram diagram = new SfDiagram();

diagram.Model = new UMLSequenceDiagramModel()
{
    Participants = new ParticipantCollection()
    {
        new UMLSequenceParticipant { ID = "User",   Content = "User",   IsActor = true  },
        new UMLSequenceParticipant { ID = "System", Content = "System", IsActor = false },
    },
    Messages = new MessageCollection()
    {
        new UMLSequenceMessage
        {
            ID = "msg1",
            FromParticipantID = "User",
            ToParticipantID   = "System",
            Content           = "Login Request",
            MessageType       = MessageType.Synchronous,
        },
        new UMLSequenceMessage
        {
            ID = "msg2",
            FromParticipantID = "System",
            ToParticipantID   = "User",
            Content           = "Auth Token",
            MessageType       = MessageType.Reply,
        },
    }
};
```

### Participant Properties

| Property | Description |
|----------|-------------|
| `ID` | Unique identifier |
| `Content` | Display label |
| `IsActor` | `true` = stick figure actor, `false` = rectangle lifeline |
| `ShowDestroyAtEnd` | Shows an X at the end of the lifeline |

### Message Types

| MessageType | Description |
|-------------|-------------|
| `Synchronous` | Filled arrowhead (blocking call) |
| `Asynchronous` | Open arrowhead (non-blocking) |
| `Reply` | Dashed return arrow |
| `Create` | Dashed arrow to create an object |
| `Delete` | Arrow to destroy an object |
| `SelfMessage` | Loop arrow on same participant |

### XAML Sequence Diagram
```xaml
<syncfusion:SfDiagram x:Name="Diagram">
    <syncfusion:SfDiagram.Model>
        <syncfusion:UMLSequenceDiagramModel>
            <syncfusion:UMLSequenceDiagramModel.Participants>
                <syncfusion:ParticipantCollection>
                    <syncfusion:UMLSequenceParticipant ID="Client" Content="Client" IsActor="True"/>
                    <syncfusion:UMLSequenceParticipant ID="Server" Content="Server" IsActor="False"/>
                </syncfusion:ParticipantCollection>
            </syncfusion:UMLSequenceDiagramModel.Participants>
            <syncfusion:UMLSequenceDiagramModel.Messages>
                <syncfusion:MessageCollection>
                    <syncfusion:UMLSequenceMessage FromParticipantID="Client"
                                                   ToParticipantID="Server"
                                                   Content="HTTP Request"
                                                   MessageType="Synchronous"/>
                    <syncfusion:UMLSequenceMessage FromParticipantID="Server"
                                                   ToParticipantID="Client"
                                                   Content="HTTP Response"
                                                   MessageType="Reply"/>
                </syncfusion:MessageCollection>
            </syncfusion:UMLSequenceDiagramModel.Messages>
        </syncfusion:UMLSequenceDiagramModel>
    </syncfusion:SfDiagram.Model>
</syncfusion:SfDiagram>
```

## Common Gotchas

- **UML shapes not found:** Merge the correct resource dictionary (`UMLActivity.xaml`, `UMLUseCase.xaml`, etc.) before referencing shape keys.
- **Sequence diagram vs shape diagram:** Use `UMLSequenceDiagramModel` + `SfDiagram.Model` for sequence diagrams; use standard `NodeViewModel` + shape keys for activity/use case/state diagrams.
- **Self message:** Set `FromParticipantID == ToParticipantID` for a self-referencing message.

## Related
- Adding labels to shapes → `references/annotations.md`
- Connectors for UML relationships → `references/connectors.md`
