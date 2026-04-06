# Interactive Features in WPF GridControl

## Column Drag-and-Drop

Allow users to reorder columns at runtime by dragging a column header to a new position:

```csharp
gridControl.AllowDragColumns = true;
```

No further configuration is needed. The user clicks a column header and drags it left or right to reposition it.

---

## Excel-Like Cell Drag-and-Drop

Allow users to drag selected cell ranges to other locations — copying both content and styles, similar to Excel's fill-handle behavior:

```csharp
// Enable drag-drop and include style information
gridControl.AllowDragDrop = true;
gridControl.Model.Options.DataObjectConsumerOptions =
    GridDataObjectConsumerOptions.Styles;
```

### Key properties

| Property | Type | Description |
|---|---|---|
| `AllowDragDrop` | `bool` | Enables Excel-like cell drag-drop |
| `DataObjectConsumerOptions` | `GridDataObjectConsumerOptions` | Controls whether styles are included (`.Styles`) |
| `DragDropDropTargetFlags` | enum | Additional flags for copy vs. move behavior |

### Events

| Event | When fired | Purpose |
|---|---|---|
| `GridOleDropAtRowCol` | On mouse release at drop target | Customize paste behavior of dropped data |
| `GridQueryCanOleDragRange` | When mouse rolls over a selection edge | Cancel to prevent specific ranges from being dragged |
| `GridQueryOleDataSourceData` | When drag begins | Provide custom clipboard formats |

```csharp
// Prevent a specific range from being dragged
gridControl.QueryCanOleDragRange += (s, e) =>
{
    if (e.Range.Top == 0)   // disallow dragging the header row
        e.Cancel = true;
};
```

---

## Resizing Rows and Columns

Resizing is **on by default**. The user hovers over a row/column border to see a resize cursor, then drags to resize.

### Disable resizing globally

```csharp
// Remove the resize mouse controllers
var rowResizer = gridControl.MouseControllerDispatcher
    .Find("ResizeRowsMouseController");
gridControl.MouseControllerDispatcher.Remove(rowResizer);

var colResizer = gridControl.MouseControllerDispatcher
    .Find("ResizeColumnsMouseController");
gridControl.MouseControllerDispatcher.Remove(colResizer);
```

### Prevent resizing of specific rows/columns

```csharp
gridControl.ResizingColumns += (s, e) =>
{
    if (e.Column == 0) e.Cancel = true;  // lock the first column
};
gridControl.ResizingRows += (s, e) =>
{
    if (e.Row == 0) e.Cancel = true;     // lock the header row
};
```

---

## Hide and Unhide Rows/Columns (Visual Resizing)

GridControl shows a thick, colored border where a row or column is hidden — acting as a visual indicator similar to Excel.

### Programmatic hide/unhide

```csharp
// Hide columns 3 and 4
gridControl.ColumnWidths.SetHidden(3, 4, true);

// Unhide rows 3 and 4
gridControl.RowHeights.SetHidden(3, 4, false);
```

### Style the hidden indicator

```csharp
gridControl.Model.HiddenBorderBrush     = Brushes.Red;
gridControl.Model.HiddenBorderThikness  = 3;   // note: intentional typo in API
```

### Interactive hide/unhide (mouse)

- **To hide:** drag a row/column border all the way to its adjacent header. The border darkens to indicate hidden content.
- **To unhide:** hover over the dark indicator — the cursor changes to a double-bar icon. Double-click to restore all hidden rows/columns at that position.

---

## Keyboard Navigation

GridControl supports Excel-like keyboard shortcuts out of the box:

| Key | Action |
|---|---|
| Arrow keys | Move cell focus |
| F2 | Activate / deactivate current cell editor |
| Alt+F4 | Open / close dropdown popup |
| Ctrl+Arrow | Jump to first or last row/column |
| Shift+Arrow | Extend selection |
| PageUp / PageDown | Scroll by page |
| Ctrl+X / C / V | Cut / Copy / Paste |
| Delete | Delete current row (GridDataControl) |

All keyboard operations can be customized by replacing or extending the `MouseControllerDispatcher`.
