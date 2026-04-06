# Advanced Features in WPF GridControl

## Table of Contents
- [Cell Comments (Comment Tips)](#cell-comments-comment-tips)
- [Input Message Tips](#input-message-tips)
- [Tooltips](#tooltips)
- [Cell Layout Customization](#cell-layout-customization)
- [Autofit Rows and Columns](#autofit-rows-and-columns)
- [Printing](#printing)
- [Serialization](#serialization)

---

## Cell Comments (Comment Tips)

Add Excel-like comment indicators to cells. A small red triangle appears in the corner; hovering shows the comment text.

```csharp
// Attach a comment to a cell
gridControl.Model[3, 2].CommentTip.Text = "Reviewed by Finance on 2026-03-24";

// Style the comment popup
gridControl.Model[3, 2].CommentTip.Background = Brushes.LightYellow;
gridControl.Model[3, 2].CommentTip.Foreground = Brushes.DarkRed;

// Show the comment indicator (red triangle)
gridControl.Model[3, 2].ShowComment = true;
```

### Show comment tip programmatically

```csharp
// Force the comment to appear without hovering
gridControl.ShowCommentTip(3, 2);
```

---

## Input Message Tips

Input message tips provide guidance when a cell becomes active — similar to Excel's data validation tooltip.

```csharp
// Set an input message on a cell
gridControl.Model[4, 3].InputTip.Text  = "Enter a value between 1 and 100";
gridControl.Model[4, 3].InputTip.Title = "Valid Range";
gridControl.Model[4, 3].ShowInputTip   = true;
```

The tip appears automatically when the user activates the cell.

---

## Tooltips

The grid supports standard WPF `ToolTip` objects on individual cells via the `GridStyleInfo.Tooltip` property:

```csharp
gridControl.Model[5, 2].ToolTip = "Last updated: 2026-01-15 by Admin";
```

For dynamic tooltips based on cell content, generate the tooltip text in `QueryCellInfo`:

```csharp
gridControl.QueryCellInfo += (s, e) =>
{
    if (e.Cell.RowIndex > 0 && e.Cell.ColumnIndex == 4)
    {
        var val = GetValue(e.Cell.RowIndex, 4);
        e.Style.CellValue = val;
        e.Style.ToolTip   = $"Raw: {GetRawValue(e.Cell.RowIndex)}";
    }
};
```

---

## Cell Layout Customization

### Covered cells (merged ranges)

```csharp
// Span cell (2,2) through (4,5)
gridControl.CoveredCells.Add(new CoveredCellInfo(2, 2, 4, 5));
gridControl.Model[2, 2].CellValue           = "Merged Header";
gridControl.Model[2, 2].HorizontalAlignment = HorizontalAlignment.Center;
```

### Covered ranges with different layouts

```csharp
// Query whether a cell is part of a covered range
var coverInfo = gridControl.CoveredCells.FindRange(row, col);
if (coverInfo != null)
{
    bool isTopLeft = coverInfo.Top == row && coverInfo.Left == col;
}
```

### Cell margins and padding

```csharp
// Add interior padding around cell content
gridControl.Model[6, 2].CellMargins = new Thickness(4, 2, 4, 2);
```

### Word wrap

```csharp
gridControl.Model[7, 2].WrapText = true;
gridControl.Model.RowHeights[7]  = 60; // allow room for wrapped text
```

---

## Autofit Rows and Columns

Resize a row or column to fit its content automatically:

```csharp
// Autofit a specific column to its content width
gridControl.Model.ColStyles[3].AutoFit = true;

// Autofit row height to content
gridControl.Model.RowStyles[5].AutoFit = true;

// Trigger autofit programmatically for all columns
for (int col = 1; col <= gridControl.Model.ColumnCount; col++)
    gridControl.AutoFitColumnWidth(col);

// Trigger autofit for all rows
for (int row = 1; row <= gridControl.Model.RowCount; row++)
    gridControl.AutoFitRowHeight(row);
```

---

## Printing

GridControl has built-in print support. Use `GridPrintManager` to print the entire grid or a specific range.

```csharp
// Print the entire grid (shows print dialog)
var printManager = new GridPrintManager(gridControl);
printManager.Print();

// Print preview
var previewWindow = new PrintPreviewWindow(printManager);
previewWindow.Show();

// Print without dialog (direct to default printer)
printManager.PrintDirect();
```

### Print options

```csharp
var printManager = new GridPrintManager(gridControl);

// Scale to fit one page wide
printManager.PrintOptions.FitToPage = true;

// Set page orientation
printManager.PrintOptions.PageOrientation =
    System.Printing.PageOrientation.Landscape;

// Print a specific cell range only
printManager.PrintRange = GridRangeInfo.Cells(1, 1, 50, 10);

printManager.Print();
```

---

## Serialization

Save and restore the complete state of a GridModel (data, styles, row/column dimensions) to/from XML.

### Save grid state to XML file

```csharp
using (var stream = new FileStream(@"C:\Data\GridState.xml", FileMode.Create))
{
    var writer = new GridModelXmlSerializer();
    writer.Serialize(gridControl.Model, stream);
}
```

### Restore grid state from XML file

```csharp
using (var stream = new FileStream(@"C:\Data\GridState.xml", FileMode.Open))
{
    var reader = new GridModelXmlSerializer();
    reader.Deserialize(gridControl.Model, stream);
}
```

### Save/restore to string (for database storage)

```csharp
// Serialize to string
string xmlState;
using (var ms = new MemoryStream())
{
    var writer = new GridModelXmlSerializer();
    writer.Serialize(gridControl.Model, ms);
    xmlState = Encoding.UTF8.GetString(ms.ToArray());
}

// Deserialize from string
using (var ms = new MemoryStream(Encoding.UTF8.GetBytes(xmlState)))
{
    var reader = new GridModelXmlSerializer();
    reader.Deserialize(gridControl.Model, ms);
}
```

> Serialization captures: cell values, styles, row/column sizes, frozen row/column counts, covered cells, and base style definitions. It does not capture event handlers or runtime-only state.
