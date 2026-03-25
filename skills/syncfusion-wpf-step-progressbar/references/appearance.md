# Appearance

## Marker Shape

Change the step marker shape using `MarkerShapeType`. Default: `Circle`.

```xaml
<Syncfusion:SfStepProgressBar MarkerShapeType="Square" SelectedIndex="3">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.MarkerShapeType = MarkerShapeType.Square;
```

| Value | Shape |
|---|---|
| `Circle` | Circular markers (default) |
| `Square` | Square markers |

For fully custom shapes (checkmarks, icons, etc.), use `MarkerTemplateSelector` — see [data-binding-and-templates.md](data-binding-and-templates.md).

---

## Orientation

Switch between horizontal and vertical layout with `Orientation`. Default: `Horizontal`.

```xaml
<Syncfusion:SfStepProgressBar Orientation="Vertical" SelectedIndex="3">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.Orientation = Orientation.Vertical;
```

- **Horizontal** — steps flow left to right; labels appear below each marker
- **Vertical** — steps flow top to bottom; labels appear to the right of each marker
- `ItemsStretch="Auto"` is only applicable in vertical orientation — see [layouts.md](layouts.md)

---

## Connector Customization

Style the lines connecting step markers using these properties:

| Property | Purpose |
|---|---|
| `ActiveConnectorColor` | Color of connectors between active (completed) steps |
| `ConnectorColor` | Color of connectors between inactive steps |
| `ConnectorThickness` | Thickness of all connector lines |

```xaml
<Syncfusion:SfStepProgressBar
    SelectedIndex="2"
    ActiveConnectorColor="Coral"
    ConnectorColor="Crimson"
    ConnectorThickness="4">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.ActiveConnectorColor = new SolidColorBrush(Colors.Coral);
stepProgressBar.ConnectorColor = new SolidColorBrush(Colors.Crimson);
stepProgressBar.ConnectorThickness = 4;
```

**Tip:** Use `ActiveConnectorColor` with a brand accent color and a lighter `ConnectorColor` to clearly distinguish completed vs pending segments.

---

## MarkerClicked Event

Handle step marker clicks to let users navigate directly to a step:

```xaml
<Syncfusion:SfStepProgressBar
    SelectedIndex="2"
    MarkerClicked="SfStepProgressBar_MarkerClicked">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
private void SfStepProgressBar_MarkerClicked(object sender, MarkerClickedEventArgs e)
{
    // Get the clicked item's index and set it as selected
    ItemsControl itemsControl = ItemsControl.ItemsControlFromItemContainer(e.StepViewItem);
    int index = itemsControl.ItemContainerGenerator.IndexFromContainer(e.StepViewItem);
    (sender as SfStepProgressBar).SelectedIndex = index;
}
```

- `e.StepViewItem` — the `StepViewItem` that was clicked
- Use `ItemsControlFromItemContainer` + `IndexFromContainer` to resolve the clicked index
- Useful for non-linear wizard flows where users can jump to any completed step

---

## Gotchas

- **Default shape is Circle** — explicitly set `MarkerShapeType="Square"` only if you need square; don't specify it for the default
- **`ActiveConnectorColor` vs `ConnectorColor`** — `ActiveConnectorColor` applies to the connector segment between two *completed* steps; `ConnectorColor` applies to segments involving at least one inactive step
- **Vertical layout labels** — in vertical mode, `Content` appears to the right of the marker and `SecondaryContent` to the left; layout swaps compared to horizontal
