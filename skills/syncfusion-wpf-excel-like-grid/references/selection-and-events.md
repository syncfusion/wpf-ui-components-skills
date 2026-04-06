# Selection and Events in WPF GridControl

## Selection Modes

Control what users can select using `AllowSelection` flags on the grid options:

```csharp
using Syncfusion.Windows.Controls.Grid;

// Cell-only selection (default)
gridControl.Model.Options.AllowSelection = GridSelectionFlags.Cell;

// Row-only selection
gridControl.Model.Options.AllowSelection = GridSelectionFlags.Row;

// Column-only selection
gridControl.Model.Options.AllowSelection = GridSelectionFlags.Column;

// Multiple selection modes combined
gridControl.Model.Options.AllowSelection =
    GridSelectionFlags.Row | GridSelectionFlags.Cell;
```

### Working with selected ranges

```csharp
// Get all currently selected ranges
var ranges = gridControl.Model.SelectedRanges;

foreach (var range in ranges)
{
    int top    = range.Top;
    int left   = range.Left;
    int bottom = range.Bottom;
    int right  = range.Right;
    // Process cells in range...
}

// Set a selection programmatically
gridControl.Model.SelectedRanges.Add(GridRangeInfo.Cells(2, 3, 5, 6));

// Clear selection
gridControl.Model.SelectedRanges.Clear();
```

---

## QueryCellInfo (Virtual Data Loading)

`QueryCellInfo` is the central event for providing cell data on-demand. The grid raises it only for cells it is about to paint, enabling virtual mode with millions of rows.

```csharp
gridControl.QueryCellInfo += Grid_QueryCellInfo;

private void Grid_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    int row = e.Cell.RowIndex;
    int col = e.Cell.ColumnIndex;

    // Row 0 = column headers
    if (row == 0)
    {
        e.Style.CellValue = $"Column {col}";
        e.Style.Font.FontWeight = FontWeights.Bold;
        return;
    }

    // Provide data from your data source
    if (row > 0 && col > 0)
    {
        e.Style.CellValue = GetDataFromSource(row, col);
    }
}
```

> The style set in `QueryCellInfo` is cached in the volatile style cache. Call `gridControl.InvalidateCell(row, col)` to force a re-query for a specific cell, or `gridControl.Model.VolatileCellStyles.Clear()` to invalidate all.

### Commit changes back to the data source

Use `CommitCellInfo` (the inverse of `QueryCellInfo`) to persist user edits:

```csharp
gridControl.CommitCellInfo += (s, e) =>
{
    // Save the edited value to your data source
    SaveToDataSource(e.Cell.RowIndex, e.Cell.ColumnIndex, e.Style.CellValue);
};
```

---

## PrepareRenderCell (View-Level Overrides)

Fires just before a cell is painted, from the **view** rather than the model. Useful for per-view styling (alternating rows, conditional highlights) without altering the underlying data:

```csharp
gridControl.PrepareRenderCell += (s, e) =>
{
    // Alternating row colors
    if (e.Cell.RowIndex % 2 == 0 && e.Cell.RowIndex > 0)
        e.Style.Background = Brushes.AliceBlue;

    // Highlight cells with negative values
    if (e.Style.CellValue is double d && d < 0)
        e.Style.Foreground = Brushes.Red;
};
```

---

## Row and Column Change Events

Track structural changes to the grid model:

```csharp
gridControl.Model.RowsInserted    += (s, e) =>
    Debug.WriteLine($"{e.Count} row(s) inserted at index {e.Index}");

gridControl.Model.ColumnsInserted += (s, e) =>
    Debug.WriteLine($"{e.Count} column(s) inserted at index {e.Index}");

gridControl.Model.RowsRemoved    += (s, e) =>
    Debug.WriteLine($"{e.Count} row(s) removed from index {e.Index}");

gridControl.Model.ColumnsRemoved += (s, e) =>
    Debug.WriteLine($"Column(s) removed from {e.Index}");

gridControl.Model.RowsMoved    += (s, e) =>
    Debug.WriteLine($"Rows moved from {e.From} to {e.To}");

gridControl.Model.ColumnsMoved += (s, e) =>
    Debug.WriteLine($"Columns moved from {e.From} to {e.To}");
```

---

## Current Cell Events

```csharp
// Fires when a new cell becomes the active (current) cell
gridControl.CurrentCellActivated += (s, e) =>
{
    var cell = gridControl.CurrentCell.CellRowColumnIndex;
    StatusBar.Text = $"Active: Row {cell.RowIndex}, Col {cell.ColIndex}";
};

// Fires when the current cell editor is about to be deactivated
gridControl.CurrentCellDeactivating += (s, e) =>
{
    // e.Cancel = true to block deactivation (e.g., for validation)
};
```

---

## Resize Events

```csharp
// Fires while the user drags a row border
gridControl.ResizingRows += (s, e) =>
{
    if (e.Row == 0) e.Cancel = true; // prevent resizing the header row
};

// Fires while the user drags a column border
gridControl.ResizingColumns += (s, e) =>
{
    if (e.Column == 0) e.Cancel = true;
};
```

---

## GridRangeInfo Helper Methods

| Method | Purpose |
|---|---|
| `GridRangeInfo.Cell(row, col)` | Single cell range |
| `GridRangeInfo.Cells(top, left, bottom, right)` | Rectangular range |
| `GridRangeInfo.Row(row)` | Entire row |
| `GridRangeInfo.Col(col)` | Entire column |
| `GridRangeInfo.Table()` | Entire table |
| `range.Contains(other)` | Test containment |
| `range.IntersectsWith(other)` | Test intersection |
