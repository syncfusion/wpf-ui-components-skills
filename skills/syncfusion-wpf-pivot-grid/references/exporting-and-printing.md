# Exporting and Printing in WPF PivotGrid

This guide covers exporting PivotGrid content to various formats (Excel, Word, PDF, CSV) and printing capabilities in the Syncfusion WPF PivotGrid control.

## Exporting Overview

The WPF PivotGrid provides options to export content to multiple formats:

- **Microsoft Excel** (XLS/XLSX) - Cell mode and PivotTable mode
- **Microsoft Word** (DOC/DOCX)
- **PDF format**
- **CSV format**

### Required Assemblies

Add the following assemblies to your project for exporting:

- **Syncfusion.PivotGridConverter.Wpf.dll**
- **Syncfusion.XlsIO.Base.dll** (for Excel export)

Include the namespace:

```csharp
using Syncfusion.Windows.Controls.PivotGrid.Converter;
```

## Export to Excel

Export PivotGrid content to Excel documents using Syncfusion XlsIO libraries.

### Export Modes

- **PivotTable Mode**: Exports as a native Excel pivot table
- **Cell Mode**: Exports as individual cell values

### Excel Export Implementation

```csharp
using Syncfusion.Windows.Controls.PivotGrid.Converter;
using Syncfusion.XlsIO;

private void Button_Export(object sender, RoutedEventArgs e)
{
    // Export to Excel document
    SaveFileDialog savedialog = new SaveFileDialog();
    savedialog.AddExtension = true;
    savedialog.FileName = "Sample";
    savedialog.DefaultExt = "xlsx";
    savedialog.Filter = "Excel file (.xlsx)|*.xlsx";
    
    if (savedialog.ShowDialog() == true)
    {
        string fileName = savedialog.FileName;
        GridExcelExport excelExport = new GridExcelExport(
            pivotGrid, 
            ExcelVersion.Excel2007);
        excelExport.ExportMode = ExportModes.PivotTable;
        excelExport.Export(fileName);
    }
}
```

### Export in PivotTable Mode

```csharp
private void ExportToPivotTable()
{
    SaveFileDialog savedialog = new SaveFileDialog
    {
        AddExtension = true,
        FileName = "Sample",
        DefaultExt = "xlsx",
        Filter = "Excel file (.xlsx)|*.xlsx"
    };
    
    if (savedialog.ShowDialog() == true)
    {
        GridExcelExport excelExport = new GridExcelExport(
            pivotGrid, 
            ExcelVersion.Excel2007);
        
        // Set export mode to PivotTable
        excelExport.ExportMode = ExportModes.PivotTable;
        
        excelExport.Export(savedialog.FileName);
        
        MessageBox.Show("Exported successfully!", "Export", 
            MessageBoxButton.OK, MessageBoxImage.Information);
    }
}
```

### Export in Cell Mode

```csharp
private void ExportToCellMode()
{
    SaveFileDialog savedialog = new SaveFileDialog
    {
        AddExtension = true,
        FileName = "Sample",
        DefaultExt = "xlsx",
        Filter = "Excel file (.xlsx)|*.xlsx"
    };
    
    if (savedialog.ShowDialog() == true)
    {
        GridExcelExport excelExport = new GridExcelExport(
            pivotGrid, 
            ExcelVersion.Excel2007);
        
        // Set export mode to Cell
        excelExport.ExportMode = ExportModes.Cell;
        
        excelExport.Export(savedialog.FileName);
        
        MessageBox.Show("Exported successfully!", "Export", 
            MessageBoxButton.OK, MessageBoxImage.Information);
    }
}
```

### Excel Version Support

```csharp
// Excel 2007 and later (.xlsx)
GridExcelExport excelExport = new GridExcelExport(
    pivotGrid, ExcelVersion.Excel2007);

// Excel 2003 (.xls)
GridExcelExport excelExport = new GridExcelExport(
    pivotGrid, ExcelVersion.Excel2003);

// Excel 2010
GridExcelExport excelExport = new GridExcelExport(
    pivotGrid, ExcelVersion.Excel2010);

// Excel 2013
GridExcelExport excelExport = new GridExcelExport(
    pivotGrid, ExcelVersion.Excel2013);
```

## Export to Word

Export PivotGrid content to Microsoft Word documents.

### Word Export Implementation

```csharp
private void Button_Export(object sender, RoutedEventArgs e)
{
    // Export to Word document
    SaveFileDialog savedialog = new SaveFileDialog();
    savedialog.AddExtension = true;
    savedialog.FileName = "Sample";
    savedialog.DefaultExt = "Doc";
    savedialog.Filter = "Word file (.Doc)|*.Doc";
    
    if (savedialog.ShowDialog() == true)
    {
        string fileName = savedialog.FileName;
        GridWordExport wordExport = new GridWordExport(pivotGrid);
        wordExport.Export(fileName);
        
        MessageBox.Show("Exported successfully!", "Export", 
            MessageBoxButton.OK, MessageBoxImage.Information);
    }
}
```

### Complete Word Export Example

```csharp
public partial class MainWindow : Window
{
    private PivotGridControl pivotGrid;
    
    public MainWindow()
    {
        InitializeComponent();
        ConfigurePivotGrid();
    }
    
    private void ConfigurePivotGrid()
    {
        pivotGrid = new PivotGridControl();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
    }
    
    private void ExportToWord_Click(object sender, RoutedEventArgs e)
    {
        SaveFileDialog savedialog = new SaveFileDialog
        {
            AddExtension = true,
            FileName = "PivotGridReport",
            DefaultExt = "Doc",
            Filter = "Word file (.Doc)|*.Doc|Word file (.Docx)|*.Docx"
        };
        
        if (savedialog.ShowDialog() == true)
        {
            try
            {
                GridWordExport wordExport = new GridWordExport(pivotGrid);
                wordExport.Export(savedialog.FileName);
                
                MessageBox.Show("Word export completed successfully!", 
                    "Export", MessageBoxButton.OK, 
                    MessageBoxImage.Information);
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Export failed: {ex.Message}", 
                    "Error", MessageBoxButton.OK, 
                    MessageBoxImage.Error);
            }
        }
    }
}
```

## Export to PDF

Export PivotGrid content to PDF documents.

### PDF Export Implementation

```csharp
private void Button_Export(object sender, RoutedEventArgs e)
{
    // Export to PDF document
    SaveFileDialog savedialog = new SaveFileDialog();
    savedialog.AddExtension = true;
    savedialog.FileName = "Sample";
    savedialog.DefaultExt = "pdf";
    savedialog.Filter = "Pdf file (.pdf)|*.pdf";
    
    if (savedialog.ShowDialog() == true)
    {
        string fileName = savedialog.FileName;
        GridPdfExport pdfExport = new GridPdfExport(pivotGrid);
        pdfExport.Export(fileName);
        
        MessageBox.Show("Exported successfully!", "Export", 
            MessageBoxButton.OK, MessageBoxImage.Information);
    }
}
```

### Complete PDF Export Example

```csharp
private void ExportToPdf_Click(object sender, RoutedEventArgs e)
{
    SaveFileDialog savedialog = new SaveFileDialog
    {
        AddExtension = true,
        FileName = "PivotGridReport",
        DefaultExt = "pdf",
        Filter = "PDF file (.pdf)|*.pdf"
    };
    
    if (savedialog.ShowDialog() == true)
    {
        try
        {
            GridPdfExport pdfExport = new GridPdfExport(pivotGrid);
            pdfExport.Export(savedialog.FileName);
            
            MessageBox.Show("PDF export completed successfully!", 
                "Export", MessageBoxButton.OK, 
                MessageBoxImage.Information);
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}", 
                "Error", MessageBoxButton.OK, 
                MessageBoxImage.Error);
        }
    }
}
```

## Export to CSV

Export PivotGrid content to CSV (Comma Separated Values) format.

### CSV Export Implementation

```csharp
private void Button_Export(object sender, RoutedEventArgs e)
{
    // Export to CSV document
    SaveFileDialog savedialog = new SaveFileDialog();
    savedialog.AddExtension = true;
    savedialog.FileName = "Sample";
    savedialog.DefaultExt = "CSV";
    savedialog.Filter = "CSV file (.csv)|*.csv";
    
    if (savedialog.ShowDialog() == true)
    {
        string fileName = savedialog.FileName;
        GridCsvExport csvExport = new GridCsvExport(pivotGrid);
        csvExport.Delimiter = ",";
        csvExport.Export(fileName);
        
        MessageBox.Show("Exported successfully!", "Export", 
            MessageBoxButton.OK, MessageBoxImage.Information);
    }
}
```

### CSV Export with Custom Delimiter

```csharp
private void ExportToCsv_Click(object sender, RoutedEventArgs e)
{
    SaveFileDialog savedialog = new SaveFileDialog
    {
        AddExtension = true,
        FileName = "PivotGridData",
        DefaultExt = "csv",
        Filter = "CSV file (.csv)|*.csv|Tab-delimited (.txt)|*.txt"
    };
    
    if (savedialog.ShowDialog() == true)
    {
        try
        {
            GridCsvExport csvExport = new GridCsvExport(pivotGrid);
            
            // Set delimiter based on file extension
            if (savedialog.FileName.EndsWith(".txt"))
                csvExport.Delimiter = "\t"; // Tab delimiter
            else
                csvExport.Delimiter = ","; // Comma delimiter
            
            csvExport.Export(savedialog.FileName);
            
            MessageBox.Show("CSV export completed successfully!", 
                "Export", MessageBoxButton.OK, 
                MessageBoxImage.Information);
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}", 
                "Error", MessageBoxButton.OK, 
                MessageBoxImage.Error);
        }
    }
}
```

## Printing

The WPF PivotGrid supports printing with print preview functionality.

### Print Properties

- **PrintHeader**: Adds/removes header information
- **PrintFooter**: Adds/removes footer information

### Print Preview with Header and Footer

**XAML Configuration**:

```xml
<Window.Resources>
    <ResourceDictionary>
        <ObjectDataProvider 
            x:Key="data" 
            ObjectType="{x:Type local:ProductSales}" 
            MethodName="GetSalesData" />
        
        <!-- Header Template -->
        <DataTemplate x:Key="HeaderTemplate">
            <Grid Height="30">
                <TextBlock 
                    Text="Pivot Grid Report Header" 
                    FontSize="15" 
                    FontWeight="Bold" 
                    HorizontalAlignment="Center"/>
            </Grid>
        </DataTemplate>
        
        <!-- Footer Template -->
        <DataTemplate x:Key="FooterTemplate">
            <Grid Height="30">
                <TextBlock 
                    Text="Page Footer - Confidential" 
                    FontSize="12" 
                    FontWeight="Bold" 
                    HorizontalAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </ResourceDictionary>
</Window.Resources>

<Grid Name="grid1">
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        syncfusion:PrintSettings.PrintHeader="True" 
        syncfusion:PrintSettings.PrintFooter="True" 
        ItemSource="{Binding Source={StaticResource data}}">
        
        <!-- PivotRows, PivotColumns, PivotCalculations -->
        
    </syncfusion:PivotGridControl>
</Grid>
```

### Invoke Print Preview

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.pivotGrid.Loaded += pivotGrid_Loaded;
    }

    void pivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Option 1: Show print preview with custom header, footer, and title
        this.pivotGrid.ShowPrintPreview(
            (DataTemplate)this.Resources["HeaderTemplate"], 
            (DataTemplate)this.Resources["FooterTemplate"], 
            "Printing PivotGrid", 
            this);

        // Option 2: Show print preview with empty template and default title
        this.pivotGrid.ShowPrintPreview(this);

        // Option 3: Show print preview with templates and default title
        this.pivotGrid.ShowPrintPreview(
            (DataTemplate)this.Resources["HeaderTemplate"], 
            (DataTemplate)this.Resources["FooterTemplate"], 
            this);
    }
}
```

### Print Preview Options

The print preview window provides:

- **Zooming**: Magnify view at 50%, 100%, 200%, or 400%
- **Page Settings**: Configure page layout and orientation
- **Print**: Send document to printer

### Print Button Implementation

```csharp
private void PrintButton_Click(object sender, RoutedEventArgs e)
{
    DataTemplate headerTemplate = (DataTemplate)this.Resources["HeaderTemplate"];
    DataTemplate footerTemplate = (DataTemplate)this.Resources["FooterTemplate"];
    
    pivotGrid.ShowPrintPreview(
        headerTemplate, 
        footerTemplate, 
        "Sales Report - " + DateTime.Now.ToString("yyyy-MM-dd"), 
        this);
}
```

### Custom Print Configuration

```csharp
private void ConfigureAndPrint()
{
    // Create custom header
    DataTemplate headerTemplate = CreateHeaderTemplate();
    
    // Create custom footer
    DataTemplate footerTemplate = CreateFooterTemplate();
    
    // Show print preview
    pivotGrid.ShowPrintPreview(
        headerTemplate, 
        footerTemplate, 
        $"Report - {DateTime.Now:yyyy-MM-dd HH:mm}", 
        this);
}

private DataTemplate CreateHeaderTemplate()
{
    string xaml = @"
        <DataTemplate xmlns='http://schemas.microsoft.com/winfx/2006/xaml/presentation'>
            <Grid>
                <TextBlock Text='Company Name' FontSize='16' FontWeight='Bold'/>
            </Grid>
        </DataTemplate>";
    
    return (DataTemplate)XamlReader.Parse(xaml);
}

private DataTemplate CreateFooterTemplate()
{
    string xaml = @"
        <DataTemplate xmlns='http://schemas.microsoft.com/winfx/2006/xaml/presentation'>
            <Grid>
                <TextBlock Text='Confidential' FontSize='10'/>
            </Grid>
        </DataTemplate>";
    
    return (DataTemplate)XamlReader.Parse(xaml);
}
```

## Export Configuration Options

### Unified Export Method

```csharp
public enum ExportFormat
{
    Excel,
    ExcelPivotTable,
    Word,
    PDF,
    CSV
}

public void ExportPivotGrid(ExportFormat format)
{
    SaveFileDialog savedialog = new SaveFileDialog
    {
        AddExtension = true,
        FileName = "PivotGridExport"
    };
    
    switch (format)
    {
        case ExportFormat.Excel:
            savedialog.DefaultExt = "xlsx";
            savedialog.Filter = "Excel file (.xlsx)|*.xlsx";
            if (savedialog.ShowDialog() == true)
            {
                var excelExport = new GridExcelExport(pivotGrid, ExcelVersion.Excel2007);
                excelExport.ExportMode = ExportModes.Cell;
                excelExport.Export(savedialog.FileName);
            }
            break;
            
        case ExportFormat.ExcelPivotTable:
            savedialog.DefaultExt = "xlsx";
            savedialog.Filter = "Excel file (.xlsx)|*.xlsx";
            if (savedialog.ShowDialog() == true)
            {
                var excelExport = new GridExcelExport(pivotGrid, ExcelVersion.Excel2007);
                excelExport.ExportMode = ExportModes.PivotTable;
                excelExport.Export(savedialog.FileName);
            }
            break;
            
        case ExportFormat.Word:
            savedialog.DefaultExt = "Doc";
            savedialog.Filter = "Word file (.Doc)|*.Doc";
            if (savedialog.ShowDialog() == true)
            {
                var wordExport = new GridWordExport(pivotGrid);
                wordExport.Export(savedialog.FileName);
            }
            break;
            
        case ExportFormat.PDF:
            savedialog.DefaultExt = "pdf";
            savedialog.Filter = "PDF file (.pdf)|*.pdf";
            if (savedialog.ShowDialog() == true)
            {
                var pdfExport = new GridPdfExport(pivotGrid);
                pdfExport.Export(savedialog.FileName);
            }
            break;
            
        case ExportFormat.CSV:
            savedialog.DefaultExt = "csv";
            savedialog.Filter = "CSV file (.csv)|*.csv";
            if (savedialog.ShowDialog() == true)
            {
                var csvExport = new GridCsvExport(pivotGrid);
                csvExport.Delimiter = ",";
                csvExport.Export(savedialog.FileName);
            }
            break;
    }
}
```

## Performance Thresholds

Export performance thresholds for optimal functionality:

| Export Format | Row Count Threshold | Column Count Threshold |
|--------------|---------------------|------------------------|
| Excel (Cell Mode) | 300 | 70 |
| Excel (PivotTable Mode) | No limit | No limit |
| Word | 1,800 | 70 |
| PDF | 800 | 70 |
| CSV | No limit | No limit |

### Handling Large Exports

```csharp
private void ExportWithProgressIndicator(ExportFormat format)
{
    var rowCount = pivotGrid.PivotEngine.RowCount;
    var colCount = pivotGrid.PivotEngine.ColumnCount;
    
    // Check thresholds
    bool isLarge = false;
    switch (format)
    {
        case ExportFormat.Excel:
            isLarge = rowCount > 300 || colCount > 70;
            break;
        case ExportFormat.Word:
            isLarge = rowCount > 1800 || colCount > 70;
            break;
        case ExportFormat.PDF:
            isLarge = rowCount > 800 || colCount > 70;
            break;
    }
    
    if (isLarge)
    {
        MessageBox.Show(
            "Large dataset detected. Export may take longer.", 
            "Export Notice", 
            MessageBoxButton.OK, 
            MessageBoxImage.Information);
    }
    
    // Perform export
    ExportPivotGrid(format);
}
```

## Best Practices

### Export Strategy Selection

1. **Excel PivotTable Mode**: Best for maintaining pivot functionality in Excel
2. **Excel Cell Mode**: Use when static data export is sufficient
3. **PDF**: Ideal for read-only distribution
4. **Word**: Good for reports requiring further editing
5. **CSV**: Best for data exchange with other systems

### Error Handling

```csharp
private void SafeExport(ExportFormat format)
{
    try
    {
        ExportPivotGrid(format);
        MessageBox.Show("Export completed successfully!", 
            "Success", MessageBoxButton.OK, 
            MessageBoxImage.Information);
    }
    catch (UnauthorizedAccessException)
    {
        MessageBox.Show("File is open or access denied. Close the file and try again.", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
    catch (IOException ex)
    {
        MessageBox.Show($"IO Error: {ex.Message}", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Export failed: {ex.Message}", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
}
```

### User Experience Enhancements

```csharp
private async Task ExportAsync(ExportFormat format)
{
    var progressWindow = new ProgressWindow();
    progressWindow.Show();
    
    try
    {
        await Task.Run(() => ExportPivotGrid(format));
        MessageBox.Show("Export completed!", "Success");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Export failed: {ex.Message}", "Error");
    }
    finally
    {
        progressWindow.Close();
    }
}
```

### Print Optimization

1. **Use Print Preview**: Allow users to review before printing
2. **Custom Headers/Footers**: Add branding and context
3. **Page Breaks**: Configure for large reports
4. **Print Settings**: Provide orientation and margin controls

## Summary

Exporting and printing in the WPF PivotGrid provides:

- **Excel Export**: Cell mode and PivotTable mode support
- **Word Export**: For editable document generation
- **PDF Export**: For read-only distribution
- **CSV Export**: For data exchange with configurable delimiters
- **Print Functionality**: Print preview with custom headers and footers
- **Performance Thresholds**: Guidelines for optimal export performance
- **Error Handling**: Robust exception management
- **Best Practices**: Format selection and user experience guidelines

Choose the appropriate export format based on your requirements, data size, and target audience to provide flexible data output options for your PivotGrid applications.
