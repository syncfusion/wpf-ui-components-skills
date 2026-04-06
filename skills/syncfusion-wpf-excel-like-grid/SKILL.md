---
name: syncfusion-wpf-excel-like-grid
description: Complete guide for implementing the Syncfusion WPF GridControl — an Excel-like, cell-based virtual grid for Windows Presentation Foundation applications. Use this when working with WPF GridControl or Syncfusion grid components, including cell types, virtual grids, formula cells, clipboard operations, and Excel import/export. Covers QueryCellInfo events, GridStyleInfo, GridModel, row/column management, and appearance customization for desktop applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF GridControl

The Syncfusion WPF **GridControl** is a high-performance, cell-oriented grid that functions as a
flexible tabular display engine. It makes no assumptions about data structure, supports virtual
(on-demand) data loading via `QueryCellInfo`, and provides Excel-like behaviors including formulas,
import/export, clipboard operations, covered cells, and 20+ built-in cell types.

## When to Use This Skill

- Adding a GridControl to a WPF application (XAML or code-behind)
- Populating cells via direct assignment or the virtual `QueryCellInfo` event
- Configuring cell types (CheckBox, ComboBox, DateTimeEdit, FormulaCell, etc.)
- Customizing cell appearance (background, font, borders, data formats)
- Managing rows and columns (insert, remove, move, freeze, hide)
- Clipboard, undo/redo, and editing behavior
- Interactive features: drag-drop columns, cell drag-drop, resizing
- Formula cell support and the built-in formula library
- Exporting grid data to Excel; importing Excel workbooks into the grid
- Selection modes, events, and virtual mode patterns
- Performance tuning, virtualization, covered ranges, zooming
- Advanced features: comments, tooltips, printing, serialization, autofit

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Required assembly references
- Adding GridControl via designer or programmatically
- Setting `RowCount` and `ColumnCount`
- Populating data by loop vs. `QueryCellInfo` virtual event
- Minimal working example

### Cell Types
📄 **Read:** [references/cell-types.md](references/cell-types.md)
- 20+ built-in cell types: Header, Static, CheckBox, Button, Image, ComboBox,
  DropDownList, DateTimeEdit, IntegerEdit, DoubleEdit, CurrencyEdit, PercentEdit,
  MaskEdit, UpDownEdit, RichText, DataBoundTemplate, NestedGrid (ScrollGrid)
- Setting `Style.CellType` on individual cells or ranges
- ComboBox with `ChoiceList` vs. `ItemsSource`
- Custom cell types (CellModel + CellRenderer pattern)

### Appearance and Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- `GridStyleInfo` class and style inheritance hierarchy
- Volatile vs. render cell styles
- Base styles (`BaseStylesMap`), TableStyle, RowStyles, ColumnStyles
- Background (solid/gradient), Foreground, Font, text orientation, borders
- Numeric and DateTime format strings; `FormatProvider` interface

### Managing Rows and Columns
📄 **Read:** [references/managing-rows-and-columns.md](references/managing-rows-and-columns.md)
- Setting row/column counts and default sizes
- Hiding and unhiding rows/columns (`SetHidden`)
- Freezing rows/columns; footer and header rows/columns
- Inserting, moving, and removing rows/columns at runtime
- Resize behavior and disabling resize controllers

### Editing, Clipboard, and Undo/Redo
📄 **Read:** [references/editing-clipboard-undo.md](references/editing-clipboard-undo.md)
- `CopyPasteOption` flags (CopyText, CopyCellData, PasteCell, CutCell, XmlCopyPaste)
- `TextDataExchange` for custom delimiters and buffer-based operations
- Clipboard events and `IGridCopyPaste` custom implementation
- `CommandStack`: Undo/Redo, `BeginTrans`, `CommitTrans`, `Rollback`
- Creating derived commands for extended undo/redo

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)
- Column drag-drop (`AllowDragColumns`)
- Excel-like cell drag-drop (`AllowDragDrop`, `DataObjectConsumerOptions`)
- Runtime row/column resizing and disabling resize controllers
- Hide/unhide rows and columns visually (`HiddenBorderBrush`, `SetHidden`)

### Formula Cells
📄 **Read:** [references/formula-cells.md](references/formula-cells.md)
- Enabling `FormulaCell` type for individual cells or the entire grid
- Built-in formula library (150+ functions: Sum, Avg, Sqrt, Pow, Cos, Sin, etc.)
- Arithmetic operators and calculation precedence
- Cross-cell references (A1 notation), named ranges
- Adding custom formula functions to the library

### Excel Import and Export
📄 **Read:** [references/excel-import-export.md](references/excel-import-export.md)
- Required assemblies (`Syncfusion.XlsIO`, `Syncfusion.GridConverter.Wpf`)
- Exporting entire or selected grid content to .xls / .xlsx
- Importing Excel workbooks: styles, formulas, conditional formatting, freeze panes,
  hyperlinks, comments
- Virtualized import for large workbooks

### Selection and Events
📄 **Read:** [references/selection-and-events.md](references/selection-and-events.md)
- Selection modes: cell, row, column (`AllowSelection` flags)
- `QueryCellInfo` event pattern for virtual data loading
- `PrepareRenderCell` for view-specific overrides
- Common grid events: `CommitCellInfo`, `CurrentCellActivated`, clipboard events,
  `RowsInserted`, `ColumnsMoved`, `ResizingRows`, etc.
- Working with `GridRangeInfo` and `SelectedRanges`

### Performance and Virtualization
📄 **Read:** [references/performance-virtualization.md](references/performance-virtualization.md)
- Virtual mode fundamentals and `QueryCellInfo` best practices
- Covered ranges (`CoveredCells`) and floating cells
- Zooming the grid
- High-frequency update patterns

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Cell comments, input-message tips, and tooltips
- Cell layout customization (covered cells, merged ranges, autofit)
- Printing grid content
- Serialization: saving and loading grid state
- Autofit rows and columns to content

## Quick Start Example

```csharp
// 1. Add assemblies: Syncfusion.Grid.Wpf, Syncfusion.GridCommon.Wpf,
//    Syncfusion.Core.dll, Syncfusion.Shared.Wpf

// 2. XAML — place GridControl inside a ScrollViewer
// <ScrollViewer>
//     <syncfusion:GridControl x:Name="gridControl" />
// </ScrollViewer>

// 3. Code-behind: set size and populate
gridControl.Model.RowCount = 100;
gridControl.Model.ColumnCount = 10;

// Option A — direct assignment
for (int i = 0; i < 100; i++)
    for (int j = 0; j < 10; j++)
        gridControl.Model[i, j].CellValue = $"{i}/{j}";

// Option B — virtual mode (preferred for large data)
gridControl.QueryCellInfo += (s, e) =>
{
    if (e.Cell.RowIndex > 0 && e.Cell.ColumnIndex > 0)
        e.Style.CellValue = $"{e.Cell.RowIndex}/{e.Cell.ColumnIndex}";
};
```

## Common Patterns

### Apply a Cell Type to a Range
```csharp
// Set an entire column to CheckBox
for (int i = 1; i <= gridControl.Model.RowCount; i++)
    gridControl.Model[i, 3].CellType = "CheckBox";
```

### Alternate Row Colors via PrepareRenderCell
```csharp
gridControl.PrepareRenderCell += (s, e) =>
{
    if (e.Cell.RowIndex > 0 && e.Cell.RowIndex % 2 == 0)
        e.Style.Background = Brushes.LightSkyBlue;
};
```

### Freeze Header and Enable Formula Cells
```csharp
gridControl.Model.FrozenRows = 1;
gridControl.Model.HeaderRows = 1;
gridControl.BaseStylesMap["Standard"].StyleInfo.CellType = "FormulaCell";
```

### Export to Excel
```csharp
gridControl.Model.ExportToExcel(@"Output.xlsx", ExcelVersion.Excel2007);
```

## Key Style Properties

| Property | Purpose |
|---|---|
| `Model[r, c].CellType` | Sets built-in or custom cell type string |
| `Model[r, c].CellValue` | Data value stored in the cell |
| `Model[r, c].Background` | Cell background brush |
| `Model[r, c].Foreground` | Cell text color |
| `Model[r, c].Font.FontSize` | Cell font size |
| `Model[r, c].Borders` | Individual border pens (Top/Bottom/Left/Right) |
| `Model[r, c].Format` | Format string (e.g., `"C"`, `"0.00"`, `"d"`) |
| `Model.RowHeights[r]` | Row height in device-independent units |
| `Model.ColumnWidths[c]` | Column width |
| `Model.FrozenRows` | Number of rows frozen at top |
| `Model.FrozenColumns` | Number of columns frozen at left |
````
