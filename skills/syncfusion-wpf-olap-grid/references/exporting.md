# Exporting

## Table of Contents
- [Export Overview](#export-overview)
- [Required Assembly](#required-assembly)
- [Excel Export](#excel-export)
- [Word Export](#word-export)
- [PDF Export](#pdf-export)
- [CSV Export](#csv-export)
- [Format Preservation](#format-preservation)
- [Complete Examples](#complete-examples)

## Export Overview

The OLAP Grid supports exporting data to multiple file formats for offline analysis, reporting, and distribution. Export functionality maintains grid formatting including conditional formatting, styles, and layout.

**Supported Formats:**
- **Excel (.xls, .xlsx)** - Full formatting, calculations, pivot structure
- **Word (.doc, .docx)** - Formatted tables for reports
- **PDF (.pdf)** - Print-ready documents
- **CSV (.csv)** - Raw data for analysis in other tools

**Use Cases:**
- Share analysis results with stakeholders
- Archive reports for compliance
- Integrate OLAP data into presentations
- Enable offline data manipulation

## Required Assembly

Add the following assembly reference to enable export functionality:

```
Syncfusion.OlapGridConverter.Wpf
```

**Assembly Location:**
```
{System Drive}:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<version>\<framework>\
```

## Excel Export

Export OLAP Grid to Excel format with full formatting preservation.

### Implementation

```csharp
private void ExportToExcel_Click(object sender, RoutedEventArgs e)
{
    SaveFileDialog saveFileDialog = new SaveFileDialog();
    saveFileDialog.AddExtension = true;
    saveFileDialog.FileName = "OlapGrid Report";
    saveFileDialog.DefaultExt = "xls";
    saveFileDialog.Filter = "Excel file (.xls)|*.xls";
    
    if (saveFileDialog.ShowDialog() == true)
    {
        if (this.olapGrid.InternalGrid != null && 
            this.olapGrid.InternalGrid.Engine != null)
        {
            GridExcelExport gridExcelExport = new GridExcelExport(
                this.olapGrid.InternalGrid.Engine);
            gridExcelExport.Export(saveFileDialog.FileName);
            MessageBox.Show("Excel document exported successfully!");
        }
    }
}
```

### VB.NET

```vbnet
Private Sub ExportToExcel_Click(sender As Object, e As RoutedEventArgs)
    Dim saveFileDialog As New SaveFileDialog()
    saveFileDialog.AddExtension = True
    saveFileDialog.FileName = "OlapGrid Report"
    saveFileDialog.DefaultExt = "xls"
    saveFileDialog.Filter = "Excel file (.xls)|*.xls"
    
    If saveFileDialog.ShowDialog() = True Then
        If Me.olapGrid.InternalGrid IsNot Nothing AndAlso 
           Me.olapGrid.InternalGrid.Engine IsNot Nothing Then
            Dim gridExcelExport As New GridExcelExport(Me.olapGrid.InternalGrid.Engine)
            gridExcelExport.Export(saveFileDialog.FileName)
            MessageBox.Show("Excel document exported successfully!")
        End If
    End If
End Sub
```

### Features Preserved

- Cell formatting and styles
- Conditional formatting colors
- Grid layout structure
- Row and column headers
- Summary cells
- Drill state (expanded/collapsed structure)

## Word Export

Export OLAP Grid to Word format for inclusion in reports and documents.

### Implementation

```csharp
private void ExportToWord_Click(object sender, RoutedEventArgs e)
{
    SaveFileDialog saveFileDialog = new SaveFileDialog();
    saveFileDialog.AddExtension = true;
    saveFileDialog.FileName = "OlapGrid Report";
    saveFileDialog.DefaultExt = "Doc";
    saveFileDialog.Filter = "Word file (.Doc)|*.Doc";
    
    if (saveFileDialog.ShowDialog() == true)
    {
        if (this.olapGrid.InternalGrid != null && 
            this.olapGrid.InternalGrid.Engine != null)
        {
            GridWordExport gridWordExport = new GridWordExport(
                this.olapGrid.InternalGrid.Engine);
            gridWordExport.Export(saveFileDialog.FileName);
            MessageBox.Show("Word document exported successfully!");
        }
    }
}
```

### VB.NET

```vbnet
Private Sub ExportToWord_Click(sender As Object, e As RoutedEventArgs)
    Dim saveFileDialog As New SaveFileDialog()
    saveFileDialog.AddExtension = True
    saveFileDialog.FileName = "OlapGrid Report"
    saveFileDialog.DefaultExt = "Doc"
    saveFileDialog.Filter = "Word file (.Doc)|*.Doc"
    
    If saveFileDialog.ShowDialog() = True Then
        If Me.olapGrid.InternalGrid IsNot Nothing AndAlso 
           Me.olapGrid.InternalGrid.Engine IsNot Nothing Then
            Dim gridWordExport As New GridWordExport(Me.olapGrid.InternalGrid.Engine)
            gridWordExport.Export(saveFileDialog.FileName)
            MessageBox.Show("Word document exported successfully!")
        End If
    End If
End Sub
```

## PDF Export

Export OLAP Grid to PDF format for print-ready documents.

### Implementation

```csharp
private void ExportToPdf_Click(object sender, RoutedEventArgs e)
{
    SaveFileDialog saveFileDialog = new SaveFileDialog();
    saveFileDialog.AddExtension = true;
    saveFileDialog.FileName = "OlapGrid Report";
    saveFileDialog.DefaultExt = "Pdf";
    saveFileDialog.Filter = "Pdf file (.pdf)|*.pdf";
    
    if (saveFileDialog.ShowDialog() == true)
    {
        if (this.olapGrid.InternalGrid != null && 
            this.olapGrid.InternalGrid.Engine != null)
        {
            GridPdfExport gridPdfExport = new GridPdfExport(
                this.olapGrid.InternalGrid.Engine);
            gridPdfExport.Export(saveFileDialog.FileName);
            MessageBox.Show("Pdf document exported successfully!");
        }
    }
}
```

### VB.NET

```vbnet
Private Sub ExportToPdf_Click(sender As Object, e As RoutedEventArgs)
    Dim saveFileDialog As New SaveFileDialog()
    saveFileDialog.AddExtension = True
    saveFileDialog.FileName = "OlapGrid Report"
    saveFileDialog.DefaultExt = "Pdf"
    saveFileDialog.Filter = "Pdf file (.pdf)|*.pdf"
    
    If saveFileDialog.ShowDialog() = True Then
        If Me.olapGrid.InternalGrid IsNot Nothing AndAlso 
           Me.olapGrid.InternalGrid.Engine IsNot Nothing Then
            Dim gridPdfExport As New GridPdfExport(Me.olapGrid.InternalGrid.Engine)
            gridPdfExport.Export(saveFileDialog.FileName)
            MessageBox.Show("Pdf document exported successfully!")
        End If
    End If
End Sub
```

## CSV Export

Export OLAP Grid to CSV format for data analysis in external tools.

### Implementation

```csharp
private void ExportToCsv_Click(object sender, RoutedEventArgs e)
{
    SaveFileDialog saveFileDialog = new SaveFileDialog();
    saveFileDialog.AddExtension = true;
    saveFileDialog.FileName = "OlapGrid Report";
    saveFileDialog.DefaultExt = "CSV";
    saveFileDialog.Filter = "Csv file (.csv)|*.csv";
    
    if (saveFileDialog.ShowDialog() == true)
    {
        if (this.olapGrid.InternalGrid != null && 
            this.olapGrid.InternalGrid.Engine IsNot Nothing)
        {
            GridCsvExport gridCsvExport = new GridCsvExport(
                this.olapGrid.InternalGrid.Engine);
            gridCsvExport.Export(saveFileDialog.FileName);
            MessageBox.Show("CSV document exported successfully!");
        }
    }
}
```

### VB.NET

```vbnet
Private Sub ExportToCsv_Click(sender As Object, e As RoutedEventArgs)
    Dim saveFileDialog As New SaveFileDialog()
    saveFileDialog.AddExtension = True
    saveFileDialog.FileName = "OlapGrid Report"
    saveFileDialog.DefaultExt = "CSV"
    saveFileDialog.Filter = "Csv file (.csv)|*.csv"
    
    If saveFileDialog.ShowDialog() = True Then
        If Me.olapGrid.InternalGrid IsNot Nothing AndAlso 
           Me.olapGrid.InternalGrid.Engine IsNot Nothing Then
            Dim gridCsvExport As New GridCsvExport(Me.olapGrid.InternalGrid.Engine)
            gridCsvExport.Export(saveFileDialog.FileName)
            MessageBox.Show("CSV document exported successfully!")
        End If
    End If
End Sub
```

### CSV Characteristics

- Plain text format
- Comma-separated values
- No formatting (colors, fonts removed)
- Useful for data import into Excel, databases, or analysis tools
- Smaller file size

## Format Preservation

The export functionality maintains the grid's format state:

**Preserved Elements:**
- Cell values and calculations
- Conditional formatting colors (Excel, Word, PDF)
- Grid layout (Normal, Excel-like, etc.)
- Row and column headers
- Summary cells
- Hierarchical structure (drill state)

**Not Preserved in CSV:**
- Formatting (colors, fonts)
- Conditional formatting
- Cell styles
- Images or icons

## Complete Examples

### Multi-Format Export Menu

```xaml
<Menu>
    <MenuItem Header="Export">
        <MenuItem Header="Export to Excel" Click="ExportToExcel_Click"/>
        <MenuItem Header="Export to Word" Click="ExportToWord_Click"/>
        <MenuItem Header="Export to PDF" Click="ExportToPdf_Click"/>
        <MenuItem Header="Export to CSV" Click="ExportToCsv_Click"/>
    </MenuItem>
</Menu>
```

### Export Helper Class

```csharp
public class OlapGridExporter
{
    private OlapGrid olapGrid;
    
    public OlapGridExporter(OlapGrid grid)
    {
        this.olapGrid = grid;
    }
    
    public void ExportToExcel(string filePath)
    {
        if (ValidateGrid())
        {
            GridExcelExport exporter = new GridExcelExport(olapGrid.InternalGrid.Engine);
            exporter.Export(filePath);
        }
    }
    
    public void ExportToWord(string filePath)
    {
        if (ValidateGrid())
        {
            GridWordExport exporter = new GridWordExport(olapGrid.InternalGrid.Engine);
            exporter.Export(filePath);
        }
    }
    
    public void ExportToPdf(string filePath)
    {
        if (ValidateGrid())
        {
            GridPdfExport exporter = new GridPdfExport(olapGrid.InternalGrid.Engine);
            exporter.Export(filePath);
        }
    }
    
    public void ExportToCsv(string filePath)
    {
        if (ValidateGrid())
        {
            GridCsvExport exporter = new GridCsvExport(olapGrid.InternalGrid.Engine);
            exporter.Export(filePath);
        }
    }
    
    private bool ValidateGrid()
    {
        return olapGrid != null && 
               olapGrid.InternalGrid != null && 
               olapGrid.InternalGrid.Engine != null;
    }
}
```

### Usage

```csharp
private void ExportButton_Click(object sender, RoutedEventArgs e)
{
    OlapGridExporter exporter = new OlapGridExporter(olapGrid1);
    
    SaveFileDialog dialog = new SaveFileDialog();
    dialog.Filter = "Excel files (*.xls)|*.xls|Word files (*.doc)|*.doc|" +
                   "PDF files (*.pdf)|*.pdf|CSV files (*.csv)|*.csv";
    
    if (dialog.ShowDialog() == true)
    {
        string extension = System.IO.Path.GetExtension(dialog.FileName).ToLower();
        
        try
        {
            switch (extension)
            {
                case ".xls":
                case ".xlsx":
                    exporter.ExportToExcel(dialog.FileName);
                    break;
                case ".doc":
                case ".docx":
                    exporter.ExportToWord(dialog.FileName);
                    break;
                case ".pdf":
                    exporter.ExportToPdf(dialog.FileName);
                    break;
                case ".csv":
                    exporter.ExportToCsv(dialog.FileName);
                    break;
            }
            
            MessageBox.Show($"Export to {extension} completed successfully!");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}", "Error", 
                          MessageBoxButton.OK, MessageBoxImage.Error);
        }
    }
}
```

## Best Practices

**File Naming:**
- Include timestamp in filename
- Use descriptive names indicating report content
- Avoid special characters in filenames

**Error Handling:**
- Validate grid has data before export
- Catch and handle export exceptions
- Provide user feedback on success/failure

**User Experience:**
- Show progress indicator for large exports
- Offer "Open exported file" option after export
- Remember user's last export location

**Performance:**
- Large grids may take time to export
- Consider background thread for export operation
- Provide cancel option for long-running exports

## Troubleshooting

**Export fails with null reference:**
- Verify `OlapDataManager` is set and has data
- Ensure grid has been bound (`DataBind()` called)
- Check that `InternalGrid.Engine` is not null

**Assembly not found:**
- Add reference to `Syncfusion.OlapGridConverter.Wpf`
- Verify assembly version matches other Syncfusion assemblies
- Check assembly is copied to output directory

**Formatting lost in export:**
- CSV format doesn't support formatting (expected)
- Verify conditional formats are applied before export
- Check grid layout is set correctly

**File save fails:**
- Verify user has write permissions to target folder
- Check disk space is available
- Ensure file is not open in another application

**Sample Location:**

```
{System Drive}:\Users\<User Name>\AppData\Local\Syncfusion\EssentialStudio\<Version>\WPF\OlapGrid.WPF\Samples\Exporting\Exporting Grid
```
