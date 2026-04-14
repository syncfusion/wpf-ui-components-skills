# Getting Started with WPF OLAP Grid

## Table of Contents
- [Overview and Prerequisites](#overview-and-prerequisites)
- [Licensing Requirements](#licensing-requirements)
- [Assembly References](#assembly-references)
- [Installation Methods](#installation-methods)
  - [Through Visual Studio](#through-visual-studio)
  - [Through Expression Blend](#through-expression-blend)
  - [Through Code-Behind](#through-code-behind)
- [Basic Implementation](#basic-implementation)
- [First Run and Verification](#first-run-and-verification)

## Overview and Prerequisites

The WPF OLAP Grid is a business intelligence control that displays multi-dimensional data from OLAP cubes. This guide covers the complete setup process, from adding assemblies to creating your first data-bound OLAP Grid.

**What you'll create:** A basic OLAP Grid displaying data from an OLAP cube with dimensions, measures, and drill-down capabilities.

**Prerequisites:**
- Visual Studio or Expression Blend
- WPF Application project
- Access to an OLAP data source (SSAS server or offline cube)
- Syncfusion WPF assemblies installed


## Assembly References

### Install the NuGet Package

To use the OLAP Grid in your WPF application, install the following NuGet package:

```powershell
Install-Package Syncfusion.OlapGrid.WPF
```

### Required Assemblies

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

## Installation Methods

There are three ways to add the OLAP Grid to your WPF application:

### Through Visual Studio

This is the quickest method using the Visual Studio toolbox.

**Steps:**

1. **Create WPF Application:**
   - Open Visual Studio
   - Go to **File > New > Project**
   - Select **WPF Application** (inside Visual C# Templates)
   - Create the project

2. **Add from Toolbox:**
   - Go to **View > Toolbox**
   - Find the OLAP Grid under **Syncfusion BI WPF** tag
   - Drag and drop onto the designer
   - Required assemblies are added automatically

3. **Add XAML Name:**

```xaml
<Window x:Class="WpfApplication.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:OlapGrid Name="olapGrid1" />
    </Grid>
</Window>
```

4. **Add Required Namespaces (Code-Behind):**

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;
```

5. **Configure in Code-Behind:**

```csharp
public partial class MainWindow : Window
{
    private OlapDataManager olapDataManager = null;
    private string _connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=localhost; Initial Catalog=AdventureWorksDW";
    
    public MainWindow()
    {
        InitializeComponent();
        
        // Initialize OlapDataManager with connection string
        olapDataManager = new OlapDataManager(_connectionString);
        
        // Set the OLAP report
        olapDataManager.SetCurrentReport(CreateOlapReport());
        
        // Bind to OlapGrid
        this.olapGrid1.OlapDataManager = olapDataManager;
        this.olapGrid1.DataBind();
    }
    
    private OlapReport CreateOlapReport()
    {
        OlapReport olapReport = new OlapReport();
        
        // Set cube name
        olapReport.CurrentCubeName = "Adventure Works";
        
        // Configure column dimension
        DimensionElement dimensionElementColumn = new DimensionElement();
        dimensionElementColumn.Name = "Customer";
        dimensionElementColumn.AddLevel("Customer Geography", "Country");
        
        // Configure measure
        MeasureElements measureElementColumn = new MeasureElements();
        measureElementColumn.Elements.Add(new MeasureElement { Name = "Reseller Sales Amount" });
        
        // Configure row dimension
        DimensionElement dimensionElementRow = new DimensionElement();
        dimensionElementRow.Name = "Date";
        dimensionElementRow.AddLevel("Fiscal", "Fiscal Year");
        
        // Add to report axes
        olapReport.CategoricalElements.Add(dimensionElementColumn);
        olapReport.CategoricalElements.Add(measureElementColumn);
        olapReport.SeriesElements.Add(dimensionElementRow);
        
        return olapReport;
    }
}
```

### Through Expression Blend

Use Blend for visual design and styling while setting up the OLAP Grid.

**Steps:**

1. **Create Project:**
   - Open Blend for Visual Studio
   - Go to **File > New Project > WPF > WPF Application**

2. **Add Assembly References:**
   - Select **Project** tab (left corner)
   - Right-click **References**
   - Select **Add Reference**
   - Browse and add the six required assemblies listed above

3. **Add from Assets:**
   - After adding assemblies, OLAP Grid appears in **Assets** tab
   - Select **Assets** tab
   - Drag OLAP Grid to the designer

4. **Configure XAML:**

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="WpfApplication.MainWindow">
    <Grid>
        <syncfusion:OlapGrid Name="olapGrid1"/>
    </Grid>
</Window>
```

5. **Code-Behind Setup:**

Use the same code-behind configuration as the Visual Studio method above.

### Through Code-Behind

For dynamic creation or programmatic control, create the OLAP Grid entirely in code.

**Steps:**

1. **Create WPF Application** in Visual Studio

2. **Add Assembly References Manually:**
   - Right-click **References**
   - Select **Add Reference**
   - Browse to the default assembly location
   - Add all six required assemblies

3. **Add Namespaces:**

```csharp
using Syncfusion.Windows.Grid.Olap;
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;
```

4. **Create Grid Programmatically:**

```csharp
namespace WpfApplication1
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Instantiate OlapGrid
            OlapGrid olapGrid1 = new OlapGrid();
            
            // Configure connection string
            string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=localhost; Initial Catalog=AdventureWorksDW";
            
            // Create OlapDataManager
            OlapDataManager olapDataManager = new OlapDataManager(connectionString);
            
            // Set OLAP report
            olapDataManager.SetCurrentReport(CreateOlapReport());
            
            // Bind to grid
            olapGrid1.OlapDataManager = olapDataManager;
            olapGrid1.DataBind();
            
            // Add to window (assuming a Grid named 'grid' exists in XAML)
            grid.Children.Add(olapGrid1);
        }
        
        private OlapReport CreateOlapReport()
        {
            OlapReport olapReport = new OlapReport();
            
            // Set cube name
            olapReport.CurrentCubeName = "Adventure Works";
            
            // Column dimension
            DimensionElement dimensionElementColumn = new DimensionElement();
            dimensionElementColumn.Name = "Customer";
            dimensionElementColumn.AddLevel("Customer Geography", "Country");
            
            // Measure
            MeasureElements measureElementColumn = new MeasureElements();
            measureElementColumn.Elements.Add(new MeasureElement { Name = "Reseller Sales Amount" });
            
            // Row dimension
            DimensionElement dimensionElementRow = new DimensionElement();
            dimensionElementRow.Name = "Date";
            dimensionElementRow.AddLevel("Fiscal", "Fiscal Year");
            
            // Build report
            olapReport.CategoricalElements.Add(dimensionElementColumn);
            olapReport.CategoricalElements.Add(measureElementColumn);
            olapReport.SeriesElements.Add(dimensionElementRow);
            
            return olapReport;
        }
    }
}
```

## Basic Implementation

### Understanding the Workflow

The OLAP Grid follows this data flow:

1. **OlapDataManager** connects to OLAP data source via connection string
2. **OlapReport** defines what data to display (cube, dimensions, measures)
3. **OlapGrid** binds to OlapDataManager and displays the data

### Connection String Patterns

**For SSAS Server:**
```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=<server>; Initial Catalog=<database>";
// Example:
string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=localhost; Initial Catalog=AdventureWorksDW";
```

**For Offline Cube:**
```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - "DataSource=<path_to_cube_file>";
// Example:
string connectionString = "YOUR_END_POINT"; // For e.g - "DataSource=C:\\Cubes\\AdventureWorks.cub";
```

### VB.NET Example

```vbnet
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Olap.Reports

Partial Public Class MainWindow
    Inherits Window
    
    Private olapDataManager As OlapDataManager = Nothing
    Private _connectionString As String = "YOUR_END_POINT"; ' For e.g - "Data Source=localhost; Initial Catalog=AdventureWorksDW"
    
    Public Sub New()
        InitializeComponent()
        
        ' Create OlapDataManager
        olapDataManager = New OlapDataManager(_connectionString)
        
        ' Set report
        olapDataManager.SetCurrentReport(CreateOlapReport())
        
        ' Bind to grid
        Me.olapGrid1.OlapDataManager = olapDataManager
        Me.olapGrid1.DataBind()
    End Sub
    
    Private Function CreateOlapReport() As OlapReport
        Dim olapReport As OlapReport = New OlapReport()
        olapReport.CurrentCubeName = "Adventure Works"
        
        Dim dimensionElementColumn As DimensionElement = New DimensionElement()
        dimensionElementColumn.Name = "Customer"
        dimensionElementColumn.AddLevel("Customer Geography", "Country")
        
        Dim measureElementColumn As MeasureElements = New MeasureElements()
        measureElementColumn.Elements.Add(New MeasureElement With {.Name = "Reseller Sales Amount"})
        
        Dim dimensionElementRow As DimensionElement = New DimensionElement()
        dimensionElementRow.Name = "Date"
        dimensionElementRow.AddLevel("Fiscal", "Fiscal Year")
        
        olapReport.CategoricalElements.Add(dimensionElementColumn)
        olapReport.CategoricalElements.Add(measureElementColumn)
        olapReport.SeriesElements.Add(dimensionElementRow)
        
        Return olapReport
    End Function
End Class
```

## First Run and Verification

After completing setup, run your application:

**Expected Output:**
- Grid displays with row and column headers
- Dimension members shown in headers
- Measure values displayed in data cells
- Drill indicators (+ icons) on expandable members
- Summary cells calculated and positioned

**Common Issues:**

**No Data Displayed:**
- Verify connection string is correct
- Check cube name matches exactly
- Ensure dimension and measure names are valid
- Verify data exists for the specified query

**Assembly Loading Errors:**
- Confirm all six required assemblies are referenced
- Check assembly versions match
- Verify licensing key is registered (for licensed versions)

**Runtime Exceptions:**
- Connection string might be invalid
- OLAP server may be inaccessible
- Cube might not exist or have different name
- Dimension/measure names might be incorrect

## Next Steps

Now that you have a basic OLAP Grid running:

1. **Explore Layouts:** Try different grid layout options (see `grid-layouts.md`)
2. **Add Conditional Formatting:** Highlight important values (see `conditional-formatting.md`)
3. **Configure Drill Operations:** Enable drill-down navigation (see `drill-operations.md`)
4. **Export Functionality:** Add export to Excel, Word, or PDF (see `exporting.md`)
5. **Advanced Features:** Implement KPI, paging, and more (see respective reference files)

The OLAP Grid is now ready for customization and enhancement based on your business requirements.
