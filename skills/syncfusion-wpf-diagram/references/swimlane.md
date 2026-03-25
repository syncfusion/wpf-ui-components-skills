# Swimlane Diagrams in WPF Diagram (SfDiagram)

## Overview

A `SwimlaneViewModel` represents a swimlane node — used to visualize a business process with responsible departments/actors. Swimlanes contain:
- **Header** — title bar at the top/left
- **Lanes** — functional rows/columns (parallel tracks)
- **Phases** — vertical/horizontal divisions that split lanes into time or process stages

## Create a Swimlane

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.Swimlanes>
        <syncfusion:SwimlaneCollection>
            <syncfusion:SwimlaneViewModel OffsetX="300" OffsetY="150"
                                          UnitHeight="300" UnitWidth="600"
                                          Orientation="Horizontal">
                <!--Swimlane header-->
                <syncfusion:SwimlaneViewModel.Header>
                    <syncfusion:SwimlaneHeaderViewModel UnitHeight="32">
                        <syncfusion:SwimlaneHeaderViewModel.Annotation>
                            <syncfusion:AnnotationEditorViewModel Content="SALES PROCESS"/>
                        </syncfusion:SwimlaneHeaderViewModel.Annotation>
                    </syncfusion:SwimlaneHeaderViewModel>
                </syncfusion:SwimlaneViewModel.Header>

                <!--Lanes-->
                <syncfusion:SwimlaneViewModel.Lanes>
                    <syncfusion:LaneCollection>
                        <syncfusion:LaneViewModel UnitHeight="100">
                            <syncfusion:LaneViewModel.Header>
                                <syncfusion:SwimlaneHeaderViewModel UnitWidth="100">
                                    <syncfusion:SwimlaneHeaderViewModel.Annotation>
                                        <syncfusion:AnnotationEditorViewModel Content="Sales"/>
                                    </syncfusion:SwimlaneHeaderViewModel.Annotation>
                                </syncfusion:SwimlaneHeaderViewModel>
                            </syncfusion:LaneViewModel.Header>
                        </syncfusion:LaneViewModel>
                        <syncfusion:LaneViewModel UnitHeight="100">
                            <syncfusion:LaneViewModel.Header>
                                <syncfusion:SwimlaneHeaderViewModel UnitWidth="100">
                                    <syncfusion:SwimlaneHeaderViewModel.Annotation>
                                        <syncfusion:AnnotationEditorViewModel Content="Marketing"/>
                                    </syncfusion:SwimlaneHeaderViewModel.Annotation>
                                </syncfusion:SwimlaneHeaderViewModel>
                            </syncfusion:LaneViewModel.Header>
                        </syncfusion:LaneViewModel>
                    </syncfusion:LaneCollection>
                </syncfusion:SwimlaneViewModel.Lanes>

                <!--Phases-->
                <syncfusion:SwimlaneViewModel.Phases>
                    <syncfusion:PhaseCollection>
                        <syncfusion:PhaseViewModel UnitWidth="300">
                            <syncfusion:PhaseViewModel.Header>
                                <syncfusion:SwimlaneHeaderViewModel UnitHeight="30">
                                    <syncfusion:SwimlaneHeaderViewModel.Annotation>
                                        <syncfusion:AnnotationEditorViewModel Content="Phase 1"/>
                                    </syncfusion:SwimlaneHeaderViewModel.Annotation>
                                </syncfusion:SwimlaneHeaderViewModel>
                            </syncfusion:PhaseViewModel.Header>
                        </syncfusion:PhaseViewModel>
                        <syncfusion:PhaseViewModel UnitWidth="300">
                            <syncfusion:PhaseViewModel.Header>
                                <syncfusion:SwimlaneHeaderViewModel UnitHeight="30">
                                    <syncfusion:SwimlaneHeaderViewModel.Annotation>
                                        <syncfusion:AnnotationEditorViewModel Content="Phase 2"/>
                                    </syncfusion:SwimlaneHeaderViewModel.Annotation>
                                </syncfusion:SwimlaneHeaderViewModel>
                            </syncfusion:PhaseViewModel.Header>
                        </syncfusion:PhaseViewModel>
                    </syncfusion:PhaseCollection>
                </syncfusion:SwimlaneViewModel.Phases>

            </syncfusion:SwimlaneViewModel>
        </syncfusion:SwimlaneCollection>
    </syncfusion:SfDiagram.Swimlanes>
</syncfusion:SfDiagram>
```

```csharp
SfDiagram diagram = new SfDiagram();
diagram.Swimlanes = new SwimlaneCollection();

SwimlaneViewModel swimlane = new SwimlaneViewModel()
{
    UnitWidth   = 600,
    UnitHeight  = 300,
    OffsetX     = 300,
    OffsetY     = 150,
    Orientation = Orientation.Horizontal,
    Header = new SwimlaneHeaderViewModel()
    {
        UnitHeight = 32,
        Annotation = new AnnotationEditorViewModel() { Content = "SALES PROCESS" },
    },
    Lanes = new LaneCollection()
    {
        new LaneViewModel()
        {
            UnitHeight = 100,
            Header = new SwimlaneHeaderViewModel()
            {
                UnitWidth  = 100,
                Annotation = new AnnotationEditorViewModel() { Content = "Sales" },
            }
        },
        new LaneViewModel()
        {
            UnitHeight = 100,
            Header = new SwimlaneHeaderViewModel()
            {
                UnitWidth  = 100,
                Annotation = new AnnotationEditorViewModel() { Content = "Marketing" },
            }
        },
    },
    Phases = new PhaseCollection()
    {
        new PhaseViewModel()
        {
            UnitWidth = 300,
            Header    = new SwimlaneHeaderViewModel()
            {
                UnitHeight = 30,
                Annotation = new AnnotationEditorViewModel() { Content = "Phase 1" },
            }
        },
    },
};

(diagram.Swimlanes as SwimlaneCollection).Add(swimlane);
```

## SwimlaneViewModel Key Properties

| Property | Description |
|----------|-------------|
| `OffsetX`, `OffsetY` | Center position of the swimlane |
| `UnitWidth`, `UnitHeight` | Size of the swimlane |
| `Orientation` | `Horizontal` (default) or `Vertical` |
| `Header` | `SwimlaneHeaderViewModel` for the title bar |
| `Lanes` | `LaneCollection` of `LaneViewModel` items |
| `Phases` | `PhaseCollection` of `PhaseViewModel` items |
| `IsSelected` | Select/deselect the swimlane |

## Orientation

| Orientation | Layout |
|-------------|--------|
| `Horizontal` | Header at left, lanes stack top-to-bottom, phases divide left-to-right |
| `Vertical` | Header at top, lanes stack left-to-right, phases divide top-to-bottom |

## Lanes

Each `LaneViewModel` represents a row (horizontal) or column (vertical) inside the swimlane.

- **Horizontal swimlane:** set `UnitHeight` on each lane
- **Vertical swimlane:** set `UnitWidth` on each lane
- Lanes auto-stack in the order they are added

```csharp
// Add lane dynamically at runtime
var swimlane = (diagram.Swimlanes as SwimlaneCollection).FirstOrDefault() as SwimlaneViewModel;
(swimlane.Lanes as LaneCollection).Add(new LaneViewModel()
{
    UnitHeight = 100,
    Header = new SwimlaneHeaderViewModel()
    {
        UnitWidth  = 100,
        Annotation = new AnnotationEditorViewModel() { Content = "IT" }
    }
});
```

## Phases

Each `PhaseViewModel` divides lanes into horizontal or vertical sub-sections.

- **Horizontal swimlane:** set `UnitWidth` on each phase
- **Vertical swimlane:** set `UnitHeight` on each phase

```csharp
// Add phase dynamically
(swimlane.Phases as PhaseCollection).Add(new PhaseViewModel()
{
    UnitWidth = 150,
    Header = new SwimlaneHeaderViewModel()
    {
        UnitHeight = 30,
        Annotation = new AnnotationEditorViewModel() { Content = "Phase 3" }
    }
});

// Remove last phase
(swimlane.Phases as PhaseCollection).Remove(
    (swimlane.Phases as PhaseCollection).LastOrDefault());
```

## Adding Nodes Inside Lanes

Nodes placed inside a lane become children of that lane:

```csharp
// Place a node inside a lane by setting its ID as the lane child
LaneViewModel lane = (swimlane.Lanes as LaneCollection)[0] as LaneViewModel;
lane.Children = new ObservableCollection<object>()
{
    new NodeViewModel()
    {
        ID = "n1", OffsetX = 200, OffsetY = 60,
        UnitWidth = 100, UnitHeight = 50,
        Shape = new EllipseGeometry() { RadiusX = 10, RadiusY = 10 },
    }
};
```

## Header Customization

Apply a `ShapeStyle` to the header for custom colors, and a `ViewTemplate` for custom text appearance:

```xaml
<Style x:Key="SwimlaneHeaderStyle" TargetType="Path">
    <Setter Property="Fill" Value="CadetBlue"/>
    <Setter Property="Stroke" Value="#41719C"/>
    <Setter Property="StrokeThickness" Value="1"/>
    <Setter Property="Stretch" Value="Fill"/>
</Style>

<DataTemplate x:Key="HeaderTemplate">
    <TextBlock Text="{Binding Content, Mode=TwoWay}"
               FontWeight="Bold" FontStyle="Italic" Foreground="White"/>
</DataTemplate>
```

```csharp
swimlane.Header = new SwimlaneHeaderViewModel()
{
    UnitHeight   = 32,
    ShapeStyle   = Resources["SwimlaneHeaderStyle"] as Style,
    Annotation   = new AnnotationEditorViewModel()
    {
        Content      = "PROCESS NAME",
        ViewTemplate = Resources["HeaderTemplate"] as DataTemplate,
    }
};
```

## Swimlane in Stencil (Palette)

Add `LaneViewModel` and `PhaseViewModel` items to a Stencil for drag-and-drop:

```xaml
<syncfusion:Stencil x:Name="stencil" ExpandMode="All">
    <syncfusion:Stencil.SymbolSource>
        <syncfusion:SymbolCollection>
            <syncfusion:LaneViewModel  ID="HLane"  Key="Swimlane Shapes" Orientation="Horizontal"/>
            <syncfusion:LaneViewModel  ID="VLane"  Key="Swimlane Shapes" Orientation="Vertical"/>
            <syncfusion:PhaseViewModel ID="HPhase" Key="Swimlane Shapes" Orientation="Horizontal"/>
            <syncfusion:PhaseViewModel ID="VPhase" Key="Swimlane Shapes" Orientation="Vertical"/>
        </syncfusion:SymbolCollection>
    </syncfusion:Stencil.SymbolSource>
    <syncfusion:Stencil.SymbolGroups>
        <syncfusion:SymbolGroups>
            <syncfusion:SymbolGroupProvider MappingName="Key"/>
        </syncfusion:SymbolGroups>
    </syncfusion:Stencil.SymbolGroups>
</syncfusion:Stencil>
```

**Drag-drop behavior:**
- Dropping a lane onto an existing swimlane (same orientation) stacks it inside.
- Dropping a lane onto an empty area creates a new swimlane.
- Phases can only be dropped onto swimlanes of the same orientation.

## Interaction

| Action | How |
|--------|-----|
| Select swimlane | Click the swimlane header |
| Drag swimlane | Drag the swimlane header |
| Select lane | Click the lane header |
| Resize lane | Drag the lane selector handles |
| Select phase | Click the phase header once |
| Resize phase | Drag the phase selector handles |
| Edit header text | Double-click the header label |

## Common Gotchas

- **Lane size:** Horizontal swimlane lanes need `UnitHeight`; vertical lanes need `UnitWidth`.
- **Phase size:** Horizontal phases need `UnitWidth`; vertical phases need `UnitHeight`.
- **Default swimlane:** Creating a `SwimlaneViewModel` adds one default lane and one default phase automatically.
- **Restricting cross-lane dragging:** Handle `ItemDroppingEvent` and cancel via `e.Cancel = true` when the target lane differs.

## Related
- Symbol palette for shapes → `references/stencil.md`
- Node constraints inside lanes → `references/nodes.md`
