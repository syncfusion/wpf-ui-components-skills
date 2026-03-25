# Export and Printing for WPF TreeGrid

## Table of Contents

- [Overview](#overview)
- [Export to Excel](#export-to-excel)
- [Excel Export Options](#excel-export-options)
- [Export to PDF](#export-to-pdf)
- [PDF Export Options](#pdf-export-options)
- [Printing](#printing)
- [Export Events](#export-events)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports exporting data to Excel and PDF formats, and printing through PdfViewerControl.

## Export to Excel

### Basic Excel Export

```csharp
using Syncfusion.UI.Xaml.TreeGrid.Converter;
using Syncfusion.XlsIO;

public void ExportToExcel()
{
    var options = new TreeGridExcelExportingOptions();
    options.ExcelVersion = ExcelVersion.Excel2013;
    
    var excelEngine = treeGrid.ExportToExcel(options);
    var workBook = excelEngine.Excel.Workbooks[0];
    
    workBook.SaveAs("TreeGrid.xlsx");
}
```

### Export with File Dialog

```csharp
public void ExportToExcelWithDialog()
{
    var options = new TreeGridExcelExportingOptions();
    
    var excelEngine = treeGrid.ExportToExcel(options);
    var workBook = excelEngine.Excel.Workbooks[0];
    
    var saveFileDialog = new SaveFileDialog
    {
        Filter = "Excel Files|*.xlsx",
        DefaultExt = "xlsx",
        FileName = "TreeGrid_Export"
    };
    
    if (saveFileDialog.ShowDialog() == true)
    {
        workBook.SaveAs(saveFileDialog.FileName);
        MessageBox.Show($"Exported to {saveFileDialog.FileName}");
    }
}
```

## Excel Export Options

### TreeGridExcelExportingOptions

```csharp
var options = new TreeGridExcelExportingOptions
{
    // Export all rows or only expanded rows
    ExportMode = ExportMode.All, // or ExportMode.Default
    
    // Include stacked headers
    ExportStackedHeaders = true,
    
    // Customize node expanding
    CanCustomizeNodeExpandCollapse = true,
    
    // Excel version
    ExcelVersion = ExcelVersion.Excel2016,
    
    // Export format
    ExportingEventHandler = (sender, args) =>
    {
        // Customize each cell
    }
};
```

### Export Specific Columns

```csharp
var options = new TreeGridExcelExportingOptions();

// Exclude specific columns
options.ExcludedColumns.Add("InternalId");
options.ExcludedColumns.Add("Password");

var excelEngine = treeGrid.ExportToExcel(options);
```

### Customize Cells During Export

```csharp
var options = new TreeGridExcelExportingOptions();

options.CellExporting += (sender, args) =>
{
    // Customize specific cells
    if (args.ColumnName == "Salary")
    {
        args.Range.CellStyle.ColorIndex = ExcelKnownColors.Light_green;
        args.Range.CellStyle.Font.Bold = true;
    }
    
    // Format numbers
    if (args.CellType == TreeGridCellType.RecordCell && args.CellValue is decimal)
    {
        args.Range.NumberFormat = "$#,##0.00";
    }
};

var excelEngine = treeGrid.ExportToExcel(options);
```

### Export with Styling

```csharp
var options = new TreeGridExcelExportingOptions
{
    ExportMode = ExportMode.All,
    ExcelVersion = ExcelVersion.Excel2016
};

options.CellExporting += (sender, args) =>
{
    // Style header cells
    if (args.CellType == TreeGridCellType.HeaderCell)
    {
        args.Range.CellStyle.Color = Color.Navy;
        args.Range.CellStyle.Font.Color = ExcelKnownColors.White;
        args.Range.CellStyle.Font.Bold = true;
        args.Range.CellStyle.Font.Size = 12;
    }
    
    // Alternate row colors
    if (args.CellType == TreeGridCellType.RecordCell)
    {
        if (args.RowIndex % 2 == 0)
            args.Range.CellStyle.Color = Color.FromArgb(240, 240, 240);
    }
};

var excelEngine = treeGrid.ExportToExcel(options);
var workBook = excelEngine.Excel.Workbooks[0];
var worksheet = workBook.Worksheets[0];

// Auto-fit columns
worksheet.UsedRange.AutofitColumns();

workBook.SaveAs("StyledTreeGrid.xlsx");
```

## Export to PDF

### Basic PDF Export

```csharp
using Syncfusion.UI.Xaml.TreeGrid.Converter;
using Syncfusion.Pdf;

public void ExportToPdf()
{
    var options = new TreeGridPdfExportingOptions();
    var pdfDocument = treeGrid.ExportToPdf(options);
    
    pdfDocument.Save("TreeGrid.pdf");
}
```

### PDF Export with Dialog

```csharp
public void ExportToPdfWithDialog()
{
    var options = new TreeGridPdfExportingOptions();
    var pdfDocument = treeGrid.ExportToPdf(options);
    
    var saveFileDialog = new SaveFileDialog
    {
        Filter = "PDF Files|*.pdf",
        DefaultExt = "pdf",
        FileName = "TreeGrid_Export"
    };
    
    if (saveFileDialog.ShowDialog() == true)
    {
        pdfDocument.Save(saveFileDialog.FileName);
        MessageBox.Show($"Exported to {saveFileDialog.FileName}");
    }
}
```

## PDF Export Options

### TreeGridPdfExportingOptions

```csharp
var options = new TreeGridPdfExportingOptions
{
    // Export mode
    ExportMode = ExportMode.All,
    
    // Include stacked headers
    ExportStackedHeaders = true,
    
    // Fit to page
    AutoColumnWidth = true,
    AutoRowHeight = true,
    
    // Repeat headers on each page
    RepeatHeaders = true,
    
    // Page orientation
    PageOrientation = PdfPageOrientation.Landscape
};
```

### Customize PDF Appearance

```csharp
var options = new TreeGridPdfExportingOptions
{
    ExportMode = ExportMode.All,
    PageOrientation = PdfPageOrientation.Landscape
};

options.CellExporting += (sender, args) =>
{
    // Customize header cells
    if (args.CellType == TreeGridCellType.HeaderCell)
    {
        args.PdfGridCell.Style.BackgroundBrush = PdfBrushes.Navy;
        args.PdfGridCell.Style.TextBrush = PdfBrushes.White;
        args.PdfGridCell.Style.Font = new PdfStandardFont(PdfFontFamily.Helvetica, 12, PdfFontStyle.Bold);
    }
    
    // Highlight specific cells
    if (args.ColumnName == "Salary" && args.CellValue is decimal salary)
    {
        if (salary > 80000)
        {
            args.PdfGridCell.Style.BackgroundBrush = PdfBrushes.LightGreen;
        }
    }
};

var pdfDocument = treeGrid.ExportToPdf(options);
pdfDocument.Save("CustomizedTreeGrid.pdf");
```

### Add Headers and Footers

```csharp
var options = new TreeGridPdfExportingOptions
{
    ExportMode = ExportMode.All,
    PageOrientation = PdfPageOrientation.Landscape
};

var pdfDocument = treeGrid.ExportToPdf(options);

// Add header
var page = pdfDocument.Pages[0];
var font = new PdfStandardFont(PdfFontFamily.Helvetica, 14, PdfFontStyle.Bold);
page.Graphics.DrawString("Employee Report", font, PdfBrushes.Black, new PointF(10, 10));

// Add footer
for (int i = 0; i < pdfDocument.Pages.Count; i++)
{
    var currentPage = pdfDocument.Pages[i];
    var footerFont = new PdfStandardFont(PdfFontFamily.Helvetica, 10);
    var footerText = $"Page {i + 1} of {pdfDocument.Pages.Count}";
    var footerSize = footerFont.MeasureString(footerText);
    var x = currentPage.GetClientSize().Width - footerSize.Width - 10;
    var y = currentPage.GetClientSize().Height - 20;
    
    currentPage.Graphics.DrawString(footerText, footerFont, PdfBrushes.Black, new PointF(x, y));
}

pdfDocument.Save("TreeGridWithHeaderFooter.pdf");
```

## Printing

### Print Using PdfViewerControl

```csharp
using Syncfusion.Windows.PdfViewer;

public void PrintTreeGrid()
{
    // Export to PDF first
    var options = new TreeGridPdfExportingOptions();
    var pdfDocument = treeGrid.ExportToPdf(options);
    
    // Save to temporary file
    var tempFile = Path.GetTempFileName() + ".pdf";
    pdfDocument.Save(tempFile);
    pdfDocument.Close();
    
    // Print using PdfViewerControl
    var pdfViewer = new PdfViewerControl();
    pdfViewer.Load(tempFile);
    pdfViewer.Print(true); // true = show print dialog
    
    // Cleanup
    File.Delete(tempFile);
}
```

### Print with Custom Settings

```csharp
public void PrintTreeGridWithSettings()
{
    var options = new TreeGridPdfExportingOptions
    {
        PageOrientation = PdfPageOrientation.Landscape
    };
    
    var pdfDocument = treeGrid.ExportToPdf(options);
    var tempFile = Path.GetTempFileName() + ".pdf";
    pdfDocument.Save(tempFile);
    pdfDocument.Close();
    
    var pdfViewer = new PdfViewerControl();
    pdfViewer.Load(tempFile);
    
    // Configure print settings
    var printDialog = new PrintDialog();
    if (printDialog.ShowDialog() == true)
    {
        pdfViewer.Print(printDialog.PrintQueue, printDialog.PrintTicket);
    }
    
    File.Delete(tempFile);
}
```

## Export Events

### Excel Export Events

```csharp
var options = new TreeGridExcelExportingOptions();

// Before exporting
options.CellExporting += (sender, args) =>
{
    Debug.WriteLine($"Exporting cell: {args.ColumnName}, Value: {args.CellValue}");
};

// After exporting
var excelEngine = treeGrid.ExportToExcel(options);
MessageBox.Show("Excel export completed");
```

### PDF Export Events

```csharp
var options = new TreeGridPdfExportingOptions();

options.CellExporting += (sender, args) =>
{
    Debug.WriteLine($"Exporting to PDF: {args.ColumnName}");
    
    // Skip specific cells
    if (args.ColumnName == "InternalNotes")
    {
        args.CellValue = string.Empty;
    }
};

var pdfDocument = treeGrid.ExportToPdf(options);
```

## Best Practices

### 1. Handle Large Datasets

```csharp
// Show progress indicator
var progressDialog = new ProgressDialog();
progressDialog.Show();

await Task.Run(() =>
{
    var options = new TreeGridExcelExportingOptions();
    var excelEngine = treeGrid.ExportToExcel(options);
    var workBook = excelEngine.Excel.Workbooks[0];
    workBook.SaveAs("LargeTreeGrid.xlsx");
});

progressDialog.Close();
```

### 2. Optimize Export Performance

```csharp
// Export only visible columns
var options = new TreeGridExcelExportingOptions();
foreach (var column in treeGrid.Columns.Where(c => c.IsHidden))
{
    options.ExcludedColumns.Add(column.MappingName);
}
```

### 3. Provide User Feedback

```csharp
public async Task ExportWithFeedbackAsync()
{
    var result = MessageBox.Show("Export to Excel?", "Confirm", MessageBoxButton.YesNo);
    if (result == MessageBoxResult.Yes)
    {
        try
        {
            var options = new TreeGridExcelExportingOptions();
            var excelEngine = treeGrid.ExportToExcel(options);
            var workBook = excelEngine.Excel.Workbooks[0];
            workBook.SaveAs("Export.xlsx");
            
            MessageBox.Show("Export completed successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}");
        }
    }
}
```

## Troubleshooting

### Issue: Export Fails

```csharp
try
{
    var options = new TreeGridExcelExportingOptions();
    var excelEngine = treeGrid.ExportToExcel(options);
    var workBook = excelEngine.Excel.Workbooks[0];
    workBook.SaveAs("Export.xlsx");
}
catch (Exception ex)
{
    MessageBox.Show($"Export error: {ex.Message}");
    Debug.WriteLine($"Stack trace: {ex.StackTrace}");
}
```

### Issue: File Access Denied

```csharp
// Ensure file is not open in another application
var fileName = "Export.xlsx";
if (File.Exists(fileName))
{
    try
    {
        File.Delete(fileName);
    }
    catch
    {
        fileName = $"Export_{DateTime.Now:yyyyMMdd_HHmmss}.xlsx";
    }
}
```

### Issue: Missing Syncfusion References

```csharp
// Required NuGet packages:
// - Syncfusion.SfGrid.WPF
// - Syncfusion.GridExport.WPF
// - Syncfusion.XlsIO.WPF (for Excel)
// - Syncfusion.Pdf.WPF (for PDF)
// - Syncfusion.PdfViewer.WPF (for printing)
```
