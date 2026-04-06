---
name: syncfusion-wpf-olap-grid
description: Implement Syncfusion WPF OLAP Grid for multi-dimensional OLAP data analysis. Use this when working with OLAP grids, multi-dimensional analysis, business intelligence grids, or SSAS data display. Covers OLAP data source connections, drill down/up operations, KPI display, dimensional analysis, and cube data visualization for WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing OLAP Grids

The Syncfusion WPF OLAP Grid is a presentation-quality business control that reads OLAP data sources to create multi-dimensional views for analyzing business data. It provides drill up/down capabilities, multiple layout options, conditional formatting, and comprehensive export features for business intelligence applications.

## When to Use This Skill

Use this skill when you need to:

- **Display OLAP data** from SSAS (SQL Server Analysis Services) or offline cubes in WPF applications
- **Create multi-dimensional analysis views** with rows, columns, and measure intersections
- **Implement drill down/up navigation** through hierarchical dimension data
- **Build business intelligence dashboards** with KPI visualization and analytical capabilities
- **Configure different grid layouts** (Normal, Excel-like, Top Summary, No Summaries)
- **Apply conditional formatting** to highlight data based on business rules
- **Export OLAP analysis** to Excel, Word, PDF, or CSV formats
- **Connect to cube data sources** and define custom OLAP reports
- **Display member properties** alongside dimension members
- **Implement paging** for large OLAP datasets

This skill covers the complete implementation lifecycle from setup and data binding to advanced features like conditional formatting, export options, and interactive drill operations.

## Component Overview

**Key Capabilities:**
- Multi-dimensional data display from OLAP cubes
- Five grid layout options for different presentation needs
- Multi-level drill up/down for rows and columns
- Conditional formatting with custom cell styles
- Export to Excel, Word, PDF, and CSV
- KPI (Key Performance Indicator) support
- Member properties display
- Paging for large datasets
- Design-time binding with query designer
- Cell selection and hyperlink support

**Core Components:**
- `OlapGrid` - Main grid control
- `OlapDataManager` - Manages OLAP data and reports
- `OlapReport` - Defines cube query (dimensions, measures, filters)
- `DimensionElement` - Represents dimension hierarchies
- `MeasureElements` - Represents measures to display

### Install the NuGet Package

To use the OLAP Grid in your WPF application, install the following NuGet package:

```powershell
Install-Package Syncfusion.OlapGrid.WPF
```

### Assembly References

The OLAP Grid requires the following assemblies:

Assembly Reference
* `Syncfusion.Grid.WPF`
* `Syncfusion.GridCommon.WPF` 
* `Syncfusion.Linq.Base`
* `Syncfusion.Olap.Base`
* `Syncfusion.OlapGridCommon.WPF`
* `Syncfusion.OlapShared.WPF`
* `Syncfusion.Shared.WPF`
* `Syncfusion.Tools.WPF`

NuGet Package
* `Syncfusion.OlapGrid.WPF`

### Important Note

**The Syncfusion OlapGrid control is supported only in .NET Framework WPF applications. It is not supported in .NET Core.** Please create a Framework-based WPF sample when working with OlapGrid.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and configure the OLAP Grid control
- Add required assemblies to WPF projects
- Understand licensing requirements
- Create basic OLAP Grid implementation (XAML, Code-behind, or Blend)
- Set up OlapDataManager and connection strings
- Use design-time binding with Query Designer
- Create your first OLAP report with dimensions and measures

### Data Source Configuration
📄 **Read:** [references/data-source-configuration.md](references/data-source-configuration.md)

When you need to:
- Configure OLAP data source connections (SSAS or offline cubes)
- Build OlapReport with dimensions and measures
- Specify cube names and connection strings
- Add categorical elements (column axis) and series elements (row axis)
- Configure dimension hierarchies and levels
- Set up measure elements
- Understand OlapDataManager workflow

### Grid Layouts
📄 **Read:** [references/grid-layouts.md](references/grid-layouts.md)

When you need to:
- Choose between five layout types (Normal, Excel-like, Top Summary, No Summaries)
- Position summary cells at top or bottom of parent members
- Display member properties with Excel-like layout
- Configure layout property in XAML or code
- Understand summary and child member positioning
- Select appropriate layout for business requirements

### Drill Operations
📄 **Read:** [references/drill-operations.md](references/drill-operations.md)

When you need to:
- Implement drill down/up navigation through hierarchies
- Configure drill position for rows and columns
- Enable multi-level drilling
- Support all level type members
- Handle drill events programmatically
- Manage drill state and navigation patterns
- Optimize drill performance

### Conditional Formatting
📄 **Read:** [references/conditional-formatting.md](references/conditional-formatting.md)

When you need to:
- Apply cell formatting based on data conditions
- Use OlapGridDataConditionalFormat class
- Define filter criteria with condition types (GreaterThan, LessThan, Equals)
- Combine conditions with predicates (And, Or)
- Customize cell styles (background, foreground, fonts, borders)
- Configure formatting in XAML or code-behind
- Apply multiple conditional formats

### Exporting
📄 **Read:** [references/exporting.md](references/exporting.md)

When you need to:
- Export OLAP Grid to Excel (ExportToExcel method)
- Export to Word documents (ExportToWord method)
- Export to PDF files (ExportToPdf method)
- Export to CSV format (GridCsvExport class)
- Preserve formatting in exported documents
- Handle file dialogs and export configuration
- Implement export error handling

### Cell Selection
📄 **Read:** [references/cell-selection.md](references/cell-selection.md)

When you need to:
- Configure cell selection modes
- Handle selection events
- Implement programmatic cell selection
- Customize selection behavior
- Support multi-cell selection
- Manage selection state

### KPI and Member Properties
📄 **Read:** [references/kpi-and-member-properties.md](references/kpi-and-member-properties.md)

When you need to:
- Display Key Performance Indicators (KPI)
- Configure KPI status and trend visualization
- Show member properties alongside dimensions
- Use Excel-like layout with member properties
- Bind member properties from cube definitions
- Configure OlapReport for KPI display

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When you need to:
- Implement paging for large OLAP datasets
- Add hyperlink functionality to cells
- Freeze row or column headers
- Customize tooltips for cells
- Use grid style dialog for formatting
- Enable RTL (Right-to-Left) support
- Configure accessibility features

### XAML Configuration
📄 **Read:** [references/xaml-configuration.md](references/xaml-configuration.md)

When you need to:
- Configure OLAP Grid declaratively in XAML
- Set properties through XAML markup
- Define conditional formats in XAML
- Manage XAML namespaces
- Use styles and templates
- Apply resource management patterns
- Follow XAML best practices

## Quick Start Example

Here's a minimal working example to create an OLAP Grid with data from an OLAP cube:

### XAML Setup

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="MyApp.MainWindow">
    <Grid>
        <syncfusion:OlapGrid Name="olapGrid1" />
    </Grid>
</Window>
```

### Code-Behind

```csharp
using Syncfusion.Windows.Grid.Olap;
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;

public partial class MainWindow : Window
{
    private OlapDataManager olapDataManager;
    private string connectionString = "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll;Initial Catalog=Adventure Works DW 2008 SE;";
    
    public MainWindow()
    {
        InitializeComponent();
        
        // Initialize OlapDataManager with connection string
        olapDataManager = new OlapDataManager(connectionString);
        
        // Create and set the OLAP report
        olapDataManager.SetCurrentReport(CreateOlapReport());
        
        // Bind to OlapGrid
        olapGrid1.OlapDataManager = olapDataManager;
        olapGrid1.DataBind();
    }
    
    private OlapReport CreateOlapReport()
    {
        OlapReport report = new OlapReport();
        
        // Set cube name
        report.CurrentCubeName = "Adventure Works";
        
        // Configure column dimension
        DimensionElement columnDimension = new DimensionElement();
        columnDimension.Name = "Customer";
        columnDimension.AddLevel("Customer Geography", "Country");
        
        // Configure measure
        MeasureElements measures = new MeasureElements();
        measures.Elements.Add(new MeasureElement { Name = "Internet Sales Amount" });
        
        // Configure row dimension
        DimensionElement rowDimension = new DimensionElement();
        rowDimension.Name = "Date";
        rowDimension.AddLevel("Fiscal", "Fiscal Year");
        
        // Add to report
        report.CategoricalElements.Add(columnDimension);
        report.CategoricalElements.Add(measures);
        report.SeriesElements.Add(rowDimension);
        
        return report;
    }
}
```

This creates a basic OLAP Grid showing Internet Sales Amount by Customer Geography (Country) and Fiscal Year.

## Common Patterns

### Pattern 1: Setting Grid Layout

```csharp
// Excel-like layout with indented child members
olapGrid1.Layout = GridLayout.ExcelLikeLayout;

// Normal layout with bottom summaries (default)
olapGrid1.Layout = GridLayout.Normal;

// Top summary layout
olapGrid1.Layout = GridLayout.NormalTopSummary;

// Layout without summaries
olapGrid1.Layout = GridLayout.NoSummaries;
```

### Pattern 2: Applying Conditional Formatting

```csharp
// Create conditional format
OlapGridDataConditionalFormat format = new OlapGridDataConditionalFormat();

// Add conditions
format.Conditions.Add(new OlapGridDataCondition() 
{ 
    ConditionType = OlapGridDataConditionType.GreaterThan,
    MeasureElement = "Internet Sales Amount",
    Value = "2000000",
    PredicateType = PredicateType.And
});

// Set cell style
format.CellStyle = new OlapGridCellStyle() 
{ 
    Background = Brushes.Yellow,
    FontFamily = new FontFamily("Calibri"),
    FontSize = 12
};

// Apply to grid
olapGrid1.ConditionalFormats.Add(format);
```

### Pattern 3: Exporting to Different Formats

```csharp
// Export to Excel
olapGrid1.ExportToExcel("SalesReport.xlsx");

// Export to Word
olapGrid1.ExportToWord("SalesReport.docx");

// Export to PDF
olapGrid1.ExportToPdf("SalesReport.pdf");

// Export to CSV
SaveFileDialog dialog = new SaveFileDialog();
dialog.Filter = "CSV file (.csv)|*.csv";
if (dialog.ShowDialog() == true)
{
    GridCsvExport csvExport = new GridCsvExport(olapGrid1.InternalGrid.Engine);
    csvExport.Export(dialog.FileName);
}
```

### Pattern 4: Multiple Dimensions and Measures

```csharp
private OlapReport CreateComplexReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";
    
    // Multiple column dimensions
    DimensionElement productDimension = new DimensionElement();
    productDimension.Name = "Product";
    productDimension.AddLevel("Product Categories", "Category");
    
    DimensionElement geographyDimension = new DimensionElement();
    geographyDimension.Name = "Customer";
    geographyDimension.AddLevel("Customer Geography", "Country");
    
    // Multiple measures
    MeasureElements measures = new MeasureElements();
    measures.Elements.Add(new MeasureElement { Name = "Internet Sales Amount" });
    measures.Elements.Add(new MeasureElement { Name = "Reseller Sales Amount" });
    
    // Row dimension
    DimensionElement dateDimension = new DimensionElement();
    dateDimension.Name = "Date";
    dateDimension.AddLevel("Fiscal", "Fiscal Year");
    
    // Build report
    report.CategoricalElements.Add(productDimension);
    report.CategoricalElements.Add(geographyDimension);
    report.CategoricalElements.Add(measures);
    report.SeriesElements.Add(dateDimension);
    
    return report;
}
```

## Key Properties and Methods

### OlapGrid Properties

- **`OlapDataManager`** - Gets or sets the OlapDataManager instance
- **`Layout`** - Gets or sets the grid layout type (GridLayout enum)
- **`ConditionalFormats`** - Collection of conditional formatting rules
- **`InternalGrid`** - Access to underlying grid engine for advanced operations

### OlapGrid Methods

- **`DataBind()`** - Binds the grid to the data source
- **`ExportToExcel(string filename)`** - Exports grid to Excel
- **`ExportToWord(string filename)`** - Exports grid to Word
- **`ExportToPdf(string filename)`** - Exports grid to PDF

### OlapDataManager

- **`SetCurrentReport(OlapReport report)`** - Sets the active OLAP report
- **Constructor accepts connection string** for OLAP data source

### OlapReport Structure

- **`CurrentCubeName`** - Name of the cube to query
- **`CategoricalElements`** - Collection for column axis (dimensions and measures)
- **`SeriesElements`** - Collection for row axis (dimensions)

## Common Use Cases

### Business Intelligence Dashboards
Create executive dashboards displaying KPIs, sales metrics, and performance indicators with drill-down capabilities for detailed analysis.

### Sales Analysis Reports
Analyze sales data across multiple dimensions (time, geography, product) with the ability to drill into specific regions or time periods.

### Financial Reporting
Display financial data from OLAP cubes with conditional formatting to highlight variances, trends, and exceptional values.

### Multi-Dimensional Data Exploration
Enable users to explore complex datasets by drilling through hierarchies and pivoting dimensions interactively.

### Data Export and Distribution
Generate formatted reports in Excel, Word, or PDF for distribution to stakeholders who need offline access to OLAP analysis.

### Comparative Analysis
Compare multiple measures (actual vs. budget, current vs. prior year) across different dimensions with Excel-like layout and member properties.

---
