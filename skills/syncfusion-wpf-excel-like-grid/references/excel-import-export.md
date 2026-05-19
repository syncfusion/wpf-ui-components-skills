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
ExcelEngine excelEngine = new ExcelEngine();
IApplication application = excelEngine.Excel;
IWorkbook myWorkbook = excelEngine.Excel.Workbooks.Add();
IWorksheet mySheet = myWorkbook.Worksheets[1];
IChart chartShape = mySheet.Charts.Add();
IChartSeries series1 = chartShape.Series.Add();
series1.SerieType = ExcelChartType.Column_Clustered;
chartShape.ChartTitle = "Column_Clustered";
series1.Values = mySheet.Range["B1:B5"];
series1.CategoryLabels = mySheet.Range["A1:A5"];
Random r = new Random();

for (int i = 1; i <= 4; i++)
{
    mySheet.Range[i, 1].Number = i;
    mySheet.Range[i, 2].Number = r.Next(4000, 6000);
}

for (int i = 1; i <= 4; i++)
{
    mySheet.Range[i + 5, 1].Number = i;
    mySheet.Range[i + 5, 2].Number = r.Next(4000, 6000);
}
IRange excelRange = mySheet.Range[21, 5];
GridRangeInfoList rangeList = gridControl.Model.SelectedRanges;
GridRangeInfo range = rangeList[0];
gridControl.Model.ExportToExcel(range, mySheet, excelRange, @"Sample2.xls", ExcelVersion.Excel97to2003);
```

### Export using an existing Excel Engine

When you need to control the `ExcelEngine` lifecycle (e.g., to add multiple sheets):

```csharp
ExcelEngine excelEngine = new ExcelEngine();
IApplication application = excelEngine.Excel;
IWorkbook myWorkbook = application.Workbooks.Add();
IWorksheet mySheet = myWorkbook.Worksheets[0];
// Pass the engine and sheet to the converter
gridControl.Model.ExportToExcel(range,excelEngine,0, mySheet.Range[5, 5]);
myWorkbook.SaveAs("Sample.xlsx");
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
