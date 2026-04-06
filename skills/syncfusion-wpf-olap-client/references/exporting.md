# Exporting in WPF OLAP Client

## Table of Contents
- [Overview](#overview)
- [Exporting OLAP Chart](#exporting-olap-chart)
  - [Through Toolbar Menu](#through-olap-chart-toolbar-menu)
  - [Through API - Chart Export](#through-api---chart-export)
- [Exporting OLAP Grid](#exporting-olap-grid)
  - [Through Toolbar Menu](#through-olap-grid-toolbar-menu)
  - [Through API - Grid Export](#through-api---grid-export)
- [Export Format Comparison](#export-format-comparison)
- [Best Practices](#best-practices)

## Overview

The OLAP Client provides comprehensive export capabilities for both OLAP Chart and OLAP Grid components. When you create an OLAP report, you can export the current view to various formats for sharing, archiving, or further analysis.

**Supported Export Methods:**
1. **Through Toolbar**: Quick export via toolbar icons
2. **Through API**: Programmatic export with custom dialogs and options

## Exporting OLAP Chart

The OLAP Chart can be exported to Microsoft Word, PDF, and image formats, as well as printed directly.

### Through OLAP Chart Toolbar Menu

Click the respective icons in the OLAP Chart toolbar to export to the corresponding format.

#### Available Export Options

| Name | Description | Format |
|------|-------------|--------|
| Export to Image | Export chart as image file | PNG, BMP, JPEG, GIF, TIFF, WDP |
| Export to Word | Export chart to Word document | .DOC |
| Export to PDF | Export chart to PDF document | .PDF |
| Print | Print the current chart view | N/A (Print) |

### Through API - Chart Export

Perform chart exports programmatically with full control over file naming, location, and format.

#### Word Export

```csharp
SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapChart Report";
saveFileDialog.AddExtension = true;
saveFileDialog.DefaultExt = "doc";
saveFileDialog.Filter = "Word (.doc)|*.doc";

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    OlapChartWordExport olapChartWordExport = new OlapChartWordExport(this.olapChart);
    olapChartWordExport.ExportintoNewDoc(fileName);
}
```

**VB.NET:**

```vbnet
Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapChart Report"
saveFileDialog.AddExtension = True
saveFileDialog.DefaultExt = "doc"
saveFileDialog.Filter = "Word (.doc)|*.doc"

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim olapChartWordExport As New OlapChartWordExport(Me.olapChart)
    olapChartWordExport.ExportintoNewDoc(fileName)
End If
```

#### PDF Export

```csharp
SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapChart Report";
saveFileDialog.AddExtension = true;
saveFileDialog.DefaultExt = "pdf";
saveFileDialog.Filter = "PDF (.pdf)|*.pdf";

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    OlapChartPdfExport chartPdfExport = new OlapChartPdfExport(this.olapChart);
    chartPdfExport.ExportIntoNewPdf(fileName);
}
```

**VB.NET:**

```vbnet
Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapChart Report"
saveFileDialog.AddExtension = True
saveFileDialog.DefaultExt = "pdf"
saveFileDialog.Filter = "PDF (.pdf)|*.pdf"

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim olapChartPdfExport As New OlapChartPdfExport(Me.olapChart)
    olapChartPdfExport.ExportIntoNewPdf(fileName)
End If
```

#### Image Export

```csharp
private const string c_imageFilesFilter = "Bitmap(*.bmp)|*.bmp|JPEG(*.jpg,*.jpeg)|*.jpg;*.jpeg|GIF(*.gif)|*.gif|TIFF(*.tiff)|*.tiff|PNG(*.png)|*.png|WDP(*.wdp)|*.wdp|All files (*.*)|*.*";

SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapChart Report";
saveFileDialog.Filter = c_imageFilesFilter;

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    string extension = new FileInfo(fileName).Extension.ToLower(CultureInfo.InvariantCulture);
    
    if (c_imageFilesFilter.Contains(extension))
    {
        using (Stream stream = File.Create(saveFileDialog.FileName))
        {
            BitmapEncoder encoder = Chart.CreateBitmapEncoderByExtension(extension);
            RenderTargetBitmap bmpSource = new RenderTargetBitmap(
                (int)chart.ActualWidth, 
                (int)chart.ActualHeight, 
                96, 96, 
                PixelFormats.Default
            );
            
            // White background
            Rectangle backgroundRect = new Rectangle();
            backgroundRect.Fill = Brushes.White;
            backgroundRect.Arrange(new Rect(chart.RenderSize));
            bmpSource.Render(backgroundRect);
            bmpSource.Render(visual);
            
            encoder.Frames.Add(BitmapFrame.Create(bmpSource));
            encoder.Save(stream);
        }
    }
}
```

**VB.NET:**

```vbnet
Dim c_imageFilesFilter As String = "Bitmap(*.bmp)|*.bmp|JPEG(*.jpg,*.jpeg)|*.jpg;*.jpeg|GIF(*.gif)|*.gif|TIFF(*.tiff)|*.tiff|PNG(*.png)|*.png|WDP(*.wdp)|*.wdp|All files (*.*)|*.*"

Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapChart Report"
saveFileDialog.Filter = c_imageFilesFilter

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim extension As String = New FileInfo(fileName).Extension.ToLower(CultureInfo.InvariantCulture)
    
    If c_imageFilesFilter.Contains(extension) Then
        Using stream As Stream = File.Create(saveFileDialog.FileName)
            Dim encoder As BitmapEncoder = Chart.CreateBitmapEncoderByExtension(extension)
            Dim bmpSource As New RenderTargetBitmap(
                CInt(chart.ActualWidth), 
                CInt(chart.ActualHeight), 
                96, 96, 
                PixelFormats.[Default]
            )
            
            ' White background
            Dim backgroundRect As New Rectangle()
            backgroundRect.Fill = Brushes.White
            backgroundRect.Arrange(New Rect(chart.RenderSize))
            bmpSource.Render(backgroundRect)
            bmpSource.Render(visual)
            
            encoder.Frames.Add(BitmapFrame.Create(bmpSource))
            encoder.Save(stream)
        End Using
    End If
End If
```

## Exporting OLAP Grid

The OLAP Grid can be exported to Microsoft Word, PDF, Microsoft Excel, and CSV formats.

### Through OLAP Grid Toolbar Menu

Click the respective export buttons in the OLAP Grid toolbar.

#### Available Export Options

| Name | Description | Format |
|------|-------------|--------|
| Export to Excel | Export grid to Excel document | .XLSX, .XLS |
| Export to Word | Export grid to Word document | .DOC |
| Export to PDF | Export grid to PDF document | .PDF |
| Export to CSV | Export grid to CSV document | .CSV |

### Through API - Grid Export

Perform grid exports programmatically with full control over file options.

#### Excel Export

```csharp
SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapGrid Report";
saveFileDialog.AddExtension = true;
saveFileDialog.DefaultExt = "xlsx";
saveFileDialog.Filter = "Excel (.xlsx)|*.xlsx";

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    GridExcelExport excelExport = new GridExcelExport(
        this.olapGrid.InternalGrid.Engine, 
        this.olapGrid.GridStyleInfo, 
        this.olapGrid.InternalGrid.Layout, 
        saveFileDialog.DefaultExt, 
        this.olapGrid.OlapDataManager.ItemSource == null ? false : true
    );
    excelExport.Export(fileName);
}
```

**VB.NET:**

```vbnet
Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapGrid Report"
saveFileDialog.AddExtension = True
saveFileDialog.DefaultExt = "xlsx"
saveFileDialog.Filter = "Excel (.xlsx)|*.xlsx"

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim excelExport As New GridExcelExport(
        Me.olapGrid.InternalGrid.Engine, 
        Me.olapGrid.GridStyleInfo, 
        Me.olapGrid.InternalGrid.Layout, 
        saveFileDialog.DefaultExt, 
        If(Me.olapGrid.OlapDataManager.ItemSource Is Nothing, False, True)
    )
    excelExport.Export(fileName)
End If
```

#### Word Export

```csharp
SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapGrid Report";
saveFileDialog.AddExtension = true;
saveFileDialog.DefaultExt = "doc";
saveFileDialog.Filter = "Word (.doc)|*.doc";

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    GridWordExport gridWordExport = new GridWordExport(
        this.olapGrid.InternalGrid.Engine, 
        this.olapGrid.InternalGrid.Layout
    );
    gridWordExport.Export(fileName, this.olapGrid.GridStyleInfo);
}
```

**VB.NET:**

```vbnet
Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapGrid Report"
saveFileDialog.AddExtension = True
saveFileDialog.DefaultExt = "doc"
saveFileDialog.Filter = "Word (.doc)|*.doc"

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim gridWordExport As New GridWordExport(
        Me.olapGrid.InternalGrid.Engine, 
        Me.olapGrid.InternalGrid.Layout
    )
    gridWordExport.Export(fileName, Me.olapGrid.GridStyleInfo)
End If
```

#### PDF Export

```csharp
SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapGrid Report";
saveFileDialog.AddExtension = true;
saveFileDialog.DefaultExt = "pdf";
saveFileDialog.Filter = "PDF (.pdf)|*.pdf";

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    GridPdfExport gridPdfExport = new GridPdfExport(
        this.olapGrid.InternalGrid.Engine, 
        this.olapGrid.GridStyleInfo
    );
    gridPdfExport.Export(fileName);
}
```

**VB.NET:**

```vbnet
Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapGrid Report"
saveFileDialog.AddExtension = True
saveFileDialog.DefaultExt = "pdf"
saveFileDialog.Filter = "PDF (.pdf)|*.pdf"

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim gridPdfExport As New GridPdfExport(
        Me.olapGrid.InternalGrid.Engine, 
        Me.olapGrid.GridStyleInfo
    )
    gridPdfExport.Export(fileName)
End If
```

#### CSV Export

```csharp
SaveFileDialog saveFileDialog = new SaveFileDialog();
saveFileDialog.FileName = "OlapGrid Report";
saveFileDialog.AddExtension = true;
saveFileDialog.DefaultExt = "csv";
saveFileDialog.Filter = "CSV (.csv)|*.csv";

if (saveFileDialog.ShowDialog() == true)
{
    string fileName = saveFileDialog.FileName;
    GridCsvExport gridCsvExport = new GridCsvExport(
        this.olapGrid.InternalGrid.Engine
    );
    gridCsvExport.Export(fileName);
}
```

**VB.NET:**

```vbnet
Dim saveFileDialog As New SaveFileDialog()
saveFileDialog.FileName = "OlapGrid Report"
saveFileDialog.AddExtension = True
saveFileDialog.DefaultExt = "csv"
saveFileDialog.Filter = "CSV (.csv)|*.csv"

If saveFileDialog.ShowDialog() = True Then
    Dim fileName As String = saveFileDialog.FileName
    Dim gridCsvExport As New GridCsvExport(
        Me.olapGrid.InternalGrid.Engine
    )
    gridCsvExport.Export(fileName)
End If
```

## Export Format Comparison

### OLAP Chart Export Formats

| Format | Use Case | Advantages | Limitations |
|--------|----------|------------|-------------|
| **PDF** | Reports, presentations, archiving | Universal format, preserves layout, professional | Static, not editable |
| **Word** | Editable documents, reports | Editable, can add annotations | Larger file size |
| **Image** (PNG, JPEG, etc.) | Presentations, web, email | Small file size, universal compatibility | Static, lossy compression (JPEG) |
| **Print** | Physical copies | Immediate hard copy | Requires printer |

### OLAP Grid Export Formats

| Format | Use Case | Advantages | Limitations |
|--------|----------|------------|-------------|
| **Excel** | Data analysis, calculations | Editable, supports formulas, widely used | Requires Excel or compatible software |
| **CSV** | Data import, interoperability | Simple, universal, small file size | No formatting, plain text only |
| **PDF** | Reports, archiving, sharing | Universal, preserves layout | Not editable |
| **Word** | Documentation, reports | Editable, can add text | Not optimized for tabular data |

## Best Practices

### Choosing the Right Export Format

**For Data Analysis:**
- Use **Excel** for OLAP Grid (supports further calculations)
- Use **CSV** for importing into other systems

**For Presentations:**
- Use **PDF** for both Chart and Grid (professional, consistent)
- Use **Image (PNG)** for Chart in PowerPoint slides

**For Documentation:**
- Use **Word** for both Chart and Grid (allows annotations)
- Combine Chart images with Grid tables in single document

**For Archiving:**
- Use **PDF** (long-term readability, preserves formatting)

### File Naming Conventions

Use descriptive, timestamped file names:

```csharp
// Good file naming pattern
string timestamp = DateTime.Now.ToString("yyyy-MM-dd_HHmmss");
string fileName = $"SalesReport_{timestamp}.pdf";

// Include report details in name
string fileName = $"SalesAnalysis_Q4_2025_{region}.xlsx";
```

### Programmatic Export Benefits

**Advantages of API-based export:**
1. **Batch Export**: Export multiple views automatically
2. **Custom File Names**: Dynamic naming based on report criteria
3. **Scheduled Exports**: Automate report generation
4. **Custom Dialogs**: Provide user-friendly export interfaces
5. **Error Handling**: Catch and handle export errors gracefully

### Combining Export with Filters

Export filtered/sorted views:

1. **Apply Filters**: Member, value, or subset filters
2. **Apply Sorting**: Order by relevant measure
3. **Export**: Save the refined view
4. **Result**: Focused, relevant exported reports

### Performance Considerations

**Large Datasets:**
- Apply filters before exporting to reduce data volume
- Use paging for very large grids
- Consider CSV for fastest export of large tabular data

**Chart Export:**
- Higher resolution images = larger file sizes
- PDF preserves vector quality for scaling
- Consider target use case (screen vs print)

### Error Handling

Implement proper error handling for exports:

```csharp
try
{
    if (saveFileDialog.ShowDialog() == true)
    {
        string fileName = saveFileDialog.FileName;
        GridExcelExport excelExport = new GridExcelExport(
            this.olapGrid.InternalGrid.Engine,
            this.olapGrid.GridStyleInfo,
            this.olapGrid.InternalGrid.Layout,
            "xlsx",
            false
        );
        excelExport.Export(fileName);
        
        MessageBox.Show("Export completed successfully!", "Success", 
                        MessageBoxButton.OK, MessageBoxImage.Information);
    }
}
catch (Exception ex)
{
    MessageBox.Show($"Export failed: {ex.Message}", "Error", 
                    MessageBoxButton.OK, MessageBoxImage.Error);
}
```

## Common Export Scenarios

### Scenario 1: Executive Dashboard Export

**Goal:** Export chart and grid as PDF for executive review

**Steps:**
1. Configure OLAP Client with desired view
2. Export Chart to PDF: `ExecutiveDashboard_Chart.pdf`
3. Export Grid to PDF: `ExecutiveDashboard_Grid.pdf`
4. Combine PDFs or send separately

### Scenario 2: Data Analysis Handoff

**Goal:** Export grid data for further analysis in Excel

**Steps:**
1. Apply filters to focus on relevant data
2. Sort by key measure
3. Export to Excel (.xlsx)
4. Analysts can perform additional calculations

### Scenario 3: Automated Weekly Reports

**Goal:** Automate weekly report generation

**Code Pattern:**
```csharp
public void GenerateWeeklyReport()
{
    // Configure report
    _olapDataManager.SetCurrentReport(BuildWeeklyReport());
    this.olapClient.DataBind();
    
    // Generate file name
    string weekNumber = GetCurrentWeekNumber();
    string pdfName = $"WeeklyReport_Week{weekNumber}_2025.pdf";
    
    // Export Grid
    GridPdfExport gridPdfExport = new GridPdfExport(
        this.olapGrid.InternalGrid.Engine,
        this.olapGrid.GridStyleInfo
    );
    gridPdfExport.Export(pdfName);
    
    // Email or save to shared location
    EmailReport(pdfName);
}
```

### Scenario 4: Presentation Preparation

**Goal:** Export chart images for PowerPoint presentation

**Steps:**
1. Configure chart with desired dimensions
2. Export as PNG (high resolution)
3. Insert into PowerPoint slides
4. Maintain visual consistency across slides

## Troubleshooting

### Export Fails

**Possible Causes:**
- File in use by another program
- Insufficient disk space
- Invalid file path
- Missing export assemblies

**Solutions:**
- Close files before exporting
- Check disk space
- Verify file path is valid
- Ensure all required Syncfusion assemblies are referenced

### Poor Export Quality

**Possible Causes:**
- Low resolution for images
- Chart size too small
- Formatting lost in export

**Solutions:**
- Increase chart dimensions before export
- Use vector formats (PDF) for scalability
- Test different export formats

### Large Export Files

**Possible Causes:**
- High resolution images
- Large datasets
- Uncompressed formats

**Solutions:**
- Apply filters to reduce data
- Use compressed formats (JPEG for charts, CSV for grids)
- Consider pagination for very large exports

## Next Steps

After mastering export:

1. Learn about report management - see [report-management.md](report-management.md)
2. Explore drill-through for detailed analysis - see [drill-through.md](drill-through.md)
3. Understand paging for large datasets - see [paging.md](paging.md)
