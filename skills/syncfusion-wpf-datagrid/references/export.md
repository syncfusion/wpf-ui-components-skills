# Export Operations in WPF DataGrid

## Table of Contents
- [Overview](#overview)
- [Required Assemblies](#required-assemblies)
- [Export to Excel](#export-to-excel)
  - [ExcelExportingOptions](#excelexportingoptions)
  - [Saving Options](#saving-options)
  - [Customization](#customization)
- [Export to PDF](#export-to-pdf)
  - [PdfExportingOptions](#pdfexportingoptions)
  - [PDF Customization](#pdf-customization)
- [Troubleshooting](#troubleshooting)

## Overview

SfDataGrid provides support to export data to Excel and PDF files with grouping, filtering, sorting, paging, unbound rows, merged cells, stacked headers, and Details View.

## Required Assemblies

### Excel Export
- `Syncfusion.SfGridConverter.WPF`
- `Syncfusion.XlsIO.Base`

### PDF Export
- `Syncfusion.SfGridConverter.WPF`
- `Syncfusion.Pdf.Base`

### NuGet Package
Install [Syncfusion.DataGridExcelExport.WPF](https://www.nuget.org/packages/Syncfusion.DataGridExcelExport.WPF) package.

## Export to Excel

### Basic Excel Export

```csharp
using Syncfusion.UI.Xaml.Grid.Converter;

// Export to Excel
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

### ExcelExportingOptions

#### ExportMode
Export actual value or display text:

```csharp
var options = new ExcelExportingOptions();
options.ExportMode = ExportMode.Text; // Export display text
// options.ExportMode = ExportMode.Value; // Export actual value (default)
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### AllowOutlining
Enable outlines based on groups:

```csharp
var options = new ExcelExportingOptions();
options.AllowOutlining = true;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### ExcludeColumns
Exclude specific columns from export:

```csharp
var options = new ExcelExportingOptions();
options.ExcludeColumns.Add("CustomerName");
options.ExcludeColumns.Add("Country");
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### ExcelVersion
Specify Excel version:

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
// ExcelVersion.Excel97to2003
// ExcelVersion.Excel2007
// ExcelVersion.Excel2010
// ExcelVersion.Excel2013
// ExcelVersion.Excel2016
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### ExportStackedHeaders
Export stacked headers:

```csharp
var options = new ExcelExportingOptions();
options.ExportStackedHeaders = true;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### ExportMergedCells
Export merged cells:

```csharp
var options = new ExcelExportingOptions();
options.ExportMergedCells = true;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### ExportUnBoundRows
Export unbound rows:

```csharp
var options = new ExcelExportingOptions();
options.ExportUnBoundRows = true;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### StartRowIndex and StartColumnIndex
Export to specific location in worksheet:

```csharp
var options = new ExcelExportingOptions();
options.StartColumnIndex = 3;
options.StartRowIndex = 3;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

### Saving Options

#### Save Directly to File

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Save as Stream

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
FileStream fileStream = new FileStream("Output.xlsx", FileMode.Create);
workBook.SaveAs(fileStream);
fileStream.Close();
```

#### Save Using File Dialog

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];

SaveFileDialog sfd = new SaveFileDialog
{
    FilterIndex = 2,
    Filter = "Excel 97 to 2003 Files(*.xls)|*.xls|Excel 2007 to 2010 Files(*.xlsx)|*.xlsx|Excel 2013 File(*.xlsx)|*.xlsx"
};

if (sfd.ShowDialog() == true)
{
    using (Stream stream = sfd.OpenFile())
    {
        if (sfd.FilterIndex == 1)
            workBook.Version = ExcelVersion.Excel97to2003;
        else if (sfd.FilterIndex == 2)
            workBook.Version = ExcelVersion.Excel2010;
        else
            workBook.Version = ExcelVersion.Excel2013;
        
        workBook.SaveAs(stream);
    }

    if (MessageBox.Show("Do you want to view the workbook?", "Workbook has been created",
                        MessageBoxButton.YesNo, MessageBoxImage.Information) == MessageBoxResult.Yes)
    {
        System.Diagnostics.Process.Start(sfd.FileName);
    }
}
```

### Customization

#### Styling Cells Based on CellType

```csharp
var options = new ExcelExportingOptions();
options.ExportingEventHandler = ExportingHandler;
options.AllowOutlining = true;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

private static void ExportingHandler(object sender, GridExcelExportingEventArgs e)
{
    if (e.CellType == ExportCellType.HeaderCell)
    {
        e.CellStyle.BackGroundBrush = new SolidColorBrush(Colors.LightPink);
        e.CellStyle.ForeGroundBrush = new SolidColorBrush(Colors.White);
        e.Handled = true;
    }
    else if (e.CellType == ExportCellType.RecordCell)
    {
        e.CellStyle.BackGroundBrush = new SolidColorBrush(Colors.LightSkyBlue);
        e.Handled = true;
    }
    else if (e.CellType == ExportCellType.GroupCaptionCell)
    {
        e.CellStyle.BackGroundBrush = new SolidColorBrush(Colors.Wheat);
        e.Handled = true;
    }
}
```

#### Customize Cell Values

```csharp
var options = new ExcelExportingOptions();
options.CellsExportingEventHandler = CellExportingHandler;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

private static void CellExportingHandler(object sender, GridCellExcelExportingEventArgs e)
{            
    if (e.CellType == ExportCellType.RecordCell && e.ColumnName == "IsClosed")
    {
        if (e.CellValue.Equals(true))
            e.Range.Cells[0].Value = "Y";
        else
            e.Range.Cells[0].Value = "N";
        e.Handled = true;
    }
}
```

#### Set Borders

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.Worksheets[0].UsedRange.BorderInside(ExcelLineStyle.Dash_dot, ExcelKnownColors.Black);
workBook.Worksheets[0].UsedRange.BorderAround(ExcelLineStyle.Dash_dot, ExcelKnownColors.Black);
workBook.SaveAs("Sample.xlsx");
```

#### Enable Filters

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.Worksheets[0].AutoFilters.FilterRange = workBook.Worksheets[0].UsedRange;
workBook.SaveAs("Sample.xlsx");
```

#### Export Selected Items

```csharp
var options = new ExcelExportingOptions();
ExcelEngine excelEngine = new ExcelEngine();
IWorkbook workBook = excelEngine.Excel.Workbooks.Create();
workBook.Worksheets.Create();
dataGrid.ExportToExcel(dataGrid.SelectedItems, options, workBook.Worksheets[0]);
workBook.Version = ExcelVersion.Excel2013;
workBook.SaveAs("Sample.xlsx");
```

#### Export All Pages

```csharp
var options = new ExcelExportingOptions();
options.ExportAllPages = true;            
var excelEngine = dataGrid.ExportToExcel(dataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

## Export to PDF

### Basic PDF Export

```csharp
using Syncfusion.UI.Xaml.Grid.Converter;

var document = dataGrid.ExportToPdf();
document.Save("Sample.pdf");
```

### PdfExportingOptions

#### AutoColumnWidth
Fit column widths based on content:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.AutoColumnWidth = true;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### AutoRowHeight
Fit row heights based on content:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.AutoRowHeight = true;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### ExcludeColumns
Exclude specific columns:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExcludeColumns.Add("CustomerName");
options.ExcludeColumns.Add("Country");
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### RepeatHeaders
Show column headers on each page:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.RepeatHeaders = true;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### FitAllColumnsInOnePage
Fit all columns on one page:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.FitAllColumnsInOnePage = true;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### ExportAllPages
Export all pages when paging is used:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportAllPages = true;            
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### ExportGroups
Control group export:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportGroups = false; // Exclude groups
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### ExportGroupSummary
Control group summary export:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportGroupSummary = false;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### ExportTableSummary
Control table summary export:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportTableSummary = false;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

#### ExportStackedHeaders
Export stacked headers:

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportStackedHeaders = true;
var document = dataGrid.ExportToPdf(options);            
document.Save("Sample.pdf");
```

### PDF Customization

#### Setting Header and Footer

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.PageHeaderFooterEventHandler = PdfHeaderFooterEventHandler;
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

static void PdfHeaderFooterEventHandler(object sender, PdfHeaderFooterEventArgs e)
{
    PdfFont font = new PdfStandardFont(PdfFontFamily.TimesRoman, 20f, PdfFontStyle.Bold);                 
    var width = e.PdfPage.GetClientSize().Width;
    PdfPageTemplateElement header = new PdfPageTemplateElement(width, 38);
    header.Graphics.DrawString("Order Details", font, PdfPens.Black, 70, 3);
    e.PdfDocumentTemplate.Top = header;
}
```

#### Change Page Orientation

```csharp
var options = new PdfExportingOptions();
var document = new PdfDocument();
document.PageSettings.Orientation = PdfPageOrientation.Landscape;
var page = document.Pages.Add();
var PDFGrid = dataGrid.ExportToPdfGrid(dataGrid.View, options);
var format = new PdfGridLayoutFormat()
{
    Layout = PdfLayoutType.Paginate,
    Break = PdfLayoutBreakType.FitPage
};
PDFGrid.Draw(page, new PointF(), format);
document.Save("Sample.pdf");
```

#### Styling Cells Based on CellType

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportingEventHandler = GridPdfExportingEventHandler;
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void GridPdfExportingEventHandler(object sender, GridPdfExportingEventArgs e)
{
    if (e.CellType == ExportCellType.HeaderCell)
        e.CellStyle.BackgroundBrush = PdfBrushes.LightSteelBlue;
    else if (e.CellType == ExportCellType.GroupCaptionCell)
        e.CellStyle.BackgroundBrush = PdfBrushes.LightGray;
    else if (e.CellType == ExportCellType.RecordCell)
        e.CellStyle.BackgroundBrush = PdfBrushes.Wheat;
}
```

#### Customize Cell Values

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.CellsExportingEventHandler = CellsExportingEventHandler;
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

private void CellsExportingEventHandler(object sender, GridCellPdfExportingEventArgs e)
{
    if (e.CellType == ExportCellType.RecordCell && e.ColumnName == "IsClosed")
    {
        if (e.CellValue.Equals("True"))
            e.CellValue = "Y";
        else
            e.CellValue = "N";
    }
}
```

#### Embedding Fonts

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportingEventHandler = GridPdfExportingEventHandler;
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void GridPdfExportingEventHandler(object sender, GridPdfExportingEventArgs e)
{    
    if (e.CellType != ExportCellType.RecordCell)
        return;

    var font = new PdfTrueTypeFont(@"..\..\Resources\segoeui.ttf", 9f, PdfFontStyle.Regular);
    e.CellStyle.Font = font;
}
```

#### Export Selected Items

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.AutoColumnWidth = true;
var document = dataGrid.ExportToPdf(dataGrid.SelectedItems, options);
document.Save("Sample.pdf");
```

#### Export DetailsView

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportDetailsView = true;
options.ExportAllDetails = true; // Export all details view
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Excluding DetailsView

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportDetailsView = true;
options.ChildGridExportingEventHandler = ChildGridExportingEventHandler;
var document = dataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void ChildGridExportingEventHandler(object sender, ChildGridPdfExportingEventArgs e)
{
    var recordEntry = e.NodeEntry as RecordEntry;
    if ((recordEntry.Data as OrderInfo).OrderID == 1002)
        e.Cancel = true;
}
```

### PDF Saving Options

#### Save to File

```csharp
var document = dataGrid.ExportToPdf();
document.Save("Sample.pdf");
```

#### Save to Stream

```csharp
FileStream fileStream = new FileStream("Sample.pdf", FileMode.Create);          
var document = dataGrid.ExportToPdf();
document.Save(fileStream);
fileStream.Close();
```

#### Save Using File Dialog

```csharp
var document = dataGrid.ExportToPdf();
SaveFileDialog sfd = new SaveFileDialog
{
    Filter = "PDF Files(*.pdf)|*.pdf"
};

if (sfd.ShowDialog() == true)
{
    using (Stream stream = sfd.OpenFile())
    {
        document.Save(stream);
    }

    if (MessageBox.Show("Do you want to view the Pdf file?", "Pdf file has been created",
                        MessageBoxButton.YesNo, MessageBoxImage.Information) == MessageBoxResult.Yes)
    {
        System.Diagnostics.Process.Start(sfd.FileName);
    }
}
```

## Troubleshooting

### Issue: Export is slow
**Solution**: Use `ExportingEventHandler` and `CellsExportingEventHandler` sparingly. Avoid customizing every cell individually. Instead, apply formatting after export using XlsIO methods.

### Issue: Columns are not visible in exported file
**Solution**: Check if columns are excluded using `ExcludeColumns` property. Ensure column `Width` is not set to 0.

### Issue: Export fails with OutOfMemoryException
**Solution**: For large datasets, export in chunks or disable features like `AutoColumnWidth` and `AutoRowHeight`.

### Issue: Exported file doesn't match DataGrid appearance
**Solution**: Use `ExportMode.Text` to export display text instead of values. Apply custom styling using event handlers.

### Issue: DetailsView not exported
**Solution**: Set `ExportDetailsView = true` in options. For nested levels, also set `ExportAllDetails = true`.

### Issue: Excel file opens with errors
**Solution**: Ensure correct `ExcelVersion` is set. Check for invalid characters in cell values.

### Issue: PDF file is blank
**Solution**: Verify that DataGrid has data. Check if `ExportGroups` or other filter options are excluding all data.
