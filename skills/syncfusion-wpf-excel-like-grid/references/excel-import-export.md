# Excel Import and Export in WPF GridControl

## Required Assemblies

Add these references to your project (in addition to the standard GridControl assemblies):

```
Syncfusion.XlsIO.Base.dll
Syncfusion.XlsIO.WPF.dll
Syncfusion.GridConverter.Wpf.dll
```

Namespace required for import operations:

```csharp
using Syncfusion.Windows.Controls.Grid.Converter;
```

---

## Exporting to Excel

`GridModel.ExportToExcel()` converts the grid's content, styles, and formats to an Excel file.

### Export entire grid

```csharp
// Export to Excel 97–2003 format (.xls)
gridControl.Model.ExportToExcel(@"C:\Output\Report.xls", ExcelVersion.Excel97to2003);

// Export to Excel 2007+ format (.xlsx)
gridControl.Model.ExportToExcel(@"C:\Output\Report.xlsx", ExcelVersion.Excel2007);
```

### Export selected range only

```csharp
// Get current selection and export that range
var selectedRanges = gridControl.Model.SelectedRanges;
gridControl.Model.ExportToExcel(@"C:\Output\Selection.xlsx",
    ExcelVersion.Excel2007, selectedRanges);
```

### Export using an existing Excel Engine

When you need to control the `ExcelEngine` lifecycle (e.g., to add multiple sheets):

```csharp
using (var excelEngine = new ExcelEngine())
{
    var application = excelEngine.Excel;
    application.DefaultVersion = ExcelVersion.Excel2007;

    var workbook = application.Workbooks.Create(1);
    var sheet = workbook.Worksheets[0];

    // Pass the engine and sheet to the converter
    gridControl.Model.ExportToExcel(sheet);

    workbook.SaveAs(@"C:\Output\Custom.xlsx");
}
```

### What is exported

- Cell values and text
- Cell styles (background, foreground, font, borders, alignment)
- Format strings
- Formula cell values
- Frozen rows/columns (as freeze panes)

---

## Importing from Excel

`GridModel.ImportFromExcel()` loads an Excel workbook into the grid, preserving the original look and feel.

### Import an entire workbook

```csharp
// Import the first sheet of the workbook
gridControl.Model.ImportFromExcel(@"C:\Data\Workbook.xlsx");
```

### What is imported

| Feature | Supported |
|---|---|
| Font family, size, style, weight | ✓ |
| Cell backgrounds and foregrounds | ✓ |
| Formulas (including cross-sheet) | ✓ |
| Conditional formatting (Greater/Less/Between/Equal) | ✓ |
| Freeze panes | ✓ |
| Hyperlinks (URL and worksheet navigation) | ✓ |
| Cell comments | ✓ |
| Named ranges | ✓ |

### Import with virtualization (large files)

For large Excel workbooks, enable virtualized import so only visible cells are loaded into memory:

```csharp
// Pass GridImportingOptions to enable virtualization
var options = new GridImportingOptions
{
    ImportStyle = true,
    ImportFormula = true
};
gridControl.Model.ImportFromExcel(@"C:\Data\LargeFile.xlsx", options);
```

> Virtualized import means the grid reads Excel cell data on-demand via `QueryCellInfo`, keeping memory usage low even for workbooks with hundreds of thousands of rows.

---

## Round-Trip Example (Import then Export)

```csharp
// 1. Load an Excel file
gridControl.Model.ImportFromExcel(@"C:\Data\Source.xlsx");

// 2. Make changes in the grid at runtime
gridControl.Model[5, 3].CellValue = "Updated";
gridControl.Model[5, 3].Background = Brushes.LightGreen;

// 3. Save back to Excel
gridControl.Model.ExportToExcel(@"C:\Data\Modified.xlsx", ExcelVersion.Excel2007);
```

---

## Common Issues

| Issue | Solution |
|---|---|
| Missing `ExportToExcel` method | Add `Syncfusion.GridConverter.Wpf` assembly reference |
| Formulas not exported | Ensure cells have `CellType = "FormulaCell"` before export |
| Import loses some formatting | Check that `ImportStyle = true` in `GridImportingOptions` |
| Large file freezes UI | Use virtualized import with `GridImportingOptions` |
