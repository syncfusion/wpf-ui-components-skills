# Performance and Virtualization in WPF GridControl

## Why GridControl Is Fast

GridControl is a **cell-based virtual grid** — it has no internal data structure. It requests cell data only when needed (via `QueryCellInfo`) and uses a weak-reference volatile style cache so unused cell data can be garbage collected. This design enables millions of rows without loading them all into memory.

---

## Virtual Mode with QueryCellInfo

The most important performance pattern: **never pre-populate all cells**. Instead, let the grid pull data on-demand.

```csharp
// Set the dimensions (no memory allocated for actual data yet)
gridControl.Model.RowCount    = 5_000_000;
gridControl.Model.ColumnCount = 50;

// Provide data only when the grid asks for it
gridControl.QueryCellInfo += (s, e) =>
{
    int r = e.Cell.RowIndex;
    int c = e.Cell.ColumnIndex;

    if (r == 0)
    {
        e.Style.CellType   = "Header";
        e.Style.CellValue  = $"Col {c}";
        return;
    }
    // Fetch only the requested cell from your data source
    e.Style.CellValue = _dataSource.GetValue(r, c);
};
```

### Invalidating cached cell data

After your data source changes, tell the grid to re-query:

```csharp
// Invalidate a single cell
gridControl.InvalidateCell(row, col);

// Invalidate a range of cells
gridControl.InvalidateCell(GridRangeInfo.Cells(startRow, startCol, endRow, endCol));

// Clear the entire volatile style cache
gridControl.Model.VolatileCellStyles.Clear();
```

---

## High-Frequency Updates

For real-time dashboards where many cells change rapidly:

1. Batch invalidation instead of invalidating cell-by-cell.
2. Suspend layout during batch updates:

```csharp
// Temporarily suspend redraw for bulk updates
gridControl.BeginUpdate();

for (int i = 1; i <= 1000; i++)
    gridControl.Model[i, 1].CellValue = _liveData[i];

// Resume and repaint once
gridControl.EndUpdate();
gridControl.Refresh();
```

---

## Covered Ranges (Merged Cells)

Merge cells across rows and/or columns using `CoveredCells`:

```csharp
// Merge cells (2,2) through (4,5) into one cell
gridControl.CoveredCells.Add(new CoveredCellInfo(2, 2, 4, 5));
gridControl.Model[2, 2].CellValue    = "Merged Area";
gridControl.Model[2, 2].HorizontalAlignment = HorizontalAlignment.Center;
gridControl.Model[2, 2].VerticalAlignment   = VerticalAlignment.Center;

// Remove a covered range
gridControl.CoveredCells.Remove(new CoveredCellInfo(2, 2, 4, 5));
```

> Cells within a covered range still exist in the model but are hidden visually. Only the top-left cell of the range is rendered.

---

## Floating Cells

A floating cell expands horizontally to show its full content when neighboring cells are empty:

```csharp
// Enable floating for a cell
gridControl.Model[3, 1].FloatCell = true;
gridControl.Model[3, 1].CellValue = "This text floats over empty neighbors →";
```

---

## Zooming

The grid supports runtime zoom via the `ZoomFactor` property:

```csharp
// Zoom to 150%
gridControl.ZoomFactor = 1.5;

// Reset to 100%
gridControl.ZoomFactor = 1.0;

// Zoom to 75%
gridControl.ZoomFactor = 0.75;
```

Zooming scales both the grid cells and all text/controls within them.

---

## Performance Tips

| Tip | Reason |
|---|---|
| Use `QueryCellInfo` instead of direct loop assignment | Loads only visible cells |
| Set `RowCount` before adding data | Avoids multiple layout passes |
| Use `BeginUpdate()` / `EndUpdate()` for bulk changes | Batches redraws |
| Avoid complex `PrepareRenderCell` logic | Called for every visible cell every paint cycle |
| Minimize covered ranges for large grids | Each covered range adds lookup cost per paint |
| Use `DefaultLineSize` instead of per-row/column sizes where possible | Stored as a single value vs. per-index storage |
