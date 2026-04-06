# Managing Rows and Columns in WPF GridControl

## Row and Column Count

Set `RowCount` and `ColumnCount` on the model before populating the grid:

```csharp
gridControl.Model.RowCount    = 500;
gridControl.Model.ColumnCount = 20;
```

---

## Row Heights and Column Widths

### Setting sizes by index

```csharp
// Individual columns
gridControl.Model.ColumnWidths[0] = 30;   // row-header column
gridControl.Model.ColumnWidths[1] = 120;
gridControl.Model.ColumnWidths[4] = 250;

// Individual rows
gridControl.Model.RowHeights[0] = 30;    // column-header row
gridControl.Model.RowHeights[5] = 50;
```

### Default size for all rows/columns

```csharp
gridControl.Model.RowHeights.DefaultLineSize    = 22;
gridControl.Model.ColumnWidths.DefaultLineSize  = 80;
```

---

## Hiding Rows and Columns

`SetHidden(from, to, hide)` hides or unhides an inclusive range:

```csharp
// Hide rows 2 through 10
gridControl.Model.RowHeights.SetHidden(2, 10, true);

// Unhide rows 2 through 10
gridControl.Model.RowHeights.SetHidden(2, 10, false);

// Hide columns 5 through 8
gridControl.Model.ColumnWidths.SetHidden(5, 8, true);

// Unhide columns 5 through 8
gridControl.Model.ColumnWidths.SetHidden(5, 8, false);
```

> Hidden rows/columns can also be hidden interactively by the user dragging a column/row border to its neighbor — see the Interactive Features reference.

---

## Freezing Rows and Columns

Frozen rows and columns stay visible when the grid is scrolled:

```csharp
// Freeze the first row (header) and first two columns
gridControl.Model.FrozenRows    = 1;
gridControl.Model.FrozenColumns = 2;
```

### Footer rows and columns

Footer rows pin to the **bottom**; footer columns pin to the **right**:

```csharp
gridControl.Model.FooterRows    = 2;
gridControl.Model.FooterColumns = 1;

// Style the footer area
gridControl.Model.FooterStyle.Background = Brushes.LightCoral;
```

### Header rows and columns

Visual header rows/columns (styled separately from data rows):

```csharp
gridControl.Model.HeaderRows    = 2;
gridControl.Model.HeaderColumns = 1;

// Style headers
gridControl.Model.HeaderStyle.Font.FontStyle  = FontStyles.Italic;
gridControl.Model.HeaderStyle.Font.FontWeight = FontWeights.Bold;
```

---

## Inserting Rows and Columns

`InsertRows(index, count)` / `InsertColumns(index, count)`:

```csharp
// Insert 1 column at position 3
gridControl.Model.InsertColumns(3, 1);

// Insert 2 rows at position 7
gridControl.Model.InsertRows(7, 2);
```

Track insertions with events:

```csharp
gridControl.Model.RowsInserted    += (s, e) => Debug.WriteLine($"Rows inserted at {e.Index}");
gridControl.Model.ColumnsInserted += (s, e) => Debug.WriteLine($"Cols inserted at {e.Index}");
```

---

## Moving Rows and Columns

`MoveRows(from, count, to)` / `MoveColumns(from, count, to)`:

```csharp
// Move 3 rows starting at index 2 to index 7
gridControl.Model.MoveRows(2, 3, 7);

// Move 2 columns starting at index 1 to index 5
gridControl.Model.MoveColumns(1, 2, 5);
```

> Users can also reorder columns interactively via drag-and-drop when `AllowDragColumns = true`. See the Interactive Features reference.

---

## Removing Rows and Columns

`RemoveRows(index, count)` / `RemoveColumns(index, count)`:

```csharp
// Remove 4 rows starting at row 3
gridControl.Model.RemoveRows(3, 4);

// Remove 2 columns starting at column 5
gridControl.Model.RemoveColumns(5, 2);
```

Track removals:

```csharp
gridControl.Model.RowsRemoved    += (s, e) => Debug.WriteLine($"Rows removed from {e.Index}");
gridControl.Model.ColumnsRemoved += (s, e) => Debug.WriteLine($"Cols removed from {e.Index}");
```

---

## Disabling Resize

Resizing is on by default. Remove the mouse controllers to disable it:

```csharp
// Disable row resize
var rowResizer = gridControl.MouseControllerDispatcher.Find("ResizeRowsMouseController");
gridControl.MouseControllerDispatcher.Remove(rowResizer);

// Disable column resize
var colResizer = gridControl.MouseControllerDispatcher.Find("ResizeColumnsMouseController");
gridControl.MouseControllerDispatcher.Remove(colResizer);
```

To prevent resizing of **specific** rows or columns only, handle the events and cancel:

```csharp
gridControl.ResizingColumns += (s, e) =>
{
    if (e.Column == 0) e.Cancel = true; // lock column 0
};
gridControl.ResizingRows += (s, e) =>
{
    if (e.Row == 0) e.Cancel = true;    // lock row 0
};
```
