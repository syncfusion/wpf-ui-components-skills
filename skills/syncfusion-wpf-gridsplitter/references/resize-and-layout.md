# Resize and Layout Operations

This guide focuses on resize-specific behaviors, increments, programmatic APIs, and merged-cell scenarios. For basic placement and layout examples, see `references/getting-started.md`.

## ResizeBehavior Property

Controls which grid rows or columns are affected when the splitter moves.

```xml
<syncfusion:SfGridSplitter ResizeBehavior="PreviousAndNext"/>
```

Common options: `PreviousAndNext`, `PreviousAndCurrent`, `CurrentAndNext` — choose based on which panels should change size.

## Resize Increments

Use `DragIncrement` and `KeyboardIncrement` to control granularity.

```xml
<syncfusion:SfGridSplitter DragIncrement="5" KeyboardIncrement="25"/>
```

- `DragIncrement`: pixel snapping for mouse drag (default 1)
- `KeyboardIncrement`: pixel step for keyboard arrows (default 20)

## Programmatic Resizing

Use `MoveSplitter(double offset)` to adjust the splitter in code.

```csharp
// Move splitter down by 50 pixels
gridSplitter.MoveSplitter(50);

// Move splitter up by 30 pixels
gridSplitter.MoveSplitter(-30);
```

Common use cases: save/restore positions, presets (compact/expanded), and animated transitions (small increments with delays).

## Working with Merged Cells

`SfGridSplitter` supports layouts where content spans multiple rows/columns. Place splitters carefully (Grid.Row / Grid.Column / RowSpan) and test interactions when cells are merged.

**Tip:** For complex multi-splitter layouts, sketch the grid and verify which rows/columns are star (`*`) vs fixed to ensure expected resize behavior.

## Orientation Handling

Orientation is inferred from placement and sizing: horizontal if `HorizontalAlignment=Stretch` and `Height` is set, vertical if `VerticalAlignment=Stretch` and `Width` is set.

```xml
<syncfusion:SfGridSplitter Grid.Row="1" HorizontalAlignment="Stretch" Height="5"/>
```

## Troubleshooting

- Splitter not moving: verify `Stretch` alignment and `ResizeBehavior` plus star sizing on adjacent rows/columns.
- Panels collapsing: set `MinHeight`/`MinWidth` on rows/columns.
- Jerky movement: reduce `DragIncrement` to `1` or enable `ShowsPreview` for heavy layouts.


This displays a preview line instead of real-time resizing, improving performance for complex layouts.
