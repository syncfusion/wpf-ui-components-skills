# Paging in WPF OLAP Client

## Overview

Paging in the OLAP Client allows you to load and render large amounts of multidimensional data without performance constraints. By splitting large cell sets into manageable segments displayed across multiple pages, paging ensures smooth interaction even with massive datasets.

## How Paging Works

When processing large cell sets:

1. Data is **split into several segments**
2. Each segment is assigned to a **separate page**
3. Users navigate between pages via the **OLAP Pager** control
4. Only the current page's data is rendered, reducing memory and rendering overhead

## OLAP Pager Control

The OLAP Pager is a user control that binds with the `OlapDataManager` object to provide paging functionality.

### Required Assembly

Include this Syncfusion assembly for OLAP Pager:

- **Syncfusion.OlapShared.Wpf**

**Default Location:**  
`{System Drive}:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<version>\<framework version>\`

## Enabling Paging

### Method 1: Enable Paging Through XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="SampleApplication.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <GroupBox Header="OlapClient" Grid.Row="0">
            <syncfusion:OlapClient Name="olapClient" 
                                   EnablePaging="True" 
                                   Background="Transparent" 
                                   SeriesStrokeThickness="0">
            </syncfusion:OlapClient>
        </GroupBox>
    </Grid>
</Window>
```

**Key Property:**
- `EnablePaging="True"` - Activates paging functionality

### Method 2: Enable Paging Through OlapReport

Configure paging programmatically through the `OlapReport` object:

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;

namespace SampleApplication
{
    public partial class MainWindow : Window
    {
        private string _connectionString;
        private OlapDataManager _olapDataManager;
        
        public MainWindow()
        {
            InitializeComponent();
            
            _connectionString = "Enter a valid connection string";
            
            // Create OlapDataManager with connection string
            _olapDataManager = new OlapDataManager(_connectionString);
            
            // Set current report with paging enabled
            _olapDataManager.SetCurrentReport(SimpleDimensions());
            
            // Bind OlapClient to OlapDataManager
            this.olapClient.OlapDataManager = _olapDataManager;
            this.olapClient.DataBind();
        }
        
        private OlapReport SimpleDimensions()
        {
            OlapReport olapReport = new OlapReport();
            olapReport.CurrentCubeName = "Adventure Works";
            
            // Enable paging in the report
            olapReport.EnablePaging = true;
            
            // Configure page sizes
            olapReport.PagerOptions.CategorialPageSize = 10;
            olapReport.PagerOptions.SeriesPageSize = 10;
            
            // Add dimension elements
            DimensionElement dimensionElement = new DimensionElement() 
            { 
                Name = "Customer", 
                HierarchyName = "Customer" 
            };
            dimensionElement.AddLevel("Customer Geography", "Country");
            olapReport.CategoricalElements.Add(dimensionElement);
            
            // Add measure elements
            MeasureElements measureElements = new MeasureElements();
            measureElements.Add(new MeasureElement { Name = "Internet Sales Amount" });
            olapReport.SeriesElements.Add(measureElements);
            
            // Add more dimensions
            dimensionElement = new DimensionElement() 
            { 
                Name = "Geography", 
                HierarchyName = "Geography" 
            };
            dimensionElement.AddLevel("Geography", "Country");
            olapReport.CategoricalElements.Add(dimensionElement);
            
            dimensionElement = new DimensionElement() { Name = "Date" };
            dimensionElement.AddLevel("Fiscal", "Fiscal Year");
            olapReport.SeriesElements.Add(dimensionElement);
            
            return olapReport;
        }
    }
}
```

**VB.NET:**

```vbnet
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Olap.Reports

Namespace SampleApplication
    Partial Public Class MainWindow
        Inherits Window
        
        Private _connectionString As String
        Private _olapDataManager As OlapDataManager
        
        Public Sub New()
            InitializeComponent()
            
            _connectionString = "Enter a valid connection string"
            
            ' Create OlapDataManager with connection string
            _olapDataManager = New OlapDataManager(_connectionString)
            
            ' Set current report with paging enabled
            _olapDataManager.SetCurrentReport(SimpleDimensions())
            
            ' Bind OlapClient to OlapDataManager
            Me.olapClient.OlapDataManager = _olapDataManager
            Me.olapClient.DataBind()
        End Sub
        
        Private Function SimpleDimensions() As OlapReport
            Dim olapReport As New OlapReport()
            olapReport.CurrentCubeName = "Adventure Works"
            
            ' Enable paging in the report
            olapReport.EnablePaging = True
            
            ' Configure page sizes
            olapReport.PagerOptions.CategorialPageSize = 10
            olapReport.PagerOptions.SeriesPageSize = 10
            
            ' Add dimension elements
            Dim dimensionElement As New DimensionElement() With {
                .Name = "Customer",
                .HierarchyName = "Customer"
            }
            dimensionElement.AddLevel("Customer Geography", "Country")
            olapReport.CategoricalElements.Add(dimensionElement)
            
            ' Add measure elements
            Dim measureElements As New MeasureElements()
            measureElements.Add(New MeasureElement With {.Name = "Internet Sales Amount"})
            olapReport.SeriesElements.Add(measureElements)
            
            ' Add more dimensions
            dimensionElement = New DimensionElement() With {
                .Name = "Geography",
                .HierarchyName = "Geography"
            }
            dimensionElement.AddLevel("Geography", "Country")
            olapReport.CategoricalElements.Add(dimensionElement)
            
            dimensionElement = New DimensionElement() With {.Name = "Date"}
            dimensionElement.AddLevel("Fiscal", "Fiscal Year")
            olapReport.SeriesElements.Add(dimensionElement)
            
            Return olapReport
        End Function
    End Class
End Namespace
```

## Paging Configuration

### Key Properties

**OlapReport.EnablePaging**
- Type: `bool`
- Purpose: Enable or disable paging
- Default: `false`

**OlapReport.PagerOptions.CategorialPageSize**
- Type: `int`
- Purpose: Number of categorical (column) items per page
- Example: `10` = 10 columns per page

**OlapReport.PagerOptions.SeriesPageSize**
- Type: `int`
- Purpose: Number of series (row) items per page
- Example: `10` = 10 rows per page

## OlapPager UI

When paging is enabled, the OLAP Pager control appears, providing navigation options.

### Pager Features

- **Page Navigation**: First, Previous, Next, Last page buttons
- **Current Page Indicator**: Shows current page number and total pages
- **Direct Page Access**: Jump to specific page number
- **Page Settings**: Open Page Settings window

## Page Settings Window

The Page Settings window allows runtime configuration of paging options.

### Configurable Options

- **Categorical Page Size**: Change number of columns per page
- **Series Page Size**: Change number of rows per page
- **Page Mode**: Different paging strategies (if applicable)

### Access Page Settings

Click the **Settings** icon/button in the OLAP Pager control to open the Page Settings window.

## Use Cases

### Large Cube Analysis

**Scenario:** Analyzing a cube with millions of cells

**Solution:**
- Enable paging with page size = 20 rows × 15 columns
- Navigate through data in manageable chunks
- Reduce memory consumption
- Maintain responsive UI

### Performance Optimization

**Scenario:** Slow rendering with large result sets

**Solution:**
- Enable paging to render smaller segments
- Configure appropriate page sizes based on screen space
- Combine with filtering to reduce total data volume

### Detailed Exploration

**Scenario:** Drilling through detailed transactional data

**Solution:**
- Use paging to explore large detail sets page by page
- Navigate forward/backward through results
- Jump to specific pages of interest

### Report Presentation

**Scenario:** Presenting reports with extensive data

**Solution:**
- Configure page size for optimal screen fit
- Print or export one page at a time
- Provide navigation for users to explore all data

## Best Practices

### Choose Appropriate Page Sizes

**Small Page Sizes (5-10 items):**
- Pros: Very fast rendering, minimal memory
- Cons: More navigation required
- Use for: Very large datasets, low-end hardware

**Medium Page Sizes (15-25 items):**
- Pros: Balanced performance and usability
- Cons: Moderate memory usage
- Use for: Most scenarios, typical datasets

**Large Page Sizes (50+ items):**
- Pros: Less navigation, more context visible
- Cons: Slower rendering, higher memory
- Use for: Smaller datasets, high-end hardware

### Balance Rows and Columns

Consider aspect ratio and readability:
- **Wide screens**: Higher CategorialPageSize (more columns)
- **Tall screens**: Higher SeriesPageSize (more rows)
- **Square displays**: Balance both sizes equally

### Combine with Filtering

Paging is most effective when combined with filtering:

1. **Apply Filters First**: Reduce total data volume
2. **Enable Paging**: Handle remaining data in pages
3. **Result**: Optimal performance with focused analysis

### Consider User Experience

- **Larger page sizes**: For users who prefer seeing more context
- **Smaller page sizes**: For users who prefer faster interaction
- **Configurable**: Allow users to adjust page sizes via Page Settings

## Performance Considerations

### Memory Usage

- **Without Paging**: All data loaded into memory simultaneously
- **With Paging**: Only current page in memory
- **Benefit**: Reduced memory footprint, especially for large cubes

### Rendering Speed

- **Without Paging**: Slow initial render for large datasets
- **With Paging**: Fast render of current page
- **Trade-off**: Small overhead for page navigation

### Query Performance

- **Impact**: Paging may require additional queries when navigating
- **Optimization**: SSAS and cube design affect query performance
- **Best Practice**: Optimize cube aggregations and indexes

## Troubleshooting

### Pager Not Appearing

**Possible Causes:**
- `EnablePaging` property not set to `true`
- Syncfusion.OlapShared.Wpf assembly not referenced
- Data volume too small to trigger paging

**Solutions:**
- Verify `EnablePaging = true` in XAML or code
- Check assembly references
- Ensure dataset exceeds page size

### Poor Paging Performance

**Possible Causes:**
- Page sizes too large
- Cube not optimized
- Network latency (online cubes)

**Solutions:**
- Reduce page sizes (CategorialPageSize, SeriesPageSize)
- Optimize cube aggregations
- Use local/offline cube for testing
- Apply filters to reduce total data volume

### Page Settings Not Saving

**Possible Causes:**
- Settings not persisted in report
- Report not saved after changes

**Solutions:**
- Save report after changing page settings
- Verify report serialization includes pager options

## Next Steps

After implementing paging:

1. Explore filtering to reduce data volume - see [filtering.md](filtering.md)
2. Learn about sorting paged data - see [sorting.md](sorting.md)
3. Combine with exporting - see [exporting.md](exporting.md)
