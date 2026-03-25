---
name: syncfusion-wpf-diagram
description: Complete guide for implementing Syncfusion WPF Diagram (SfDiagram) in Windows Presentation Foundation applications. Use this when working with diagrams like flowcharts, org charts, BPMN, or UML in WPF. Covers creating diagrams, adding nodes and connectors, configuring automatic layouts, implementing BPMN and UML shapes, setting up symbol palettes, data binding, export functionality, and appearance customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Diagram (SfDiagram)

The Syncfusion WPF Diagram control (SfDiagram) is a feature-rich diagramming library for building interactive flowcharts, org charts, BPMN diagrams, UML diagrams, network diagrams, and more in WPF applications. It provides nodes, connectors, annotations, ports, automatic layouts, and extensive customization options.

## When to Use This Skill

- Creating any visual diagram in a WPF application (flowchart, org chart, mindmap, etc.)
- Adding nodes, connectors, annotations, or ports to a diagram
- Implementing BPMN or UML diagrams
- Setting up automatic diagram layouts (hierarchical, org, mindmap, flowchart, radial)
- Configuring a symbol palette (stencil) for drag-and-drop diagram building
- Binding a data source to generate diagrams dynamically
- Enabling diagram interaction (selection, drag, resize, zoom, keyboard shortcuts)
- Exporting or printing diagrams
- Customizing diagram appearance (gridlines, rulers, themes, page settings)

## Quick Start

### 1. Install NuGet Package
```powershell
Install-Package Syncfusion.SfDiagram.WPF
```

### 2. Add Namespace in XAML
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### 3. Declare SfDiagram
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf" ...>
    <Grid>
        <syncfusion:SfDiagram x:Name="diagram">
            <syncfusion:SfDiagram.Nodes>
                <syncfusion:NodeCollection/>
            </syncfusion:SfDiagram.Nodes>
            <syncfusion:SfDiagram.Connectors>
                <syncfusion:ConnectorCollection/>
            </syncfusion:SfDiagram.Connectors>
        </syncfusion:SfDiagram>
    </Grid>
</Window>
```

### 4. Add a Node and Connector in Code-Behind
```csharp
using Syncfusion.UI.Xaml.Diagram;

// Add nodes
var node1 = new NodeViewModel { ID = "Start", UnitWidth = 120, UnitHeight = 40,
    OffsetX = 200, OffsetY = 100, Shape = this.Resources["Ellipse"] };
var node2 = new NodeViewModel { ID = "End", UnitWidth = 120, UnitHeight = 40,
    OffsetX = 200, OffsetY = 250, Shape = this.Resources["Rectangle"] };
(diagram.Nodes as NodeCollection).Add(node1);
(diagram.Nodes as NodeCollection).Add(node2);

// Add connector
var connector = new ConnectorViewModel { SourceNodeID = "Start", TargetNodeID = "End" };
(diagram.Connectors as ConnectorCollection).Add(connector);
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet setup
- Adding SfDiagram via XAML or code
- License key registration
- Merging built-in shape resource dictionaries
- Creating a minimal flowchart example

### Nodes
📄 **Read:** [references/nodes.md](references/nodes.md)
- Defining and positioning nodes (XAML and C#)
- Built-in shapes and custom shape templates
- Node appearance: fill, stroke, shadow, tooltip
- Node groups and containers
- Node constraints (lock, select, resize, rotate)
- Node events

### Connectors
📄 **Read:** [references/connectors.md](references/connectors.md)
- Defining connectors between nodes
- Connector types: straight, orthogonal, cubic Bezier, arc, quadratic
- Appearance: line style, arrows, dashes
- Connector routing and segments
- Connector constraints and interaction
- Connector splitting at intersection

### Annotations
📄 **Read:** [references/annotations.md](references/annotations.md)
- Adding text labels to nodes and connectors
- Positioning and alignment of annotations
- Appearance customization (font, background, border)
- Editing annotations at runtime
- Annotation events

### Ports
📄 **Read:** [references/ports.md](references/ports.md)
- Defining connection ports on nodes
- Port types and visual appearance
- Hover effects on ports
- Automatic port creation
- Port-to-port connector creation

### Interaction
📄 **Read:** [references/interaction.md](references/interaction.md)
- Selection (single, multi, select by type)
- Drag and drop nodes and connectors
- Resize, rotate, and flip
- Zoom and pan
- Keyboard shortcuts
- Tooltips and user handles
- Scroll settings, drag limits, snapping

### Commands
📄 **Read:** [references/commands.md](references/commands.md)
- Alignment (left, right, top, bottom, center)
- Clipboard operations (cut, copy, paste, duplicate)
- Delete nodes and connectors
- Undo/Redo via commands
- Nudge (arrow key movement)
- Rotate, flip, fit-to-page
- Spacing, sizing, Z-order
- Expand/collapse nodes
- Custom command manager

### Automatic Layouts
📄 **Read:** [references/automatic-layouts.md](references/automatic-layouts.md)
- Hierarchical Tree Layout
- Organization Chart Layout
- Mind Map Layout
- Flowchart Layout
- Radial Tree Layout
- Force Directed Layout
- Layout configuration (spacing, orientation, margin)

### BPMN Shapes
📄 **Read:** [references/bpmn-shapes.md](references/bpmn-shapes.md)
- BPMN activities (tasks, sub-processes)
- BPMN events (start, intermediate, end)
- BPMN gateways
- BPMN data objects and data stores
- BPMN connectors and text annotations
- BPMN groups and symbol palette

### UML Diagrams
📄 **Read:** [references/uml-diagrams.md](references/uml-diagrams.md)
- UML shape types (class, component, use case, activity, state, sequence)
- UML sequence diagram elements
- Configuring UML shapes and relationships

### Swimlane
📄 **Read:** [references/swimlane.md](references/swimlane.md)
- Creating swimlane diagrams
- Defining lanes and phases
- Swimlane symbol palette
- Swimlane interaction and customization

### Stencil (Symbol Palette)
📄 **Read:** [references/stencil.md](references/stencil.md)
- Setting up a drag-and-drop symbol palette
- Symbol groups and categories
- Symbol filtering and search
- Adding custom shapes to palette
- Stencil appearance

### Data Source
📄 **Read:** [references/data-source.md](references/data-source.md)
- Binding a flat data collection to diagram
- Parent-child data relationships
- Self-referencing hierarchical data
- Dynamic data update

### Serialization & Undo/Redo
📄 **Read:** [references/serialization-undo-redo.md](references/serialization-undo-redo.md)
- Save and load diagram as XML
- Serialize to/from JSON or stream
- Undo/Redo history manager
- Custom undo/redo entries

### Diagram Customization
📄 **Read:** [references/diagram-customization.md](references/diagram-customization.md)
- Gridlines (visibility, style, snapping)
- Rulers (appearance, measurement units)
- Page settings (size, orientation, margins, background)
- Scroll settings and auto-scroll
- Themes and styles
- Localization
- Context menu
- Diagram Ribbon
- Drawing tools
- Overview control

### Export & Print
📄 **Read:** [references/export-print.md](references/export-print.md)
- Export diagram to PNG, JPEG, BMP
- Export to PDF
- Print with print settings
- Preview before printing

### Performance & Constraints
📄 **Read:** [references/performance-constraints.md](references/performance-constraints.md)
- Virtualization for large diagrams
- Diagram-level constraints
- Node and connector constraints
- Performance optimization tips

## Common Patterns

### Pattern: Flowchart with Automatic Layout
When users want an auto-arranged diagram from data — read `references/automatic-layouts.md` and `references/data-source.md`.

### Pattern: Org Chart from Business Data
Bind employee data with parent-child relationships and apply Organization Layout — read `references/data-source.md` and `references/automatic-layouts.md`.

### Pattern: BPMN Process Diagram
For business process modeling with standardized shapes — read `references/bpmn-shapes.md`.

### Pattern: Interactive Diagram Builder
For apps where users drag shapes from a palette — read `references/stencil.md` and `references/interaction.md`.

### Pattern: Save/Load Diagram State
For persisting diagram state across sessions — read `references/serialization-undo-redo.md`.
