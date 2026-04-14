---
name: syncfusion-wpf-olap-client
description: Implement Syncfusion WPF OLAP Client for business intelligence and multidimensional data analysis. Use this when working with OLAP Client controls, cube analysis, SSAS (SQL Server Analysis Services), or multidimensional data. Covers cube database connections (offline cubes, Mondrian, ActivePivot), dimension browsing, OLAP reports, drill-through operations, and KPI analysis.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF OLAP Client

The OLAP Client control for WPF provides comprehensive business intelligence capabilities for browsing and analyzing multidimensional data organized as dimensions, measures, named sets, and KPIs in cube format. It visualizes results in both graphical (chart) and tabular (grid) formats, and allows creating and editing reports on-the-fly for later use.

## When to Use This Skill

Use this skill when you need to:

- **Analyze multidimensional data** from OLAP cube databases (SSAS, Mondrian, ActivePivot)
- **Browse cube structures** with dimensions, hierarchies, measures, and KPIs
- **Create business intelligence dashboards** combining charts and grids
- **Build OLAP reports** with drag-and-drop axis configuration
- **Connect to cube data sources** (offline cubes, local/online SQL Server, Mondrian, ActivePivot)
- **Filter and slice data** by members, values, or subsets
- **Sort and page** large multidimensional datasets
- **Export BI reports** to PDF, Excel, Word, CSV, or images
- **Implement drill-through** operations for detailed data exploration
- **Work with named sets and calculated members** in OLAP reports
- **Save and load report definitions** for reusable analysis

## Component Overview

The OLAP Client is a comprehensive BI control that includes:

- **Cube Selector**: Switch between multiple cubes in a data source
- **Cube Dimension Browser**: Tree view of measures, dimensions, hierarchies, levels, named sets, and KPIs
- **Axis Element Builder**: Configure categorical (column), series (row), and slicer axes through drag-and-drop
- **Member/Measure Editors**: Select specific members or measures for filtering
- **Toolbar**: Quick access to report management, filtering, sorting, and export operations
- **OLAP Grid**: Tabular representation of multidimensional data
- **OLAP Chart**: Graphical visualization of cube data

### Install the NuGet Package

To use the OLAP Client in your WPF application, install the following NuGet package:

```powershell
Install-Package Syncfusion.OlapClient.WPF
```

### Important Note

**The Syncfusion OlapClient control is supported only in .NET Framework WPF applications. It is not supported in .NET Core.** Please create a Framework-based WPF sample when working with OlapClient.

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and required assemblies
- Three setup methods: Visual Studio designer, Expression Blend, Code-behind
- Adding OLAP Client to WPF applications
- OlapDataManager configuration
- Connection string basics
- First DataBind and rendering

### Data Binding and Connections
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding to offline cube files (.cub)
- Connecting to local SQL Server Analysis Services
- Connecting to online SQL Server via XML/A
- Connecting to Mondrian Server
- Connecting to ActivePivot Server
- Connection string patterns and authentication

### OLAP Client Elements and UI Components
📄 **Read:** [references/olap-client-elements.md](references/olap-client-elements.md)
- Cube Selector for switching cubes
- Cube Dimension Browser structure and node types
- Attribute hierarchies vs user-defined hierarchies
- Axis Element Builder (categorical, series, slicer)
- Drag-and-drop operations for building reports
- Measure Editor and Member Editor dialogs
- Toolbar features and report manipulation
- OLAP Grid and OLAP Chart integration

### Filtering Data
📄 **Read:** [references/filtering.md](references/filtering.md)
- Filtering by member (check/uncheck selection)
- Filtering by value (column and row filters)
- Filter expressions and conditions
- Subset filtering to limit record counts
- ShowFilterSortButtons and ShowSubsetFilters properties

### Sorting Data
📄 **Read:** [references/sorting.md](references/sorting.md)
- Row and column sorting
- Sorting dialog and options
- Sort order configuration
- Combining sorting with filtering

### Paging Large Datasets
📄 **Read:** [references/paging.md](references/paging.md)
- Enabling and configuring paging
- Page size settings
- Pager customization
- Performance optimization for large cubes

### Exporting Reports
📄 **Read:** [references/exporting.md](references/exporting.md)
- Export to PDF, Excel, Word, CSV, and images
- Exporting OLAP Grid vs OLAP Chart
- Export options and configuration
- Programmatic export methods
- Toolbar export buttons

### Drill-Through Operations
📄 **Read:** [references/drill-through.md](references/drill-through.md)
- Drill-down through dimension hierarchies
- Enabling expander options
- ShowExpander property
- Drill-through dialog for detailed data

### Named Sets
📄 **Read:** [references/named-sets.md](references/named-sets.md)
- Understanding named sets in cubes
- Using named sets in reports
- Dragging named sets to axes
- Named set vs dimension differences

### Calculated Members
📄 **Read:** [references/calculated-members.md](references/calculated-members.md)
- Creating custom calculations
- MDX expressions for calculated members
- Adding calculated members to reports
- Common calculation patterns

### Report Management and Serialization
📄 **Read:** [references/report-management.md](references/report-management.md)
- Creating, adding, removing, and renaming reports
- Saving reports to local system (XML files)
- Loading reports from local system
- Report collections and navigation
- Serializing reports as XML
- Storing and retrieving reports as streams
- MDX query display

## Quick Start Example

### Basic OLAP Client Setup

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="OLAP Client" Height="700" Width="900">
    <Grid>
        <syncfusion:OlapClient x:Name="olapClient1" 
                               HorizontalAlignment="Stretch" 
                               VerticalAlignment="Stretch"/>
    </Grid>
</Window>
```

### Code-Behind Connection

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Windows.Client.Olap;

namespace OlapClientApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            // Defer initialization that depends on visual tree until Loaded
            this.Loaded += MainWindow_Loaded;
        }

        private void MainWindow_Loaded(object sender, RoutedEventArgs e)
        {
            // Example connection string (replace with your server or offline cube path)
            string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=http://<your-server-name>/olap/msmdpump.dll; " + "Initial Catalog=<YourCatalogName>;";
            // Create OlapDataManager with connection and assign to control
            OlapDataManager olapDataManager = new OlapDataManager(connectionString);
            this.olapClient1.OlapDataManager = olapDataManager;
            this.olapClient1.DataBind();
        }
    }
}
```

## Common Patterns

### Pattern 1: Connecting to Local Offline Cube

```csharp
// For offline .cub files on local machine
string connectionString = "YOUR_END_POINT"; // For e.g - @"Datasource=<path_to_cube_file>; Provider=msolap;";
OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

### Pattern 2: Connecting to Local SQL Server

```csharp
// For SQL Server Analysis Services on localhost
string connectionString = "YOUR_END_POINT"; // For e.g - "Data source=localhost; Initial Catalog=Adventure Works DW";
OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

### Pattern 3: Connecting to Mondrian Server

```csharp
// For Mondrian OLAP server
string connectionString = "YOUR_END_POINT"; // For e.g - @"Data Source = http://<your-server-name>:<port>/mondrian/xmla; Initial Catalog = <YourCatalogName>;";
OlapDataManager olapDataManager = new OlapDataManager(connectionString);
// Set provider to Mondrian
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

### Pattern 4: Creating OLAP Client Programmatically

```csharp
// Complete code-behind instantiation
OlapClient olapClient1 = new OlapClient();
string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=http://<your-server-name>/olap/msmdpump.dll; " + "Initial Catalog=<YourCatalogName>;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
olapClient1.OlapDataManager = olapDataManager;
olapClient1.DataBind();

// Add to window grid
grid.Children.Add(olapClient1);
```

### Pattern 5: Hiding Filter/Sort Buttons

```csharp
// Control toolbar button visibility
this.olapClient1.ShowFilterSortButtons = false;
this.olapClient1.ShowSubsetFilters = false;
```

## Key Components Overview

### OlapDataManager
The core data manager that handles:
- Connection details and cube authentication
- Current report management
- Cube name and schema information
- Pivot engine for rendering

**Usage:**
```csharp
OlapDataManager olapDataManager = new OlapDataManager(connectionString);
```

### Axis Element Builder
Drag-and-drop interface for report construction with three axes:
- **Categorical (Column)**: Elements displayed along chart y-axis and grid columns
- **Series (Row)**: Elements displayed along chart x-axis and grid rows
- **Slicer**: Filter to narrow data focus without displaying on chart/grid

### Cube Dimension Browser
Tree view organizing cube elements:
- Measures (quantitative analysis)
- Dimensions (data categorization)
- Hierarchies (attribute and user-defined)
- Levels (hierarchy depth)
- Named Sets (predefined tuple collections)

## Common Use Cases

### Business Intelligence Dashboards
Create interactive BI dashboards combining OLAP Grid and OLAP Chart for comprehensive data analysis with filtering, sorting, and drill-down capabilities.

### Ad-Hoc Reporting
Enable end users to build custom reports on-the-fly by dragging dimensions and measures to axes, applying filters, and saving reports for later use.

### Sales Analysis
Analyze sales data across multiple dimensions (time, geography, product categories) with ability to slice/dice data and drill through to detailed transactions.

### Financial Reporting
Build financial reports with calculated members for custom metrics, named sets for grouped analysis, and export capabilities for sharing.

### Multi-Cube Analysis
Connect to databases with multiple cubes and switch between them using the cube selector for different analytical perspectives.

### Offline Analysis
Work with offline cube files for scenarios requiring disconnected data analysis without continuous server connection.

## Required Assemblies

When setting up the OLAP Client, ensure these Syncfusion assemblies are referenced:

- Syncfusion.Grid.Wpf
- Syncfusion.Olap.Base
- Syncfusion.Chart.Wpf
- Syncfusion.OlapChart.Wpf
- Syncfusion.OlapGrid.Wpf
- Syncfusion.OlapGridCommon.Wpf
- Syncfusion.OlapClient.Wpf
- Syncfusion.OlapShared.Wpf
- Syncfusion.OlapTools.Wpf

