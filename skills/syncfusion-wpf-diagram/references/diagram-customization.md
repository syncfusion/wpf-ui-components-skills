# Diagram Customization in WPF Diagram (SfDiagram)

## Gridlines

Show gridlines using `SnapSettings.SnapConstraints`:

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.SnapSettings>
        <syncfusion:SnapSettings SnapConstraints="ShowLines"/>
    </syncfusion:SfDiagram.SnapSettings>
</syncfusion:SfDiagram>
```

```csharp
diagram.SnapSettings = new SnapSettings()
{
    SnapConstraints = SnapConstraints.ShowLines,
};
```

### SnapConstraints Gridline Options

| Value | Effect |
|-------|--------|
| `None` | No gridlines |
| `HorizontalLines` | Horizontal lines only |
| `VerticalLines` | Vertical lines only |
| `ShowLines` | Both horizontal and vertical |

### Customize Gridline Appearance

```csharp
Style pathStyle = new Style(typeof(Path));
pathStyle.Setters.Add(new Setter(Shape.StrokeProperty, new SolidColorBrush(Colors.Blue)));
pathStyle.Setters.Add(new Setter(Shape.StrokeDashArrayProperty, new DoubleCollection() { 3, 3 }));

diagram.SnapSettings = new SnapSettings()
{
    SnapConstraints = SnapConstraints.ShowLines,
    HorizontalGridlines = new Gridlines() { Strokes = new Gridlinestyle { pathStyle } },
    VerticalGridlines   = new Gridlines() { Strokes = new Gridlinestyle { pathStyle } },
};

public class Gridlinestyle : List<Style> { }
```

### Gridline Spacing (LinesInterval)

`LinesInterval` is a `List<double>` where **odd-indexed** values = line thickness, **even-indexed** values = space between lines:

```csharp
// Pattern: thickness, gap, thickness, gap, ...
var intervals = new Intervals { 0.25, 10, 0.5, 20, 1.0, 30 };
diagram.SnapSettings.HorizontalGridlines = new Gridlines() { LinesInterval = intervals };
diagram.SnapSettings.VerticalGridlines   = new Gridlines() { LinesInterval = intervals };

public class Intervals : List<double> { }
```

### Static Gridlines (No Zoom)

```csharp
diagram.SnapSettings.HorizontalGridlines = new Gridlines() { DynamicZoom = false };
diagram.SnapSettings.VerticalGridlines   = new Gridlines() { DynamicZoom = false };
```

---

## Rulers

Rulers show measurement guides along the top and left edges of the diagram canvas:

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.HorizontalRuler>
        <syncfusion:Ruler/>
    </syncfusion:SfDiagram.HorizontalRuler>
    <syncfusion:SfDiagram.VerticalRuler>
        <syncfusion:Ruler Orientation="Vertical"/>
    </syncfusion:SfDiagram.VerticalRuler>
</syncfusion:SfDiagram>
```

```csharp
diagram.HorizontalRuler = new Ruler();
diagram.VerticalRuler   = new Ruler() { Orientation = Orientation.Vertical };
```

Customize position indicator marker color:

```xaml
<syncfusion:Ruler MarkerBrush="Red"/>
```

---

## Page Settings

Control the diagram page size and orientation:

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.PageSettings>
        <syncfusion:PageSettings PageOrientation="Portrait"
                                 PageWidth="300"
                                 PageHeight="400"
                                 ShowPageBreaks="True"
                                 MultiplePage="True"/>
    </syncfusion:SfDiagram.PageSettings>
</syncfusion:SfDiagram>
```

```csharp
diagram.PageSettings = new PageSettings()
{
    PageOrientation = PageOrientation.Portrait,  // or Landscape
    PageWidth       = 300,
    PageHeight      = 400,
    ShowPageBreaks  = true,   // visual page break guides
    MultiplePage    = true,   // grow page dynamically in multiples
};
```

| Property | Description |
|----------|-------------|
| `PageWidth` / `PageHeight` | Fixed page size |
| `PageOrientation` | `Portrait` or `Landscape` |
| `ShowPageBreaks` | Show dashed page break lines |
| `MultiplePage` | Expand canvas in page-size multiples as elements grow |

---

## Themes

Apply a built-in theme to automatically style all nodes and connectors:

```xaml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/BasicShapes.xaml"/>
</ResourceDictionary.MergedDictionaries>

<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.Theme>
        <syncfusion:OfficeTheme/>
    </syncfusion:SfDiagram.Theme>
</syncfusion:SfDiagram>
```

```csharp
diagram.Theme = new OfficeTheme();
```

**Built-in theme classes** (each has 4 style variants):
- `OfficeTheme`
- `MaterialTheme`
- `FabricTheme`
- `BootstrapTheme`
- `HighContrastTheme`
- *(~20 total built-in themes)*

Apply a variant to a specific node via `ThemeStyleId`:

```csharp
node.ThemeStyleId = StyleId.Variant2;
```

---

## Drawing Tools

Control what the user can draw interactively:

```csharp
// Enable single-select and pan
diagram.Tool = Tool.SingleSelect | Tool.ZoomPan;

// Enable continuous drawing mode
diagram.Tool        = Tool.ContinuesDraw;
diagram.DrawingTool = DrawingTool.Node;

// Subscribe to specify what is drawn
(diagram.Info as IGraphInfo).GetDrawType += Diagram_GetDrawType;

private void Diagram_GetDrawType(object sender, DrawTypeEventArgs args)
{
    // Provide a NodeViewModel or ConnectorViewModel
    args.DrawItem = new NodeViewModel()
    {
        UnitWidth  = 100,
        UnitHeight = 60,
        Shape      = App.Current.Resources["Rectangle"],
    };
}
```

### Tool Enum Values

| Value | Description |
|-------|-------------|
| `None` | Disable all tools |
| `SingleSelect` | Click to select one element |
| `MultipleSelect` | Drag to select multiple elements |
| `ZoomPan` | Pan the diagram canvas |
| `DrawOnce` | Draw a single node/connector, then revert to select |
| `ContinuesDraw` | Draw continuously until mode is changed |

### DrawingTool Enum Values

| Value | Description |
|-------|-------------|
| `Node` | Draw a rectangular node |
| `Connector` | Draw a connector |
| `PolyLine` | Draw a polyline connector |
| `FreeHand` | Draw a freehand connector |
| `TextNode` | Draw a text-only node |
| `Ellipse` | Draw an ellipse node |

---

## Overview Control

`Overview` displays a miniature bird's-eye view of the diagram with pan/zoom support:

```xaml
<syncfusion:Overview Source="{Binding ElementName=diagram}"
                     Height="200" Width="300"
                     ShowZoomSlider="True"/>

<syncfusion:SfDiagram x:Name="diagram"/>
```

- `Source` — binds the overview to a diagram
- `ShowZoomSlider` — show/hide the zoom slider (default `true`)
- Users can drag the overview viewport rectangle to pan the main diagram

---

## Context Menu

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.Menu>
        <syncfusion:DiagramMenu>
            <syncfusion:DiagramMenu.MenuItems>
                <syncfusion:DiagramMenuItemCollection>
                    <syncfusion:DiagramMenuItem Header="Cut"
                        Command="{Binding ElementName=diagram,
                                  Path=Info.Commands.Cut}"/>
                    <syncfusion:DiagramMenuItem Header="Copy"
                        Command="{Binding ElementName=diagram,
                                  Path=Info.Commands.Copy}"/>
                    <syncfusion:DiagramMenuItem Header="Paste"
                        Command="{Binding ElementName=diagram,
                                  Path=Info.Commands.Paste}"/>
                </syncfusion:DiagramMenuItemCollection>
            </syncfusion:DiagramMenu.MenuItems>
        </syncfusion:DiagramMenu>
    </syncfusion:SfDiagram.Menu>
</syncfusion:SfDiagram>
```

Disable the default context menu:

```xaml
<syncfusion:SfDiagram Constraints="Default,Virtualize" Menu="{x:Null}"/>
```

---

## Localization

Override default string resources by adding a `.resx` file named `Syncfusion.SfDiagram.WPF.{culture}.resx` with the culture code (e.g., `fr-FR`, `de-DE`) to your project.

Set the application culture:

```csharp
System.Threading.Thread.CurrentThread.CurrentCulture   = new CultureInfo("fr-FR");
System.Threading.Thread.CurrentThread.CurrentUICulture = new CultureInfo("fr-FR");
```

---

## Common Gotchas

- **Gridlines not snapping:** Ensure `SnapConstraints` includes both `ShowLines` and `SnapToLines` if you want snap-to-grid behavior.
- **Page breaks not visible:** Set both `ShowPageBreaks = true` and define `PageWidth`/`PageHeight`.
- **Theme not applied:** Merge `BasicShapes.xaml` resource dictionary in addition to setting `diagram.Theme`.
- **Drawing tool keeps drawing:** Switch `diagram.Tool = Tool.SingleSelect` to exit drawing mode.

## Related
- Snapping details → `references/interaction.md`
- Export with page settings → `references/export-print.md`
- Constraints → `references/performance-constraints.md`
